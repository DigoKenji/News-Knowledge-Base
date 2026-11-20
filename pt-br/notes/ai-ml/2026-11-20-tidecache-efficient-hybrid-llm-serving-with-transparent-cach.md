---
type: Paper Summary
title: 'TideCache: Serving Eficiente de LLMs Híbridos com Gerenciamento Transparente de Cache e Streaming por Camadas'
description: TideCache permite que servidores de modelos híbridos realoquem memória de GPU entre tipos de cache e transmitam prefixos a partir da memória do host, concluindo até 2,04× mais turnos de agentes.
resource: https://arxiv.org/abs/2611.26619
tags:
- ai-ml
- kv-cache
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.26619
  title: 'TideCache: Efficient Hybrid LLM Serving with Transparent Cache Management and Layer-Wise Streaming'
  author: arXiv
status: stable
source_url: https://arxiv.org/abs/2611.26619
source: arXiv
source_type: arxiv
published: '2026-11-19'
domain: ai-ml
concepts:
- kv-cache
- inference-tradeoffs
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d8165b5b5ef75b2fc91d5
lang: pt-br
translation_of: /notes/ai-ml/2026-11-20-tidecache-efficient-hybrid-llm-serving-with-transparent-cach.md
---

## 🎯 Em uma frase
O TideCache permite que um servidor de LLM híbrido mova memória de GPU entre tipos de cache e transmita prefixos de atenção global a partir da memória do host, concluindo até 2,04× mais turnos de agentes do que o SGLang com HiCache em sete configurações de modelo e plataforma.

## ❓ O problema
Modelos de pesos abertos hoje misturam algumas camadas de atenção global, cujo cache KV (as chaves e valores armazenados dos tokens passados) cresce com o tamanho do contexto, com muitas camadas de estado limitado, como atenção com janela deslizante ou camadas recorrentes (Mamba, Gated DeltaNet, Kimi Delta Attention), cuja memória por requisição é fixa. Cada tipo de cache precisa de uma parcela diferente da memória de uma GPU, dependendo do tamanho do contexto e de quantas requisições estão rodando. Uma divisão estática deixa um pool cheio enquanto o outro fica ocioso, e o compartilhamento dinâmico é difícil de integrar porque altera o layout de memória do qual os kernels de atenção e a indexação do cache dependem. A descarga para a memória do host ajuda, mas os esquemas existentes mantêm os prefixos restaurados na GPU durante todo o turno, o que limita quantas sessões rodam ao mesmo tempo.

## 💡 A ideia
A observação-chave é que as camadas de estado limitado criam intervalos de computação durante os quais o KV histórico da próxima camada de atenção global ainda não é necessário. Durante o decode, o enlace CPU-para-GPU transporta quase nada, então o sistema pode copiar o prefixo de cada camada de atenção global, que reside no host, para um pequeno buffer na GPU logo antes de essa camada rodar, e reutilizar esse buffer na camada seguinte. A memória de GPU liberada pode guardar caches de janela, estados recorrentes e mais requisições. O remapeamento em nível de página mantém cada cache em endereços virtuais fixos, então os backends de atenção não precisam mudar.

## ⚙️ Como funciona
### Transparent Cache Pool (TCP)
O TCP gerencia uma arena de páginas de GPU, alocada uma vez em páginas de 2 MiB pela API de memória virtual da CUDA. Cada componente de cache (KV global, KV de janela, estado recorrente) recebe um subpool: uma faixa de endereços virtuais fixa, apoiada por um número variável de páginas físicas. Para crescer um subpool, o TCP mapeia mais páginas da arena na sua faixa; para encolhê-lo, desmapeia as páginas que nenhuma requisição ativa usa. O rebalanceamento roda uma vez por época de E = 8 passagens forward, com no máximo um movimento em andamento. Uma thread de trabalho faz o desmapeamento e o mapeamento fora do caminho das passagens forward, e todos os ranks aplicam o movimento na mesma época, para que seus lotes continuem idênticos. As fontes de página são tentadas em ordem crescente de custo: páginas livres, linhas ociosas de outros subpools e, por fim, prefixos de cache frios que não foram tocados há τ segundos. Prefixos quentes nunca são recuperados, porque descartar um que será reutilizado após uma pausa de ferramenta força um recálculo.

### Cache Streamer
O subpool de KV global tem duas regiões. A região residente guarda os prefixos das requisições que não são transmitidas, além de todo o KV recém-gerado. A região de streaming mapeia as L camadas de atenção global em M = 2 slots físicos, com a camada ℓ usando o slot (ℓ − 1) mod M, de modo que um prefixo transmitido ocupa na GPU apenas M/L da sua pegada completa por camada. A cada passo, o streamer atribui prefixos transmitidos a linhas contíguas de slots, junta linhas adjacentes do host em cópias contínuas e as submete com cudaMemcpyBatchAsync em um stream H2D separado. Duas flags por camada (FREE e READY), verificadas dentro do grafo CUDA, ordenam cada cópia depois do leitor anterior e cada leitura de atenção depois de sua cópia, de modo que o grafo não é dividido em segmentos. O artigo chama isso de Graph-Compatible Handshake; ele evita tanto as bolhas de lançamento da divisão do grafo quanto a largura de banda desperdiçada ao copiar um intervalo fixo.

A limitação de capacidade vem desta relação: transmitir A linhas exige L·A·b bytes por passo, e as transferências precisam ficar escondidas sob a computação, então L·A·b ≤ η·BW·T. A Equação 1 do artigo dá:

```text
C = η · BW · T / (L · b)        C: streamed rows per step that can be hidden
r = C · (1 − M/L) / D           r: relative capacity gain
```

Símbolos: η é um fator de folga de escalonamento menor que 1; BW é a largura de banda sustentada do enlace; T é o tempo de computação por passo de decode; L é o número de camadas de atenção global; b é o número de bytes por linha e camada; M é o número de slots de streaming; D é a capacidade em linhas de um pool de KV global totalmente residente com o mesmo orçamento de GPU. Os slots custam o equivalente a M·C/L linhas residentes, então o ganho líquido é C·(1 − M/L), e a capacidade efetiva chega a (1 + r)·D. Os autores observam que este é um limite agregado de largura de banda: a transferência de cada camada também precisa cumprir o seu próprio prazo.

Exemplo trabalhado da ablação do GPT-OSS-120B: um lote de 29 requisições com contextos de 8K em GH200 mantém 8,76 GB de KV global em BF16 distribuídos por 18 camadas de atenção global. Mover o lote inteiro a 400 GB/s leva 21,9 ms por passo, mais do que o passo de decode medido de 19,7 ms, então apenas um subconjunto das requisições pode ser transmitido. Com L = 18, b = 2 KiB, M = 2, D = 147.648, T = 13,16 ms, BW = 400 GB/s e η = 0,9, o limite é C ≈ 128,5K e o ganho de capacidade é 77%. A configuração usada tem 126.976 linhas de streaming mais 133.056 linhas residentes, resultando em 260.032 tokens após o arredondamento de página, 1,76× a capacidade original.

### Request Scheduler
O escalonador decide como admitir a requisição do topo da fila de espera do SGLang. Para essa requisição, ele lê seus slots de estado ns, suas linhas de KV global ainda não em cache nf e o comprimento h do seu prefixo em cache no host. A execução residente precisa de nf + h linhas na região residente. A execução transmitida mantém nf linhas residentes e usa h linhas de streaming, desde que o total de linhas transmitidas fique dentro de C. Uma requisição só é transmitida quando a região residente está disputada: uma estimativa de PRESSURE projeta a demanda a partir das requisições em espera, do crescimento do decode sobre o comprimento médio de saída e das chegadas durante um turno. Prefixos menores que hmin não valem a pena para transmissão. Se nenhuma das opções couber, o escalonador nomeia o subpool bloqueante, e o TCP o cresce entre as passagens forward. Uma requisição selecionada para streaming pula aquela iteração se os slots estiverem cheios, para que requisições que precisam de outros recursos possam prosseguir.

## 🧪 Evidências
| Configuração | Linha de base | Este artigo | Métrica |
|---|---|---|---|
| Sete configurações, GB300 e GH200, misturas de chat e de agentes de programação | SGLang w/ HiCache (ajustado) | Até 2,04× (GH200); até 1,44× (GB300) | Aumento de turnos de agentes por segundo |
| Sete configurações | SGLang w/ HiCache (ajustado) | 20–59% menor | Latência P50 por token geral |
| Sete configurações | SGLang w/ HiCache (ajustado) | 1,40–3,05× | TTFT P50 de agentes |
| Carga em mudança, Qwen3.8-27B, GH200, 900 s | SGLang w/ HiCache | 183 vs 115 turnos (59% a mais) | Turnos de agentes concluídos |
| Carga em mudança, mesma execução | SGLang w/ HiCache | 39,3 ms vs 63,4 ms (38% menor) | Latência P50 geral por token |
| Carga em mudança, mesma execução | SGLang w/ HiCache | 5,68 s vs 25,70 s (78% menor) | TTFT P50 de agentes |
| Kimi K3, varredura da razão de divisão | Melhor divisão testada do HiCache (razão 3) | 94 vs 67 turnos (40% acima) | Turnos concluídos |
| Somente GPU, Kimi K3 e GLM-5.3-Flash | Melhor de duas partições fixas | 96,3–113,6% | Turnos/s |
| GPT-OSS-120B, GH200, contextos de 8K | SGLang w/ HiCache | Lote de 16 a 29; 148K a 260K tokens | Maior lote sustentado; capacidade efetiva de KV global |
| Mesma ablação | SGLang w/ HiCache | 1,19K a 1,47K tokens/s (ganho de 24%) | Vazão máxima de decode |
| Tamanho de lote igual, todos os lotes sustentados | SGLang w/ HiCache | −1,1% a +6,3% | Tempo do passo de decode |

Os resultados vêm de misturas sintéticas de traços do ShareGPT e do Claude Code em um nó de 4 GPUs GB300 e em uma GH200, com janelas de medição de 600 s (900 s para a carga em mudança) após o aquecimento. As linhas de base são ajustadas: os autores varrem as razões de divisão e relatam a melhor configuração do HiCache, e a varredura do Kimi K3 mostra que a melhor linha de base ainda fica 40% atrás do TideCache em turnos. As ablações mostram que o TCP sozinho acompanha a melhor partição fixa, e que o streaming acrescenta turnos na carga em mudança (183 vs 174). O artigo reporta três execuções apenas para a medição de vazão de decode; os números de ponta a ponta vêm de execuções únicas por configuração.

## ⚠️ Limitações
- Os autores afirmam que o limite de streaming é agregado e que a transferência de cada camada ainda precisa cumprir seu prazo.
- Na revisão avaliada, o Unified Memory Pool do SGLang não se integrava ao HiCache, então a comparação é apenas em GPU; o UMP também não é suportado no backend DSA do GLM (N/A).
- Kimi K3 e Qwen3.8-2.4T foram executados apenas com as primeiras 48 e 36 camadas, para caber na HBM.
- A Figura 9 usa um único ponto de carga por configuração, escolhido onde os prefixos de sessão excedem a capacidade residente de KV global.
- A repartição não consegue elevar o orçamento total de cache da GPU; quando a demanda combinada excede a HBM, ela apenas muda quais requisições esperam.
- 🔎 Contexto adicional: todas as cargas são misturas sintéticas construídas a partir de traços públicos, em duas plataformas superchip de nó único. O artigo não testa múltiplos nós, multi-tenant ou tráfego de produção.
- 🔎 Contexto adicional: o limite da Equação 1 é linear na largura de banda do enlace. A Tabela 1 lista 128 GB/s para PCIe 5.0 ×16 contra 900 GB/s para NVLink-C2C, então o mesmo design esconderia bem menos linhas transmitidas em um host PCIe. O artigo não avalia sistemas PCIe.

## 🏗️ O que isso significa para construir sistemas
Esta é uma interpretação, não uma afirmação dos autores. O design fica na camada de pool de memória e de admissão de um motor de inferência, abaixo do radix cache existente e da camada de cache no host, então se aplicaria a uma equipe que roda modelos híbridos de pesos abertos em hardware com um enlace CPU-GPU rápido. Os custos são um worker de remapeamento, slots com buffer duplo e cerca de 9 mil linhas adicionadas ao SGLang, além de um escalonador que precisa de estimativas de tempo de execução precisas da demanda. Para confiar nele, seria preciso reproduzir os ganhos com a sua própria mistura de tráfego e medir a largura de banda host-para-GPU que o seu hardware sustenta de fato. A lição transferível é dimensionar a memória de modelos com cache misto pela regra de crescimento de cada tipo de cache (por token ou por requisição) e contar os enlaces ociosos como capacidade.

## 🔑 Principais conclusões
- Modelos híbridos têm dois formatos de cache: o KV global cresce com o contexto, enquanto os estados limitados são fixos por requisição. Uma divisão estática da GPU pode bloquear requisições que o outro pool poderia atender.
- Um único arena com endereços virtuais fixos permite que o TCP mova páginas entre tipos de cache sem alterar os backends de atenção nem o HiCache.
- O decode deixa o enlace CPU-GPU quase ocioso, e as camadas de estado limitado fornecem a computação necessária para esconder a transferência do próximo prefixo de atenção global.
- O streaming troca largura de banda do enlace por memória de GPU. A Equação 1 dimensiona o limite escondido; na ablação do GPT-OSS-120B, a capacidade efetiva de KV global sobe de 148K para 260K tokens.
- Os ganhos foram medidos contra uma linha de base ajustada, em duas plataformas superchip, com tráfego sintético, então se aplicam com mais clareza a cargas de agentes limitadas por memória.
- A sobrecarga medida no passo de decode fica entre −1,1% e +6,3% nos tamanhos de lote que os dois sistemas sustentam, então o custo de execução relatado é pequeno.

## 📚 Plano de leitura
### Do artigo
- [SGLang HiCache](https://www.lmsys.org/blog/2025-09-10-sglang-hicache/) — a camada de cache no host que o TideCache estende e usa como principal linha de base.
- [Pie: Pooling CPU memory for LLM inference](https://arxiv.org/abs/2411.09317) — troca de KV por camadas entre host e dispositivo; o design de descarga mais próximo na comparação.
- [HiSparse](https://arxiv.org/abs/2608.07009) — busca entradas de KV selecionadas durante o decode, especializado em atenção esparsa.
- [Jenga](https://arxiv.org/abs/2503.18292) — um alocador de dois níveis para caches heterogêneos, a alternativa de layout de software mais próxima.
- [SuperInfer](https://arxiv.org/abs/2601.20309) — faz rodízio de requisições pela GPU em superchips; outra referência de descarga voltada a superchips.
- [Relatório técnico do Ring-2.6](https://arxiv.org/abs/2606.15079) — o modelo híbrido de 1T cujos tamanhos de cache motivam o design.
- [SGLang](https://arxiv.org/abs/2312.07104) — o motor de serving e o radix cache sobre os quais o TideCache é construído.

### Para aprofundar
- vAttention: Dynamic memory management for serving LLMs without PagedAttention (ASPLOS 2025) — a primitiva de memória virtual da CUDA que o TideCache reutiliza entre componentes de cache.
- vLLM Hybrid KV Cache Manager (documentação de design do vLLM) — o design concorrente de compartilhamento dinâmico, com páginas de tamanho uniforme compartilhadas.
- "Unified memory in SGLang" (blog do SGLang, 2026) — a abordagem de compartilhamento de buffers de bytes que o artigo audita contra o HiCache.
- CUDA driver API: Virtual memory management (documentação da NVIDIA) — necessária para acompanhar os passos de mapeamento e desmapeamento do TCP.
- Prism: Unleashing GPU sharing for cost-efficient multi-LLM serving (2025) — remapeia páginas físicas entre modelos em uma mesma GPU.
