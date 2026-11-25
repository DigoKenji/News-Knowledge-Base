---
type: Foundation
title: Application Architecture
description: A description of the structure and interaction of the applications, as groups of capabilities that provide key business functions and manage the data assets (TOGAF 9.2).
tags:
- foundation
- architecture
- application-architecture
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
  section: Part I §3.3 Application Architecture
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.3 Application Architecture
  tier: B
- title: 'A Guide to the Business Architecture Body of Knowledge (BIZBOK Guide)'
  publisher: Business Architecture Guild
  edition: v9.0 (2020)
  section: 'Appendix A: Glossary, Application Architecture (PDF p. 1024); FEAPO definition, adopted 2017'
  tier: B
evidence:
  bodies:
  - The Open Group
  - Business Architecture Guild (BIZBOK Guide; FEAPO-adopted definitions)
  meets_standard: yes
  gaps:
  - 'BIZBOK v9.0 (2020) is not the current Guild version; FEAPO wording is stable since 2017'
related:
- architecture-domain
- data-architecture
- technology-architecture
- business-capability
domain: architecture
lang: en
---

# Application Architecture

## Working definition
Application architecture describes the structure and interaction of the enterprise's applications, seen as groups of capabilities that provide key business functions and manage the data assets. *Follows TOGAF 9.2 §3.3, the course baseline (TOGAF 10 anchors applications to business capabilities instead of functions).*

## Authoritative definitions
> A description of the structure and interaction of the applications as groups of capabilities that provide key business functions and manage the data assets.
> — The Open Group, TOGAF Standard 9.2, Part I §3.3 Application Architecture

> A description of the structure and interaction of the applications that provide key business capabilities and manage the data assets.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.3 Application Architecture

> Application architecture represents the specification and structural partitioning of technology-based automation into business logic, user experience, and data perspectives as an enabler of business architecture and strategy.
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), Appendix A: Glossary, *Application Architecture* (FEAPO definition, adopted 2017)

## How they differ
- **BIZBOK v9 (FEAPO wording)** describes application architecture by its internal partitioning (business logic, user experience, data) and frames it explicitly as an enabler of business architecture and strategy, while TOGAF describes the structure and interaction of the applications that provide business functions or capabilities and manage the data assets.
- 9.2 framed applications as *groups of capabilities that provide key business functions*; 10 says applications *provide key business capabilities*. The capability, not the function, is now the business anchor — consistent with business architecture's capability focus.
- Not the same as software architecture: TOGAF's application architecture is about the portfolio and interactions of applications, not the inside of one application.
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Domain subdomain `application-architecture`. Inside-the-application design (patterns, microservices) is filed under `integration-and-software-architecture`.

## In the course
Técnicas de Arquitetura (Fowler PoEAA, Enterprise Integration Patterns, microservices material). Lectures not ingested yet.

## Open questions
- None.
