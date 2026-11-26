---
type: Concept
title: Software architecture
description: The fundamental structure of a software system - its main elements, their relationships and the principles governing its design and evolution - and the discipline of making those high-impact decisions.
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
- title: ISO/IEC/IEEE (2022). ISO/IEC/IEEE 42010 - Architecture description
  resource: 'https://www.iso.org/standard/74393.html'
  kind: standard
- title: Bass, L., Clements, P. and Kazman, R. (2021). Software Architecture in Practice, 4th ed. Addison-Wesley
  resource: 'https://www.oreilly.com/library/view/software-architecture-in/9780136885979/'
  kind: book
- title: Perry, D. E. and Wolf, A. L. (1992). Foundations for the Study of Software Architecture. ACM SIGSOFT Software Engineering Notes 17(4)
  resource: 'https://doi.org/10.1145/141874.141884'
  kind: paper
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 8 Is This Architecture? (Defining Software Architecture; Architectural Decisions)'
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Software architecture

**Definition.** ISO/IEC/IEEE 42010 defines architecture as the fundamental concepts or properties of a system in its environment, embodied in its elements, relationships, and the principles of its design and evolution (the same wording the KB's TOGAF foundation quotes). Bass, Clements and Kazman define software architecture as the set of structures needed to reason about the system, comprising software elements, the relations among them and the properties of both.

Domain: [architecture](/domains/architecture.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Software architecture sits between enterprise architecture (TOGAF's business, data, application and technology domains, see the foundations) and detailed design. It is driven mainly by quality attributes, documented in views and decision records (ADRs), and evaluated with methods such as ATAM. A practical test: architecture is the set of decisions that are expensive to change later.

## Example

Choosing two repositories (code and knowledge), scheduled cloud tasks and an OKF bundle for Project_News were architecture decisions, recorded as ADRs.

## Narrower concepts

- [Architectural style](/concepts/architecture/architectural-style.md)
- [Quality attributes and trade-offs](/concepts/architecture/quality-attributes.md)
- [Coupling and cohesion](/concepts/architecture/coupling-cohesion.md)
- [Architecture decision record](/concepts/backend-systems/adr.md)
- [ATAM tradeoff analysis](/concepts/backend-systems/atam.md)
- [Modular monolith](/concepts/architecture/modular-monolith.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (Architect Elevator added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 8 Is This Architecture? (Defining Software Architecture; Architectural Decisions): Compares the Garlan-Perry, ISO/IEC 42010 and TOGAF definitions and proposes a practical test: an architecture document must contain nontrivial decisions and their rationale.

## Sources

1. ISO/IEC/IEEE (2022). *ISO/IEC/IEEE 42010 - Architecture description*. <https://www.iso.org/standard/74393.html> — Standard definition of architecture and architecture description (paywalled; cited).
2. Bass, L., Clements, P. and Kazman, R. (2021). *Software Architecture in Practice*, 4th ed. Addison-Wesley. <https://www.oreilly.com/library/view/software-architecture-in/9780136885979/> — Structures, quality attributes and architecture evaluation.
3. Perry, D. E. and Wolf, A. L. (1992). *Foundations for the Study of Software Architecture*. ACM SIGSOFT Software Engineering Notes 17(4). <https://doi.org/10.1145/141874.141884> — Early formulation of software architecture as elements, form and rationale.
4. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 8 Is This Architecture? (Defining Software Architecture; Architectural Decisions). Compares the Garlan-Perry, ISO/IEC 42010 and TOGAF definitions and proposes a practical test: an architecture document must contain nontrivial decisions and their rationale.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
