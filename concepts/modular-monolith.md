---
type: Concept
title: Modular monolith
description: An application deployed as a single unit but internally divided into modules with strong, enforced boundaries - each owning its data and exposing an explicit interface - giving much of the structure of microservices without the cost of a distributed system.
tags:
- architecture
domain: architecture
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Parnas, D. L. (1972). On the Criteria to Be Used in Decomposing Systems into Modules. Communications of the ACM 15(12)
  resource: 'https://dl.acm.org/doi/10.1145/361598.361623'
  kind: paper
- title: Fowler, M. (2015). MonolithFirst
  resource: 'https://martinfowler.com/bliki/MonolithFirst.html'
  kind: practitioner
- title: Shopify Engineering (2019). Deconstructing the Monolith - Designing Software that Maximizes Developer Productivity
  resource: 'https://shopify.engineering/deconstructing-monolith-designing-software-maximizes-developer-productivity'
  kind: practitioner
- title: Spring. Spring Modulith Reference Documentation
  resource: 'https://docs.spring.io/spring-modulith/reference/'
  kind: docs
relations:
- type: uses
  target: bounded-context
---

# Modular monolith

**Definition.** A modular monolith is a monolith designed around modules, usually aligned with business capabilities or bounded contexts. Modules talk only through public interfaces (in-process calls or internal events), do not reach into each other's internals or tables, and the boundaries are checked by tooling, not just convention.

Domain: [architecture](/domains/architecture.md). Expanded on 2026-11-24 with independent sources; still a draft waiting for Kenji to certify.

## How it works

The principle goes back to Parnas (1972): decompose systems by hiding design decisions behind module interfaces. In practice: one module per domain (orders, billing, catalog); separate schemas or table ownership per module; dependency rules enforced by build tools (Java modules, Spring Modulith, ArchUnit, Shopify's Packwerk for Rails); and communication by interfaces or in-process events so a module can later be extracted as a service. Shopify runs one of the largest Rails monoliths this way. Martin Fowler's "Monolith First" argues most systems should start as a well-structured monolith and split only when there is a proven need.

## Why it matters and trade-offs

You get simple deployment, in-process calls, local transactions and easy refactoring, plus clear ownership and a path to microservices. You do not get independent deployment or scaling per module, a fault in one module can take down the whole process, and boundaries erode unless they are enforced automatically.

## Example

An e-commerce app ships as one Spring Boot service, but the billing module can only call the orders module's public API, and a test fails the build if billing imports orders' internal classes or queries its tables.

## Sources

1. Parnas, D. L. (1972). *On the Criteria to Be Used in Decomposing Systems into Modules*. Communications of the ACM 15(12). <https://dl.acm.org/doi/10.1145/361598.361623> — Information hiding as the basis for module boundaries.
2. Fowler, M. (2015). *MonolithFirst*. <https://martinfowler.com/bliki/MonolithFirst.html> — Start with a well-structured monolith, split later.
3. Shopify Engineering (2019). *Deconstructing the Monolith - Designing Software that Maximizes Developer Productivity*. <https://shopify.engineering/deconstructing-monolith-designing-software-maximizes-developer-productivity> — Large-scale production case of a componentised monolith.
4. Spring. *Spring Modulith Reference Documentation*. <https://docs.spring.io/spring-modulith/reference/> — Tooling that verifies module boundaries and supports in-process events.

## Relations
- **uses** [bounded-context](/concepts/bounded-context.md)

## Notes
- [Agoda Reduces API Code Review Time by 64% with Modular Architecture](/notes/architecture/2026-11-24-agoda-reduces-api-code-review-time-by-64-with-modular-archit.md)
