---
type: Paper Summary
title: 'Cap-and-Track: otimizando o despacho de requisições de inferência de LLM com capacidade variável'
description: 'Despacho orientado a recusas, com limites por réplica, mantém as frotas de LLM ocupadas conforme a capacidade muda, servindo um classificador com 43% menos H100s.'
resource: https://arxiv.org/abs/2611.24891
tags:
- backend-systems
- adaptive-concurrency-limit
- inference-tradeoffs
- kv-cache
- prefill-decode-disaggregation
- retry-storm
- goodput
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.24891
  title: 'Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity'
  author: arXiv cs.AI/cs.LG/cs.CL (top 3 by relevance)
status: stable
source_url: https://arxiv.org/abs/2611.24891
source: arXiv cs.AI/cs.LG/cs.CL (top 3 by relevance)
source_type: arxiv
published: '2026-11-18'
domain: backend-systems
concepts:
- adaptive-concurrency-limit
- inference-tradeoffs
- kv-cache
- prefill-decode-disaggregation
- retry-storm
- goodput
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d817a8222d1d1816ddf60
lang: pt-br
translation_of: /notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md
---

## 🎯 Em uma frase
O Cap-and-Track mantém as frotas de inferência de LLM cheias conforme a capacidade muda, recusando o trabalho excedente em cada réplica saturada e fazendo o despachante seguir essas recusas; em um A/B de produção com o mesmo tráfego, ele atendeu um classificador com 43% menos H100s do que seu antecessor.

## ❓ O problema
Duas grandezas se movem sob um sistema de serving de LLM. A demanda é o trabalho que os chamadores enviam, que oscila com ciclos diários e backfills em lote, e o tamanho da saída de cada requisição é desconhecido quando ela chega, então seu custo também é desconhecido. A oferta é a capacidade de serving por trás de um backend: pools de provedores compartilhados ficam congestionados, réplicas são preemptadas, e nova capacidade demora, porque uma nova réplica leva uma mediana de 136 s (classificador) a 159 s (zero-shot) só para carregar seu modelo. A solução usual é uma taxa global de despacho definida manualmente por tipo de job, que funciona apenas para a mistura de tráfego para a qual foi ajustada. A parte difícil é que os engines enfileiram trabalho por padrão, então, uma vez sobrecarregados, as requisições passam de seus timeouts; os autores mediram o goodput (requisições concluídas dentro do timeout, por segundo) caindo para zero no replay.

## 💡 A ideia
Faça o recurso restrito dizer "não" imediatamente, em vez de aceitar trabalho que ele não consegue terminar, e mantenha o excedente em uma fila durável fora da execução. Uma recusa passa a ser um sinal barato e honesto do teto de capacidade atual, então o despachante pode encontrar esse teto sondando para cima e recuando apenas com base nas recusas, sem prever o tamanho da saída e sem uma taxa para toda a frota que precise ser reajustada. A analogia dos autores: o CAP-AND-TRACK ajusta o despacho como o controle de congestionamento ajusta um emissor de rede. O CAP recusa trabalho cedo, onde a capacidade satura. O TRACK sobe a taxa com sondagens e recua diante dessas recusas, como um emissor faz diante de perda.

## ⚙️ Como funciona
### CAP: uma recusa imediata no recurso saturado
Cada réplica de inferência recebe um limite por réplica. Os deployments de classificador e zero-shot definem o max num seqs do vLLM como 128, e os limites de requisição e de processamento do model server como 140 por réplica; juntos, chamados de "o cap". O cap fica deliberadamente um pouco acima do limite de batch, para que algumas requisições aguardem logo atrás de um batch quase cheio e um slot liberado seja reocupado sem uma ida e volta ao despachante. Uma requisição acima do cap recebe uma exceção de sobrecarga, que o gateway converte em HTTP 429 e devolve imediatamente, sem consumir tokens reservados. O cap não precisa de ajuste preciso: no replay, caps de duas e quatro vezes a concorrência do platô mantiveram o goodput dentro de 2,4% do goodput no platô.

### TRACK: um controlador de taxa que segue as recusas
A cada 15 segundos, um líder calcula, para cada fila, a parcela de recusas em toda a frota: a fração dos sucessos e dos 429s que são 429s. Timeouts ficam fora do cálculo, porque um timeout pode significar falha de rede, situação em que recuar só desperdiça capacidade. A taxa é então atualizada por uma regra de aumento e redução multiplicativos (MIMD, em inglês), mostrada abaixo em forma simplificada.

```text
refusal_share = 429s / (successes + 429s)

if refusal_share <= 2% (the production target):
    if dispatch >= 75% of target rate:  rate = rate * 1.03
    else:                               hold (utilization gate)
else:
    rate = rate * 0.99   if refusal_share <= 50%
    rate = rate * 0.85   if 50% < refusal_share <= 65%
    rate = rate * 0.5    if refusal_share > 65%
```

Aqui `rate` é a taxa de despacho da fila e `dispatch` é a taxa efetivamente alcançada. O passo de 3% é pequeno o bastante para que a taxa trace um dente de serra logo acima do teto, e as três faixas de corte agem com mais força só quando mais da metade das tentativas é recusada. O gate de utilização impede que a taxa suba durante horas de baixa demanda, o que nas versões iniciais deixava os alvos chegarem ao máximo e depois sobrecarregava o backend quando o trabalho retornava.

### Filas prioritárias duráveis
O trabalho espera na FOQS, a fila prioritária durável da Meta, e só é despachado na taxa do TRACK. Cada item carrega uma pontuação de prioridade inteira; prioridades maiores saem primeiro, empates saem aproximadamente em FIFO, e os itens não ganham prioridade com a idade, por escolha de produto. O chamador pode dar a um item um tempo de vida (TTL), verificado antes do despacho, o que limita sua espera sob prioridade estrita. Trabalho recusado permanece durável, então um backlog continua visível e recuperável, em vez de expirar dentro de um engine.

### Líder, alocação e congelamentos
Há um líder por domínio de capacidade, escolhido como o worker com o hostname lexicograficamente menor; ele reúne as contagens de todos os workers, decide a taxa de cada fila para a frota inteira e envia a cada worker a sua parte. Cada fila é alocada apenas nos workers necessários para sua taxa, usando um anel de hash consistente, de modo que um worker que entra ou sai move cerca de 1/N das alocações. Reinícios e mudanças no número de workers causariam cortes desnecessários, então o líder congela as taxas até 10 polls após a última mudança no número de workers, e um congelamento só começa quando pelo menos 10% dos workers saem. As retentativas são locais: uma requisição recusada volta para o início da fila de prontos do seu worker, com a mídia já preparada, e só a expiração do lease a devolve à FOQS.

### Lições operacionais
As lições da Seção 7 dos autores são concretas. Ative o caminho de recusa do engine, porque a versão do vLLM implantada limitava as sequências em execução, mas não a sua fila de espera, e o limite de requisições do model server vinha desligado por padrão. Mantenha pouco trabalho esperando no engine e defina em conjunto tamanho de batch, limite de espera e timeout: subir o batch de 256 para 320 sequências acrescentou 10–15% de throughput, mas com até 300 requisições esperando atrás dele, muitas expiravam; reduzir o limite de espera de 300 para 100 caiu as tentativas expiradas de 15,4% para 1,6%. Mantenha o despacho com estado, porque seguir o teto exige resultados recentes e a taxa atual, que jobs sem estado não guardam.

## 🧪 Evidências
| Cenário | Referência | Este artigo | Métrica |
|---|---|---|---|
| Classificador, A/B de produção, mesmo tráfego | Antecessor: 68 H100s (perfilados), com erros em cerca de dois terços da sua taxa perfilada por placa | Shaper: 39 H100s com cobertura e scores iguais (45 em produção) | H100s; "43% a menos" como reportado |
| Zero-shot, A/B de produção, cerca de 600 MI300X cada, 23 dias | Antecessor, mesma ordem de prioridade | Shaper drenou primeiro todos os backlogs compartilhados, em uma mediana de 3 horas | Tempo de drenagem do backlog |
| Zero-shot, ambos os caminhos nos limites de despacho | Antecessor: mediana de 41 de 128 sequências | Shaper: mediana de 124 de 128 sequências, 9% mais requisições ao modelo por GPU | Sequências por batch do engine; requisições por GPU |
| Replay, uma A100, teto estável (Tabela C1, cap 40) | Taxa fixa de 120% da capacidade: 109,5 req/s por host, 15,5% recusadas | TRACK: 108,5 req/s por host, 3,8% recusadas | Goodput por host; parcela de recusas |
| Replay, uma A100, formato de classificador, cerca de o dobro da capacidade | Sem cap: goodput 0,01 req/s (classificador), 0 (generativo) | Com cap: 8,64 req/s (classificador), 1,83 (generativo) | Goodput por segundo |
| Capacidade reservada de provedor, lançamento | Sistema estático, 23 dias anteriores: 88,2% de utilização, 16,1% recusadas | Shaper, primeiros 4 dias completos: 94,8% de utilização | Parcela da reserva usada |
| Reservada, regime estacionário após atualização do provedor | Sem referência após a atualização | 95,8% em 15 semanas; 99,1% em uma semana com backlog | Parcela da reserva usada |
| Backfill de classificador, últimos três dias | Metas de frescor por faixa (30 min a 24 h) | Faixa superior cumpriu a meta em 99% dos itens; segunda faixa 89%; as duas faixas mais baixas 44–55% | Parcela dos itens dentro da meta |

Os resultados mostram que o TRACK, sem nenhuma configuração de capacidade, fica perto da melhor taxa ajustada manualmente em um teto estável e acompanha o teto quando ele se move, inclusive após a perda de quase toda a capacidade sob demanda. Eles não mostram que ele supera uma taxa fixa perfeitamente ajustada: no replay, a taxa fixa de 100% foi um pouco melhor em goodput (107,0 req/s, com 1,1% recusadas) no caso estável. As comparações de produção são A/Bs com o mesmo tráfego, em frotas internas de uma única empresa, com duas cargas de trabalho, e os números de tamanho de frota são um limite inferior. O trabalho de menor prioridade esperou mais nos picos com a frota menor; a faixa mais baixa esperou uma mediana de 6,6 horas, contra meia hora do antecessor.

## ⚠️ Limitações
- Os resultados de produção fora do replay e dos dois A/Bs são observacionais, sem controle de backlog.
- O testbed com engine real usou o formato de requisição de classificador, um modelo de 8B em uma A100 80 GB e saídas de um token; cargas generativas foram avaliadas apenas em produção.
- Os autores não aplicaram o CAP-AND-TRACK a tráfego interativo ou de sessões de agentes.
- Os tamanhos de frota serve-on-arrival (média de 25 H100s, 99 na hora mais carregada) são limites inferiores, e a estimativa de "cerca de 48 GPUs a menos" e a estimativa de drenagem 1,6 vez mais rápida não foram testadas.
- Trabalho de menor prioridade espera mais nos picos de backfill; os autores aceitam isso como o preço de uma frota menor.
- 🔎 Contexto adicional: todos os números de produção e de A/B vêm das frotas de uma única empresa e do seu próprio sistema antecessor. Nada aqui é uma replicação independente, e as cargas são majoritariamente com muito prefill (cerca de 17 tokens de entrada por token de saída), que os autores esperam que ganhe mais em cargas com muito decode.
- 🔎 Contexto adicional: o replay usou um modelo pequeno em uma única GPU, então as figuras de goodput (cerca de 108 req/s por host) descrevem o comportamento do despacho, não o que um modelo maior sustentaria.

## 🏗️ O que isso significa para construir sistemas
Isto é interpretação, não afirmação dos autores. O padrão poderia ser plugado como despachante na frente de qualquer backend com capacidade limitada que consiga devolver um "não" barato e rotulado no seu ponto de admissão, como um model server com cap por réplica, uma cota de provedor ou um roteador interno. O custo são mais retentativas e mais carga de rede gerada pelas tentativas recusadas, além de uma fila durável e um líder com estado que precisa sobreviver a reinícios e congelamentos. Antes de confiar nele, você precisaria de um limite para a fila de espera do próprio engine, de um timeout maior que a espera esperada na fila, e de uma meta de frescor por faixa de prioridade medida no seu próprio tráfego.

## 🔑 Principais conclusões
- Uma recusa imediata no recurso saturado, e não uma fila mais longa, é o que impede o colapso do goodput: sem o cap, o goodput caiu para 0,01 req/s no formato de classificador no replay.
- O TRACK aprende o teto apenas a partir de 429s rotulados, aumentando 3% a cada poll saudável de 15 segundos e reduzindo por 0,99, 0,85 ou 0,5 conforme a parcela de recusas, então não precisa prever o tamanho da saída.
- No A/B de produção, o shaper atendeu o classificador com 39 H100s contra 68 do antecessor, mantendo cobertura e scores.
- As faixas de menor prioridade absorvem o custo de uma frota menor: 44–55% dos itens cumpriram suas metas nos últimos três dias do backfill, então o tamanho da frota deve ser escolhido pelo frescor, não só pela vazão.
- O despacho com estado, com líder por fila, alocação por hash consistente e congelamentos foi a maior parte do esforço de engenharia, e é isso que torna a regra simples segura em uma frota que muda.

## 📚 Plano de leitura
### A partir do artigo
- [Kwon et al., PagedAttention and vLLM (SOSP 2023)](https://doi.org/10.1145/3600006.3613165) — o engine cujo batching e cache KV o design depende.
- [Netflix concurrency-limits](https://github.com/Netflix/concurrency-limits) — o limitador adaptativo mais próximo, que lê o aumento de latência como sobrecarga em vez de recusas imediatas.
- [Envoy adaptive concurrency filter](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/adaptive_concurrency_filter.html) — um limitador de concorrência de produção, com o qual os autores contrastam o TRACK.
- [CONCUR, arXiv:2601.22705](https://arxiv.org/abs/2601.22705) — controle de admissão baseado em congestionamento para inferência em lote com agentes, guiado por sinais do cache KV.
- [DCTCP, data center TCP (SIGCOMM 2010)](https://doi.org/10.1145/1851182.1851192) — o design de controle de congestionamento que os autores usam como analogia para marcações e recusas.

### Para aprofundar
- Sarathi-Serve (OSDI 2024) — o tradeoff entre throughput e latência na inferência de LLM, a razão de manter os batches cheios.
- Metastable failures in distributed systems (HotOS 2021) — por que uma sobrecarga alimentada por retentativas pode se manter depois que o gatilho desaparece.
- Site Reliability Engineering, capítulo "Handling overload" — o tratamento clássico de load shedding no qual o trabalho relacionado do artigo se apoia.
- Lei de Little — como taxa de requisições e tempo de espera definem o tamanho da fila que um timeout precisa cobrir.

## 🔗 Conceitos
- [adaptive-concurrency-limit](/pt-br/concepts/adaptive-concurrency-limit.md)
- [inference-tradeoffs](/pt-br/concepts/inference-tradeoffs.md)
- [kv-cache](/pt-br/concepts/kv-cache.md)
- [prefill-decode-disaggregation](/pt-br/concepts/prefill-decode-disaggregation.md)
- [retry-storm](/pt-br/concepts/retry-storm.md)
- [goodput](/pt-br/concepts/goodput.md)
