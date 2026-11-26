---
type: Concept
title: Domain-driven design (DDD)
description: An approach to software design that models the code closely on the business domain, using a shared language with domain experts and dividing large systems into bounded contexts with explicit boundaries.
tags:
- architecture
- software-architecture
- core
domain: architecture
subdomain: software-architecture
also_in:
- software-engineering/design
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Evans, E. (2003). Domain-Driven Design - Tackling Complexity in the Heart of Software. Addison-Wesley
  resource: 'https://www.domainlanguage.com/ddd/'
  kind: book
- title: Fowler, M. (2014). BoundedContext
  resource: 'https://martinfowler.com/bliki/BoundedContext.html'
  kind: practitioner
- title: Microsoft. Using domain analysis to model microservices (Azure Architecture Center)
  resource: 'https://learn.microsoft.com/en-us/azure/architecture/microservices/model/domain-analysis'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Domain-driven design (DDD)

**Definition.** Eric Evans introduced DDD in 2003. Strategic design splits a complex domain into bounded contexts, each with its own model and ubiquitous language, and maps how they relate (context maps). Tactical design provides building blocks inside a context: entities, value objects, aggregates, domain events, repositories and services.

Domain: [architecture](/domains/architecture.md) · subdomain `software-architecture`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

DDD is the most common way to find good boundaries for microservices or modular-monolith modules, and it links software design to business architecture (capabilities). It pays off for complex business logic and is overkill for simple CRUD applications.

## Example

"Customer" means a lead with contact data in the Sales context but an account with a credit limit in the Billing context; DDD keeps two models instead of one confused shared class.

## Narrower concepts

- [Bounded context](/concepts/architecture/bounded-context.md)
- [Microservices](/concepts/architecture/microservices.md)
- [Modular monolith](/concepts/architecture/modular-monolith.md)

## Sources

1. Evans, E. (2003). *Domain-Driven Design - Tackling Complexity in the Heart of Software*. Addison-Wesley. <https://www.domainlanguage.com/ddd/> — Origin of DDD, strategic and tactical patterns.
2. Fowler, M. (2014). *BoundedContext*. <https://martinfowler.com/bliki/BoundedContext.html> — Bounded contexts and context relationships explained.
3. Microsoft. *Using domain analysis to model microservices* (Azure Architecture Center). <https://learn.microsoft.com/en-us/azure/architecture/microservices/model/domain-analysis> — Applying DDD to find service boundaries.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
