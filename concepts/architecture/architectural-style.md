---
type: Concept
title: Architectural style
description: A named family of architectures that share the same kinds of components, connectors and constraints - such as layered, monolith, microservices, event-driven or client-server - each with known strengths and weaknesses.
tags:
- architecture
- software-architecture
- core
domain: architecture
subdomain: software-architecture
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Garlan, D. and Shaw, M. (1994). An Introduction to Software Architecture. CMU-CS-94-166
  resource: 'https://www.cs.cmu.edu/afs/cs/project/able/ftp/intro_softarch/intro_softarch.pdf'
  kind: paper
- title: Fielding, R. T. (2000). Architectural Styles and the Design of Network-based Software Architectures. PhD dissertation, UC Irvine
  resource: 'https://ics.uci.edu/~fielding/pubs/dissertation/top.htm'
  kind: paper
- title: Richards, M. and Ford, N. (2020). Fundamentals of Software Architecture. O'Reilly
  resource: 'https://www.oreilly.com/library/view/fundamentals-of-software/9781492043447/'
  kind: book
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Architectural style

**Definition.** Garlan and Shaw described architectural styles as vocabularies of component and connector types plus constraints on how they can be combined. Fielding's dissertation, which defined REST, treats a style as a coordinated set of constraints. Richards and Ford group modern styles into monolithic (layered, modular monolith, microkernel) and distributed (service-based, event-driven, microservices, space-based).

Domain: [architecture](/domains/architecture.md) · subdomain `software-architecture`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Choosing a style is one of the first architecture decisions and should follow the quality attributes: microservices buy independent deployment and scaling at the cost of operational complexity; a modular monolith buys simplicity at the cost of independent scaling. Real systems often mix styles.

## Example

Project_News is mostly a pipeline style (fetch → select → summarise → file → digest) triggered on a schedule, with an event-driven hand-off from Hermes through the inbox folder.

## Narrower concepts

- [Microservices](/concepts/architecture/microservices.md)
- [Event-driven architecture](/concepts/architecture/event-driven-architecture.md)
- [Modular monolith](/concepts/architecture/modular-monolith.md)

## Sources

1. Garlan, D. and Shaw, M. (1994). *An Introduction to Software Architecture*. CMU-CS-94-166. <https://www.cs.cmu.edu/afs/cs/project/able/ftp/intro_softarch/intro_softarch.pdf> — Classic definition and catalogue of architectural styles.
2. Fielding, R. T. (2000). *Architectural Styles and the Design of Network-based Software Architectures*. PhD dissertation, UC Irvine. <https://ics.uci.edu/~fielding/pubs/dissertation/top.htm> — Styles as sets of constraints; derivation of REST.
3. Richards, M. and Ford, N. (2020). *Fundamentals of Software Architecture*. O'Reilly. <https://www.oreilly.com/library/view/fundamentals-of-software/9781492043447/> — Modern catalogue of styles with trade-off ratings.

## Relations
- **related** [Design pattern](/concepts/software-engineering/design-pattern.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
