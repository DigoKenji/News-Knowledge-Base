---
type: Foundation
title: Solution Architecture
description: A description of one discrete business operation or activity and how information systems and IT support it — typically scoped to a single project or release.
tags:
- foundation
- architecture
- solution-architecture
status: draft
generated:
  by: claude-opus (interactive foundation session)
  at: '2026-11-18T18:30:00-03:00'
  updated: '2026-11-19T19:00:00-03:00'
baseline: TOGAF 9.2
sources:
- title: The TOGAF Standard, Version 9.2
  publisher: The Open Group
  edition: 9.2 (2018)
  section: Part I §3.69 Solution Architecture
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.73 Solution Architecture; §4.78 Strategic Architecture; §4.68 Segment Architecture; Appendix B.41 System
  tier: B
evidence:
  bodies:
  - The Open Group
  meets_standard: partial
  gaps:
  - 'A second body. TOGAF 10 has no definition of ''solution'' itself; candidates: ISO/IEC/IEEE 15288 (system, via TOGAF B.41), or a tier C source on solution architecture'
related:
- enterprise-architecture
- architecture
- architecture-domain
domain: architecture
lang: en
---

# Solution Architecture

## Working definition
Solution architecture describes one focused business operation or change — usually one project or release — and how IT supports it, translating requirements into a solution vision, high-level specifications and implementation tasks. A *solution* is the combination of business and IT elements delivered to meet that need. *Follows TOGAF 9.2 §3.69, the course baseline, including its project-level note; the definition of “solution” is the KB's interpretation, since TOGAF does not define the word.*

## Authoritative definitions
> A description of a discrete and focused business operation or activity and how IS/IT supports that operation.
> Note: A Solution Architecture typically applies to a single project or project release, assisting in the translation of requirements into a solution vision, high-level business and/or IT system specifications, and a portfolio of implementation tasks.
> — The Open Group, TOGAF Standard 9.2, Part I §3.69 Solution Architecture

> A description of a discrete and focused business operation or activity and how IS/IT supports that operation.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.73 Solution Architecture

> A combination of interacting elements organized to achieve one or more stated purposes. (Source: ISO/IEC/IEEE 15288:2015)
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), Appendix B.41 System

## How they differ
- TOGAF 10 keeps the definition but drops the 9.2 note tying it to a single project or release.
- TOGAF 10 places it in a hierarchy of levels: Strategic Architecture (§4.78, executive, long-term), Segment Architecture (§4.68, program or portfolio), and Solution Architecture (project).
- Neither edition defines *solution*. Industry usage varies (a product, a project deliverable, a set of systems); the KB adopts the interpretation in the working definition and flags it.
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Scope term: the level below [Enterprise Architecture](/foundations/enterprise-architecture.md). Can touch all four [architecture domains](/foundations/architecture-domain.md).

## In the course
Técnicas de Arquitetura (ASO desenho, solution design deck). Lectures not ingested yet.

## Open questions
- Kenji asked “what is a solution?”: accept the KB interpretation, or make *solution* its own foundation once a tier A/B definition is found?
