---
type: Concept
title: Technical debt
description: The future cost created by choosing a quick or expedient solution now instead of a better one - like financial debt, it charges "interest" as extra effort on every later change until it is paid down by refactoring.
tags:
- software-engineering
- design
- core
domain: software-engineering
subdomain: design
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Cunningham, W. (1992). The WyCash Portfolio Management System. OOPSLA 1992 experience report
  resource: 'https://c2.com/doc/oopsla92.html'
  kind: paper
- title: Kruchten, P., Nord, R. L. and Ozkaya, I. (2012). Technical Debt - From Metaphor to Theory and Practice. IEEE Software 29(6)
  resource: 'https://doi.org/10.1109/MS.2012.167'
  kind: paper
- title: Fowler, M. (2009). TechnicalDebtQuadrant
  resource: 'https://martinfowler.com/bliki/TechnicalDebtQuadrant.html'
  kind: practitioner
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 12 If You Never Kill Anything, You Will Live Among Zombies'
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Technical debt

**Definition.** Ward Cunningham coined the metaphor in 1992: shipping first-time code is like going into debt, which is fine if it is paid back promptly with a rewrite; the danger is when it is never repaid. Fowler's quadrant separates deliberate from inadvertent and prudent from reckless debt.

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `design`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Technical debt is a management concept: it lets engineers explain to business stakeholders why speed drops over time and why time for refactoring is an investment. It accumulates in code, architecture, tests, documentation and infrastructure. Some debt is rational (to hit a market window); unmanaged debt slows delivery and causes incidents.

## Example

Hard-coding the list of news sources to launch faster is deliberate, prudent debt; moving them into config/sources.yaml later pays it off.

## Narrower concepts

- [Strangler fig migration](/concepts/architecture/strangler-fig.md)
- [Modular monolith](/concepts/architecture/modular-monolith.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (Architect Elevator added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 12 If You Never Kill Anything, You Will Live Among Zombies: Legacy systems as "zombies" kept alive by fear of change; the cure is to make change routine and to retire systems deliberately.

## Sources

1. Cunningham, W. (1992). *The WyCash Portfolio Management System*. OOPSLA 1992 experience report. <https://c2.com/doc/oopsla92.html> — Origin of the technical debt metaphor.
2. Kruchten, P., Nord, R. L. and Ozkaya, I. (2012). *Technical Debt - From Metaphor to Theory and Practice*. IEEE Software 29(6). <https://doi.org/10.1109/MS.2012.167> — Research framing of technical debt and its types.
3. Fowler, M. (2009). *TechnicalDebtQuadrant*. <https://martinfowler.com/bliki/TechnicalDebtQuadrant.html> — Deliberate vs inadvertent, prudent vs reckless debt.
4. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 12 If You Never Kill Anything, You Will Live Among Zombies. Legacy systems as "zombies" kept alive by fear of change; the cure is to make change routine and to retire systems deliberately.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
