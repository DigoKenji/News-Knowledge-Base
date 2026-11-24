---
type: Paper Summary
title: 'LiveLoRA: migração exata de KV cache entre atualizações de LoRA para agentes LLM'
description: LiveLoRA migra sessões ativas de agentes para um novo adaptador LoRA sem reexecutar o histórico, alcançando um ganho de velocidade de publicação de 68.98x em um benchmark de 32 sessões.
resource: https://arxiv.org/abs/2611.31105
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.31105
  title: 'LiveLoRA: Exact KV Cache Migration Across LoRA Updates for LLM Agents'
  author: Tianyi Huang, Samuel Xu, Ivy Gu, Nathan Huang, Siqi Zhang
status: stable
source_url: https://arxiv.org/abs/2611.31105
source: arXiv
source_type: arxiv
published: '2026-11-23'
domain: ai-ml
concepts:
- kv-cache
- rope
- ai-agent
- inference-tradeoffs
- lora
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d81b8a6ddddfb69a23a84
lang: pt-br
translation_of: /notes/ai-ml/2026-11-24-livelora-exact-kv-cache-migration-across-lora-updates-for-ll.md
---

## 🎯 Em uma frase
Sob premissas declaradas, o LiveLoRA move o estado de atenção em cache de cada sessão ativa de agente para uma nova versão do adaptador LoRA sem reexecutar seu histórico, alcançando um ganho mediano de velocidade na publicação de 68.98× sobre o re-prefill com histórico completo, para 32 sessões de 8K tokens cada.

## ❓ O problema
Um agente LLM mantém uma conversa longa, e enquanto roda o modelo mantém um **KV cache**: as chaves e os valores de atenção calculados para cada token passado, de modo que cada novo token não recalcula todo o histórico. Um **adaptador LoRA** é um pequeno conjunto de matrizes treináveis de baixo posto somadas a pesos congelados do modelo, então atualizar um adaptador muda os números que produziram o cache construído sob a versão antiga. Continuar com esse cache mistura duas versões do modelo, então a solução simples é o **re-prefill**: passar todo o histórico pelo modelo de novo sob o novo adaptador. Esse custo cresce com o tamanho do histórico e com o número de sessões ativas, e os autores afirmam que ele pode dominar a latência de publicação. Eles observam que trabalhos relacionados reutilizam ou invalidam caches entre adaptadores, mas não estudam mover o cache de uma sessão ativa de uma versão publicada para a seguinte.

## 💡 A ideia
A observação-chave é que a entrada do bloco decodificador final de cada token não muda quando apenas o adaptador do bloco final muda. Se as projeções de redução de chave e valor do adaptador forem fixas, o cache de cada token pode ser armazenado como as coordenadas compactas de baixo posto dessa entrada invariante, mais as projeções base congeladas. Uma nova versão do adaptador pode então reconstruir todas as chaves e valores históricos com produtos de matrizes, e só o último token precisa de uma passagem pelo bloco final para produzir novos logits. Os autores enquadram isso como a camada de execução de "um sistema operacional para agentes": uma versão de parâmetros fica visível junto com o estado de inferência necessário para executá-la. O adaptador, o cache e os logits são confirmados juntos, então os três se movem como uma única versão.

## ⚙️ Como funciona
### Estrutura do adaptador do bloco final
Apenas o último dos L blocos decodificadores carrega adaptadores; embeddings, blocos anteriores, camadas de normalização e a cabeça de saída permanecem congelados. Para cada posição de token t, a entrada do bloco final é u_t = N_L(h_{L-1,t}), o estado oculto normalizado após o bloco L-1, e ela é a mesma para toda versão do adaptador. As projeções de query, de saída de atenção e de MLP usam LoRA padrão. Para chaves e valores, as reduções A_K e A_V são fixas (seguindo LoRA-FA) e apenas as expansões B_K e B_V mudam por versão:

```text
k_t = R_t( W_K u_t + b_K + s_K B_K A_K u_t )     (3)
v_t = W_V u_t + b_V + s_V B_V A_V u_t           (4)
```

W_K e W_V são as projeções base congeladas, b_K e b_V são vieses, s_K e s_V são escalas, e R_t é o mapa de posição rotacional (RoPE) aplicado por cabeça de atenção. Como A_K e A_V nunca mudam, as coordenadas A_K u_t e A_V u_t são idênticas entre as versões.

### Migração em forma fechada
Na primeira avaliação do token t, o runtime armazena a chave base antes do RoPE (k̃_t = W_K u_t + b_K), o valor base (v̄_t = W_V u_t + b_V) e as coordenadas z_K,t = A_K u_t e z_V,t = A_V u_t. Para migrar para uma versão candidata v', as chaves e os valores históricos são reconstruídos a partir dessas peças armazenadas:

```text
k_t^(v') = R_t( k̃_t + s_K B_K^(v') z_K,t )     (6)
v_t^(v') = v̄_t + s_V B_V^(v') z_V,t           (7)
```

B^(v') são as expansões da candidata. A reconstrução parte do estado base imutável em vez de acumular deltas de parâmetros, o que evita a acumulação de erro entre versões. Depois que cada chave e valor é reconstruído, o bloco L é reexecutado apenas na última posição com os adaptadores Q, O e MLP candidatos, o que fornece os logits candidatos. O Teorema 1 afirma que, em aritmética exata e para esta família de adaptadores, o resultado coincide com o re-prefill completo sob a versão candidata, e que a coincidência vale para toda continuação fixa; a prova é uma indução sobre os tokens futuros. O teorema não afirma igualdade bit a bit entre kernels de ponto flutuante.

### Protocolo de publicação
A publicação ocorre em três fases. Sob o lock de serving, o coordenador captura o adaptador, o conjunto de sessões e referências ao estado imutável de cada sessão. As chaves, os valores e os logits candidatos são preparados fora do lock e sincronizados na GPU. No commit, o coordenador toma o lock de novo, verifica que as referências e os prefixos capturados não mudaram, carrega o candidato e troca o adaptador e as referências de estado juntos; se um prefixo tiver mudado, a atualização é rejeitada. Uma exceção antes do commit deixa a versão antiga publicada. A garantia é apenas atomicidade de falhas dentro do processo; recuperação durável e publicação distribuída estão fora da implementação.

## 🧪 Evidências
| Configuração | Linha de base | Este artigo | Métrica |
|---|---|---|---|
| Publicação, 32 sessões x 8,192 tokens | Re-prefill completo: 4,534.46 ms | LiveLoRA: 65.71 ms | Ganho mediano pareado 68.98 [67.08, 71.39] |
| Publicação, 1 sessão x 32,700 tokens | Re-prefill completo: 919.21 ms | LiveLoRA: 18.03 ms | Ganho mediano pareado 51.06 [50.10, 52.07] |
| Publicação, 1 sessão x 2,048 tokens | Re-prefill completo: 40.78 ms | LiveLoRA: 17.15 ms | Ganho mediano pareado 2.38 [2.29, 2.48] |
| Decodificação, contexto de 8K | Decodificação nativa com cache: 20.829 ms/token | LiveLoRA: 21.818 ms/token | Custo adicional 4.75% (serving direto protegido: 0.81%) |
| Publicação mais decodificação, 8K, atualização a cada 32 tokens | Re-prefill completo: 1,706.36 ms | LiveLoRA: 1,429.29 ms | Economia de 16.24% |
| Publicação mais decodificação, 8K, atualização a cada 128 tokens | Re-prefill completo: 5,892.03 ms | LiveLoRA: 5,662.15 ms | Economia de 3.90% |
| Armazenamento de sessão, 8K | Replay de fronteira retida: 32.41 MiB | LiveLoRA compacto: 9.48 MiB | 3.42x menos armazenamento adicional |
| K/V obsoletos, sem migração | Discordância top-1 de 8.0-17.3% (seis casos de escala); 35.4% sob estresse | K/V migrados: erro máximo observado zero em relação à referência | Discordância top-1 e erro de K/V |
| Retenção de entrada normalizada, cadência 256 | Replay de fronteira retida: 15.95 ms de publicação | LiveLoRA normalizado: 6.55 ms de publicação | Latência de publicação; totais de 19,945.65 contra 19,940.61 ms, praticamente empatados |

As linhas de 68.98x e 51.06x medem apenas o custo de publicação, em um estudo de histórico fixo com atualizações gaussianas de adaptador e históricos de tokens aleatórios, e os autores afirmam que a qualidade da adaptação em nível de tarefa não foi avaliada. Quando a decodificação é incluída, a economia em relação ao re-prefill completo cai para 16.24% e 3.90% nos traces de 8K pareados, e os autores relatam custos dentro de 0.60% do replay de fronteira, usando 3.42x menos armazenamento. As verificações de correção são feitas em um modelo de 3B em uma GPU: as chaves e os valores migrados coincidem exatamente com a referência, e as diferenças de logits decorrem do kernel de atenção customizado, e não da matemática da migração.

## ⚠️ Limitações
- Apenas o bloco final é adaptado. A forma compacta exige projeções K/V de redução fixas; reduções treináveis exigem o armazenamento maior de entradas normalizadas, e adaptar blocos anteriores exige o replay do sufixo adaptado, cujo desempenho os autores não estabelecem.
- A qualidade da adaptação em nível de tarefa não foi avaliada. As atualizações são gaussianas e os históricos são de tokens aleatórios, escolhidos para isolar a migração de estado da regra de aprendizado.
- Os resultados são afirmações de aritmética exata. Em BF16 os logits migrados e os de referência diferem (por exemplo, diferença máxima de logit de 1.1875 na cadência de 32), e as latências absolutas não podem ser atribuídas a mudanças isoladas de código por causa da variação entre hosts.
- Um modelo (Qwen2.5-3B-Instruct) em uma GPU NVIDIA H200, com sequências contíguas, RoPE padrão, e atenção por janela deslizante e escalonamento de RoPE desativados.
- A coordenação é local ao processo. Caches paginados, serving distribuído e recuperação durável não estão integrados.
- As cifras de armazenamento são contagens lógicas de tensores, excluindo armazenamento compartilhado e espaços de trabalho de publicação, e os autores afirmam que não são picos do alocador.
- Os intervalos de atualização de 8K, de 32 tokens e de 128 tokens são pontos de sensibilidade, não estimativas da frequência de publicação em produção. As cifras de ponto de equilíbrio (cerca de 600 tokens por atualização contra o re-prefill, cerca de 30 contra o replay de fronteira) assumem custos constantes.
- O artigo não traz link para código nem para dados.
- 🔎 Contexto adicional: a Tabela 5 relata sobrecarga de decodificação de 19.17% a 24.24% no estudo original de histórico fixo, bem acima dos 4.75-4.88% do destaque da Tabela 2. Os autores usam runtimes e escopos diferentes para cada um e não conciliam os dois, então a sobrecarga do destaque não deve ser citada isoladamente.
- 🔎 Contexto adicional: a principal linha de base é o re-prefill com histórico completo, a opção mais fraca. Os autores mencionam o reuso de prefixo no SGLang como complementar, mas não o comparam, então o número de 68.98x não mostra o ganho sobre um cache de prefixo de produção.

## 🏗️ O que isso significa para construir sistemas
Esta é uma interpretação, não uma afirmação dos autores. Uma camada de serving para muitos agentes poderia colocar um registro de adaptadores e um coordenador de publicação na frente do gerenciador de KV cache, mantendo coordenadas compactas por sessão junto do cache normal, de modo que um novo adaptador troque as sessões sem um pico de re-prefill. A Tabela 5 mostra o custo: a 8K para uma sessão, o KV cache tem 288.0 MiB e o estado do LiveLoRA totaliza 299.2 MiB. Antes de confiar nisso, você precisaria de sua própria recuperação durável, pois a garantia é local ao processo, e de uma suíte de regressão que compare os logits migrados com o re-prefill denso no seu modelo e nos seus kernels. Também precisaria confirmar que seus adaptadores cabem na família de projeções fixas e medir o ganho contra o reuso de prefixo que você já usa.

## 🔑 Principais conclusões
- A publicação de um novo adaptador para uma sessão levou 18.03 ms com 32,700 tokens, contra 919.21 ms do re-prefill completo.
- O número de 68.98x é para 32 sessões de 8K tokens cada (65.71 ms contra 4,534.46 ms); o ganho em uma sessão com 2,048 tokens é de 2.38x.
- Incluindo a decodificação, a economia em relação ao re-prefill completo a 8K é de 16.24% quando as atualizações ocorrem a cada 32 tokens e de 3.90% a cada 128 tokens.
- Fixar as projeções K/V de redução é o que mantém pequeno o estado retido: 9.48 MiB contra 32.41 MiB do replay de fronteira retida a 8K.
- Execuções pareadas adicionam 4.75-4.88% de sobrecarga de decodificação sobre a decodificação nativa com cache, e 0.68-0.81% sobre um serving direto igualmente protegido.
- O método é correto em aritmética exata para esta família de adaptadores; em BF16 os logits diferem levemente, e os autores não observam divergência de token guloso em seus testes.

## 📚 Plano de leitura
### Do artigo
- [LOCAL: Enabling learning on-device contiguously for agent LLMs](https://arxiv.org/abs/2608.15241) — o trabalho anterior mais próximo sobre versões de adaptadores e invalidação de cache, com o qual o artigo contrasta sua fronteira invariante.
- [LRAgent: Efficient KV cache sharing for multi-LoRA LLM agents](https://arxiv.org/abs/2602.01053) — compartilhamento de cache entre adaptadores, a principal comparação com trabalhos relacionados.
- [ForkKV: Scaling multi-LoRA agent serving via copy-on-write disaggregated KV cache](https://arxiv.org/abs/2604.06370) — base compartilhada e estado de cache dependente do adaptador para serving multi-LoRA.
- [HCache: Fast state restoration in LLM serving](https://arxiv.org/abs/2410.05004) — restauração de KV a partir de ativações armazenadas, a alternativa à recomputação discutida pelo artigo.
- [DroidSpeak: KV cache sharing for cross-LLM communication](https://arxiv.org/abs/2411.02820) — recomputação seletiva quando caches são compartilhados entre modelos ajustados.
- [SGLang: Efficient execution of structured language model programs](https://arxiv.org/abs/2312.07104) — reuso de prefixo, uma técnica de serving que o artigo não compara.
- [Qwen2.5 technical report](https://arxiv.org/abs/2412.15115) — o modelo usado em todos os experimentos.

### Para aprofundar
- LoRA: Low-Rank Adaptation of Large Language Models (Hu et al., ICLR 2022) — o método de adaptador sobre o qual este design se baseia.
- LoRA-FA: Efficient and effective low rank representation fine-tuning — a ideia de reduções fixas que o design reaproveita.
- RoFormer: Enhanced Transformer with Rotary Position Embedding — necessário para acompanhar a etapa do RoPE na Eq. (3).
- Revisiting dynamic evaluation: Online adaptation for large language models — o cenário de cache em streaming por trás dos intervalos de atualização do artigo.

## 🔗 Conceitos
- [kv-cache](/pt-br/concepts/kv-cache.md)
- [rope](/pt-br/concepts/rope.md)
- [ai-agent](/pt-br/concepts/ai-agent.md)
- [inference-tradeoffs](/pt-br/concepts/inference-tradeoffs.md)
- [Adaptação de baixo posto (LoRA)](/pt-br/concepts/lora.md)
