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
evidence:
  bodies:
  - The Open Group
  meets_standard: partial
  gaps:
  - DAMA-DMBOK2 (DAMA International), chapter on Data Architecture — book, Kenji to provide
  - Data Architecture course slides (FIAP, Aula02–04) as tier E
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

## How they differ
- TOGAF 10 drops **“and interaction”**: data architecture describes structure only, while interactions are left to application architecture.
- Data-platform patterns (data lake, streaming) from the Data Architecture course sit at solution level; TOGAF's definition is enterprise level.
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Domain subdomain `data-architecture`; with application architecture it forms TOGAF's Information Systems Architecture (ADM Phase C).

## In the course
Data Architecture (FIAP): data lake, streaming, computer-vision systems — `Materiais/Data Archictecture/`. Lectures not ingested yet.

## Open questions
- None.
