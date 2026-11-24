---
type: Concept
title: Technical debt
description: The future cost created by choosing a quick or expedient solution now instead of a better one - like financial debt, it charges "interest" as extra effort on every later change until it is paid down by refactoring.
tags:
- software-engineering
- core
domain: software-engineering
subdomain: core
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
---

# Technical debt

**Definition.** Ward Cunningham coined the metaphor in 1992: shipping first-time code is like going into debt, which is fine if it is paid back promptly with a rewrite; the danger is when it is never repaid. Fowler's quadrant separates deliberate from inadvertent and prudent from reckless debt.

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Technical debt is a management concept: it lets engineers explain to business stakeholders why speed drops over time and why time for refactoring is an investment. It accumulates in code, architecture, tests, documentation and infrastructure. Some debt is rational (to hit a market window); unmanaged debt slows delivery and causes incidents.

## Example

Hard-coding the list of news sources to launch faster is deliberate, prudent debt; moving them into config/sources.yaml later pays it off.

## Narrower concepts

- [Strangler fig migration](/concepts/backend-systems/strangler-fig.md)
- [Modular monolith](/concepts/architecture/modular-monolith.md)

## Sources

1. Cunningham, W. (1992). *The WyCash Portfolio Management System*. OOPSLA 1992 experience report. <https://c2.com/doc/oopsla92.html> — Origin of the technical debt metaphor.
2. Kruchten, P., Nord, R. L. and Ozkaya, I. (2012). *Technical Debt - From Metaphor to Theory and Practice*. IEEE Software 29(6). <https://doi.org/10.1109/MS.2012.167> — Research framing of technical debt and its types.
3. Fowler, M. (2009). *TechnicalDebtQuadrant*. <https://martinfowler.com/bliki/TechnicalDebtQuadrant.html> — Deliberate vs inadvertent, prudent vs reckless debt.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
