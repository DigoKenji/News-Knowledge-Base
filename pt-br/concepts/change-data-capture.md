---
type: Concept
title: Captura de alterações de dados (CDC)
description: Capturar cada inserção, atualização e exclusão confirmada num banco de dados - geralmente lendo seu log de transações - e publicá-las como um fluxo ordenado de eventos de mudança para outros sistemas consumirem.
tags:
- data
domain: data
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 11 Stream Processing (Change Data Capture). O'Reilly
  resource: 'https://dataintensive.net/'
  kind: book
- title: Das, S. et al. (2012). All Aboard the Databus! LinkedIn's Scalable Consistent Change Data Capture Platform. SoCC 2012
  resource: 'https://dl.acm.org/doi/10.1145/2391229.2391247'
  kind: paper
- title: Debezium. Debezium Features (reference documentation)
  resource: 'https://debezium.io/documentation/reference/stable/features.html'
  kind: docs
lang: pt-br
translation_of: /concepts/change-data-capture.md
---

# Captura de alterações de dados (CDC)

**Definição.** Change data capture (CDC) transforma um banco de dados numa fonte de eventos. Em vez de outros sistemas consultarem tabelas periodicamente ou a aplicação escrever em dois lugares, um conector de CDC observa as mudanças confirmadas em nível de linha e as emite, na ordem de commit, para um log ou stream como o Kafka.

Domínio: [data](/pt-br/domains/data.md). Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Abordagens: CDC baseado em log lê o log de replicação do banco (binlog do MySQL, WAL do PostgreSQL via logical decoding, redo do Oracle, tabelas de CDC do SQL Server), com baixo overhead e captura de exclusões e da ordem exata; CDC por triggers grava as mudanças em tabelas-sombra; CDC por consulta lê periodicamente uma coluna de "última atualização", mais simples mas perde exclusões e estados intermediários. Ferramentas: Debezium, AWS DMS, Fivetran e o antigo Databus do LinkedIn. Um pipeline típico faz um snapshot inicial e depois transmite as mudanças; os consumidores as aplicam em índices de busca, caches, data lakes (Iceberg, Delta) ou outros serviços. O padrão outbox combina CDC com uma tabela de saída para que serviços publiquem eventos de domínio com confiabilidade.

## Por que importa e trade-offs

O CDC mantém sistemas derivados sincronizados quase em tempo real sem escritas duplas, uma fonte clássica de inconsistência. Custos: dependência de detalhes internos do banco e da retenção do log, mudanças de schema que precisam fluir adiante, entrega at-least-once que exige consumidores idempotentes, e exposição da estrutura interna das tabelas como contrato implícito.

## Exemplo

A United Airlines (nota de 2026-11-22) transmite mudanças dos bancos operacionais para tabelas Apache Iceberg para que a análise veja reservas em minutos em vez de depois de um lote noturno.

## Fontes

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 11 Stream Processing (Change Data Capture). O'Reilly. <https://dataintensive.net/> — CDC como forma de manter sistemas de dados derivados sincronizados; problemas das escritas duplas.
2. Das, S. et al. (2012). *All Aboard the Databus! LinkedIn's Scalable Consistent Change Data Capture Platform*. SoCC 2012. <https://dl.acm.org/doi/10.1145/2391229.2391247> — Projeto revisado por pares de um sistema de CDC em produção.
3. Debezium. *Debezium Features* (reference documentation). <https://debezium.io/documentation/reference/stable/features.html> — CDC baseado em log vs por consulta, snapshots, captura de exclusões.

## Relações
- (nenhuma ainda)

## Notas
- [Como a United Airlines transmite alterações de banco de dados para o Apache Iceberg com o Amazon Data Firehose](/pt-br/notes/data/2026-11-22-how-united-airlines-streams-database-changes-to-apache-icebe.md)
