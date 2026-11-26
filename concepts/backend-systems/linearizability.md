---
type: Concept
title: Linearizability
description: The strongest single-object consistency model - every operation appears to happen instantly at one point between its start and its end, so all clients see one up-to-date order of operations consistent with real time.
tags:
- backend-systems
- distributed-systems
domain: backend-systems
subdomain: distributed-systems
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Herlihy, M. and Wing, J. (1990). Linearizability - A Correctness Condition for Concurrent Objects. ACM TOPLAS 12(3)
  resource: 'https://dl.acm.org/doi/10.1145/78969.78972'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 9 Consistency and Consensus. O'Reilly
  resource: 'https://dataintensive.net/'
  kind: book
- title: Jepsen. Consistency Models - Linearizability
  resource: 'https://jepsen.io/consistency/models/linearizable'
  kind: practitioner
- title: Bailis, P. (2014). Linearizability versus Serializability
  resource: 'https://www.bailis.org/blog/linearizability-versus-serializability/'
  kind: practitioner
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Linearizability

**Definition.** A system is linearizable if, for every execution, there is a total order of operations that (1) matches each object's sequential specification and (2) respects real time: if operation A finished before B started, A comes before B. Informally, the system behaves as if there were a single copy of the data, even if it is replicated.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `distributed-systems`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Defined by Herlihy and Wing (1990). Once any read returns a new value, all later reads must return it too: no going back in time. Implementations need coordination: single-leader replication with reads from the leader (and leader leases), or consensus protocols (Raft, Paxos) used by etcd, ZooKeeper writes and Spanner (which adds TrueTime for external consistency). The CAP theorem says a linearizable system cannot stay available during a network partition. It differs from serializability, which is about transactions over many objects and has no real-time requirement; strict serializability combines both.

## Why it matters and trade-offs

It makes distributed systems easy to reason about and is required for locks, leader election, unique constraints and counters that must not go backwards. It costs latency (coordination round trips, often across regions) and availability under partitions, which is why many systems offer weaker models (sequential, causal, eventual) by default.

## Example

Alice updates her profile photo and the call returns. If Bob then loads the page and sees the old photo from a lagging replica, the system is not linearizable.

## Sources

1. Herlihy, M. and Wing, J. (1990). *Linearizability - A Correctness Condition for Concurrent Objects*. ACM TOPLAS 12(3). <https://dl.acm.org/doi/10.1145/78969.78972> — Formal definition.
2. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 9 Consistency and Consensus. O'Reilly. <https://dataintensive.net/> — Practical explanation, implementations, cost, relation to CAP.
3. Jepsen. *Consistency Models - Linearizability*. <https://jepsen.io/consistency/models/linearizable> — Position in the hierarchy of consistency models; what it does and does not guarantee.
4. Bailis, P. (2014). *Linearizability versus Serializability*. <https://www.bailis.org/blog/linearizability-versus-serializability/> — The difference between the two terms and strict serializability.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
