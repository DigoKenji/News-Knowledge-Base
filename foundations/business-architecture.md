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
- title: 'A Guide to the Business Architecture Body of Knowledge (BIZBOK Guide)'
  publisher: Business Architecture Guild
  edition: v9.0 (2020)
  section: 'Appendix A: Glossary, Business Architecture (PDF p. 1025); FEAPO definition, adopted 2017'
  tier: B
- title: 'Business Architecture: A Practical Guide'
  publisher: 'Whelan, J. and Meaden, G. (2012). Gower'
  edition: 1st (2012)
  section: 'ch. 1 About Business Architecture, pp. 2-3; Glossary, pp. 254-255'
  tier: C
evidence:
  bodies:
  - The Open Group
  - Business Architecture Guild (BIZBOK Guide; FEAPO-adopted definitions)
  meets_standard: yes
  gaps:
  - 'BIZBOK v9.0 (2020) is not the current Guild version (v15+); re-check the wording if Kenji gets the current glossary'
related:
- business-capability
- architecture-domain
- enterprise-architecture
domain: architecture
lang: en
---

# Business Architecture

## Working definition
Business architecture is a structured view of the business itself — what it can do (capabilities), how it delivers value end to end, what information it relies on and how it is organized — and of how these relate to its strategies, products, policies, initiatives and stakeholders. *Follows TOGAF 9.2 §3.24, the course baseline (TOGAF 10 adds processes to that list); BIZBOK v9 uses the same wording.*

## Authoritative definitions
> A representation of holistic, multi-dimensional business views of: capabilities, end-to-end value delivery, information, and organizational structure; and the relationships among these business views and strategies, products, policies, initiatives, and stakeholders.
> — The Open Group, TOGAF Standard 9.2, Part I §3.24 Business Architecture

> A representation of holistic, multi-dimensional business views of: capabilities, end-to-end value delivery, information, and organizational structure; and the relationships among these business views and strategies, products, policies, processes, initiatives, and stakeholders.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.27 Business Architecture

> Business architecture represents holistic, multidimensional business views of capabilities, end-to-end value delivery, information, and organizational structure as well as the relationships among these business views and strategies, products, policies, initiatives, and stakeholders.
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), Appendix A: Glossary, *Business Architecture* (FEAPO definition, adopted 2017)

## How they differ
- **BIZBOK v9 (Business Architecture Guild, FEAPO wording) matches TOGAF 9.2 almost word for word**: the same four views and the same list of related things, with "represents" instead of "a representation of". This confirms the search snippet noted earlier. Whelan and Meaden (2012, p. 2) stress there is no single agreed definition and treat business architecture as "a collection of assets, methods, processes and resources" that enable the organization's goals (tier C).
- TOGAF 10 adds **processes** to the list of things business views relate to; otherwise identical to 9.2.
- The four core views (capabilities, value delivery, information, organization) match the BIZBOK Guide's core domains (confirmed by the BIZBOK v9 glossary entry above).
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Domain subdomain `business-architecture`. Core building blocks: [Business capability](/foundations/business-capability.md), value stream (TOGAF 10 §4.85), organization, information. Processes are modelled with BPMN (OMG).

## In the course
MBA Business Architecture (2026): business architecture, blueprints, BPMN, digital strategy — `Materiais/Business Archictecture/MBA_Business_Architecture_2026_v2.pdf`. Lectures not ingested yet.

## Open questions
- Should value stream be the next foundation entry (TOGAF 10 §4.85 + Series Guide G178)?
