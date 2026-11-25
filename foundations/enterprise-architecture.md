---
type: Foundation
title: Enterprise Architecture
description: Architecture at the scope of a whole enterprise (or a large area of it), used to align processes, information and technology with strategy and to steer change.
tags:
- foundation
- architecture
- enterprise-architecture
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
  section: Part I §1.3 Executive Overview, "Why is an Enterprise Architecture needed?"
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §1.1 Executive Overview, "Why is an Enterprise Architecture Needed?"
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §3 (Enterprise Agility), "Enterprise Architecture provides a framework for change"
  tier: B
- title: 'A Guide to the Business Architecture Body of Knowledge (BIZBOK Guide)'
  publisher: Business Architecture Guild
  edition: v9.0 (2020)
  section: 'Appendix A: Glossary, Enterprise Architecture (PDF p. 1030); FEAPO definition, adopted 2017'
  tier: B
evidence:
  bodies:
  - The Open Group
  - Business Architecture Guild (BIZBOK Guide; FEAPO-adopted definitions)
  meets_standard: 'partial'
  gaps:
  - 'TOGAF 10 still has no formal entry in its Definitions chapter; the Open Group side rests on the purpose statement (9.2 §1.3), so evidence is ''partial'' (two bodies, one formal definition)'
related:
- enterprise
- architecture
- architecture-domain
- solution-architecture
domain: architecture
lang: en
---

# Enterprise Architecture

## Working definition
Enterprise Architecture is the architecture of an enterprise as a whole: it describes how business, data, applications and technology fit together across the enterprise, and it is used to steer change so that the enterprise stays integrated and supports its strategy. *Follows TOGAF 9.2 Part I §1.3, the course baseline; TOGAF gives a purpose, not a formal definition, so this sentence is the KB's synthesis and needs a second body.*

## Authoritative definitions
> The purpose of Enterprise Architecture is to optimize across the enterprise the often fragmented legacy of processes (both manual and automated) into an integrated environment that is responsive to change and supportive of the delivery of the business strategy.
> — The Open Group, TOGAF Standard 9.2, Part I §1.3 Executive Overview

> The purpose of Enterprise Architecture is to optimize across the enterprise the often fragmented legacy of processes (both manual and automated) into an integrated environment that is responsive to change and supportive of the delivery of the business strategy.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §1.1 Executive Overview

> Enterprise Architecture provides a framework for change, linked to both strategic direction and business value.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), section on enterprise agility

> Enterprise architecture represents the holistic planning, analysis, design, and implementation for the development and execution of strategy by applying principles and practices to guide organizations through the integration and interoperation of all other architecture domains.
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), Appendix A: Glossary, *Enterprise Architecture* (FEAPO definition, adopted 2017)

## How they differ
- **BIZBOK v9 (FEAPO wording) gives the formal definition TOGAF lacks**: enterprise architecture as the discipline that turns strategy into execution by integrating all the other architecture domains. It fits TOGAF's purpose statement (9.2 §1.3, 10 §1.1), but now there is a quotable entry from a second body.
- The purpose sentence is word for word the same in TOGAF 9.2 §1.3 and TOGAF 10 §1.1.
- TOGAF describes Enterprise Architecture by its **purpose** (optimize, integrate, enable change) rather than defining what it **is**. The working definition fills that gap by combining §4.8 Architecture with §4.43 Enterprise; that combination is the KB's interpretation, not a quote.
- TOGAF 10 stresses that EA is “not just another name for IT Architecture” (§1.1).
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Scope term in the [Architecture](/domains/architecture.md) domain, above [Solution Architecture](/foundations/solution-architecture.md). TOGAF 10 also names intermediate levels: Strategic Architecture (§4.78) and Segment Architecture (§4.68).

## In the course
Técnicas de Arquitetura (TOGAF). Lectures not ingested yet.

## Open questions
- Accept a purpose-based working definition until a second body is quoted?
