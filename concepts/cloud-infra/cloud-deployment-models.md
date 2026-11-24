---
type: Concept
title: Deployment models (public, private, hybrid)
description: Who the cloud infrastructure is provisioned for and where it runs - public (open to anyone, run by a provider), private (one organisation), community (a group with shared concerns) or hybrid (two or more combined) - plus the common multi-cloud practice.
tags:
- cloud-infra
- core
domain: cloud-infra
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Mell, P. and Grance, T. (2011). NIST SP 800-145 - The NIST Definition of Cloud Computing
  resource: 'https://doi.org/10.6028/NIST.SP.800-145'
  kind: standard
- title: ISO/IEC (2023). ISO/IEC 22123-2 - Information technology - Cloud computing - Part 2 Concepts
  resource: 'https://www.iso.org/standard/80352.html'
  kind: standard
- title: Microsoft. Azure Architecture Center - Hybrid architecture design
  resource: 'https://learn.microsoft.com/en-us/azure/architecture/hybrid/hybrid-start-here'
  kind: docs
foundation_candidate: true
---

# Deployment models (public, private, hybrid)

**Definition.** NIST SP 800-145 defines four deployment models: private cloud (exclusive use by a single organisation), community cloud (exclusive use by a community with shared concerns), public cloud (open use by the general public, on the provider's premises) and hybrid cloud (two or more distinct infrastructures bound together by technology that enables data and application portability). Multi-cloud, using several public providers, is a later practice described in ISO/IEC 22123.

Domain: [cloud-infra](/domains/cloud-infra.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

The choice is driven by regulation and data residency, latency to on-premises systems, existing investments and cost. Hybrid is common in banks and governments; multi-cloud promises less lock-in but multiplies skills, tooling and egress costs, so it should be a deliberate decision rather than a default.

## Example

A hospital keeps patient records in its own data centre (private) for regulatory reasons, runs its public website and analytics in a public cloud, and connects the two with a private network link (hybrid).

## Narrower concepts

- [Well-Architected Framework](/concepts/cloud-infra/well-architected.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. Mell, P. and Grance, T. (2011). *NIST SP 800-145 - The NIST Definition of Cloud Computing*. <https://doi.org/10.6028/NIST.SP.800-145> — Definitions of private, community, public and hybrid cloud.
2. ISO/IEC (2023). *ISO/IEC 22123-2 - Information technology - Cloud computing - Part 2 Concepts*. <https://www.iso.org/standard/80352.html> — Deployment models including multi-cloud (paywalled; cited, not quoted).
3. Microsoft. *Azure Architecture Center - Hybrid architecture design*. <https://learn.microsoft.com/en-us/azure/architecture/hybrid/hybrid-start-here> — Practitioner guidance on when and how to adopt hybrid and multi-cloud.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
