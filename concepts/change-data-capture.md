---
type: Concept
title: Change data capture
description: Capturing every insert, update and delete committed in a database - usually by reading its transaction log - and publishing them as an ordered stream of change events for other systems to consume.
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
---

# Change data capture

**Definition.** Change data capture (CDC) turns a database into a source of events. Instead of other systems polling tables or the application writing to two places, a CDC connector observes committed row-level changes and emits them, in commit order, to a log or stream such as Kafka.

Domain: [data](/domains/data.md). Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Approaches: log-based CDC reads the database's replication log (MySQL binlog, PostgreSQL WAL via logical decoding, Oracle redo, SQL Server CDC tables), which is low-overhead and captures deletes and exact order; trigger-based CDC writes changes to shadow tables; query-based CDC polls a "last updated" column, simpler but it misses deletes and intermediate states. Tools: Debezium, AWS DMS, Fivetran, and LinkedIn's earlier Databus. A typical pipeline takes an initial snapshot, then streams changes; consumers apply them to search indexes, caches, data lakes (Iceberg, Delta) or other services. The outbox pattern pairs CDC with an outbox table so services publish domain events reliably.

## Why it matters and trade-offs

CDC keeps derived systems in sync in near real time without dual writes, which are a classic source of inconsistency. Costs: dependency on database internals and log retention, schema changes that must flow downstream, at-least-once delivery that requires idempotent consumers, and exposing internal table structure as an implicit contract.

## Example

United Airlines (2026-11-22 note) streams changes from operational databases into Apache Iceberg tables so analytics see bookings within minutes instead of after a nightly batch.

## Sources

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 11 Stream Processing (Change Data Capture). O'Reilly. <https://dataintensive.net/> — CDC as keeping derived data systems in sync; problems with dual writes.
2. Das, S. et al. (2012). *All Aboard the Databus! LinkedIn's Scalable Consistent Change Data Capture Platform*. SoCC 2012. <https://dl.acm.org/doi/10.1145/2391229.2391247> — Peer-reviewed production CDC system design.
3. Debezium. *Debezium Features* (reference documentation). <https://debezium.io/documentation/reference/stable/features.html> — Log-based vs query-based CDC, snapshots, delete capture.

## Relations
- (none yet)

## Notes
- [How United Airlines streams database changes to Apache Iceberg with Amazon Data Firehose](/notes/data/2026-11-22-how-united-airlines-streams-database-changes-to-apache-icebe.md)
