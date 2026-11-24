---
type: Concept
title: Requirements and quality attributes
description: Statements of what a system must do (functional requirements) and how well it must do it (non-functional requirements or quality attributes such as performance, security, availability and maintainability).
tags:
- software-engineering
- core
domain: software-engineering
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: ISO/IEC/IEEE (2018). ISO/IEC/IEEE 29148 - Requirements engineering
  resource: 'https://www.iso.org/standard/72089.html'
  kind: standard
- title: ISO/IEC (2023). ISO/IEC 25010 - Product quality model
  resource: 'https://www.iso.org/standard/78176.html'
  kind: standard
- title: IEEE Computer Society (2024). SWEBOK Guide V4.0, Software Requirements
  resource: 'https://www.computer.org/education/bodies-of-knowledge/software-engineering'
  kind: standard
foundation_candidate: true
---

# Requirements and quality attributes

**Definition.** ISO/IEC/IEEE 29148 covers requirements engineering: eliciting, analysing, specifying, validating and managing requirements. Functional requirements describe behaviour ("the user can export a report"); non-functional requirements constrain qualities ("export finishes in under 3 seconds for 10,000 rows"). ISO/IEC 25010 provides the standard product quality model that names these qualities.

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Requirements errors are the most expensive to fix late, so they drive much of project success. Non-functional requirements shape the architecture more than functional ones: two systems with the same features but different scale or security needs end up with very different designs.

## Example

Functional - "send a daily digest to Telegram". Non-functional - "deliver it by 07:30 BRT on 99% of days and cost under $20 a month".

## Narrower concepts

- [Quality attributes and trade-offs](/concepts/architecture/quality-attributes.md)
- [ATAM tradeoff analysis](/concepts/backend-systems/atam.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. ISO/IEC/IEEE (2018). *ISO/IEC/IEEE 29148 - Requirements engineering*. <https://www.iso.org/standard/72089.html> — Requirements engineering processes and good requirement characteristics (paywalled; cited).
2. ISO/IEC (2023). *ISO/IEC 25010 - Product quality model*. <https://www.iso.org/standard/78176.html> — Standard list of quality characteristics (paywalled; cited).
3. IEEE Computer Society (2024). *SWEBOK Guide V4.0*, Software Requirements. <https://www.computer.org/education/bodies-of-knowledge/software-engineering> — Functional vs non-functional requirements, elicitation and validation.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
