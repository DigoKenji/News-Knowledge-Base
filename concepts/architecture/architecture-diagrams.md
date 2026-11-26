---
type: Concept
title: 'Architecture diagrams and diagram-driven design'
description: 'Using diagrams not only to communicate an architecture but to test it - a good diagram shows components, the relationships (lines) between them and the decisions behind them, at a level of detail chosen for its audience.'
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
  locator: 'ch. 21 Emphasis Over Completeness; ch. 22 Diagram-Driven Design; ch. 23 Drawing the Line; ch. 24 Sketching Bank Robbers'
- title: 'Brown, S. The C4 model for visualising software architecture'
  resource: 'https://c4model.com/'
  kind: docs
- title: 'ISO/IEC/IEEE 42010 - Architecture description (overview site)'
  resource: 'http://www.iso-architecture.org/42010/'
  kind: standard
---

# Architecture diagrams and diagram-driven design

**Definition.** Hohpe's "diagram-driven design" holds that cheating in a picture is harder than cheating in words: if you cannot draw a clear diagram of the system, the design is probably not clear either. He stresses that an architecture without lines is likely not one, because the relationships between components, not their list or position, explain how a system behaves. Diagrams should favour emphasis over completeness, use a consistent visual vocabulary, and match the audience, which is why he coaches engineers to sketch the story of the system rather than an inventory of boxes.

Domain: [architecture](/domains/architecture.md) · subdomain `architecture-practice`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-26); draft waiting for Kenji to certify.

## How it fits

This is the practitioner side of what ISO/IEC/IEEE 42010 formalises as views and viewpoints for different stakeholders, and of notations such as Simon Brown's C4 model (context, containers, components, code), which give each zoom level a fixed set of elements and insist that every line is labelled.

## Example

A slide shows six boxes labelled with product names. Redrawn with labelled arrows ("publishes order events", "reads customer profile, sync HTTPS"), it reveals a synchronous dependency loop nobody had noticed.

## Broader concepts

- [Software architecture](/concepts/architecture/software-architecture.md)

## Related concepts

- [The architect elevator (the architect's role)](/concepts/architecture/architect-elevator.md)
- [Architecture decision record](/concepts/backend-systems/adr.md)

## Sources

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 21 Emphasis Over Completeness; ch. 22 Diagram-Driven Design; ch. 23 Drawing the Line; ch. 24 Sketching Bank Robbers. Diagrams as a design tool, why lines carry the architecture, and how to help engineers draw useful pictures.
2. Brown, S. The C4 model for visualising software architecture. <https://c4model.com/> — The C4 model for visualising software architecture at four zoom levels.
3. ISO/IEC/IEEE 42010 - Architecture description (overview site). <http://www.iso-architecture.org/42010/> — Overview of ISO/IEC/IEEE 42010 architecture descriptions: stakeholders, concerns, views and viewpoints.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
