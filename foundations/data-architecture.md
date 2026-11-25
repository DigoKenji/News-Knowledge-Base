---
type: Foundation
title: Data Architecture
description: A description of the structure and interaction of an enterprise's major types and sources of data, its logical and physical data assets, and its data management resources.
tags:
- foundation
- architecture
- data-architecture
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
  section: Part I §3.36 Data Architecture
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.39 Data Architecture
  tier: B
- title: 'A Guide to the Business Architecture Body of Knowledge (BIZBOK Guide)'
  publisher: Business Architecture Guild
  edition: v9.0 (2020)
  section: 'Appendix A: Glossary, Data Architecture (PDF p. 1030); FEAPO definition, adopted 2017'
  tier: B
evidence:
  bodies:
  - The Open Group
  - Business Architecture Guild (BIZBOK Guide; FEAPO-adopted definitions)
  meets_standard: yes
  gaps:
  - 'DAMA-DMBOK2 (DAMA International), chapter on Data Architecture, for the practitioner view (optional now)'
related:
- architecture-domain
- application-architecture
domain: architecture
lang: en
---

# Data Architecture

## Working definition
Data architecture describes the structure and interaction of the enterprise's major types and sources of data, its logical and physical data assets, and the resources that manage them. *Follows TOGAF 9.2 §3.36, the course baseline; pending DAMA-DMBOK2.*

## Authoritative definitions
> A description of the structure and interaction of the enterprise’s major types and sources of data, logical data assets, physical data assets, and data management resources.
> — The Open Group, TOGAF Standard 9.2, Part I §3.36 Data Architecture

> A description of the structure of the enterprise’s major types and sources of data, logical data assets, physical data assets, and data management resources.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.39 Data Architecture

> Data architecture represents integration of value specifications for qualitative and quantitative variables and their alignment with business architecture and strategy.
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), Appendix A: Glossary, *Data Architecture* (FEAPO definition, adopted 2017)

## How they differ
- **BIZBOK v9 (FEAPO wording)** is abstract ("value specifications for qualitative and quantitative variables") and stresses alignment with business architecture; TOGAF talks concretely about logical and physical data assets and data management resources. DAMA-DMBOK2 would still give the practitioner view.
- TOGAF 10 drops **“and interaction”**: data architecture describes structure only, while interactions are left to application architecture.
- Data-platform patterns (data lake, streaming) from the Data Architecture course sit at solution level; TOGAF's definition is enterprise level.
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Domain subdomain `data-architecture`; with application architecture it forms TOGAF's Information Systems Architecture (ADM Phase C).

## In the course
Data Architecture (FIAP): data lake, streaming, computer-vision systems — `Materiais/Data Archictecture/`. Lectures not ingested yet.

## Open questions
- None.
