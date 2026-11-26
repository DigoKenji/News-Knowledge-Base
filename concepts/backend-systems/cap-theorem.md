---
type: Concept
title: CAP theorem
description: 'The result that a replicated data store cannot be both linearizably consistent and fully available while the network is partitioned - it must choose one during a partition.'
tags:
- backend-systems
- distributed-systems
domain: backend-systems
subdomain: distributed-systems
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 9 Consistency and Consensus: The Cost of Linearizability; The CAP theorem; The Unhelpful CAP Theorem'
- title: 'Gilbert, S. and Lynch, N. (2002). Brewer''s Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services. ACM SIGACT News 33(2)'
  resource: 'https://doi.org/10.1145/564585.564601'
  kind: paper
- title: 'Brewer, E. (2012). CAP Twelve Years Later: How the "Rules" Have Changed. IEEE Computer 45(2)'
  resource: 'https://doi.org/10.1109/MC.2012.37'
  kind: paper
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# CAP theorem

**Definition.** The CAP theorem, conjectured by Eric Brewer in 2000 and proved by Gilbert and Lynch in 2002, says a replicated data store cannot guarantee both consistency (linearizability) and availability (every request to a non-failed node gets a response) when the network is partitioned. Since partitions happen whether you like it or not, the real choice is what the system does during one: refuse some requests to stay consistent, or answer them and risk stale data.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `distributed-systems`. Expanded on 2026-11-26 with independent sources.

## How it fits

Brewer himself later warned that 'pick two of three' oversimplifies: the choice is per operation and only during partitions, and latency trade-offs matter all the time (PACELC). Kleppmann calls CAP unhelpful for design and prefers precise consistency models.

## Example

During a link failure between two data centres, a bank's ledger rejects writes in the minority side (consistent), while a shopping cart keeps accepting items on both sides and merges later (available).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (DDIA added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 9 Consistency and Consensus: The Cost of Linearizability; The CAP theorem; The Unhelpful CAP Theorem: Argues "pick 2 of 3" is misleading because partitions are not optional; better read as "consistent or available when partitioned".

## Sources

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 9 Consistency and Consensus: The Cost of Linearizability; The CAP theorem; The Unhelpful CAP Theorem. Argues "pick 2 of 3" is misleading because partitions are not optional; better read as "consistent or available when partitioned".
2. Gilbert, S. and Lynch, N. (2002). Brewer's Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services. ACM SIGACT News 33(2). <https://doi.org/10.1145/564585.564601> — The formal proof.
3. Brewer, E. (2012). CAP Twelve Years Later: How the "Rules" Have Changed. IEEE Computer 45(2). <https://doi.org/10.1109/MC.2012.37> — Brewer's own correction of the 'two of three' reading.

## Relations
- [Saga pattern](/concepts/backend-systems/saga-pattern.md) **mitigates** this

## Notes
Notes tagged with this concept are listed in the concept index after each run.
