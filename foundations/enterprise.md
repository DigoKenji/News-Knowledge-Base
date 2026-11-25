---
type: Foundation
title: Enterprise
description: Any collection of organizations with common goals, described at the highest level that covers all their missions and functions.
tags:
- foundation
- architecture
- enterprise-architecture
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
  section: Part I §3.38 Enterprise; §1.3 Executive Overview, "What is an enterprise?"
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.43 Enterprise; §1.1 Executive Overview, "What is an Enterprise?"
  tier: B
- title: 'SEBoK - Guide to the Systems Engineering Body of Knowledge, glossary: Enterprise'
  publisher: 'BKCASE Editorial Board (INCOSE, IEEE Systems Council, Stevens Institute/SERC)'
  edition: 'v2.x (online, accessed 2026-11-25)'
  section: 'Glossary entry, quoting the standards listed'
  resource: 'https://sebokwiki.org/wiki/Enterprise_(glossary)'
  tier: B
evidence:
  bodies:
  - The Open Group
  - ISO (ISO 15704, via SEBoK)
  - US Federal CIO Council (FEAF, via SEBoK)
  meets_standard: yes
  gaps:
  - 'Both quotes are via SEBoK; the ISO 15704 original is paywalled'
related:
- enterprise-architecture
- architecture
domain: architecture
lang: en
---

# Enterprise

## Working definition
An enterprise is any collection of organizations that share common goals: a company, a division, a government department or a partnership. *Follows TOGAF 9.2 Part I §1.3 and §3.38, the course baseline: the enterprise is whatever the architecture is meant to cover, and it can span several legal organizations.*

## Authoritative definitions
> The highest level (typically) of description of an organization and typically covers all missions and functions. An enterprise will often span multiple organizations.
> — The Open Group, TOGAF Standard 9.2, Part I §3.38 Enterprise

> The TOGAF standard considers an "enterprise" to be any collection of organizations that have common goals.
> — The Open Group, TOGAF Standard 9.2, Part I §1.3 Executive Overview

> The highest level (typically) of description of an organization and typically covers all missions and functions. An enterprise will often span multiple organizations.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.43 Enterprise

> The TOGAF Standard considers an “enterprise” to be any collection of organizations that have common goals.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §1.1 Executive Overview

> one or more organizations sharing a definite mission, goals, and objectives to offer an output such as a product or service
> — ISO 15704:2000, as reproduced in the [SEBoK glossary: Enterprise](https://sebokwiki.org/wiki/Enterprise_(glossary))

> An organization (or cross organizational entity) supporting a defined business scope and mission that includes interdependent resources (people, organizations and technologies) that must coordinate their functions and share information in support of a common mission (or set of related missions).
> — US Federal CIO Council, Federal Enterprise Architecture Framework (1999), as reproduced in the SEBoK glossary

## How they differ
- **Two more bodies agree with TOGAF's core idea.** ISO 15704 and the US federal EA framework both define an enterprise as one or more organizations united by a shared mission and goals, which matches TOGAF's "collection of organizations that have common goals". FEAF adds the interdependent resources (people, organizations, technology) that must share information.
- The formal definition is word for word the same in 9.2 and 10.
- TOGAF 10 §1.1 adds the scope rule: “Enterprise” in Enterprise Architecture can mean the entire enterprise or “one or more specific areas of interest within the enterprise”, and may include partners, suppliers and customers.
- **Baseline.** The KB follows TOGAF 9.2 (course guidance, 2026-11-19: 9.2 is consolidated, while TOGAF 10 is still being updated). TOGAF 10 wording is kept here to show what changed.

## Where it sits
Defines the scope of [Enterprise Architecture](/foundations/enterprise-architecture.md). Not a synonym of *company*: one company can contain several enterprises, and one enterprise can span several companies.

## In the course
Business Architecture frames the enterprise through purpose, mission, vision and strategy. Lectures not ingested yet.

## Open questions
- For MBA case studies (e.g. Tem Tudo para Carro), treat each case company as one enterprise?
