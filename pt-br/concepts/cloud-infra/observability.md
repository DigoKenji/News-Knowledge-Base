---
type: Concept
title: Observabilidade (logs, métricas, traces)
description: A capacidade de entender o que está acontecendo dentro de um sistema em execução a partir dos sinais que ele emite - principalmente logs (eventos), métricas (números ao longo do tempo) e traces distribuídos (o caminho de uma requisição entre serviços) - para que problemas desconhecidos possam ser investigados, não apenas os conhecidos alertados.
tags:
- cloud-infra
- core
domain: cloud-infra
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: OpenTelemetry Authors. OpenTelemetry documentation - Observability primer and Signals
  resource: 'https://opentelemetry.io/docs/concepts/observability-primer/'
  kind: standard
- title: Sigelman, B. H. et al. (2010). Dapper, a Large-Scale Distributed Systems Tracing Infrastructure. Google Technical Report
  resource: 'https://research.google/pubs/dapper-a-large-scale-distributed-systems-tracing-infrastructure/'
  kind: paper
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 6 "Monitoring Distributed Systems"
  resource: 'https://sre.google/sre-book/monitoring-distributed-systems/'
  kind: book
lang: pt-br
translation_of: /concepts/cloud-infra/observability.md
---

# Observabilidade (logs, métricas, traces)

**Definição.** O termo vem da teoria de controle: um sistema é observável se seu estado interno pode ser inferido a partir de suas saídas. Em software, o OpenTelemetry - um padrão da CNCF - define os sinais (traces, métricas, logs) e uma forma neutra de fornecedor para produzi-los e exportá-los. O tracing distribuído, descrito no artigo Dapper do Google, propaga um id de trace por todas as chamadas para que a latência de uma requisição possa ser decomposta por serviço.

Domínio: [cloud-infra](/pt-br/domains/cloud-infra.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Monitoramento diz que algo está errado; observabilidade ajuda a perguntar por quê, especialmente em sistemas distribuídos em que falhas atravessam muitos serviços. Ela alimenta SLOs, resposta a incidentes e planejamento de capacidade. Os custos crescem rápido com o volume de dados e rótulos de alta cardinalidade, então amostragem e retenção precisam de projeto.

## Exemplo

Um alerta dispara: a latência p99 do checkout subiu para 3 s. O trace de uma requisição lenta mostra 2,7 s gastos no serviço de estoque, as métricas dele mostram esgotamento do pool de conexões e os logs mostram uma nova consulta introduzida no deploy desta manhã.

## Conceitos mais específicos

- [Falha silenciosa](/pt-br/concepts/backend-systems/silent-failure.md)
- [Orçamento de erros de SLO](/pt-br/concepts/cloud-infra/slo-error-budget.md)
- [Engenharia do caos](/pt-br/concepts/backend-systems/chaos-engineering.md)

## Fontes

1. OpenTelemetry Authors. *OpenTelemetry documentation - Observability primer and Signals*. <https://opentelemetry.io/docs/concepts/observability-primer/> — Definição de observabilidade e de traces, métricas e logs.
2. Sigelman, B. H. et al. (2010). *Dapper, a Large-Scale Distributed Systems Tracing Infrastructure*. Google Technical Report. <https://research.google/pubs/dapper-a-large-scale-distributed-systems-tracing-infrastructure/> — Projeto de tracing distribuído, modelo de trace e span.
3. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 6 "Monitoring Distributed Systems". <https://sre.google/sre-book/monitoring-distributed-systems/> — Sinais de ouro, sintomas versus causas, filosofia de alertas.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
