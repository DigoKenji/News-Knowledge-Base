---
type: Concept
title: 'Architecture as options'
description: 'Seeing architecture as buying options - the right, but not the obligation, to make a change later at a known cost - so that teams can defer irreversible decisions until they know more; options are worth most when uncertainty is high.'
tags:
- architecture
- architecture-practice
domain: architecture
subdomain: architecture-practice
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
sources:
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 9 Architecture Is Selling Options (Reversing Irreversible Decision Making; Deferring Decisions with Options; An Architecture Option: Elasticity; Strike Prices; Uncertainty Increases an Option''s Value)'
- title: 'Fowler, M. (2003). Who Needs an Architect? IEEE Software 20(5)'
  resource: 'https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf'
  kind: paper
- title: 'Sullivan, K. J. et al. (2001). The Structure and Value of Modularity in Software Design. ESEC/FSE 2001'
  resource: 'https://doi.org/10.1145/503209.503224'
  kind: paper
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Architecture as options

**Definition.** Many definitions describe architecture as the decisions that are hard to reverse, made early. Hohpe argues that the start of a project is when you know least, and follows Fowler's point that an architect should work to eliminate irreversibility. He borrows the financial option: paying something now for the right to decide later at fixed terms. A layer of indirection, a modular boundary or a horizontally scalable design costs effort up front (the option premium) and lowers the cost of a future change (the strike price). As with financial options, the more uncertain the future, the more an option is worth.

Domain: [architecture](/domains/architecture.md) · subdomain `architecture-practice`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-26); draft waiting for Kenji to certify.

## How it fits

The framing gives architects business vocabulary to explain why flexibility is worth paying for, and when it is not: an option nobody will exercise in a stable context is waste. It links to modularity research that values designs by the options they create, and to quality attributes such as elasticity, which is an option to scale at near-zero strike price on an automated cloud platform (while the strike price for switching providers stays high).

## Example

A team unsure which payment providers it will need puts payments behind an internal interface. That costs a few days now; adding or swapping a provider later becomes a small change instead of a rewrite.

## Broader concepts

- [Software architecture](/concepts/architecture/software-architecture.md)

## Related concepts

- [Quality attributes and trade-offs](/concepts/architecture/quality-attributes.md)
- [Coupling and cohesion](/concepts/architecture/coupling-cohesion.md)
- [Architecture decision record](/concepts/architecture/adr.md)

## Sources

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 9 Architecture Is Selling Options (Reversing Irreversible Decision Making; Deferring Decisions with Options; An Architecture Option: Elasticity; Strike Prices; Uncertainty Increases an Option's Value). Architecture as selling options to the business, with elasticity as the worked example and Black-Scholes as the intuition for why uncertainty raises option value.
2. Fowler, M. (2003). *Who Needs an Architect? IEEE Software 20(5)*. <https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf> — Source of the line that an architect's key task is to eliminate irreversibility in software designs.
3. Sullivan, K. J. et al. (2001). *The Structure and Value of Modularity in Software Design*. ESEC/FSE 2001. <https://doi.org/10.1145/503209.503224> — Applies real-options theory to modular software design (independent of Hohpe).

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
