---
type: Concept
title: Continuous integration and continuous delivery (CI/CD)
description: Continuous integration merges every developer's changes into the main branch frequently and verifies each one with an automated build and tests; continuous delivery keeps the software always releasable, with an automated pipeline to production.
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
- title: Fowler, M. (2024). Continuous Integration. martinfowler.com
  resource: 'https://martinfowler.com/articles/continuousIntegration.html'
  kind: practitioner
- title: Humble, J. and Farley, D. (2010). Continuous Delivery. Addison-Wesley
  resource: 'https://continuousdelivery.com/'
  kind: book
- title: DORA. Capabilities - Continuous delivery
  resource: 'https://dora.dev/capabilities/continuous-delivery/'
  kind: practitioner
---

# Continuous integration and continuous delivery (CI/CD)

**Definition.** Martin Fowler describes CI as members of a team integrating their work frequently, at least daily, with each integration verified by an automated build including tests. Continuous delivery (Humble and Farley) extends this so that any version that passes the pipeline can be released at the push of a button; continuous deployment releases it automatically.

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

CI/CD is the engine of DevOps: it shortens the path from code to users, reduces the risk of each release by making it small, and is measured by the DORA metrics (deployment frequency, lead time, change failure rate, recovery time). Canary and blue-green deployments limit the impact of a bad release.

## Example

A pull request triggers GitHub Actions to build, run tests and lint; after merge, the pipeline deploys to staging and then, after checks, to production.

## Narrower concepts

- [Canary / blue-green deploy](/concepts/cloud-infra/canary-deploy.md)
- [Software testing](/concepts/software-engineering/software-testing.md)

## Sources

1. Fowler, M. (2024). *Continuous Integration*. martinfowler.com. <https://martinfowler.com/articles/continuousIntegration.html> — Definition and practices of CI.
2. Humble, J. and Farley, D. (2010). *Continuous Delivery*. Addison-Wesley. <https://continuousdelivery.com/> — Deployment pipelines and always-releasable software.
3. DORA. *Capabilities - Continuous delivery*. <https://dora.dev/capabilities/continuous-delivery/> — Research linking CD to delivery performance.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
