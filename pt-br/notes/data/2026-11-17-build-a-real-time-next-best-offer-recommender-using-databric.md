---
type: Article Summary
title: Construa um recomendador de próxima melhor oferta em tempo real com Feature Views do Databricks
description: As Feature Views permitem que uma única definição de feature atenda tanto o treinamento com correção temporal (point-in-time) quanto o ranqueamento online de baixa latência, evitando o train/serve skew.
resource: https://www.databricks.com/blog/build-real-time-next-best-offer-recommender-using-databricks-feature-views
tags:
- data
- data-science
- feature-store
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://www.databricks.com/blog/build-real-time-next-best-offer-recommender-using-databricks-feature-views
  title: Build a real-time next-best-offer recommender using Databricks Feature Views
  author: Tian Tan
status: stable
source_url: https://www.databricks.com/blog/build-real-time-next-best-offer-recommender-using-databricks-feature-views
source: Databricks
source_type: blog
published: '2026-11-16'
domain: data
subdomain: data-science
concepts:
- feature-store
- inference-tradeoffs
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d8138b6bde7d04a65dc14
lang: pt-br
translation_of: /notes/data/2026-11-17-build-a-real-time-next-best-offer-recommender-using-databric.md
---

# Construa um recomendador de próxima melhor oferta em tempo real com Feature Views do Databricks

**Fonte:** https://www.databricks.com/blog/build-real-time-next-best-offer-recommender-using-databricks-feature-views
**Autor:** Tian Tan
**Data:** 2026-11-16
**Conceitos:** [Feature store](/pt-br/concepts/feature-store.md) · [Throughput/latência/custo de inferência](/pt-br/concepts/inference-tradeoffs.md)

---

## 🎯 Tese central
Definir cada feature uma única vez como uma Feature View permite que a mesma lógica atenda tanto o treinamento com correção temporal (point-in-time) quanto o serving online de baixa latência, o que elimina o train/serve skew causado pela manutenção de pipelines batch e de streaming separados.

## 🧭 Por que o autor escreveu o artigo
Um recomendador em tempo real precisa de features calculadas da mesma forma no treinamento e no serving, e recuperadas em milissegundos na inferência. O artigo usa um recomendador de próxima melhor oferta (NBO, na sigla em inglês) em funcionamento, para o varejo bancário, para argumentar que as Feature Views tornam essa consistência possível sem pipelines duplicados. Se a tese se sustentar, as features com que um modelo treina são as mesmas definições que ele usa em produção.

## Abertura: capturando a intenção do cliente na sessão
> 🔎 Contexto adicional: a abertura do artigo não tem título, então este título é um rótulo que eu acrescentei. Seu conteúdo é mantido por completo abaixo.

Quando um cliente do varejo bancário consulta suas contas, explora cartões de crédito ou abre uma calculadora de empréstimo, cada interação sinaliza a intenção atual dele. Uma oferta relevante precisa chegar até ele enquanto ele ainda está na sessão. Os dados brutos de clickstream precisam ser agregados em features com janelas de tempo para dois fins: treinar com valores históricos como existiam em um dado momento, e servir valores atuais com latência de milissegundos. O artigo diz que construir esses pipelines separadamente cria definições de features duplicadas, que podem divergir com o tempo e degradar a acurácia do modelo.

Em seguida, o artigo apresenta um Solution Accelerator de código aberto que usa Feature Views para construir um recomendador NBO em tempo real para serviços financeiros. O acelerador tem duas partes: um recomendador que usa features batch, e uma extensão que acrescenta streaming, a intenção dentro da sessão. Ambas são construídas de ponta a ponta no Databricks, com notebooks executáveis que podem ser clonados e implantados.

### Intenção de sessão
A intenção de sessão é o que o cliente quer agora, inferido do que ele faz na visita atual, como consultar contas, navegar por cartões de crédito ou usar uma calculadora de empréstimo. O ponto do artigo é que esse sinal só é útil enquanto o cliente ainda está na sessão, então ele precisa chegar ao modelo com baixa latência. É por isso que o acelerador acrescenta um caminho de streaming por cima do recomendador batch.

### Features com janela de tempo
Features com janela de tempo resumem eventos em um intervalo limitado, como uma média de saldo de 30 dias ou uma contagem dos últimos 10 minutos. A abordagem do artigo usa essas features tanto no treinamento quanto no serving, então a lógica de janelamento precisa ser a mesma dos dois lados.

## Features consistentes e em tempo real para treinamento e serving
Recomendações em tempo real precisam de consistência e de velocidade ao mesmo tempo. As features precisam ser calculadas da mesma forma durante o treinamento e o serving para evitar o train/serve skew, que pode degradar a acurácia do modelo. Na inferência, essas features também precisam ser recuperadas e pontuadas em milissegundos. Tradicionalmente, atender aos dois requisitos significava um pipeline batch para as features de treinamento, mais um job de streaming escrito à mão para o serving online, o que, segundo o artigo, cria uma lógica duplicada que pode divergir com o tempo.

> ❌ **Anti-padrão — lógica de features de treinamento e de serving separadas:** Um pipeline batch constrói as features de treinamento enquanto um job de streaming escrito à mão constrói as features online. **Por que as pessoas fazem isso:** o artigo diz que atender aos requisitos de consistência e de velocidade tradicionalmente significava manter sistemas separados, então cada requisito ganhava a sua própria implementação. **Consequência:** a lógica duplicada diverge com o tempo, o que, segundo o artigo, pode degradar a acurácia do modelo. **Correção:** declarar a lógica de features uma vez como uma Feature View e deixar o Databricks reutilizá-la para treinamento e serving.

As [Feature Views](https://www.databricks.com/blog/introducing-feature-views) eliminam a necessidade de definições de features separadas. Você declara a lógica uma vez, e o Databricks a reutiliza no treinamento e no serving:
- As features batch são materializadas em tabelas Delta offline, para treinamento com correção temporal (point-in-time), e em um online store, para serving de baixa latência.
- As features de streaming são agregadas continuamente no online store, enquanto os valores históricos são calculados com correção temporal a partir dos eventos de origem para o treinamento. Diferentemente das features batch, elas não são materializadas em um store offline.

### Feature Views
Uma Feature View é uma definição de feature declarada sobre uma tabela batch ou sobre uma fonte de streaming. A mesma declaração atende aos dois caminhos: o treinamento lê valores históricos das tabelas Delta offline ou dos eventos de origem, e o serving lê valores atuais do online store. Como a lógica é declarada uma única vez, não há uma segunda implementação para manter sincronizada. O artigo apresenta isso como a principal escolha de design do acelerador.

### Treinamento com correção temporal usando create_training_set
No momento do treinamento, a API `create_training_set` recupera os valores das features como existiam em cada evento rotulado, o que impede o vazamento de dados futuros.
> 🔎 Contexto adicional: vazamento de dados futuros significa que um exemplo de treinamento é construído com informações que não estariam disponíveis quando o rótulo ocorreu. O modelo então aprende com um sinal que não terá no momento da predição.

### Consulta online por customer_id
Na inferência, o endpoint do Model Serving busca as features batch no online store usando `customer_id`. O artigo observa que os dois fluxos usam as mesmas definições de features, o que evita inconsistências entre transformações offline e online implementadas separadamente.

## Recomendações de próxima melhor oferta em tempo real
O acelerador implementa um recomendador NBO em tempo real para o varejo bancário. Enquanto o cliente navega por contas, explora cartões de crédito ou usa uma calculadora de empréstimo, o sistema ranqueia um catálogo de ofertas, incluindo upgrades de cartão de crédito, produtos de poupança e empréstimos pessoais, e recomenda a mais relevante durante a sessão. O recomendador combina o contexto estável do cliente, como saldos, gastos, tempo de relacionamento e faixa de risco, com a atividade recente de clickstream que captura a intenção dentro da sessão. Um único modelo de ranqueamento usa os dois tipos de dados, e as Feature Views mantêm as definições consistentes entre o treinamento offline e o serving online.

### Contexto estável e intenção dentro da sessão
O ranqueador recebe dois tipos de entrada ao mesmo tempo. O contexto estável do cliente, como saldos, gastos, tempo de relacionamento e faixa de risco, descreve quem ele é. A atividade recente de clickstream descreve o que ele está fazendo agora. O artigo usa um único modelo sobre os dois, em vez de modelos separados para cada um, então a camada de features precisa servir os dois níveis de frescor a partir de um só lugar.
> 🔎 Contexto adicional: o artigo não informa com que frequência as features estáveis são atualizadas. O contraste aqui é entre um contexto que muda devagar e a atividade da sessão com minutos de idade.

## Como construímos
O acelerador tem três etapas, entregues como notebooks executáveis em duas partes que compartilham catálogo, schema, online store e definições de features.

### Etapa 1: definir features batch e de streaming
As Feature Views permitem declarar features sobre tabelas batch e sobre streams Kafka ao vivo com a mesma abordagem. A tabela de features do artigo:

| Feature | Declarada como | O que dá ao modelo |
|---|---|---|
| Saldo médio de 30 dias | SlidingWindow | Comportamento contínuo |
| Gasto de 90 dias | TumblingWindow | Comportamento em período fixo |
| Nível de fidelidade, faixa de risco, renda, tempo de relacionamento | ColumnSelection | Contexto estável do cliente |
| Visualizações por categoria nos últimos 10 minutos | RollingWindow sobre um stream | Intenção mais recente dentro da sessão |
| Interesse por categoria em 30 dias | SawtoothWindow | Janela longa, com borda inicial sempre fresca |
| Razão gasto/renda | CustomUDF | Derivada, calculada por requisição |

Para features de streaming, você aponta a declaração para um tópico Kafka em vez de uma tabela. As Feature Views então agregam os eventos continuamente no online store, sem um job de streaming escrito à mão e sem lógica de features duplicada. O artigo diz que o SawtoothWindow mantém janelas longas de streaming viáveis: uma contagem de 30 dias por categoria continua fresca à medida que novos eventos chegam, sem recalcular todo o histórico a cada evento.

#### Tipos de janela (SlidingWindow, TumblingWindow, RollingWindow, SawtoothWindow)
O artigo usa esses nomes sem defini-los, então o que segue é um contexto acrescentado.
> 🔎 Contexto adicional: uma janela deslizante (sliding) cobre um período final que se move com o tempo atual, uma janela em blocos (tumbling) cobre períodos fixos e não sobrepostos, e uma janela rolante (rolling) cobre o intervalo mais recente em um stream ao vivo. Uma janela sawtooth, como o artigo a usa, mantém uma janela longa cuja borda inicial continua fresca à medida que novos eventos chegam.

#### ColumnSelection e CustomUDF
ColumnSelection pega colunas estáveis diretamente de uma tabela, o que o artigo usa para nível de fidelidade, faixa de risco, renda e tempo de relacionamento. CustomUDF executa uma função definida pelo usuário, que o artigo usa para a razão gasto/renda, calculada por requisição em vez de armazenada.

### Etapa 2: treinar com features de correção temporal
Usando o [Feature Engineering SDK](https://docs.databricks.com/aws/en/machine-learning/feature-store/python-api), um join com correção temporal (point-in-time) reconstrói o valor de cada feature como ele existia no momento do rótulo histórico, o que impede o vazamento de dados futuros. O conjunto de treinamento resultante alimenta um ranqueador LightGBM registrado no Unity Catalog. Como treinamento e serving usam as mesmas declarações de features, as features não podem divergir por meio de transformações offline e online implementadas separadamente.
> 🔎 Contexto adicional: um join com correção temporal associa cada evento rotulado apenas aos valores de feature que já estavam disponíveis no timestamp desse evento.

### Etapa 3: servir recomendações em tempo real
O ranqueador roda em um endpoint do Model Serving otimizado para rota. Cada requisição contém `{customer_id, candidate offers, session context}`. O endpoint recupera as features do cliente no online store pela chave e pontua todas as 40 ofertas diretamente, sem uma etapa separada de redução de candidatos. Um segundo endpoint de Feature Serving, também otimizado para rota, expõe os valores das features, para que a aplicação possa mostrar por que uma oferta foi selecionada sem consultar o online store diretamente.

## Benchmark de latência, frescor de features e throughput
Duas medidas importam mais para um recomendador em tempo real: a rapidez com que um clique se torna uma feature visível ao modelo, e se treinamento e serving produzem valores de feature idênticos. O artigo fez um benchmark de ponta a ponta do acelerador na configuração menor, um endpoint de serving pequeno e um online store de nó único, sem ajustes. Eventos de streaming se tornaram visíveis ao modelo em cerca de um décimo de segundo, e treinamento e serving produziram valores idênticos.

A Figura 1 do artigo mostra três latências: a latência de requisição dos dois caminhos de serving e o frescor das features, desde a ingestão do evento até a disponibilidade online. Cada par de barras mostra p50 e p95, e uma linha tracejada marca o orçamento de 50 ms que os dois caminhos de serving atendem. A figura em si não está no texto extraído, então apenas os valores declarados são usados aqui. Uma requisição completa de ranqueamento, incluindo a recuperação online das features e a inferência, cabe no orçamento de 50 ms no menor tamanho de endpoint. Acrescentar a feature de streaming ao vivo aumenta a latência em apenas 3–4 ms.

### Throughput e planejamento de capacidade
O endpoint de ranqueamento sustentou cerca de 56 requisições por segundo (RPS). O endpoint de Feature Serving sustentou cerca de 390 RPS antes que a latência p95 ultrapassasse 50 ms. O artigo relata que o throughput medido acompanhou a concorrência dividida pelo tempo de serviço, com diferença de até 15%, e apresenta esta fórmula de dimensionamento:

> concorrência necessária = 4 × ceil(RPS alvo ÷ RPS medido no seu SLO)

Aplicando os números do artigo, sustentar 150 requisições de ranqueamento por segundo dentro de um objetivo de nível de serviço (SLO) de p95 de 50 ms exige concorrência 12.
> 🔎 Contexto adicional: 150 ÷ 56 é cerca de 2,68, que arredondado para cima dá 3, e 4 × 3 = 12.

### Como medimos
Os benchmarks usaram os notebooks do acelerador com endpoints aquecidos, um cliente na mesma região e carga leve. As chaves das requisições foram distribuídas entre 2.000 clientes com distribuição de Zipf, para aumentar os cache misses. Nessas condições, a latência de ranqueamento mediu 33 ms no p50 e 48 ms no p95. O frescor das features foi medido por evento, desde a escrita no Kafka até o valor se tornar legível no online store. Para consistência, os valores materializados offline foram comparados com o online store para cada cliente usando igualdade null-safe.

O artigo lista duas limitações. Primeiro, os endpoints de ranqueamento escalam para zero, então a primeira requisição depois de um período ocioso pode sofrer um cold start de dezenas de segundos. Segundo, a latência relatada combina a recuperação de features e a inferência, então a medida de 27 ms citada no artigo não é diretamente comparável com benchmarks que informam apenas a latência de leitura de features.
> 🔎 Contexto adicional: o resumo e o FAQ do artigo citam 34 ms de p95 para o ranqueamento, enquanto esta seção informa 48 ms de p95 para o mesmo endpoint. O texto não concilia os dois números, então ambos são relatados aqui como foram escritos.

> ❌ **Anti-padrão — comparar latências que não são comparáveis:** Colocar lado a lado a latência de ranqueamento de ponta a ponta e a latência apenas de leitura de features de outro benchmark. **Por que as pessoas fazem isso:** o artigo não diz; os números são fáceis de pôr lado a lado em uma tabela. **Consequência:** a comparação exagera ou subestima a diferença, porque um dos números inclui a inferência do modelo e o outro não. **Correção:** comparar coisas equivalentes e declarar quais componentes cada medida inclui.

## O caso de negócio: iteração mais rápida, resultados mensuráveis
O artigo é explícito ao dizer que o acelerador demonstra um recomendador em tempo real funcionando, não um ganho de receita comprovado. Como usa dados sintéticos, os autores não afirmam melhorias de conversão. Em vez disso, ele dá às equipes um ponto de partida para medir o impacto no negócio. Seus três benefícios declarados são:
- Menos manutenção de pipelines: uma única camada de features substitui três pipelines mantidos à mão. Batch, streaming e serving compartilham definições declarativas, com governança e rastreamento de linhagem no Unity Catalog para apoiar auditorias em serviços financeiros regulados.
- Implementação mais rápida: clone o repositório, implante o bundle e execute. A Parte 1 fornece um ranqueador governado com consultas online de features, enquanto a Parte 2 acrescenta a intenção ao vivo dentro da sessão sem uma pilha de features separada.
- Uma base para testes A/B: compare o ranqueador com consciência de intenção e streaming contra um modelo apenas batch, usando ofertas aceitas incrementais por sessão elegível e margem de contribuição downstream, com taxas de reclamação e de opt-out como guardrails.

O artigo também diz que, antes da produção, as equipes precisam implementar controles de elegibilidade, consentimento, adequação e supressão. Ranquear ofertas é uma etapa de modelagem, não a decisão final sobre a oferta, e o acelerador demonstra a infraestrutura de engenharia de features e de serving, não essas regras de negócio.

> ❌ **Anti-padrão — tratar o ranqueador como a decisão sobre a oferta:** Mostrar diretamente ao cliente a oferta melhor ranqueada, assim que o modelo a pontuou. **Por que as pessoas fazem isso:** o artigo não diz; uma pontuação alta pode parecer uma resposta final. **Consequência:** a oferta pode contornar as regras de elegibilidade, consentimento, adequação e supressão que, segundo o artigo, precisam existir antes da produção. **Correção:** passar as ofertas ranqueadas por esses controles de regras de negócio antes de qualquer oferta ser mostrada.

## Comece com o Solution Accelerator
O acelerador permite começar com features batch e depois acrescentar streaming em tempo real sem reconstruir sua infraestrutura de features.
- [Parte 1 — fundamentos de Feature Views](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/notebooks/part1_feature_views): clone o repositório, configure um perfil da Databricks CLI, implante o bundle e execute a Parte 1 para construir um ranqueador governado com consultas online de features.
- [Parte 2 — streaming em tempo real](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/notebooks/part2_realtime_two_stage): configure uma conexão Kafka do Unity Catalog e a acessibilidade do Amazon MSK, depois execute a Parte 2. Ela reutiliza o catálogo, as features, o online store e o ranqueador da Parte 1.
- [Demo e monitoramento](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/apps/recommender-ui): inicie o app de recomendação no Databricks e abra o dashboard de latência do AI/BI.

O acelerador é implantado com [Declarative Automation Bundles (DABs)](https://docs.databricks.com/aws/en/dev-tools/bundles/). O texto extraído se refere a um comando de implantação, mas não o inclui. O artigo observa que a ordem de implantação importa, porque o app depende dos endpoints de serving da Parte 2, que os notebooks criam em tempo de execução. Tudo roda em compute serverless governado pelo Unity Catalog.

> 📋 **Checklist — ordem de implantação**
> - [ ] Implante os jobs.
> - [ ] Execute a Parte 1 e a Parte 2.
> - [ ] Implante o app.

> 📋 **Checklist — pré-requisitos da Parte 2**
> - [ ] Configure uma conexão Kafka do Unity Catalog.
> - [ ] Confirme a acessibilidade do Amazon MSK.

## Saiba mais sobre Feature Views
O artigo diz que as Feature Views permitem definir features uma única vez para treinamento com correção temporal (point-in-time) e serving online de baixa latência na AWS, no Azure e no GCP. Sua leitura complementar é o repositório do acelerador, a documentação do Databricks Feature Store e a documentação de consulta aos endpoints do Model Serving otimizados para rota.

## FAQ
**O que são Feature Views?** Elas permitem definir transformações de features uma única vez e reutilizá-las no treinamento de modelos e no serving online. Suportam features batch e de streaming, com valores históricos com correção temporal para o treinamento e acesso de baixa latência a valores atuais para a inferência.

**Como as Feature Views evitam o train/serve skew?** Elas usam definições de features compartilhadas entre treinamento e serving, o que evita inconsistências causadas pela manutenção de lógicas de transformação separadas. A API `create_training_set` também usa joins com correção temporal para reconstruir os valores das features como existiam em cada rótulo histórico, o que impede o vazamento de dados futuros.

**As Feature Views suportam features de streaming em tempo real?** Sim. Elas podem agregar continuamente eventos do Kafka em um online store, sem um job de streaming escrito à mão. O acelerador usa RollingWindow para a atividade recente da sessão e SawtoothWindow para janelas históricas mais longas.

**Qual desempenho o acelerador de próxima melhor oferta alcança?** Na configuração de benchmark, o endpoint de ranqueamento alcançou latência p95 de 34 ms, incluindo a recuperação online de features e a inferência. Sustentou cerca de 56 requisições por segundo, enquanto o endpoint de Feature Serving sustentou cerca de 390 requisições por segundo antes de ultrapassar o orçamento de latência p95 de 50 ms.

**O que preciso para rodar o acelerador?** Um workspace do Databricks, um perfil configurado da Databricks CLI e o repositório do acelerador no GitHub. A Parte 1 implanta o recomendador batch usando Declarative Automation Bundles. A Parte 2 exige, além disso, uma conexão Kafka do Unity Catalog e conectividade com o Amazon MSK para incorporar a atividade ao vivo da sessão.

## 🏛️ Para o Kenji
> Esta é a sua interpretação, não a do autor.

Qualquer arquitetura em que treinamento batch e inferência online compartilham features enfrenta o mesmo risco de divergência que o artigo descreve, então este padrão se aplica a revisões de features e ao design de model serving. A pergunta que ele levanta: quais das nossas features atuais têm implementações separadas de treinamento e de serving, e como detectaríamos quando elas divergirem?

## 🧠 Modelos mentais
### Declare uma vez, sirva em todo lugar
**O modelo:** uma feature é uma única declaração, e os caminhos offline (treinamento) e online (serving) são dois padrões de leitura sobre essa declaração, não duas implementações.
**Como aplicar:** ao revisar um sistema de ML, procure qualquer feature calculada em dois lugares e trate essa duplicação como um risco de divergência a eliminar.

### A concorrência dimensiona o endpoint
**O modelo:** a regra prática do artigo é que a concorrência necessária é igual a 4 × ceil(RPS alvo ÷ RPS medido no seu SLO), porque o throughput medido acompanhou a concorrência dividida pelo tempo de serviço, com diferença de até 15%.
**Como aplicar:** faça benchmark de um tamanho de endpoint dentro do seu SLO, depois aplique a fórmula para encontrar a concorrência para a sua carga-alvo.

### Meça o frescor separadamente da latência da requisição
**O modelo:** latência da requisição e frescor das features são medidas diferentes. Uma é o que o usuário espera, e a outra é quanto tempo um clique leva para ficar visível ao modelo.
**Como aplicar:** reporte as duas, e mantenha cada uma rotulada com os componentes que inclui.

## 🔑 Principais conclusões
- Uma única declaração de feature atende tanto o treinamento com correção temporal (point-in-time) quanto o serving online, então as features com que um modelo treina são as features que ele vê em produção.
- Segundo o artigo, features de streaming são agregadas de um tópico Kafka para o online store sem um job de streaming escrito à mão.
- O SawtoothWindow mantém uma contagem de 30 dias por categoria fresca à medida que os eventos chegam, sem recalcular todo o histórico a cada evento.
- O ranqueador pontuou diretamente todas as 40 ofertas em cada requisição, sem etapa de redução de candidatos. O streaming ao vivo acrescentou apenas 3–4 ms, e a requisição completa coube no orçamento de 50 ms no menor endpoint.
- O planejamento de capacidade usa concorrência = 4 × ceil(RPS alvo ÷ RPS medido no SLO). Nos testes do artigo, 150 RPS com SLO de p95 de 50 ms exigem concorrência 12.
- Eventos de streaming se tornaram visíveis ao modelo em cerca de 0,1 s, um valor que o artigo mede separadamente da latência da requisição.
- Ranquear ofertas é uma etapa de modelagem. Os controles de elegibilidade, consentimento, adequação e supressão precisam ser construídos antes da produção.

## 📚 Plano de leitura
### A partir do artigo
- [Solution Accelerator no GitHub](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator) — o repositório completo e os notebooks, o ponto de partida para tudo o que o artigo descreve.
- [Parte 1 — fundamentos de Feature Views](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/notebooks/part1_feature_views) — o ranqueador batch e suas declarações de features.
- [Parte 2 — streaming em tempo real](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/notebooks/part2_realtime_two_stage) — a extensão de streaming e seus pré-requisitos de Kafka.
- [Demo e monitoramento](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/apps/recommender-ui) — o app de recomendação e o dashboard de latência.
- [Introducing Feature Views](https://www.databricks.com/blog/introducing-feature-views) — o contexto sobre as definições de features em que o artigo se apoia.
- [Feature Views](https://docs.databricks.com/aws/en/machine-learning/feature-store/feature-views) — a referência para as declarações de janela e de colunas da Etapa 1.
- [Feature Engineering SDK](https://docs.databricks.com/aws/en/machine-learning/feature-store/python-api) — a API por trás do conjunto de treinamento com correção temporal.
- [Declarative Automation Bundles (DABs)](https://docs.databricks.com/aws/en/dev-tools/bundles/) — o método de implantação que o acelerador usa.
- [Consultar endpoints do Model Serving otimizados para rota](https://docs.databricks.com/aws/en/machine-learning/model-serving/query-route-optimization) — o tipo de endpoint por trás do ranqueamento e do feature serving.

### Para ir além
- Joins com correção temporal para dados de treinamento — como rótulos históricos são associados a valores de features sem vazamento.
- Train/serve skew — o modo de falha que este design pretende eliminar.
- Lei de Little — a relação entre concorrência, latência e throughput por trás da fórmula de dimensionamento do artigo (acrescentado por mim; o artigo não a cita pelo nome).
- Scale-to-zero e cold starts em model serving — relevantes para o atraso de dezenas de segundos na primeira requisição que o artigo relata.
- Controles de consentimento, adequação e supressão para ofertas financeiras — as regras de negócio que, segundo o artigo, precisam ser adicionadas antes da produção.
