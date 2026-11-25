---
type: Foundation
title: Architecture domain
description: 'One of the areas an enterprise architecture is divided into; TOGAF uses four primary domains: business, data, application and technology.'
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
- title: The TOGAF Standard, Version 9.2
  publisher: The Open Group
  edition: 9.2 (2018)
  section: Part I §3.11 Architecture Domain
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.12 Architecture Domain
  tier: B
- title: 'A Guide to the Business Architecture Body of Knowledge (BIZBOK Guide)'
  publisher: 'Business Architecture Guild'
  edition: 'v9.0 (2020)'
  section: '§6.1 IT Architecture Overview (PDF p. 619) and §6.2 Enterprise Architecture, figure 6.2.1 (PDF p. 633)'
  tier: B
evidence:
  bodies:
  - The Open Group
  - Business Architecture Guild (BIZBOK Guide)
  meets_standard: yes
  gaps:
  - 'BIZBOK has no glossary entry for ''architecture domain'' itself; the evidence is its §6.1-6.2 usage, which matches TOGAF''s four domains'
related:
- business-architecture
- data-architecture
- application-architecture
- technology-architecture
- enterprise-architecture
domain: architecture
lang: en
---

# Architecture domain

## Working definition
An architecture domain is one area of concern into which an enterprise architecture is divided. TOGAF has four primary ones (business, data, application, technology); other domains, such as security, may also be considered. *Follows TOGAF 9.2 §3.11, the course baseline.*

## Authoritative definitions
> The architectural area being considered. The TOGAF framework has four primary architecture domains: business, data, application, and technology. Other domains may also be considered (e.g., security).
> — The Open Group, TOGAF Standard 9.2, Part I §3.11 Architecture Domain

> The architectural area being considered. The TOGAF framework follows the tradition of dividing Enterprise Architecture into four primary architecture domains: business, data, application, and technology. Other domains (motivation, security, governance, etc.) may span those four primary domains.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.12 Architecture Domain

> EA has a number of foundational concepts and domains. Business architecture is one of the architectural domains.
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), §6.2 Enterprise Architecture

> Consider the following definitions for each of these four IT architecture domains. [data, application, technical architecture, shadow systems]
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), §6.1 IT Architecture Overview

## How they differ
- **BIZBOK v9 uses the same partition.** Business architecture is one domain of EA, and IT architecture has data, application and technical architecture as its domains. BIZBOK adds a fourth IT category, "shadow systems" (automation the business deploys outside IT's view), which TOGAF does not name.
- TOGAF 10 rewords 9.2: it “follows the tradition of dividing Enterprise Architecture” into the four domains, and names motivation, security and governance as domains that may span them; 9.2 only gives security as an example.
- Enterprise and solution are **not** domains in TOGAF: they are scopes or levels (see [Enterprise Architecture](/foundations/enterprise-architecture.md), [Solution Architecture](/foundations/solution-architecture.md)). Data and application are sometimes grouped as *Information Systems Architecture* (TOGAF ADM Phase C).
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Organizes the [Architecture](/domains/architecture.md) domain's subdomains (BDAT).

## In the course
Técnicas de Arquitetura; Data Architecture course covers the data domain. Lectures not ingested yet.

## Open questions
- Track security architecture as a fifth subdomain, or as a cross-cutting concern as TOGAF suggests?
