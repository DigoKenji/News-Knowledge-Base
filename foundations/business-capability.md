---
type: Foundation
title: Business capability
description: A particular ability a business may possess or exchange to achieve a specific purpose — what the business does, independent of who does it or how.
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
  section: Part I §3.25 Business Capability; §3.30 Capability
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.28 Business Capability; §4.33 Capability
  tier: B
- title: 'A Guide to the Business Architecture Body of Knowledge (BIZBOK Guide)'
  publisher: Business Architecture Guild
  edition: v9.0 (2020)
  section: 'Appendix A: Glossary, Capability (PDF p. 1029); the Guild cites Ulrich Homann, 2006'
  tier: B
- title: 'Business Architecture: A Practical Guide'
  publisher: 'Whelan, J. and Meaden, G. (2012). Gower'
  edition: 1st (2012)
  section: 'ch. 1 Terminology, pp. 6-7; Glossary, p. 255'
  tier: C
evidence:
  bodies:
  - The Open Group
  - Business Architecture Guild (BIZBOK Guide; FEAPO-adopted definitions)
  meets_standard: yes
  gaps:
  - 'BIZBOK v9.0 (2020) is not the current Guild version (v15+); re-check the wording if Kenji gets the current glossary'
  - 'TOGAF Series Guide G211 Business Capabilities v2 (same body; useful for the ''what not how'' principle)'
related:
- business-architecture
- application-architecture
domain: architecture
lang: en
---

# Business capability

## Working definition
A business capability is something the business is able to do to achieve a specific purpose (for example *manage customer orders*), described independently of the organization, processes and systems that deliver it. *Follows TOGAF 9.2 §3.25, the course baseline (identical in TOGAF 10); the “independent of how” part is the KB's summary, supported by Whelan and Meaden (2012) but not stated verbatim by either body.*

## Authoritative definitions
> A particular ability that a business may possess or exchange to achieve a specific purpose.
> — The Open Group, TOGAF Standard 9.2, Part I §3.25 Business Capability

> A particular ability that a business may possess or exchange to achieve a specific purpose.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.28 Business Capability

> An ability that an organization, person, or system possesses.
> Note: This a general-purpose definition. See Section 4.28, “Business Capability” for how this concept is refined for usage in Business Architecture.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.33 Capability

> A particular ability or capacity that a business may possess or exchange to achieve a specific purpose or outcome.
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), Appendix A: Glossary, *Capability* (the Guild cites Ulrich Homann, 2006)

## How they differ
- **BIZBOK v9 (Business Architecture Guild)** uses the same core wording as TOGAF 9.2 and adds "or capacity" and "or outcome"; both trace back to Homann (2006). Whelan and Meaden (2012, Glossary p. 255) use the general sense (an ability an organization, person or system possesses) and add that capabilities are high-level and need a mix of organization, people, processes, systems and technology to deliver (tier C), which supports the KB's "independent of how" summary.
- Business capability is identical in 9.2 and 10. The general *Capability* entry changed its note: 9.2 gave examples (“Enterprise Architecture, marketing, customer contact, or outbound telemarketing”), 10 points to Business Capability instead.
- The phrase *possess or exchange* means a capability can be bought or sourced from a partner, not only built in-house.
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Core building block of [Business Architecture](/foundations/business-architecture.md); applications support capabilities ([Application Architecture](/foundations/application-architecture.md)).

## In the course
MBA Business Architecture. Lectures not ingested yet.

## Open questions
- None.
