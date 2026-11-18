---
type: Article Summary
title: Engenharia de inferência para engenheiros de software
description: Migrar para modelos abertos significa escolher modelo, provedor e forma de implantação, e passar de APIs multi-tenant para GPUs dedicadas.
resource: https://newsletter.pragmaticengineer.com/p/inference-engineering-for-software-engineers
tags:
- ai-ml
- llm-engineering
- inference-tradeoffs
- kv-cache
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://newsletter.pragmaticengineer.com/p/inference-engineering-for-software-engineers
  title: Inference engineering for software engineers
  author: Pragmatic Engineer
status: stable
source_url: https://newsletter.pragmaticengineer.com/p/inference-engineering-for-software-engineers
source: Pragmatic Engineer
source_type: blog
published: '2026-11-17'
domain: ai-ml
subdomain: llm-engineering
concepts:
- inference-tradeoffs
- kv-cache
selected_by: discovery
content: open
complete: 'no'
notion_url: https://app.notion.com/p/41cbaf74172d81578e4ec98970018783
paywalled: true
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-inference-engineering-for-software-engineers.md
---

## 🎯 Tese central
Migrar das APIs dos laboratórios de fronteira para modelos de pesos abertos servidos por provedores de inferência é um caminho de engenharia prático, e costuma ser feito em etapas: primeiro uma troca direta para uma API multi-tenant, depois GPUs dedicadas, quando custo, latência ou controle exigirem.

## 🧭 Por que o autor escreveu
Empresas com grandes contas de LLM estão migrando para modelos abertos para reduzir o gasto com IA. O artigo cita a Uber cortando o custo por token em 50%, a Pinterest em 90% e a AT&T em 56%. O autor argumenta que a engenharia de inferência está se tornando uma habilidade que engenheiros de software devem entender, e mapeia as opções de fornecedores, os motivos para trocar e os problemas operacionais que vêm junto. Se a tese se sustentar, a escolha deixa de ser "API de fronteira ou nada" e passa a ser uma decisão gradual sobre modelo, provedor e implantação.

## Abertura: dois caminhos para modelos abertos
A introdução do artigo indica duas rotas. A primeira é o "bare metal": com tempo e dinheiro, você compra algumas dezenas de GPUs NVIDIA e as executa por conta própria. O exemplo do artigo é a Revolut, que roda 56 unidades B200 separadas para atender às suas necessidades de inferência. A configuração dá muito trabalho, mas, uma vez em funcionamento, o custo operacional é o mais baixo possível.

A segunda rota é um provedor de inferência. A maioria das empresas contrata um provedor que opera grandes frotas de GPUs e facilita o onboarding e o uso rápido de modelos abertos. O artigo então lista os desafios dessa rota: latências de cauda, taxas de acerto de cache, rate limits e outros. Ele apresenta isso como o núcleo da "engenharia de inferência", um conjunto de práticas que o autor acredita que engenheiros de software deveriam aprender.

O autor conversou com três provedores, Modal, Fireworks AI e Baseten. O roteiro do artigo cobre: o que são os provedores de inferência e em que se diferenciam dos laboratórios; quando trocar; como escolher modelo, provedor e implantação; a Etapa 1 (substituição por API multi-tenant); os sinais de que ela ficou pequena; a Etapa 2 (nós de GPU dedicados); e o decoding especulativo.

> 🔎 Contexto adicional: "Multi-tenant" significa que muitos clientes compartilham o mesmo endpoint e o mesmo hardware. "Dedicado" significa capacidade reservada para um único cliente.

### Bare metal
Possuir ou alugar as GPUs por conta própria. Esforço fixo alto, com o menor custo marginal por token depois que o sistema está rodando.

### Engenharia de inferência
As práticas de executar cargas de LLM de forma confiável e barata: latência, cache, capacidade, rate limits e escolhas de implantação. O artigo a apresenta como cada vez mais útil para engenheiros de software, não apenas para especialistas em ML.

## 1. O que são provedores de inferência e em que se diferenciam dos laboratórios de IA?
### Provedor de inferência
Um provedor de inferência vende acesso a LLMs que ele executa em nome dos usuários. Por fora, é muito parecido com a OpenAI ou a Anthropic: um endpoint de API, streaming de tokens e cobrança por uso de tokens. A diferença está no modelo por trás do endpoint. Os provedores de inferência executam modelos de pesos abertos, enquanto os laboratórios de fronteira treinam os próprios modelos de fronteira. Os provedores também operam infraestrutura de GPU semelhante à dos laboratórios de fronteira.

### Três estratégias de fornecedores
- **Modal** se vê como um provedor de infraestrutura de nuvem cuja oferta é programável por meio de seus SDKs. Ele oferece inferência nativamente por esses SDKs e se apoia no ecossistema open-source para inferência eficiente. Melhorias de desempenho são incorporadas ao upstream. Richard Gong, Inference Lead da Modal, diz: "quando você executa código na Modal, deveria conseguir ver todo o código que está rodando".
- **Fireworks** se posiciona como um provedor para cargas de trabalho especializadas de IA. A maior parte do negócio são implantações dedicadas, muitas vezes com fine-tuning e garantias de latência. A Fireworks também oferece pesquisadores forward-deployed para ajudar no post-training de modelos customizados. Seu CTO, Dmytro Dzhulgakov, diz: "nosso slogan é inteligência especializada, o que significa que vamos ajustar tudo, do modelo à infraestrutura, para o seu caso de uso específico".
- **Baseten** se descreve como um lugar tudo-em-um para construir agentes. Começa com uma camada de infraestrutura que oferece garantias de uptime, condições comerciais flexíveis e nenhum vendor lock-in. Depois adiciona camadas para cargas agênticas sobre a inferência: sandboxes, serviços de governança e post-training em self-serve.

### Por que os provedores não são intercambiáveis
Para os clientes, os provedores oferecem APIs de LLM que podem funcionar como substitutos diretos das APIs dos laboratórios de fronteira. O artigo lista quatro diferenças:
- **Sem pré-treinamento.** Uma única rodada de pré-treinamento de um LLM em nível de fronteira pode custar milhões de dólares, então só hyperscalers e laboratórios de fronteira especializados a fazem. Os provedores fazem post-training de modelos de pesos abertos, porque isso é significativamente mais barato.
- **Preços competitivos.** O principal negócio dos provedores é oferecer modelos abertos a taxas competitivas.
- **Hardware dedicado.** Uma grande parte do apelo é o hardware dedicado, com a possibilidade de trabalhar com o fornecedor na camada de infraestrutura.
- **Eficiência e customização.** Os provedores oferecem modelos abertos semelhantes, já commoditizados, e competem em eficiência e em serviços de customização.

Como exemplo de eficiência, o artigo cita o modelo Ember-1, da Fireworks, anunciado em setembro de 2026. Ele passa por post-training sobre a base Kimi K3 e entrega qualidade semelhante com 40% menos tokens de raciocínio. Isso dá à Fireworks uma vantagem de custo sobre o Kimi K3 puro.

### Uptime dos laboratórios de fronteira
O artigo observa que o uptime das APIs dos laboratórios de fronteira fica aquém dos padrões dos provedores de nuvem. Os números autodeclarados citados são 99,5% para a Anthropic, 99,97% para a API da OpenAI e 99,2% para o ChatGPT. O artigo contrasta esses números com os quatro noves (99,99%+) que os provedores de nuvem como a AWS são conhecidos por manter. Concorrentes podem ganhar participação de mercado explorando essa lacuna.

O artigo também aponta que esses números de uptime não contam desempenho degradado como indisponibilidade. Como todas essas APIs são multi-tenant, uma degradação atinge alguns clientes sem que eles consigam vê-la ou agir sobre ela. Para um serviço de 99,99%, o autor argumenta, não dá para depender de um único provedor de fronteira no caminho crítico.

### Confiabilidade: gateways com fallback
Provedores de inferência também saem do ar, então muitas empresas executam um gateway de LLM com lógica de fallback para outro provedor ou para uma API de fronteira. O artigo indica um deep dive sobre a Ramp, cujo projeto de roteador de LLM começou exatamente com esse problema.

## 2. Quando trocar dos laboratórios de fronteira?
O artigo diz que a migração para um provedor independente costuma ser motivada por três fatores: custo, controle e necessidade de customização.

### Razão mais comum: custo
Muitas empresas começam a olhar para os provedores por causa de pressões de custo. Um modelo de fronteira costuma ser mais capaz do que um pedido específico exige, então um modelo mais barato pode ser igualmente bom para tarefas mais simples, e poucas pessoas notariam a diferença. Nesses casos, é "óbvio" direcionar algumas requisições para um modelo mais barato hospedado por um provedor.

**Roteamento inteligente** é a forma óbvia de cortar custos. Um "roteador inteligente" é um roteador de LLM que entende o prompt. Ele envia cada requisição para o provedor mais eficiente em custo capaz de resolvê-la. O artigo diz que essa abordagem impulsionou o corte de cerca de 50% no custo por token da Uber.

Os provedores oferecem endpoints compatíveis com a OpenAI ou com a Anthropic, então trocar é trivial: aponte sua aplicação para o novo endpoint, que é operado por um roteador inteligente.

A parte difícil é decidir qual modelo servir. Benchmarks genéricos costumam ser insuficientes, porque raramente refletem os padrões de tráfego de uma equipe. A maioria dos provedores pede amostras de prompts ou conjuntos de avaliação para julgar qual modelo se encaixa. Usar um novo endpoint parece usar a API de um laboratório de fronteira, com cobrança por token, mas sem SLA de tempo de resposta.

> ❌ **Anti-pattern — escolher um modelo pelos benchmarks genéricos:** escolher um modelo porque ele vai bem nos rankings públicos. **Por que as pessoas fazem isso:** 🔎 as pontuações públicas são a comparação mais fácil de obter. **Consequência:** as pontuações não representam o seu tráfego, então um modelo que parece bom no papel pode render menos na sua carga de trabalho. **Correção:** montar um conjunto de avaliação privado a partir dos seus próprios prompts e traces, ou rodar um teste A/B com usuários reais.

### Mais controle sobre a latência
Com o endpoint de um laboratório de fronteira, há pouco controle sobre a latência de resposta ou a disponibilidade do modelo. Uma configuração dedicada com um provedor dá controle sobre ambos:
- **Latência:** os tempos de resposta ficam mais previsíveis, porque a aplicação tem capacidade dedicada que escala com o seu uso.
- **Disponibilidade do modelo:** você executa o modelo que escolheu nas suas próprias GPUs e o mantém pelo tempo que precisar. Isso evita o risco de um laboratório trocar de modelo da noite para o dia.

### Fine-tuning e customização
Hardware dedicado com modelos open-source torna possível trabalhar com um provedor para ajustar qualidade, latência ou throughput. Richard Gong diz que cerca de 10% dos clientes da Modal chegam a esse ponto, o que ele descreve como sinal de que a customização é para "usuários avançados". O artigo cita dois cenários típicos:
- **Ajustar um modelo menor.** Você alcança qualidade semelhante com menos parâmetros, usando otimizações de desempenho que modelos prontos não oferecem. Um modelo pequeno com post-training pode superar o original no seu cenário especializado.
- **Casos de uso de nicho.** Você melhora a qualidade onde os modelos de propósito geral não são bons o suficiente, muitas vezes porque os dados não são públicos. A equipe da Fireworks diz que projetos jurídicos e médicos tendem a se beneficiar.

## 3. Escolhendo modelo, provedor e estratégia de implantação
A maioria dos provedores oferece os mesmos modelos, então as diferenças estão em desempenho e qualidade. A qualidade pode variar pelo tamanho padrão do contexto, já que alguns provedores o limitam abaixo do que o modelo suporta. Quantização (compressão dos pesos para uma precisão numérica menor) e a temperatura padrão (o quanto as saídas são aleatórias) também importam. A abordagem prática do artigo tem três decisões, nessa ordem: o modelo, o provedor (com base em qualidade, desempenho e custo) e o modelo de implantação (serviço multi-tenant ou nós de GPU dedicados).

### Escolhendo o modelo
A abordagem mais óbvia é rodar um conjunto de avaliação contra os modelos candidatos. Satya Nadella, CEO da Microsoft, é citado no artigo argumentando que avaliações privadas se tornarão uma parte importante da propriedade intelectual das empresas de tecnologia:

> "Benchmarks públicos são ótimos, mas as suas avaliações privadas vão importar mais: você precisa de uma avaliação própria para o resultado que importa para você. Depois, você compara o desempenho entre todos esses modelos e otimiza continuamente. Nossos clientes começam com um modelo de fronteira e depois dizem: 'Eu tenho todos esses traces. Este é um fluxo determinístico, então não preciso do próximo grande lançamento de modelo. Quero otimizar o custo.' Eles trocam por [outro] modelo, usam os traces para fazer fine-tuning dele – e pronto."

O artigo usa a Artificial Analysis como exemplo público. Ela mantém um conjunto de avaliação que compara o custo de tarefas genéricas entre modelos. Para uma equipe que constrói sobre o Sonnet 5.5, da Anthropic, o custo médio é de US$ 5,46 por tarefa nesse índice. Se o Kimi K3 for suficiente, ele custa cerca de 60% menos, a US$ 2,00 por tarefa. O artigo insiste que isso é custo por tarefa, não por token. Modelos precisam de números diferentes de tokens para a mesma tarefa, então os preços por token enganam.

O artigo acrescenta que uma pontuação pública não basta para decidir que um modelo é bom o suficiente. O ideal é ter um conjunto de avaliação para comparar. Sem ele, um experimento A/B funciona. Por exemplo, você pode deixar alguns engenheiros usarem o novo modelo em fluxos de codificação agêntica e coletar o feedback deles.

### Escolhendo o provedor
Os provedores ajustam modelos abertos para ganhar desempenho sem degradar muito a qualidade, então as saídas variam entre eles. O artigo recomenda o índice de acurácia de endpoint da Artificial Analysis. Ele compara os resultados de cada provedor com a versão self-hosted da própria Artificial Analysis, onde 100% significa resultados idênticos. As pontuações dos provedores variam de 93% a 102%. Diferenças pequenas ficam dentro do mesmo nível da referência, por isso barras tecnicamente abaixo de 100% aparecem em verde. Como os LLMs são não determinísticos, a medição tem um intervalo de confiança de 95%, o que acrescenta mais uma camada de complexidade à avaliação.

### Modelo de implantação: multi-tenant ou dedicado?
Todos os provedores oferecem uma API multi-tenant e GPUs dedicadas. A maioria das implantações segue um caminho de graduação: começam como substitutos diretos de uma API de fronteira e migram para infraestrutura dedicada conforme o uso cresce.

Trocar para um modelo aberto por meio de um provedor reduz o custo por token. Você continua em um ambiente multi-tenant, porém, com os mesmos tipos de gargalos de desempenho de uma API de fronteira. Com uma implantação dedicada, você controla o desempenho e pode ajustar o modelo com técnicas como o post-training.

### O caminho típico de graduação
Para uma startup que usa LLMs, o artigo descreve este caminho:
1. Usar os endpoints de API de um laboratório de fronteira.
2. Migrar para um modelo aberto por meio do endpoint de API de um provedor.
3. Alugar GPUs dedicadas do provedor.

A migração para um provedor costuma coincidir com o product-market fit. Os três provedores disseram que, quando um cliente chega à Etapa 1, o produto já tem tração. Os custos por token só importam em níveis não triviais de atividade dos usuários. A Etapa 2, GPUs dedicadas, implica compromissos orçamentários significativos, um produto mais maduro e uso em larga escala.

### Capacidade de GPU e compromissos
Conseguir capacidade de inferência pode ser difícil por causa da escassez global de GPUs. O artigo linka um texto anterior sobre a escassez de CPUs dentro dos provedores de nuvem e diz que a escassez de GPUs é ainda mais urgente. Mesmo grandes provedores têm dificuldade para obter as GPUs de que precisam. Os provedores entrevistados rodam em vários provedores de nuvem, e alguns também gerenciam o próprio bare metal, para obter o máximo de capacidade possível. Rodar em várias nuvens adiciona complexidade operacional, mas vale a pena pelo acesso a mais GPUs. Fazer o onboarding rápido de uma nova nuvem com capacidade ociosa está se tornando um diferencial-chave.

GPUs dedicadas provavelmente serão caras. Capacidade escassa geralmente significa compromissos orçamentários de longo prazo logo no início: você paga mais e se compromete por mais tempo.

## 4. Etapa 1: substituição de API, usando uma configuração de inferência multi-tenant
### Substituindo APIs de fronteira em ferramentas de codificação
Para equipes com grandes contas de LLM, o corte de custo mais fácil é substituir modelos de fronteira em ferramentas como Claude Code, Codex e OpenCode por um modelo aberto de nível de fronteira. O artigo aponta para os dados do OpenCode e para os rankings do OpenRouter. As escolhas populares que ele cita são DeepSeek 4.1 Flash, GLM 5.3 e outras, incluindo Kimi K3.1 Pro.

O método mais fácil é apontar a aplicação para a API do provedor. Na prática, isso significa uma API multi-tenant, onde outros clientes acessam o mesmo endpoint, como acontece com um laboratório de fronteira. No começo funciona bem. Depois, você pode enfrentar latência de time-to-first-token (TTFT), custos de cache e outros problemas de latência. O artigo observa que esses também são problemas das APIs dos laboratórios de fronteira, mas que, com um provedor, você pode resolver a maioria deles migrando para uma implantação dedicada.

### Latência: TTFT e tempo de inferência
Time-to-first-token é o tempo entre o momento em que uma requisição chega à API do LLM e o momento em que o cliente recebe os primeiros tokens. O ponto do artigo é que o TTFT tem uma cauda longa. Uma cauda direita longa significa que uma parcela significativa das requisições leva o dobro da latência mediana ou mais. Por isso, as latências p95 e p99 importam tanto quanto a p50.

O TTFT pode ser medido em dois lugares:
- **Tempo de inferência:** o TTFT do lado do provedor mostra o tempo puro de inferência. Alguns provedores o expõem em dashboards ou nas respostas. Os laboratórios de fronteira normalmente não fazem nenhuma das duas coisas.
- **Latência percebida pelo usuário:** o TTFT do lado do cliente inclui o caminho completo, incluindo o transporte de rede e o handshake TLS.

A inferência do lado do servidor é só parte do tempo que o usuário espera. Para obter uma latência p95 rápida para o usuário, o artigo sugere otimizações do lado do consumidor, como connection pooling.

### Por que o TTFT varia
Os provedores executam muitas cópias do motor de inferência, e um único motor pode atender várias requisições ao mesmo tempo. Uma nova réplica é criada quando as requisições concorrentes excedem a capacidade atual. O escalonamento de réplicas é uma das razões pelas quais o TTFT apenas da inferência varia tanto. O artigo lista outras causas:
- **Vizinhos barulhentos:** sua requisição é agrupada em lote com uma requisição mais cara, então demora mais do que demoraria com uma parecida.
- **Cold start:** uma requisição espera numa fila porque nenhuma réplica tem capacidade. Uma nova réplica está iniciando, o que não é instantâneo.
- **Cache frio:** uma requisição grande cai em uma réplica sem o KV cache relevante carregado (um cache "frio", o oposto de um "quente"). Os provedores tentam manter as requisições fixas na mesma réplica. Isso falha quando o cache expirou ou a requisição caiu em outro lugar. O KV cache precisa então ser copiado do armazenamento frio, se disponível, ou recalculado por completo antes do primeiro token.

O artigo diz que a utilização do prompt cache é observável e acionável. A API de completions da OpenAI devolve métricas de uso de cache em suas respostas. Construir aplicações que exploram o prompt cache é uma forma-chave de melhorar o desempenho.

### Cache: custo e latência (parcial)
A prévia do artigo termina aqui. Ele começa a explicar que a maioria dos principais endpoints de API de LLM são APIs REST sem estado, modeladas na API de chat completions da OpenAI. Como a maioria dos casos de uso de LLM tem estado, isso cria problemas para o cache. Dmytro Dzhulgakov, CTO da Fireworks, chama a decisão de tornar a API sem estado de "o pecado original". O cliente se comporta como se falasse com uma API sem estado, mas o provedor precisa então garantir um cache eficiente em um ambiente distribuído.

> 🔎 Contexto adicional: o texto disponível para esta seção termina aqui. O restante da discussão sobre cache, a Etapa 2 com GPUs dedicadas e o decoding especulativo não estão cobertos nesta nota.

## 🧠 Modelos mentais
### Graduação da Etapa 1 para a Etapa 2
**O modelo:** a adoção avança em etapas. Você começa com uma API de fronteira, migra para o endpoint multi-tenant de um provedor e só depois aluga GPUs dedicadas, quando já tiver tração e orçamento.
**Como aplicar:** alinhe o compromisso de infraestrutura ao seu uso. A economia por token vem primeiro, e a capacidade dedicada chega quando o volume e a maturidade do produto justificarem o compromisso.

### Roteamento inteligente
**O modelo:** nem toda requisição precisa do modelo mais capaz. Um roteador que entende o prompt envia cada requisição ao provedor mais barato que consiga atendê-la.
**Como aplicar:** meça quais requisições um modelo mais barato atende de forma aceitável, roteie apenas essas para ele e mantenha um caminho de fallback para o restante.

### Custo por tarefa, não por token
**O modelo:** modelos usam quantidades diferentes de tokens para concluir a mesma tarefa, então os preços por token podem enganar.
**Como aplicar:** compare os modelos candidatos pelo custo de concluir as suas tarefas reais, usando o seu próprio conjunto de avaliação.

### A cauda da latência importa mais do que a mediana
**O modelo:** o TTFT tem uma cauda direita longa, então as requisições mais lentas definem como a aplicação parece para alguns usuários.
**Como aplicar:** acompanhe p95 e p99, meça a partir do cliente e reduza o overhead do lado do cliente, como o estabelecimento de conexões.

## 🔑 Principais conclusões
- Modelos abertos servidos por provedores de inferência podem reduzir bastante o custo por token, e o artigo cita reduções de 50% (Uber), 90% (Pinterest) e 56% (AT&T).
- As três razões para trocar dos laboratórios de fronteira são custo, controle de latência e disponibilidade, e customização.
- Use o seu próprio conjunto de avaliação e os seus padrões de tráfego para escolher um modelo. Benchmarks públicos não capturam a sua carga de trabalho.
- A acurácia dos endpoints dos provedores varia de 93% a 102% de uma referência, então compare provedores pela qualidade, além do preço.
- APIs multi-tenant trazem latência de cauda do TTFT, cold starts e caches frios. GPUs dedicadas resolvem a maioria desses problemas, mas exigem compromissos de longo prazo e capacidade escassa.
- Os números de uptime dos laboratórios de fronteira (99,5% a 99,97%) ficam aquém de quatro noves, e o desempenho degradado não é contado como indisponibilidade. Planeje fallbacks para os caminhos críticos.

## 🏛️ Para o Kenji
Sua interpretação, não a do autor: o caminho de três etapas do artigo se encaixa bem em um registro de decisão de arquitetura. Escolha de modelo, escolha de provedor e modelo de implantação são decisões separadas, cada uma deveria ter um responsável e um critério de avaliação, e um gateway de LLM com fallback é um lugar natural para colocar controles de governança. Uma pergunta que ele levanta para o seu trabalho de governança de IA: como você avalia e audita um modelo que um provedor ajustou discretamente, quando a pontuação de acurácia do endpoint é apenas uma medição com 95% de confiança?

## 📚 Plano de leitura
### A partir do artigo
- [Modal](https://modal.com/) — o primeiro perfil de provedor, com a visão de infraestrutura programável.
- [Fireworks AI](https://fireworks.ai/) — o provedor de cargas especializadas, útil para a discussão de fine-tuning.
- [Baseten](https://www.baseten.co/) — a plataforma tudo-em-um para agentes, para a camada que vai da infraestrutura aos agentes.
- [Palestra da Revolut na AI Engineer NYC](https://ai.engineer/nyc/2026/talks/revolut) — como um banco executa a própria frota de GPUs, a opção de bare metal.
- [Anúncio do Ember-1, da Fireworks](https://fireworks.ai/blog/ember-1) — o exemplo de post-training por trás da afirmação de 40% menos tokens de raciocínio.
- [Artificial Analysis](https://artificialanalysis.ai/) — o índice de custo por tarefa e o índice de acurácia de endpoints.
- [Rankings do OpenRouter](https://openrouter.ai/rankings#top-models) — quais modelos abertos as equipes de engenharia realmente usam.
- [Deep dive sobre o roteador de LLM da Ramp](https://newsletter.pragmaticengineer.com/p/why-ramp-built-inspect) — um gateway real construído em torno do problema de fallback.
- [Conceitos de quantização de modelos](https://developer.nvidia.com/blog/model-quantization-concepts-methods-and-why-it-matters/) — o texto introdutório da NVIDIA sobre o trade-off de precisão.

### Para ir além
- Avaliações privadas como propriedade intelectual — a entrevista com Satya Nadella discutida no artigo, sobre por que as suas próprias avaliações vão importar mais do que benchmarks públicos.
- Decoding especulativo — o tema final do artigo (não está no texto disponível), em que um modelo rascunho barato propõe tokens para o modelo principal verificar.
- Prompt caching e APIs de LLM sem estado — a discussão sobre cache que o artigo continua, vale ler junto com as suas próprias métricas de cache.
