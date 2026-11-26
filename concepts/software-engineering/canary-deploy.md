---
type: Concept
title: Canary / blue-green deploy
description: 'Releasing a new version to a small share of traffic (canary) or to a parallel environment switched over at once (blue-green), so problems are caught and rolled back quickly.'
tags:
- software-engineering
- delivery
domain: software-engineering
subdomain: delivery
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Sato, D. (2014). CanaryRelease (martinfowler.com)'
  resource: 'https://martinfowler.com/bliki/CanaryRelease.html'
  kind: practitioner
- title: 'Fowler, M. (2010). BlueGreenDeployment'
  resource: 'https://martinfowler.com/bliki/BlueGreenDeployment.html'
  kind: practitioner
- title: 'Google SRE. Canarying Releases (The Site Reliability Workbook, ch. 16)'
  resource: 'https://sre.google/workbook/canarying-releases/'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; moved to software-engineering; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Canary / blue-green deploy

**Definition.** Canary and blue-green are release strategies that limit the damage of a bad version. In a canary release the new version first gets a small share of real traffic or users while metrics are compared with the old version, and the share grows only if they stay healthy. In a blue-green deployment two identical environments exist; the new version is deployed to the idle one and traffic is switched over at once, with the old environment kept ready for instant rollback.

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `delivery`. Expanded on 2026-11-26 with independent sources.

## How it fits

Both belong to continuous delivery: they turn deployment into a reversible, observable step. Canaries need good observability and automated analysis to decide; blue-green needs double capacity and care with database schema changes that both versions must tolerate.

## Example

Version 2.4 goes to 1% of traffic for 30 minutes; its error rate is 0.8% against 0.2% for 2.3, so the pipeline stops the rollout and routes everything back.

## Sources

1. Sato, D. (2014). CanaryRelease (martinfowler.com). <https://martinfowler.com/bliki/CanaryRelease.html> — Definition of canary releases.
2. Fowler, M. (2010). BlueGreenDeployment. <https://martinfowler.com/bliki/BlueGreenDeployment.html> — Definition of blue-green deployment.
3. Google SRE. Canarying Releases (The Site Reliability Workbook, ch. 16). <https://sre.google/workbook/canarying-releases/> — Canary analysis and metric selection at Google.

## Notes
Notes tagged with this concept are listed in the concept index after each run.
