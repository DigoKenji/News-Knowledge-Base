---
type: Concept
title: DevOps
description: A culture and set of practices that unite software development and IT operations - shared ownership, automation, fast feedback and measurement - so teams deliver changes quickly and reliably.
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
- title: Forsgren, N., Humble, J. and Kim, G. (2018). Accelerate - The Science of Lean Software and DevOps. IT Revolution
  resource: 'https://itrevolution.com/product/accelerate/'
  kind: book
- title: DORA. DORA research program
  resource: 'https://dora.dev/'
  kind: practitioner
- title: AWS. What is DevOps?
  resource: 'https://aws.amazon.com/devops/what-is-devops/'
  kind: docs
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 36 The Infinite Loop; ch. 13 Never Send a Human to Do a Machine''s Job'
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# DevOps

**Definition.** DevOps emerged around 2009 to break the wall between developers (who want change) and operations (who want stability). Its practices include CI/CD, infrastructure as code, monitoring and observability, blameless postmortems and "you build it, you run it" ownership. The DORA research programme (the book Accelerate) showed that elite performers deploy more often and also fail less.

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

SRE (Google's approach) is one concrete implementation of DevOps with SLOs and error budgets. DevOps is often misread as a job title or a tool; the research says culture and capabilities, not tools, predict performance.

## Example

The team that writes the summarizer also owns its scheduled task, its alerts and its incident fixes, instead of handing it to a separate ops team.

## Narrower concepts

- [Continuous integration and continuous delivery (CI/CD)](/concepts/software-engineering/ci-cd.md)
- [SLO error budget](/concepts/cloud-infra/slo-error-budget.md)
- [Canary / blue-green deploy](/concepts/cloud-infra/canary-deploy.md)
- [Infrastructure as code](/concepts/cloud-infra/infrastructure-as-code.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (Architect Elevator added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 36 The Infinite Loop; ch. 13 Never Send a Human to Do a Machine's Job: Build-Measure-Learn and "operate to learn": operations exists to feed insight back into the product, and automation enables the loop.

## Sources

1. Forsgren, N., Humble, J. and Kim, G. (2018). *Accelerate - The Science of Lean Software and DevOps*. IT Revolution. <https://itrevolution.com/product/accelerate/> — Research-based capabilities and the four key metrics.
2. DORA. *DORA research program*. <https://dora.dev/> — Ongoing State of DevOps research.
3. AWS. *What is DevOps?*. <https://aws.amazon.com/devops/what-is-devops/> — Practitioner definition and practices.
4. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 36 The Infinite Loop; ch. 13 Never Send a Human to Do a Machine's Job. Build-Measure-Learn and "operate to learn": operations exists to feed insight back into the product, and automation enables the loop.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
