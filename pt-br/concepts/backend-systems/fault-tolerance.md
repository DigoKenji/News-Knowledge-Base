---
type: Concept
title: Tolerância a falhas e redundância
description: A capacidade de um sistema de continuar entregando serviço correto quando alguns de seus componentes falham, geralmente obtida por redundância (cópias reservas de componentes ou dados), detecção de erros e recuperação.
tags:
- backend-systems
- reliability
- core
domain: backend-systems
subdomain: reliability
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Avizienis, A., Laprie, J.-C., Randell, B. and Landwehr, C. (2004). Basic Concepts and Taxonomy of Dependable and Secure Computing. IEEE Transactions on Dependable and Secure Computing 1(1)
  resource: 'https://doi.org/10.1109/TDSC.2004.2'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 1 "Reliability" and ch. 8
  resource: 'https://dataintensive.net/'
  kind: book
- title: Brooker, M. Timeouts, retries, and backoff with jitter. Amazon Builders' Library
  resource: 'https://builder.aws.com/content/3EumjoZascWd1oZiEgL8ORlv3qE/timeouts-retries-and-backoff-with-jitter'
  kind: blog
lang: pt-br
translation_of: /concepts/backend-systems/fault-tolerance.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Tolerância a falhas e redundância

**Definição.** Avizienis, Laprie, Randell e Landwehr apresentam a cadeia padrão: uma falta (defeito ou causa) pode ser ativada como erro (estado interno incorreto), que pode se propagar como falha (o serviço se desvia do requerido). Tolerância a falhas é o conjunto de meios que evitam a falha do serviço na presença de faltas: detecção de erros, recuperação (rollback, rollforward, failover) e mascaramento por redundância.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `reliability`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A redundância só ajuda quando as cópias falham de forma independente: falhas correlacionadas (mesmo bug, mesma região, mesma configuração errada) a derrotam. Técnicas práticas incluem réplicas entre zonas, novas tentativas com backoff e idempotência, timeouts, circuit breakers e degradação graciosa. O vocabulário falta, erro e falha mantém a análise de incidentes precisa.

## Exemplo

Um banco de dados roda um primário e duas réplicas em três zonas de disponibilidade. Quando a zona do primário perde energia, uma réplica é promovida automaticamente em 30 segundos; os clientes repetem as escritas que falharam com a mesma chave de idempotência, então nenhum pedido se perde ou duplica.

## Conceitos mais específicos

- [Falha em cascata](/pt-br/concepts/backend-systems/cascading-failure.md)
- [Falha silenciosa](/pt-br/concepts/backend-systems/silent-failure.md)
- [Falha correlacionada](/pt-br/concepts/backend-systems/correlated-failure.md)
- [Tempestade de retentativas](/pt-br/concepts/backend-systems/retry-storm.md)
- [Engenharia do caos](/pt-br/concepts/backend-systems/chaos-engineering.md)
- [Idempotência](/pt-br/concepts/backend-systems/idempotency.md)

## Fontes

1. Avizienis, A., Laprie, J.-C., Randell, B. and Landwehr, C. (2004). *Basic Concepts and Taxonomy of Dependable and Secure Computing*. IEEE Transactions on Dependable and Secure Computing 1(1). <https://doi.org/10.1109/TDSC.2004.2> — Cadeia falta-erro-falha e definição de tolerância a falhas.
2. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 1 "Reliability" and ch. 8. <https://dataintensive.net/> — Faltas versus falhas, faltas de hardware, software e humanas na prática.
3. Brooker, M. *Timeouts, retries, and backoff with jitter*. Amazon Builders' Library. <https://builder.aws.com/content/3EumjoZascWd1oZiEgL8ORlv3qE/timeouts-retries-and-backoff-with-jitter> — Técnicas práticas para tolerar faltas sem amplificá-las.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
