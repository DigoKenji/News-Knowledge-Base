---
type: Concept
title: Escalabilidade (vertical e horizontal)
description: A capacidade de um sistema de lidar com carga crescente acrescentando recursos - uma máquina maior (vertical, scale up) ou mais máquinas (horizontal, scale out) - mantendo o desempenho aceitável e o custo proporcional.
tags:
- backend-systems
- core
domain: backend-systems
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Bondi, A. B. (2000). Characteristics of Scalability and Their Impact on Performance. Proceedings of WOSP 2000 (ACM)
  resource: 'https://doi.org/10.1145/350391.350432'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 1 "Scalability"
  resource: 'https://dataintensive.net/'
  kind: book
- title: Amazon Web Services. Reliability Pillar - AWS Well-Architected Framework ("Use horizontal scaling")
  resource: 'https://docs.aws.amazon.com/wellarchitected/latest/framework/reliability.html'
  kind: docs
lang: pt-br
translation_of: /concepts/backend-systems/scalability.md
---

# Escalabilidade (vertical e horizontal)

**Definição.** Bondi caracteriza escalabilidade como a capacidade de um sistema de acomodar um número crescente de elementos ou de carga, processar volumes crescentes de trabalho de forma elegante e ser passível de ampliação. A escala vertical acrescenta CPU, memória ou disco a uma máquina; a escala horizontal acrescenta máquinas e divide o trabalho entre elas, o que exige serviços sem estado ou dados particionados.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O conselho de Kleppmann é primeiro descrever a carga (requisições por segundo, proporção leitura/escrita, fan-out) e depois perguntar o que acontece com o desempenho quando ela cresce. A escala vertical é simples mas tem teto e um ponto único de falha; a horizontal não tem teto fixo, mas exige balanceamento de carga, particionamento e lidar com os problemas de sistemas distribuídos. A capacidade elástica da nuvem tornou a escala horizontal o padrão.

## Exemplo

Uma API web com 500 requisições/s passa de um servidor de 4 núcleos para um de 32 (vertical). Com 20.000 requisições/s, ela roda 40 instâncias idênticas sem estado atrás de um balanceador de carga e particiona o banco de dados por id de cliente (horizontal).

## Conceitos mais específicos

- [Replicação e particionamento (sharding)](/pt-br/concepts/backend-systems/replication-partitioning.md)
- [Balanceamento de carga e contrapressão (backpressure)](/pt-br/concepts/backend-systems/load-balancing-backpressure.md)
- [Cache (caching)](/pt-br/concepts/backend-systems/caching.md)
- [Limite adaptativo de concorrência](/pt-br/concepts/backend-systems/adaptive-concurrency-limit.md)

## Fontes

1. Bondi, A. B. (2000). *Characteristics of Scalability and Their Impact on Performance*. Proceedings of WOSP 2000 (ACM). <https://doi.org/10.1145/350391.350432> — Definição e tipos de escalabilidade (carga, espaço, estrutural).
2. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 1 "Scalability". <https://dataintensive.net/> — Descrição da carga, escalar para cima versus escalar para fora.
3. Amazon Web Services. *Reliability Pillar - AWS Well-Architected Framework* ("Use horizontal scaling"). <https://docs.aws.amazon.com/wellarchitected/latest/framework/reliability.html> — Orientação prática de escalar horizontalmente para evitar pontos únicos de falha.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
