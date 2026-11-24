---
type: Concept
title: Software testing
description: Executing software under controlled conditions to find defects and gain confidence it meets its requirements - at several levels (unit, integration, system/end-to-end) and with automated tests run on every change.
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
- title: ISO/IEC/IEEE (2022). ISO/IEC/IEEE 29119-1 - Software testing - General concepts
  resource: 'https://www.iso.org/standard/81291.html'
  kind: standard
- title: ISTQB. ISTQB Glossary
  resource: 'https://glossary.istqb.org/'
  kind: standard
- title: Vocke, H. (2018). The Practical Test Pyramid. martinfowler.com
  resource: 'https://martinfowler.com/articles/practical-test-pyramid.html'
  kind: practitioner
foundation_candidate: true
---

# Software testing

**Definition.** ISO/IEC/IEEE 29119 is the international standard for software testing concepts, processes and techniques. Unit tests check one small piece in isolation, integration tests check that pieces work together, and system or end-to-end tests check the whole application as a user would. Testing can show the presence of bugs, never prove their absence (Dijkstra).

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

The test pyramid recommends many fast unit tests, fewer integration tests and few slow end-to-end tests. Automated tests are what make CI/CD and refactoring safe. Specialised forms extend testing to production-like failure: deterministic simulation, chaos engineering and canary releases.

## Example

The KB validator is a test: on every run it checks that all documents have frontmatter and all links resolve, and fails the run if not.

## Narrower concepts

- [Deterministic simulation testing](/concepts/backend-systems/deterministic-simulation-testing.md)
- [Chaos engineering](/concepts/backend-systems/chaos-engineering.md)
- [Canary / blue-green deploy](/concepts/cloud-infra/canary-deploy.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. ISO/IEC/IEEE (2022). *ISO/IEC/IEEE 29119-1 - Software testing - General concepts*. <https://www.iso.org/standard/81291.html> — Standard testing concepts and levels (paywalled; cited).
2. ISTQB. *ISTQB Glossary*. <https://glossary.istqb.org/> — Widely used testing terminology (unit, integration, system, acceptance).
3. Vocke, H. (2018). *The Practical Test Pyramid*. martinfowler.com. <https://martinfowler.com/articles/practical-test-pyramid.html> — Test pyramid and automated testing in practice.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
