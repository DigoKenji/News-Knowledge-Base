---
type: Concept
title: Replication and partitioning (sharding)
description: Replication keeps copies of the same data on several nodes for availability and read scale; partitioning (sharding) splits the data so each node holds only part of it, for write and storage scale. Most large databases combine both.
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
- title: DeCandia, G. et al. (2007). Dynamo - Amazon's Highly Available Key-value Store. SOSP 2007
  resource: 'https://doi.org/10.1145/1294261.1294281'
  kind: paper
- title: Corbett, J. C. et al. (2012). Spanner - Google's Globally-Distributed Database. OSDI 2012
  resource: 'https://www.usenix.org/conference/osdi12/technical-sessions/presentation/corbett'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 5 "Replication" and ch. 6 "Partitioning"
  resource: 'https://dataintensive.net/'
  kind: book
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Replication and partitioning (sharding)

**Definition.** Replication strategies are single-leader (one node accepts writes and streams them to followers), multi-leader, and leaderless (quorum reads and writes, as in Amazon's Dynamo). Partitioning assigns each record to a partition by key range or by hash of the key; consistent hashing limits how much data moves when nodes join or leave. Each partition is usually replicated.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `distributed-systems`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Replication raises the questions of replication lag and consistency models; partitioning raises hot spots (one popular key overloading one shard), rebalancing and cross-partition queries and transactions. Systems like Spanner combine partitioning with consensus-replicated groups and synchronised clocks to offer global transactions.

## Example

A chat service shards messages by conversation id across 64 partitions, and each partition has one leader and two followers in different zones. Reading history can go to any follower; sending a message goes to the partition's leader.

## Narrower concepts

- [Consensus](/concepts/backend-systems/consensus.md)
- [Change data capture](/concepts/data/change-data-capture.md)
- [Hybrid memory-disk cache](/concepts/backend-systems/hybrid-memory-disk-cache.md)

## Sources

1. DeCandia, G. et al. (2007). *Dynamo - Amazon's Highly Available Key-value Store*. SOSP 2007. <https://doi.org/10.1145/1294261.1294281> — Leaderless replication, quorums and consistent-hashing partitioning.
2. Corbett, J. C. et al. (2012). *Spanner - Google's Globally-Distributed Database*. OSDI 2012. <https://www.usenix.org/conference/osdi12/technical-sessions/presentation/corbett> — Sharding combined with Paxos-replicated groups at global scale.
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 5 "Replication" and ch. 6 "Partitioning". <https://dataintensive.net/> — Leader models, replication lag, partitioning schemes and rebalancing.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
