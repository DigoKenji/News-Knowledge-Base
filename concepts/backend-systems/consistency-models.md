---
type: Concept
title: Consistency models
description: The rules a distributed data store promises about which values reads can return when data is copied across nodes - from strong (every read sees the latest write) to eventual (replicas converge if writes stop).
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
- title: Jepsen. Consistency Models (map and definitions)
  resource: 'https://jepsen.io/consistency'
  kind: docs
- title: Vogels, W. (2009). Eventually Consistent. Communications of the ACM 52(1)
  resource: 'https://doi.org/10.1145/1435417.1435432'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 5 and ch. 9 "Consistency and Consensus"
  resource: 'https://dataintensive.net/'
  kind: book
---

# Consistency models

**Definition.** A consistency model is a contract between a data store and its clients about the order and visibility of operations. Strong models such as linearizability make the system behave like a single copy; weaker models such as causal consistency, read-your-writes or eventual consistency allow stale reads in exchange for lower latency and higher availability. Jepsen maintains a widely used map of these models and how they relate.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Choosing a model is a trade-off between correctness guarantees, latency and availability during network partitions (CAP, and PACELC for the no-partition case). Database consistency (the C in ACID, about invariants) is a different idea from replica consistency; mixing them up is a common source of confusion.

## Example

A social network stores likes with eventual consistency: a friend may see 41 likes while you see 42 for a second. Its bank-transfer service uses a linearizable store, so two withdrawals can never both read the same old balance.

## Narrower concepts

- [Linearizability](/concepts/backend-systems/linearizability.md)
- [CAP theorem](/concepts/backend-systems/cap-theorem.md)
- [Replication and partitioning (sharding)](/concepts/backend-systems/replication-partitioning.md)
- [Transaction and ACID](/concepts/data/transactions-acid.md)

## Sources

1. Jepsen. *Consistency Models* (map and definitions). <https://jepsen.io/consistency> — Hierarchy of consistency models and their definitions.
2. Vogels, W. (2009). *Eventually Consistent*. Communications of the ACM 52(1). <https://doi.org/10.1145/1435417.1435432> — Eventual consistency and client-side variants, from a large-scale operator.
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 5 and ch. 9 "Consistency and Consensus". <https://dataintensive.net/> — Replication lag anomalies and the spectrum from eventual to linearizable.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
