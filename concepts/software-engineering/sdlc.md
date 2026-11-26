---
type: Concept
title: Software development life cycle (SDLC)
description: The sequence of activities that takes software from idea to retirement - requirements, design, implementation, testing, deployment, operation and maintenance - and the process model (waterfall, iterative, agile) that organises them.
tags:
- software-engineering
- process
- core
domain: software-engineering
subdomain: process
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: ISO/IEC/IEEE (2017). ISO/IEC/IEEE 12207 - Software life cycle processes
  resource: 'https://www.iso.org/standard/63712.html'
  kind: standard
- title: IEEE Computer Society (2024). SWEBOK Guide V4.0, Software Engineering Process
  resource: 'https://www.computer.org/education/bodies-of-knowledge/software-engineering'
  kind: standard
- title: Boehm, B. (1988). A Spiral Model of Software Development and Enhancement. IEEE Computer 21(5)
  resource: 'https://doi.org/10.1109/2.59'
  kind: paper
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Software development life cycle (SDLC)

**Definition.** ISO/IEC/IEEE 12207 defines the software life cycle processes an organisation can use, from acquisition and requirements through development, operation, maintenance and disposal. A life cycle model chooses how these activities are sequenced: waterfall runs them once in order, iterative and spiral models repeat them in cycles with risk checks, and agile methods run them in short increments.

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `process`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

The SDLC frames where each practice belongs: requirements engineering at the start, testing and security throughout, CI/CD and DevOps connecting development to operations. Modern practice favours short cycles because feedback from real users and production reduces the risk of building the wrong thing.

## Example

Waterfall - 12 months of requirements, design and build, then one release. Agile - a working increment every two weeks, with requirements refined along the way.

## Narrower concepts

- [Requirements and quality attributes](/concepts/software-engineering/software-requirements.md)
- [Software testing](/concepts/software-engineering/software-testing.md)
- [Continuous integration and continuous delivery (CI/CD)](/concepts/software-engineering/ci-cd.md)
- [Secure software development (SSDLC)](/concepts/security/secure-software-development.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. ISO/IEC/IEEE (2017). *ISO/IEC/IEEE 12207 - Software life cycle processes*. <https://www.iso.org/standard/63712.html> — Standard set of life cycle processes (paywalled; cited, not quoted).
2. IEEE Computer Society (2024). *SWEBOK Guide V4.0*, Software Engineering Process. <https://www.computer.org/education/bodies-of-knowledge/software-engineering> — Life cycle models and process definitions.
3. Boehm, B. (1988). *A Spiral Model of Software Development and Enhancement*. IEEE Computer 21(5). <https://doi.org/10.1109/2.59> — Iterative, risk-driven alternative to waterfall.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
