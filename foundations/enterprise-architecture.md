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
sources:
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
evidence:
  bodies:
  - The Open Group
  meets_standard: 'no'
  gaps:
  - TOGAF 10 has no formal entry for Enterprise Architecture in its Definitions chapter (§4); it explains it in §1.1 instead
  - 'A second body: Gartner IT glossary (tier D, blocked from cloud; Kenji can open it), FEAF v2 (US federal, public), or Ross, Weill & Robertson (2006)'
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
Enterprise Architecture is the architecture of an enterprise as a whole: it describes how business, data, applications and technology fit together across the enterprise, and it is used to steer change so that the enterprise stays integrated and supports its strategy. *Follows TOGAF 10 §1.1, the only tier B text available so far; to be confirmed by a second body.*

## Authoritative definitions
> The purpose of Enterprise Architecture is to optimize across the enterprise the often fragmented legacy of processes (both manual and automated) into an integrated environment that is responsive to change and supportive of the delivery of the business strategy.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §1.1 Executive Overview

> Enterprise Architecture provides a framework for change, linked to both strategic direction and business value.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), section on enterprise agility

## How they differ
- TOGAF describes Enterprise Architecture by its **purpose** (optimize, integrate, enable change) rather than defining what it **is**. The working definition fills that gap by combining §4.8 Architecture with §4.43 Enterprise; that combination is the KB's interpretation, not a quote.
- TOGAF 10 stresses that EA is “not just another name for IT Architecture” (§1.1).

## Where it sits
Scope term in the [Architecture](/domains/architecture.md) domain, above [Solution Architecture](/foundations/solution-architecture.md). TOGAF 10 also names intermediate levels: Strategic Architecture (§4.78) and Segment Architecture (§4.68).

## In the course
Técnicas de Arquitetura (TOGAF). Lectures not ingested yet.

## Open questions
- Accept a purpose-based working definition until a second body is quoted?
