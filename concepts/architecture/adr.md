---
type: Concept
title: Architecture decision record
description: 'A short document that records one significant architecture decision - its context, the options considered, the choice and its consequences - kept with the code.'
tags:
- architecture
- architecture-practice
domain: architecture
subdomain: architecture-practice
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 6 Making Decisions; ch. 8 Is This Architecture? (Architectural Decisions)'
- title: 'Nygard, M. (2011). Documenting Architecture Decisions. Cognitect blog'
  resource: 'https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions'
  kind: practitioner
- title: 'ADR GitHub organization. Architectural Decision Records (templates and tooling)'
  resource: 'https://adr.github.io/'
  kind: docs
- title: 'ISO/IEC/IEEE 42010 - Architecture description (overview site)'
  resource: 'http://www.iso-architecture.org/42010/'
  kind: standard
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; moved to architecture; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Architecture decision record

**Definition.** An architecture decision record captures one architecturally significant decision in a short, numbered text file: title, status (proposed, accepted, superseded), context, decision and consequences. Michael Nygard proposed the format in 2011 so that the reasons behind a system's shape survive team changes; records are never edited after acceptance, only superseded by new ones.

Domain: [architecture](/domains/architecture.md) · subdomain `architecture-practice`. Expanded on 2026-11-26 with independent sources.

## How it fits

ADRs are the lightweight, versioned form of what ISO/IEC/IEEE 42010 calls architecture rationale, and they pass Hohpe's test that architecture documentation must contain decisions. Kept in the repository next to the code, they are reviewed like code. News-Automation itself records its design in docs/adr/.

## Example

ADR-0006 in News-Automation records that Claude never fetches Medium itself and that Hermes hands article text to the inbox, with the alternatives that were rejected and why.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (Architect Elevator added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 6 Making Decisions; ch. 8 Is This Architecture? (Architectural Decisions): Judge a decision by the information and risk at the time, not by its outcome; architecture documents are worth having when they record decisions and rationale.

## Sources

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 6 Making Decisions; ch. 8 Is This Architecture? (Architectural Decisions). Judge a decision by the information and risk at the time, not by its outcome; architecture documents are worth having when they record decisions and rationale.
2. Nygard, M. (2011). Documenting Architecture Decisions. Cognitect blog. <https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions> — Origin of the ADR format.
3. ADR GitHub organization. Architectural Decision Records (templates and tooling). <https://adr.github.io/> — Templates (Nygard, MADR) and practice guidance.
4. ISO/IEC/IEEE 42010 - Architecture description (overview site). <http://www.iso-architecture.org/42010/> — Architecture decisions and rationale as part of an architecture description.

## Relations
- [ATAM tradeoff analysis](/concepts/architecture/atam.md) **part-of** this

## Notes
Notes tagged with this concept are listed in the concept index after each run.
