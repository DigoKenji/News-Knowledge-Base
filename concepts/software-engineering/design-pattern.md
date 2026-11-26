---
type: Concept
title: Design pattern
description: A named, reusable solution to a problem that recurs in a given context, described with its trade-offs - from code-level object-oriented patterns to architecture and integration patterns.
tags:
- software-engineering
- design
- core
domain: software-engineering
subdomain: design
also_in:
- architecture/software-architecture
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Gamma, E., Helm, R., Johnson, R. and Vlissides, J. (1994). Design Patterns - Elements of Reusable Object-Oriented Software. Addison-Wesley
  resource: 'https://www.oreilly.com/library/view/design-patterns-elements/0201633612/'
  kind: book
- title: Fowler, M. (2002). Patterns of Enterprise Application Architecture - catalog
  resource: 'https://martinfowler.com/eaaCatalog/'
  kind: book
- title: Hohpe, G. and Woolf, B. (2003). Enterprise Integration Patterns
  resource: 'https://www.enterpriseintegrationpatterns.com/'
  kind: book
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Design pattern

**Definition.** The idea comes from architect Christopher Alexander and entered software with the "Gang of Four" book (1994), which catalogued 23 object-oriented patterns in three groups: creational (Factory, Builder, Singleton), structural (Adapter, Facade, Decorator) and behavioural (Observer, Strategy, Command). Each pattern has a name, the problem, the solution and the consequences.

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `design`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Patterns give teams a shared vocabulary ("use a facade here") and capture experience. The same format was later used for enterprise application patterns (Fowler), enterprise integration patterns (Hohpe and Woolf) and distributed-system patterns such as saga, strangler fig and dead-letter queue. Overusing patterns where a simple solution works is a known anti-pattern.

## Example

Strategy - the summarizer picks a prompt per source type (article, paper, LinkedIn post) behind one interface, so adding a new type does not change the calling code.

## In architecture work

The same idea at system scale is an [architectural style](/concepts/architecture/architectural-style.md) (microservices, event-driven, layered). Design patterns stay at code and module level; integration patterns such as those of Hohpe and Woolf sit between the two. The concept is cross-listed in the Architecture theme under software architecture.

## Narrower concepts

- [Saga pattern](/concepts/backend-systems/saga-pattern.md)
- [Strangler fig migration](/concepts/architecture/strangler-fig.md)
- [Dead-letter queue](/concepts/backend-systems/dead-letter-queue.md)

## Sources

1. Gamma, E., Helm, R., Johnson, R. and Vlissides, J. (1994). *Design Patterns - Elements of Reusable Object-Oriented Software*. Addison-Wesley. <https://www.oreilly.com/library/view/design-patterns-elements/0201633612/> — The original catalogue of 23 object-oriented patterns.
2. Fowler, M. (2002). *Patterns of Enterprise Application Architecture* - catalog. <https://martinfowler.com/eaaCatalog/> — Patterns applied to enterprise applications.
3. Hohpe, G. and Woolf, B. (2003). *Enterprise Integration Patterns*. <https://www.enterpriseintegrationpatterns.com/> — Pattern language for messaging and integration.

## Relations
- **related** [Architectural style](/concepts/architecture/architectural-style.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
