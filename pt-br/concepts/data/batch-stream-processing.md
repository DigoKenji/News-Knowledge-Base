---
type: Concept
title: Processamento em lote versus em fluxo (streaming)
description: O processamento em lote roda sobre um conjunto limitado de dados acumulados ao longo do tempo (por exemplo os logs da noite anterior); o processamento em fluxo trata um fluxo ilimitado de eventos continuamente, produzindo resultados segundos após cada evento.
tags:
- data
- core
domain: data
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Dean, J. and Ghemawat, S. (2004). MapReduce - Simplified Data Processing on Large Clusters. OSDI 2004
  resource: 'https://www.usenix.org/conference/osdi-04/mapreduce-simplified-data-processing-large-clusters'
  kind: paper
- title: Akidau, T. et al. (2015). The Dataflow Model - A Practical Approach to Balancing Correctness, Latency, and Cost in Massive-Scale, Unbounded, Out-of-Order Data Processing. PVLDB 8(12)
  resource: 'https://doi.org/10.14778/2824032.2824076'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 10 "Batch Processing" and ch. 11 "Stream Processing"
  resource: 'https://dataintensive.net/'
  kind: book
lang: pt-br
translation_of: /concepts/data/batch-stream-processing.md
---

# Processamento em lote versus em fluxo (streaming)

**Definição.** O MapReduce popularizou o processamento em lote em grande escala: dividir uma entrada grande e finita, processar as partes em paralelo, combinar os resultados. O processamento em fluxo trata dados como uma sequência ilimitada de eventos; o modelo Dataflow de Akidau et al. enquadra as perguntas que todo pipeline precisa responder - o que é calculado, onde no tempo do evento (janelas), quando os resultados são emitidos (watermarks e gatilhos) e como dados atrasados os refinam - e mostra o lote como caso particular do fluxo.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O lote é mais simples, barato e fácil de reexecutar; o fluxo dá atualidade (detecção de fraude, painéis ao vivo, alertas) ao custo de lidar com eventos fora de ordem, estado e semântica exatamente-uma-vez. Muitas plataformas usam fluxo para ingestão (Kafka, captura de dados de mudança) e lote para recálculos pesados.

## Exemplo

Um banco recalcula os escores de risco dos clientes num job em lote noturno sobre todas as transações, enquanto um job de fluxo pontua cada pagamento com cartão em 200 ms e o bloqueia se parecer fraude.

## Conceitos mais específicos

- [Captura de alterações de dados (CDC)](/pt-br/concepts/data/change-data-capture.md)
- [Fila de mensagens e mensageria assíncrona](/pt-br/concepts/backend-systems/message-queue.md)

## Fontes

1. Dean, J. and Ghemawat, S. (2004). *MapReduce - Simplified Data Processing on Large Clusters*. OSDI 2004. <https://www.usenix.org/conference/osdi-04/mapreduce-simplified-data-processing-large-clusters> — Modelo de processamento em lote em grande escala.
2. Akidau, T. et al. (2015). *The Dataflow Model - A Practical Approach to Balancing Correctness, Latency, and Cost in Massive-Scale, Unbounded, Out-of-Order Data Processing*. PVLDB 8(12). <https://doi.org/10.14778/2824032.2824076> — Dados ilimitados, tempo do evento, janelas, watermarks e gatilhos.
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 10 "Batch Processing" and ch. 11 "Stream Processing". <https://dataintensive.net/> — Comparação de sistemas em lote e em fluxo e seus trade-offs.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
