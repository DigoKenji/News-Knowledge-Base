---
type: Foundation
title: Application Architecture
description: A description of the structure and interaction of the applications that provide key business capabilities and manage the data assets.
tags:
- foundation
- architecture
- application-architecture
status: draft
generated:
  by: claude-opus (interactive foundation session)
  at: '2026-11-18T18:30:00-03:00'
sources:
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.3 Application Architecture
  tier: B
- title: The TOGAF Standard, Version 9.2
  publisher: The Open Group
  edition: 9.2 (2018)
  section: Part I §3.3 Application Architecture
  tier: B
evidence:
  bodies:
  - The Open Group
  meets_standard: partial
  gaps:
  - 'A second body: ISO/IEC/IEEE 42010 (no domain-specific definition), or a textbook such as Fowler, Patterns of Enterprise Application Architecture (2002, tier C, in Materiais)'
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
Application architecture describes which applications the enterprise uses, how they interact, which business capabilities they support and which data they manage. *Follows TOGAF 10 §4.3.*

## Authoritative definitions
> A description of the structure and interaction of the applications that provide key business capabilities and manage the data assets.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.3 Application Architecture

> A description of the structure and interaction of the applications as groups of capabilities that provide key business functions and manage the data assets.
> — The Open Group, TOGAF Standard 9.2, Part I §3.3 Application Architecture

## How they differ
- 9.2 framed applications as *groups of capabilities that provide key business functions*; 10 says applications *provide key business capabilities*. The capability, not the function, is now the business anchor — consistent with business architecture's capability focus.
- Not the same as software architecture: TOGAF's application architecture is about the portfolio and interactions of applications, not the inside of one application.

## Where it sits
Domain subdomain `application-architecture`. Inside-the-application design (patterns, microservices) is filed under `integration-and-software-architecture`.

## In the course
Técnicas de Arquitetura (Fowler PoEAA, Enterprise Integration Patterns, microservices material). Lectures not ingested yet.

## Open questions
- None.
