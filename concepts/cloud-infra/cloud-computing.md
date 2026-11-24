---
type: Concept
title: Cloud computing
description: A model for on-demand network access to a shared pool of configurable computing resources (servers, storage, networks, applications) that can be provisioned and released quickly with minimal management effort, paying for what is used.
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
- title: Mell, P. and Grance, T. (2011). NIST SP 800-145 - The NIST Definition of Cloud Computing. National Institute of Standards and Technology
  resource: 'https://doi.org/10.6028/NIST.SP.800-145'
  kind: standard
- title: ISO/IEC (2023). ISO/IEC 22123-1 - Information technology - Cloud computing - Part 1 Vocabulary
  resource: 'https://www.iso.org/standard/82758.html'
  kind: standard
- title: Armbrust, M. et al. (2010). A View of Cloud Computing. Communications of the ACM 53(4)
  resource: 'https://doi.org/10.1145/1721654.1721672'
  kind: paper
foundation_candidate: true
---

# Cloud computing

**Definition.** NIST SP 800-145 defines cloud computing as a model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources that can be rapidly provisioned and released with minimal management effort or service provider interaction. It names five essential characteristics (on-demand self-service, broad network access, resource pooling, rapid elasticity, measured service), three service models and four deployment models. ISO/IEC 22123-1 provides the international vocabulary.

Domain: [cloud-infra](/domains/cloud-infra.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Cloud changed infrastructure from a capital purchase into an operating expense and made elasticity, managed services and global reach available to small teams. Armbrust et al. framed the economics: the illusion of infinite capacity, no up-front commitment and pay-per-use. The trade-offs are cost discipline (FinOps), shared responsibility for security and some dependency on a provider.

## Example

A startup launches on a cloud provider with no hardware: it starts two small virtual machines and a managed database through an API, scales to 40 machines on launch day and back to two the next week, paying only for the hours used.

## Narrower concepts

- [Service models (IaaS, PaaS, SaaS)](/concepts/cloud-infra/cloud-service-models.md)
- [Deployment models (public, private, hybrid)](/concepts/cloud-infra/cloud-deployment-models.md)
- [Serverless](/concepts/cloud-infra/serverless.md)
- [Well-Architected Framework](/concepts/cloud-infra/well-architected.md)
- [FinOps and cloud cost](/concepts/cloud-infra/finops.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. Mell, P. and Grance, T. (2011). *NIST SP 800-145 - The NIST Definition of Cloud Computing*. National Institute of Standards and Technology. <https://doi.org/10.6028/NIST.SP.800-145> — Definition, five essential characteristics, service and deployment models.
2. ISO/IEC (2023). *ISO/IEC 22123-1 - Information technology - Cloud computing - Part 1 Vocabulary*. <https://www.iso.org/standard/82758.html> — International cloud vocabulary (paywalled; cited, not quoted).
3. Armbrust, M. et al. (2010). *A View of Cloud Computing*. Communications of the ACM 53(4). <https://doi.org/10.1145/1721654.1721672> — Economics of cloud computing and its obstacles.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
