---
type: Concept
title: Strangler fig migration
description: 'Migrating a legacy system gradually by routing features one at a time to a new implementation around it, until the old system can be switched off.'
tags:
- architecture
- software-architecture
domain: architecture
subdomain: software-architecture
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Fowler, M. (2004). StranglerFigApplication'
  resource: 'https://martinfowler.com/bliki/StranglerFigApplication.html'
  kind: practitioner
- title: 'Microsoft. Strangler Fig pattern (Azure Architecture Center)'
  resource: 'https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig'
  kind: docs
- title: 'AWS. Strangler fig pattern (Prescriptive Guidance: cloud design patterns)'
  resource: 'https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/strangler-fig.html'
  kind: docs
relations:
- type: uses
  target: /concepts/architecture/bounded-context.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; moved to architecture; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Strangler fig migration

**Definition.** The strangler fig pattern replaces a legacy system gradually: a façade or routing layer sits in front of it, new or rewritten features are built in a new system, and traffic for each feature is switched over one at a time until the old system handles nothing and can be removed. Martin Fowler named it in 2004 after fig vines that grow around a host tree and eventually replace it.

Domain: [architecture](/domains/architecture.md) · subdomain `software-architecture`. Expanded on 2026-11-26 with independent sources.

## How it fits

It avoids the risk of a big-bang rewrite: value is delivered early and each step can be rolled back. Its costs are the routing layer, keeping data in sync between old and new during the transition, and the discipline to actually finish. Bounded contexts are a common guide for which slice to move next.

## Example

An API gateway sends /invoices to a new service while every other path still reaches the monolith; months later /payments follows, and the monolith's billing module is deleted.

## Sources

1. Fowler, M. (2004). StranglerFigApplication. <https://martinfowler.com/bliki/StranglerFigApplication.html> — Origin and rationale of the pattern.
2. Microsoft. Strangler Fig pattern (Azure Architecture Center). <https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig> — Façade, issues and when not to use it.
3. AWS. Strangler fig pattern (Prescriptive Guidance: cloud design patterns). <https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/strangler-fig.html> — Step-by-step migration with a proxy layer.

## Relations
- **uses** [Bounded context](/concepts/architecture/bounded-context.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
