---
type: Concept
title: 'The architect elevator (the architect''s role)'
description: 'Hohpe''s picture of the architect''s job in a large organisation - riding between the penthouse (strategy, leadership) and the engine room (code, operations) to translate in both directions - valued by how many floors you span, not how high you go.'
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
  locator: 'ch. 1 The Architect Elevator; ch. 4 Enterprise Architect or Architect in the Enterprise?; ch. 5 An Architect Stands on Three Legs'
- title: 'Fowler, M. (2003). Who Needs an Architect? IEEE Software 20(5)'
  resource: 'https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf'
  kind: paper
- title: 'Bredemeyer, D. and Malan, R. (2002-2006). The Role of the Architect. Bredemeyer Consulting white paper'
  resource: 'https://www.bredemeyer.com/pdf_files/role.pdf'
  kind: practitioner
---

# The architect elevator (the architect's role)

**Definition.** In large organisations the people who hold technical knowledge often cannot decide, and the people who decide lack technical knowledge. Hohpe describes the architect as the connecting element: someone who works closely with engineers on real systems, can explain technical topics to executives without losing the essence, and can turn business strategy into technical decisions. He calls this riding the architect elevator. The elevator must be ridden down as well as up, because architects only learn whether their decisions work by seeing the implementation; architects who stay in the penthouse fall into "authority without responsibility". He grounds the role on three legs: skill, impact and leadership.

Domain: [architecture](/domains/architecture.md) · subdomain `architecture-practice`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-26); draft waiting for Kenji to certify.

## How it fits

The metaphor explains why flat, digital companies need fewer enterprise architects (they live in a bungalow) and why traditional enterprises need several architects who meet in the middle of a tall building. It complements the TOGAF view of enterprise architecture as a discipline with the practitioner view of what an architect actually does day to day, and Bredemeyer and Malan's competency model (technology, business strategy, organisational politics, consulting, leadership).

## Example

The board wants faster time to market. The architect translates that downwards into elastic cloud infrastructure and stateless, horizontally scalable services, and translates upwards why the old batch integration blocks the goal and what it costs to replace.

## Broader concepts

- [Software architecture](/concepts/architecture/software-architecture.md)

## Related concepts

- [Architecture as options](/concepts/architecture/architecture-options.md)
- [Architecture decision record](/concepts/backend-systems/adr.md)

## Sources

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 1 The Architect Elevator; ch. 4 Enterprise Architect or Architect in the Enterprise?; ch. 5 An Architect Stands on Three Legs. The elevator metaphor, the penthouse-to-engine-room feedback loop, and skill, impact and leadership as the three legs of the role.
2. Fowler, M. (2003). *Who Needs an Architect? IEEE Software 20(5)*. <https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf> — Fowler's essay on what an architect should be: a guide who works with the team rather than a remote decision maker.
3. Bredemeyer, D. and Malan, R. (2002-2006). The Role of the Architect. Bredemeyer Consulting white paper. <https://www.bredemeyer.com/pdf_files/role.pdf> — Competency framework for the architect role: technology, business strategy, organisational politics, consulting and leadership.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
