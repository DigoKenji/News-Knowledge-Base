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
sources:
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.12 Architecture Domain
  tier: B
evidence:
  bodies:
  - The Open Group
  meets_standard: partial
  gaps:
  - 'A second body: ArchiMate 3.2 layers (same body), ISO/IEC/IEEE 42010:2022 (uses ''architecture aspects'' instead), or Zachman Framework (columns/rows; tier D)'
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
An architecture domain is one area of concern into which an enterprise architecture is divided. TOGAF uses four primary ones (business, data, application, technology); other concerns such as security or governance cut across them. *Follows TOGAF 10 §4.12.*

## Authoritative definitions
> The architectural area being considered. The TOGAF framework follows the tradition of dividing Enterprise Architecture into four primary architecture domains: business, data, application, and technology. Other domains (motivation, security, governance, etc.) may span those four primary domains.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.12 Architecture Domain

## How they differ
- Enterprise and solution are **not** domains in TOGAF: they are scopes or levels (see [Enterprise Architecture](/foundations/enterprise-architecture.md), [Solution Architecture](/foundations/solution-architecture.md)). Data and application are sometimes grouped as *Information Systems Architecture* (TOGAF ADM Phase C).

## Where it sits
Organizes the [Architecture](/domains/architecture.md) domain's subdomains (BDAT).

## In the course
Técnicas de Arquitetura; Data Architecture course covers the data domain. Lectures not ingested yet.

## Open questions
- Track security architecture as a fifth subdomain, or as a cross-cutting concern as TOGAF suggests?
