---
type: Concept
title: Coupling and cohesion
description: Two measures of modular design - coupling is how much one module depends on others (aim low), cohesion is how strongly the elements inside one module belong together (aim high).
tags:
- architecture
- core
domain: architecture
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Stevens, W. P., Myers, G. J. and Constantine, L. L. (1974). Structured Design. IBM Systems Journal 13(2)
  resource: 'https://doi.org/10.1147/sj.132.0115'
  kind: paper
- title: Parnas, D. L. (1972). On the Criteria to Be Used in Decomposing Systems into Modules. Communications of the ACM 15(12)
  resource: 'https://dl.acm.org/doi/10.1145/361598.361623'
  kind: paper
- title: ISO/IEC/IEEE (2017). ISO/IEC/IEEE 24765 - Systems and software engineering - Vocabulary
  resource: 'https://www.iso.org/standard/71952.html'
  kind: standard
---

# Coupling and cohesion

**Definition.** Stevens, Myers and Constantine introduced both terms in 1974 as part of structured design. Low coupling means a change in one module rarely forces changes in others; high cohesion means a module does one well-defined job. Parnas's information hiding (1972) is the underlying principle: hide each design decision likely to change behind a stable interface.

Domain: [architecture](/domains/architecture.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

These two ideas judge almost every structural choice: where to draw module, service or bounded-context boundaries, and whether a split into microservices will help or create a "distributed monolith" (high coupling across the network). They apply at every scale, from classes to enterprise systems.

## Example

If changing the Notion page layout forces edits in the summarizer, the fetcher and the digest code, those modules are tightly coupled; putting Notion details behind one sync module lowers coupling.

## Narrower concepts

- [Modular monolith](/concepts/architecture/modular-monolith.md)
- [Bounded context](/concepts/backend-systems/bounded-context.md)
- [Microservices](/concepts/architecture/microservices.md)

## Sources

1. Stevens, W. P., Myers, G. J. and Constantine, L. L. (1974). *Structured Design*. IBM Systems Journal 13(2). <https://doi.org/10.1147/sj.132.0115> — Original definitions of coupling and cohesion.
2. Parnas, D. L. (1972). *On the Criteria to Be Used in Decomposing Systems into Modules*. Communications of the ACM 15(12). <https://dl.acm.org/doi/10.1145/361598.361623> — Information hiding as the basis for low coupling.
3. ISO/IEC/IEEE (2017). *ISO/IEC/IEEE 24765 - Systems and software engineering - Vocabulary*. <https://www.iso.org/standard/71952.html> — Standard vocabulary entries for coupling and cohesion (paywalled; cited).

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
