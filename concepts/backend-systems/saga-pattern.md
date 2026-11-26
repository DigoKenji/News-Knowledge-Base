---
type: Concept
title: Saga pattern
description: 'Running a business transaction across services as a sequence of local transactions, each with a compensating action that undoes it if a later step fails.'
tags:
- backend-systems
- messaging-and-workflows
domain: backend-systems
subdomain: messaging-and-workflows
also_in:
- architecture/software-architecture
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
relations:
- type: mitigates
  target: /concepts/backend-systems/cap-theorem.md
sources:
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 17 Your Coffee Shop Doesn''t Use Two-Phase Commit (Write-off; Retry; Compensating Action)'
- title: 'Garcia-Molina, H. and Salem, K. (1987). Sagas. SIGMOD 1987'
  resource: 'https://doi.org/10.1145/38713.38742'
  kind: paper
- title: 'Richardson, C. Pattern: Saga (microservices.io)'
  resource: 'https://microservices.io/patterns/data/saga.html'
  kind: practitioner
- title: 'Microsoft. Saga distributed transactions pattern (Azure Architecture Center)'
  resource: 'https://learn.microsoft.com/en-us/azure/architecture/patterns/saga'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Saga pattern

**Definition.** A saga splits a long business transaction into a sequence of local transactions, each committed in its own service or database; if a step fails, compensating transactions undo the steps already done. Garcia-Molina and Salem described sagas in 1987 for long-lived database transactions; microservices adopted them because two-phase commit across services is fragile and blocks. Sagas are coordinated either by choreography (services react to each other's events) or by orchestration (a coordinator tells each service what to do).

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `messaging-and-workflows`. Expanded on 2026-11-26 with independent sources.

## How it fits

Sagas give up isolation: other transactions can see intermediate states, so designs add semantic locks, commutative updates or reordering. Each step must be idempotent and each compensation must be possible, which is easy for refunds and hard for sent emails.

## Example

Book trip: reserve flight → reserve hotel → charge card. The charge fails, so the orchestrator cancels the hotel and then the flight.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (Architect Elevator added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Write-off; Retry; Compensating Action): Three alternatives to two-phase commit when a step fails: write the loss off, retry, or undo completed steps with compensating actions.

## Sources

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Write-off; Retry; Compensating Action). Three alternatives to two-phase commit when a step fails: write the loss off, retry, or undo completed steps with compensating actions.
2. Garcia-Molina, H. and Salem, K. (1987). Sagas. SIGMOD 1987. <https://doi.org/10.1145/38713.38742> — Original definition with compensating transactions.
3. Richardson, C. Pattern: Saga (microservices.io). <https://microservices.io/patterns/data/saga.html> — Choreography versus orchestration in microservices.
4. Microsoft. Saga distributed transactions pattern (Azure Architecture Center). <https://learn.microsoft.com/en-us/azure/architecture/patterns/saga> — Isolation anomalies and countermeasures.

## Relations
- **mitigates** [CAP theorem](/concepts/backend-systems/cap-theorem.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
