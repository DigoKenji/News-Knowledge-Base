---
type: Concept
title: Replicação e particionamento (sharding)
description: Replicação mantém cópias dos mesmos dados em vários nós para disponibilidade e escala de leitura; particionamento (sharding) divide os dados para que cada nó guarde apenas uma parte, para escala de escrita e armazenamento. A maioria dos bancos grandes combina os dois.
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
- title: DeCandia, G. et al. (2007). Dynamo - Amazon's Highly Available Key-value Store. SOSP 2007
  resource: 'https://doi.org/10.1145/1294261.1294281'
  kind: paper
- title: Corbett, J. C. et al. (2012). Spanner - Google's Globally-Distributed Database. OSDI 2012
  resource: 'https://www.usenix.org/conference/osdi12/technical-sessions/presentation/corbett'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 5 "Replication" and ch. 6 "Partitioning"
  resource: 'https://dataintensive.net/'
  kind: book
lang: pt-br
translation_of: /concepts/backend-systems/replication-partitioning.md
---

# Replicação e particionamento (sharding)

**Definição.** As estratégias de replicação são líder único (um nó aceita escritas e as transmite aos seguidores), multilíder e sem líder (leituras e escritas por quórum, como no Dynamo da Amazon). O particionamento atribui cada registro a uma partição por faixa de chave ou por hash da chave; o hashing consistente limita quantos dados se movem quando nós entram ou saem. Cada partição geralmente é replicada.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A replicação levanta as questões de atraso de replicação e modelos de consistência; o particionamento levanta pontos quentes (uma chave popular sobrecarregando um shard), rebalanceamento e consultas e transações entre partições. Sistemas como o Spanner combinam particionamento com grupos replicados por consenso e relógios sincronizados para oferecer transações globais.

## Exemplo

Um serviço de chat particiona mensagens por id de conversa em 64 partições, e cada partição tem um líder e dois seguidores em zonas diferentes. Ler o histórico pode ir a qualquer seguidor; enviar uma mensagem vai ao líder da partição.

## Conceitos mais específicos

- [Consenso](/pt-br/concepts/backend-systems/consensus.md)
- [Captura de alterações de dados (CDC)](/pt-br/concepts/data/change-data-capture.md)
- [Cache híbrido memória-disco](/pt-br/concepts/backend-systems/hybrid-memory-disk-cache.md)

## Fontes

1. DeCandia, G. et al. (2007). *Dynamo - Amazon's Highly Available Key-value Store*. SOSP 2007. <https://doi.org/10.1145/1294261.1294281> — Replicação sem líder, quóruns e particionamento por hashing consistente.
2. Corbett, J. C. et al. (2012). *Spanner - Google's Globally-Distributed Database*. OSDI 2012. <https://www.usenix.org/conference/osdi12/technical-sessions/presentation/corbett> — Sharding combinado com grupos replicados por Paxos em escala global.
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 5 "Replication" and ch. 6 "Partitioning". <https://dataintensive.net/> — Modelos de líder, atraso de replicação, esquemas de particionamento e rebalanceamento.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
