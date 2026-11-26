---
type: Concept
title: Sistema distribuído
description: Um conjunto de computadores independentes que se comunicam por rede e se coordenam para parecer aos usuários um único sistema coerente - ao preço de falhas parciais, ausência de relógio comum e mensagens não confiáveis.
tags:
- backend-systems
- distributed-systems
- core
domain: backend-systems
subdomain: distributed-systems
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: van Steen, M. and Tanenbaum, A. S. (2023). Distributed Systems, 4th edition
  resource: 'https://www.distributed-systems.net/index.php/books/ds4/'
  kind: book
- title: Lamport, L. (1978). Time, Clocks, and the Ordering of Events in a Distributed System. Communications of the ACM 21(7)
  resource: 'https://doi.org/10.1145/359545.359563'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 8 "The Trouble with Distributed Systems"
  resource: 'https://dataintensive.net/'
  kind: book
lang: pt-br
translation_of: /concepts/backend-systems/distributed-system.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Sistema distribuído

**Definição.** Tanenbaum e van Steen definem um sistema distribuído como uma coleção de elementos computacionais autônomos que aparece para os usuários como um único sistema coerente. Os elementos não compartilham memória nem relógio global: apenas trocam mensagens, que podem atrasar, se perder ou chegar fora de ordem, e qualquer um deles pode falhar enquanto os outros continuam funcionando.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `distributed-systems`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Quase todo backend moderno é distribuído: serviços, bancos de dados, caches e filas em muitas máquinas. Distribuir traz escala, tolerância a falhas e alcance geográfico, mas acrescenta problemas que uma única máquina nunca tem: falha parcial, ordenação de eventos (os relógios lógicos de Lamport), consistência entre réplicas e acordo (consenso). O resto deste tema trata de gerenciar esses problemas.

## Exemplo

Uma loja online roda servidores web, serviço de pedidos, serviço de pagamentos e banco de dados em dezenas de máquinas em duas regiões. Quando o link de rede de uma réplica do banco cai, os pedidos não podem se perder nem ser cobrados duas vezes, mesmo que algumas máquinas não enxerguem outras.

## Conceitos mais específicos

- [Latência, vazão e latência de cauda](/pt-br/concepts/backend-systems/latency-throughput.md)
- [Escalabilidade (vertical e horizontal)](/pt-br/concepts/backend-systems/scalability.md)
- [Tolerância a falhas e redundância](/pt-br/concepts/backend-systems/fault-tolerance.md)
- [Modelos de consistência](/pt-br/concepts/backend-systems/consistency-models.md)
- [Replicação e particionamento (sharding)](/pt-br/concepts/backend-systems/replication-partitioning.md)
- [Consenso](/pt-br/concepts/backend-systems/consensus.md)
- [Teorema CAP](/pt-br/concepts/backend-systems/cap-theorem.md)

## Fontes

1. van Steen, M. and Tanenbaum, A. S. (2023). *Distributed Systems*, 4th edition. <https://www.distributed-systems.net/index.php/books/ds4/> — Definição ("aparece para os usuários como um único sistema coerente") e objetivos de projeto.
2. Lamport, L. (1978). *Time, Clocks, and the Ordering of Events in a Distributed System*. Communications of the ACM 21(7). <https://doi.org/10.1145/359545.359563> — Ausência de relógio global; ordenação "aconteceu antes" e relógios lógicos.
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 8 "The Trouble with Distributed Systems". <https://dataintensive.net/> — Falhas parciais, redes e relógios não confiáveis na prática.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
