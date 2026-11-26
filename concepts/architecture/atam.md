---
type: Concept
title: ATAM tradeoff analysis
description: 'Architecture Tradeoff Analysis Method - an SEI method that evaluates an architecture against prioritised quality-attribute scenarios to expose risks, sensitivity points and trade-offs.'
tags:
- architecture
- architecture-practice
domain: architecture
subdomain: architecture-practice
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Kazman, R., Klein, M. and Clements, P. (2000). ATAM: Method for Architecture Evaluation. CMU/SEI-2000-TR-004'
  resource: 'https://insights.sei.cmu.edu/library/atam-method-for-architecture-evaluation/'
  kind: standard
- title: 'Bass, L., Clements, P. and Kazman, R. (2021). Software Architecture in Practice, 4th edition. Addison-Wesley'
  resource: 'https://books.google.com/books?vid=ISBN9780136886099'
  kind: book
relations:
- type: part-of
  target: /concepts/architecture/adr.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; moved to architecture; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# ATAM tradeoff analysis

**Definition.** The Architecture Tradeoff Analysis Method is the SEI's structured evaluation of a software architecture. Stakeholders build a utility tree of quality-attribute scenarios ranked by importance and difficulty; the architects walk through how the architecture meets each one; the evaluators record risks, non-risks, sensitivity points (decisions that strongly affect one attribute) and trade-off points (decisions that affect several attributes in opposite directions).

Domain: [architecture](/domains/architecture.md) · subdomain `architecture-practice`. Expanded on 2026-11-26 with independent sources.

## How it fits

It turns vague quality goals into testable scenarios and makes trade-offs explicit before they are expensive to change. Its outputs feed decision records; a full ATAM takes days and many people, so lighter variants are common for smaller systems.

## Example

Scenario 'a region fails during peak hour and checkout recovers within 5 minutes' exposes that synchronous cross-region replication is a trade-off point between availability and latency.

## Sources

1. Kazman, R., Klein, M. and Clements, P. (2000). ATAM: Method for Architecture Evaluation. CMU/SEI-2000-TR-004. <https://insights.sei.cmu.edu/library/atam-method-for-architecture-evaluation/> — The method definition and its steps.
2. Bass, L., Clements, P. and Kazman, R. (2021). Software Architecture in Practice, 4th edition. Addison-Wesley. <https://books.google.com/books?vid=ISBN9780136886099> — Quality-attribute scenarios and architecture evaluation in context.

## Relations
- **part-of** [Architecture decision record](/concepts/architecture/adr.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
