---
type: Concept
title: Consensus
description: The problem of getting several nodes to agree on one value (or one ordered log of commands) even when some nodes crash or messages are delayed - solved in practice by algorithms such as Paxos and Raft.
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
- title: Lamport, L. (2001). Paxos Made Simple. ACM SIGACT News 32(4)
  resource: 'https://lamport.azurewebsites.net/pubs/paxos-simple.pdf'
  kind: paper
- title: Ongaro, D. and Ousterhout, J. (2014). In Search of an Understandable Consensus Algorithm. USENIX ATC 2014
  resource: 'https://raft.github.io/raft.pdf'
  kind: paper
- title: Fischer, M. J., Lynch, N. A. and Paterson, M. S. (1985). Impossibility of Distributed Consensus with One Faulty Process. Journal of the ACM 32(2)
  resource: 'https://doi.org/10.1145/3149.214121'
  kind: paper
---

# Consensus

**Definition.** A consensus algorithm lets a group of processes agree on a value such that all non-faulty processes decide the same value, the value was proposed by someone, and they eventually decide. Fischer, Lynch and Paterson proved no deterministic algorithm can guarantee this in a fully asynchronous system with even one crash, so practical algorithms (Paxos, Raft) are always safe and make progress when the network is calm enough. They need a majority (quorum) of nodes alive.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Consensus underlies leader election, distributed locks, configuration stores (etcd, ZooKeeper) and strongly consistent replicated databases. Running it on every write costs latency, so systems usually reserve it for the control plane or for metadata, and use cheaper replication for bulk data.

## Example

A Kubernetes cluster stores its state in a 3-node etcd cluster running Raft. If one etcd node dies, the other two still form a majority and keep accepting changes; if two die, the cluster refuses writes rather than risk two conflicting versions of the truth.

## Narrower concepts

- [Control plane](/concepts/backend-systems/control-plane.md)
- [Linearizability](/concepts/backend-systems/linearizability.md)

## Sources

1. Lamport, L. (2001). *Paxos Made Simple*. ACM SIGACT News 32(4). <https://lamport.azurewebsites.net/pubs/paxos-simple.pdf> — The Paxos algorithm and the safety requirements of consensus.
2. Ongaro, D. and Ousterhout, J. (2014). *In Search of an Understandable Consensus Algorithm*. USENIX ATC 2014. <https://raft.github.io/raft.pdf> — Raft - leader election, log replication and majority quorums.
3. Fischer, M. J., Lynch, N. A. and Paterson, M. S. (1985). *Impossibility of Distributed Consensus with One Faulty Process*. Journal of the ACM 32(2). <https://doi.org/10.1145/3149.214121> — FLP impossibility result in asynchronous systems.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
