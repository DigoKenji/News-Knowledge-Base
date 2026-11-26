---
type: Concept
title: Saga pattern
description: Saga pattern (microservices).
tags:
- backend-systems
- microservices
domain: backend-systems
subdomain: microservices
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
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Saga pattern

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `microservices`.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (Architect Elevator added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Write-off; Retry; Compensating Action): Three alternatives to two-phase commit when a step fails: write the loss off, retry, or undo completed steps with compensating actions.

## Sources

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Write-off; Retry; Compensating Action). Three alternatives to two-phase commit when a step fails: write the loss off, retry, or undo completed steps with compensating actions.

## Relations
- **mitigates** [CAP theorem](/concepts/backend-systems/cap-theorem.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
