---
type: Foundation
title: Architecture
description: The fundamental concepts or properties of a system in its environment, embodied in its elements, relationships and the principles of its design and evolution (TOGAF 9.2 / ISO 42010:2011).
tags:
- foundation
- architecture
status: draft
generated:
  by: claude-opus (interactive foundation session)
  at: '2026-11-18T18:30:00-03:00'
  updated: '2026-11-19T19:00:00-03:00'
baseline: TOGAF 9.2
sources:
- title: ISO/IEC/IEEE 42010 Systems and software engineering — Architecture description
  publisher: ISO/IEC/IEEE
  edition: 2011 (superseded)
  section: §3 (quoted in TOGAF 9.2 §3.7, definition 1)
  tier: A
  via: TOGAF 9.2 §3.7
- title: The TOGAF Standard, Version 9.2
  publisher: The Open Group
  edition: 9.2 (2018)
  section: Part I §3.7 Architecture
  tier: B
- title: ISO/IEC/IEEE 42010 Software, systems and enterprise — Architecture description
  publisher: ISO/IEC/IEEE
  edition: '2022'
  section: §3 (quoted in TOGAF 10 §4.8, definition 1)
  tier: A
  via: TOGAF 10 §4.8
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.8 Architecture
  tier: B
- title: 'SEBoK - Guide to the Systems Engineering Body of Knowledge, glossary: Architecture'
  publisher: 'BKCASE Editorial Board (INCOSE, IEEE Systems Council, Stevens Institute/SERC)'
  edition: 'v2.x (online, accessed 2026-11-25)'
  section: 'Glossary entry, quoting the standards listed'
  resource: 'https://sebokwiki.org/wiki/Architecture_(glossary)'
  tier: B
evidence:
  bodies:
  - ISO/IEC/IEEE
  - The Open Group
  meets_standard: yes
  gaps:
  - 'The ISO 2011/2015 wording is now confirmed from a source independent of TOGAF (SEBoK); the 42010:2022 wording is still quoted via TOGAF 10 (the standard itself is paywalled)'
related:
- architecture-domain
- enterprise-architecture
- solution-architecture
domain: architecture
lang: en
---

# Architecture

## Working definition
An architecture is the set of fundamental concepts or properties of a system in its environment, embodied in its elements, their relationships, and the principles of its design and evolution. *Follows TOGAF 9.2 §3.7 (definition 1, taken from ISO/IEC/IEEE 42010:2011), the course baseline. TOGAF 10 and ISO 42010:2022 broaden “system” to “entity” and stress governing principles; see below.*

## Authoritative definitions
> 1. The fundamental concepts or properties of a system in its environment embodied in its elements, relationships, and in the principles of its design and evolution. (Source: ISO/IEC/IEEE 42010: 2011)
> 2. The structure of components, their inter-relationships, and the principles and guidelines governing their design and evolution over time.
> — The Open Group, TOGAF Standard 9.2, Part I §3.7 Architecture

> 1. The fundamental concepts or properties of an entity in its environment and governing principles for the realization and evolution of this entity and its related lifecycle processes (Source: ISO/IEC/IEEE 42010:2022)
> 2. The structure of components, their inter-relationships, and the principles and guidelines governing their design and evolution over time
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.8 Architecture

> fundamental concepts or properties of a system in its environment embodied in its elements, relationships, and in the principles of its design and evolution
> — ISO/IEC/IEEE 42010:2011 §3.2 (and ISO/IEC/IEEE 15288:2015 §4.5), as reproduced in the [SEBoK glossary: Architecture](https://sebokwiki.org/wiki/Architecture_(glossary))

## How they differ
- **ISO wording confirmed outside TOGAF.** SEBoK (a public body of knowledge run by INCOSE, the IEEE Systems Council and SERC) reproduces the ISO/IEC/IEEE 42010:2011 and 15288:2015 definition word for word as TOGAF 9.2 quotes it. The 2022 revision (quoted in TOGAF 10) widens "system" to "entity" and adds life cycle processes.
- **System → entity.** ISO 42010:2011 defined the architecture of a *system*; the 2022 edition (as quoted by TOGAF 10) speaks of an *entity*, so the definition now covers enterprises, systems of systems and other things that are not software systems.
- **Embodied in elements → governing principles.** 2011 said the concepts are *embodied in its elements, relationships*; 2022 drops that phrase and adds *governing principles for the realization and evolution* and *related lifecycle processes*. Architecture is less "the parts" and more "the decisions and principles".
- **Two meanings in TOGAF.** Both TOGAF editions keep a second, more structural meaning (components, inter-relationships, principles). Definition 2 is unchanged between 9.2 and 10.
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Root term of the [Architecture](/domains/architecture.md) domain. Specialised by [Architecture domain](/foundations/architecture-domain.md) (business, data, application, technology) and by scope ([Enterprise Architecture](/foundations/enterprise-architecture.md), [Solution Architecture](/foundations/solution-architecture.md)).

## In the course
Covered in *Técnicas de Arquitetura* (TOGAF 9.2 material in `Materiais/Tecnicas de Arquitetura/`). Lectures not ingested yet.

## Open questions
- Adopt definition 1 (ISO, abstract) as the KB's meaning and treat definition 2 (structural) as a description of an architecture's content?
