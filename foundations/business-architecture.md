---
type: Foundation
title: Business Architecture
description: A representation of the business — capabilities, value delivery, information and organization — and how these relate to strategies, products, policies, initiatives and stakeholders (TOGAF 9.2).
tags:
- foundation
- architecture
- business-architecture
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
  section: Part I §3.24 Business Architecture
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.27 Business Architecture
  tier: B
evidence:
  bodies:
  - The Open Group
  meets_standard: partial
  gaps:
  - 'BIZBOK Guide v15 public glossary (Appendix A), Business Architecture Guild: Kenji downloads the PDF from businessarchitectureguild.org (cloud fetch is blocked) into FIAP-MBA/Referencias; a search snippet suggests the Guild''s wording matches TOGAF''s, but it is not verified'
related:
- business-capability
- architecture-domain
- enterprise-architecture
domain: architecture
lang: en
---

# Business Architecture

## Working definition
Business architecture is a structured view of the business itself — what it can do (capabilities), how it delivers value end to end, what information it relies on and how it is organized — and of how these relate to its strategies, products, policies, initiatives and stakeholders. *Follows TOGAF 9.2 §3.24, the course baseline (TOGAF 10 adds processes to that list); pending the BIZBOK glossary.*

## Authoritative definitions
> A representation of holistic, multi-dimensional business views of: capabilities, end-to-end value delivery, information, and organizational structure; and the relationships among these business views and strategies, products, policies, initiatives, and stakeholders.
> — The Open Group, TOGAF Standard 9.2, Part I §3.24 Business Architecture

> A representation of holistic, multi-dimensional business views of: capabilities, end-to-end value delivery, information, and organizational structure; and the relationships among these business views and strategies, products, policies, processes, initiatives, and stakeholders.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.27 Business Architecture

## How they differ
- TOGAF 10 adds **processes** to the list of things business views relate to; otherwise identical to 9.2.
- The four core views (capabilities, value delivery, information, organization) match the BIZBOK Guide's core domains; to confirm with the BIZBOK glossary text.
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Domain subdomain `business-architecture`. Core building blocks: [Business capability](/foundations/business-capability.md), value stream (TOGAF 10 §4.85), organization, information. Processes are modelled with BPMN (OMG).

## In the course
MBA Business Architecture (Prof. Rogério Kobayashi, 2026): business architecture, blueprints, BPMN, digital strategy — `Materiais/Business Archictecture/MBA_Business_Architecture_2026_v2.pdf`. Lectures not ingested yet.

## Open questions
- Should value stream be the next foundation entry (TOGAF 10 §4.85 + Series Guide G178)?
