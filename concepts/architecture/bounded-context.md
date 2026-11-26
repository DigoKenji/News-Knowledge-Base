---
type: Concept
title: Bounded context
description: 'A DDD boundary inside which one domain model and its language apply consistently; the same term may mean something different in another bounded context.'
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
- title: 'Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley'
  resource: 'https://books.google.com/books?vid=ISBN9780321125217'
  kind: book
- title: 'Fowler, M. (2014). BoundedContext'
  resource: 'https://martinfowler.com/bliki/BoundedContext.html'
  kind: practitioner
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; moved to architecture; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Bounded context

**Definition.** A bounded context is the explicit boundary within which one domain model applies and its terms have one meaning. Eric Evans introduced it in Domain-Driven Design: large systems inevitably contain several models, so instead of forcing one unified model, each context keeps its own, and a context map describes how they relate and translate at the edges.

Domain: [architecture](/domains/architecture.md) · subdomain `software-architecture`. Expanded on 2026-11-26 with independent sources.

## How it fits

It is the strategic core of DDD and a common guide for drawing microservice or module boundaries: one bounded context per service or module keeps the language consistent inside and makes integration explicit outside. Fowler stresses that the same word ('customer', 'product') legitimately means different things in different contexts.

## Example

In Sales a 'product' has price and promotions; in Shipping it has weight and dimensions. Two bounded contexts each own their Product model and exchange only a product id.

## Sources

1. Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. <https://books.google.com/books?vid=ISBN9780321125217> — Part IV Strategic Design, ch. 14 Maintaining Model Integrity (bounded context, context map).
2. Fowler, M. (2014). BoundedContext. <https://martinfowler.com/bliki/BoundedContext.html> — Short explanation with the multiple-meanings example.

## Relations
- [Strangler fig migration](/concepts/architecture/strangler-fig.md) **uses** this

## Notes
Notes tagged with this concept are listed in the concept index after each run.
