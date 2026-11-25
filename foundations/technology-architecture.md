---
type: Foundation
title: Technology Architecture
description: A description of the structure and interaction of the technology services and technology components.
tags:
- foundation
- architecture
- technology-architecture
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
  section: Part I §3.77 Technology Architecture
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.81 Technology Architecture
  tier: B
- title: 'A Guide to the Business Architecture Body of Knowledge (BIZBOK Guide)'
  publisher: Business Architecture Guild
  edition: v9.0 (2020)
  section: 'Appendix A: Glossary, Technical Architecture (PDF p. 1038); FEAPO definition, adopted 2017'
  tier: B
evidence:
  bodies:
  - The Open Group
  - Business Architecture Guild (BIZBOK Guide; FEAPO-adopted definitions)
  meets_standard: yes
  gaps:
  - 'Terminology: the KB keeps TOGAF''s ''technology architecture'' as the title and lists ''technical architecture'' as the BIZBOK/FEAPO synonym'
related:
- architecture-domain
- application-architecture
domain: architecture
lang: en
---

# Technology Architecture

## Working definition
Technology architecture describes the technology services (such as hosting, networking, storage, platforms) and the technology components that provide them, and how they interact. *Follows TOGAF 9.2 §3.77, the course baseline; identical in TOGAF 10.*

## Authoritative definitions
> A description of the structure and interaction of the technology services and technology components.
> — The Open Group, TOGAF Standard 9.2, Part I §3.77 Technology Architecture

> A description of the structure and interaction of the technology services and technology components.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.81 Technology Architecture

> Technical architecture represents the logical and physical interconnection of infrastructure elements to enable the deployment and management of data architecture, application architecture, business architecture, and strategy.
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), Appendix A: Glossary, *Technical Architecture* (FEAPO definition, adopted 2017)

## How they differ
- **Different name, same domain.** BIZBOK v9 (FEAPO wording) says *technical* architecture (its version history notes it replaced "technology architecture") and defines it as the interconnection of infrastructure elements that lets the other three domains be deployed and managed; TOGAF describes the same layer as the structure and interaction of technology services and components.
- Identical in 9.2 and 10.
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Domain subdomain `technology-architecture`; overlaps the KB's Cloud & Infrastructure domain, which holds the engineering detail.

## In the course
FinOps course (IT financial management) applies cost views to technology architecture. Lectures not ingested yet.

## Open questions
- None.
