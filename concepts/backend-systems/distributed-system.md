---
type: Concept
title: Distributed system
description: A set of independent computers that communicate over a network and coordinate to appear to their users as a single coherent system - with partial failures, no shared clock and unreliable messages as the price.
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
- title: van Steen, M. and Tanenbaum, A. S. (2023). Distributed Systems, 4th edition
  resource: 'https://www.distributed-systems.net/index.php/books/ds4/'
  kind: book
- title: Lamport, L. (1978). Time, Clocks, and the Ordering of Events in a Distributed System. Communications of the ACM 21(7)
  resource: 'https://doi.org/10.1145/359545.359563'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 8 "The Trouble with Distributed Systems"
  resource: 'https://dataintensive.net/'
  kind: book
---

# Distributed system

**Definition.** Tanenbaum and van Steen define a distributed system as a collection of autonomous computing elements that appears to its users as a single coherent system. The elements share no memory and no global clock: they only exchange messages, which can be delayed, lost or reordered, and any one of them can fail while the others keep running.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Almost every modern backend is distributed: services, databases, caches and queues on many machines. Distribution buys scale, fault tolerance and geographic reach, but adds problems a single machine never has: partial failure, ordering of events (Lamport's logical clocks), consistency between replicas and agreement (consensus). The rest of this theme is about managing those problems.

## Example

An online store runs its web servers, order service, payment service and database on dozens of machines in two regions. When one database replica's network link drops, orders must neither be lost nor charged twice, even though some machines cannot see others.

## Narrower concepts

- [Latency, throughput and tail latency](/concepts/backend-systems/latency-throughput.md)
- [Scalability (vertical and horizontal)](/concepts/backend-systems/scalability.md)
- [Fault tolerance and redundancy](/concepts/backend-systems/fault-tolerance.md)
- [Consistency models](/concepts/backend-systems/consistency-models.md)
- [Replication and partitioning (sharding)](/concepts/backend-systems/replication-partitioning.md)
- [Consensus](/concepts/backend-systems/consensus.md)
- [CAP theorem](/concepts/backend-systems/cap-theorem.md)

## Sources

1. van Steen, M. and Tanenbaum, A. S. (2023). *Distributed Systems*, 4th edition. <https://www.distributed-systems.net/index.php/books/ds4/> — Definition ("appears to its users as a single coherent system") and design goals.
2. Lamport, L. (1978). *Time, Clocks, and the Ordering of Events in a Distributed System*. Communications of the ACM 21(7). <https://doi.org/10.1145/359545.359563> — No global clock; happened-before ordering and logical clocks.
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 8 "The Trouble with Distributed Systems". <https://dataintensive.net/> — Partial failures, unreliable networks and clocks in practice.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
