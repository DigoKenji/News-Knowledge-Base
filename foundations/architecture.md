---
type: Foundation
title: Architecture
description: The fundamental concepts or properties of something in its environment, plus the principles that govern how it is built and how it evolves.
tags:
- foundation
- architecture
status: draft
generated:
  by: claude-opus (interactive foundation session)
  at: '2026-11-18T18:30:00-03:00'
sources:
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
evidence:
  bodies:
  - ISO/IEC/IEEE
  - The Open Group
  meets_standard: partial
  gaps:
  - ISO/IEC/IEEE 42010:2022 original text (paywalled at iso.org); the ISO quote is via TOGAF 10 until checked
related:
- architecture-domain
- enterprise-architecture
- solution-architecture
domain: architecture
lang: en
---

# Architecture

## Working definition
An architecture is the set of fundamental concepts or properties of a system or enterprise in its environment, together with the principles that govern how it is realized and how it evolves. *Follows ISO/IEC/IEEE 42010:2022 (via TOGAF 10), because it is the international standard and TOGAF adopts it as its first meaning.*

## Authoritative definitions
> 1. The fundamental concepts or properties of an entity in its environment and governing principles for the realization and evolution of this entity and its related lifecycle processes (Source: ISO/IEC/IEEE 42010:2022)
> 2. The structure of components, their inter-relationships, and the principles and guidelines governing their design and evolution over time
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.8 Architecture

> 1. The fundamental concepts or properties of a system in its environment embodied in its elements, relationships, and in the principles of its design and evolution. (Source: ISO/IEC/IEEE 42010: 2011)
> 2. The structure of components, their inter-relationships, and the principles and guidelines governing their design and evolution over time.
> — The Open Group, TOGAF Standard 9.2, Part I §3.7 Architecture

## How they differ
- **System → entity.** ISO 42010:2011 defined the architecture of a *system*; the 2022 edition (as quoted by TOGAF 10) speaks of an *entity*, so the definition now covers enterprises, systems of systems and other things that are not software systems.
- **Embodied in elements → governing principles.** 2011 said the concepts are *embodied in its elements, relationships*; 2022 drops that phrase and adds *governing principles for the realization and evolution* and *related lifecycle processes*. Architecture is less "the parts" and more "the decisions and principles".
- **Two meanings in TOGAF.** Both TOGAF editions keep a second, more structural meaning (components, inter-relationships, principles). Definition 2 is unchanged between 9.2 and 10.

## Where it sits
Root term of the [Architecture](/domains/architecture.md) domain. Specialised by [Architecture domain](/foundations/architecture-domain.md) (business, data, application, technology) and by scope ([Enterprise Architecture](/foundations/enterprise-architecture.md), [Solution Architecture](/foundations/solution-architecture.md)).

## In the course
Covered in *Técnicas de Arquitetura* (TOGAF 9.2 material in `Materiais/Tecnicas de Arquitetura/`). Lectures not ingested yet.

## Open questions
- Adopt definition 1 (ISO, abstract) as the KB's meaning and treat definition 2 (structural) as a description of an architecture's content?
