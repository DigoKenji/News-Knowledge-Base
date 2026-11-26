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
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
---

# CAP theorem

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `distributed-systems`.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (DDIA added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 9 Consistency and Consensus: The Cost of Linearizability; The CAP theorem; The Unhelpful CAP Theorem: Argues "pick 2 of 3" is misleading because partitions are not optional; better read as "consistent or available when partitioned".

## Sources

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 9 Consistency and Consensus: The Cost of Linearizability; The CAP theorem; The Unhelpful CAP Theorem. Argues "pick 2 of 3" is misleading because partitions are not optional; better read as "consistent or available when partitioned".

## Relations
- [Saga pattern](/concepts/backend-systems/saga-pattern.md) **mitigates** this

## Notes
Notes tagged with this concept are listed in the concept index after each run.
