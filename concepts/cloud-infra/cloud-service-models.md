---
type: Concept
title: Service models (IaaS, PaaS, SaaS)
description: The three standard ways cloud capability is offered - infrastructure (IaaS, rent virtual machines, storage and networks), platform (PaaS, deploy your code on a managed runtime) and software (SaaS, use a finished application) - differing in how much the provider manages.
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
- title: Amazon Web Services. Shared Responsibility Model
  resource: 'https://aws.amazon.com/compliance/shared-responsibility-model/'
  kind: docs
foundation_candidate: true
---

# Service models (IaaS, PaaS, SaaS)

**Definition.** NIST SP 800-145 defines Software as a Service (the consumer uses the provider's applications), Platform as a Service (the consumer deploys its own applications using the provider's languages, libraries and tools, without managing the underlying infrastructure) and Infrastructure as a Service (the consumer provisions processing, storage and networks and controls operating systems and applications). ISO/IEC 22123 describes the same capabilities in terms of cloud capability types and service categories.

Domain: [cloud-infra](/domains/cloud-infra.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

The models map to the shared responsibility model: the higher up (SaaS), the more the provider secures and operates; with IaaS the customer patches the OS and configures the network. Newer offerings (containers as a service, functions as a service, AI model APIs) sit between and beyond the original three.

## Example

Renting a virtual machine on which you install PostgreSQL is IaaS; deploying a web app to a managed platform that runs and scales it is PaaS; using Gmail or Notion is SaaS.

## Narrower concepts

- [Serverless](/concepts/cloud-infra/serverless.md)
- [Virtualisation and containers](/concepts/cloud-infra/virtualization-containers.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. Mell, P. and Grance, T. (2011). *NIST SP 800-145 - The NIST Definition of Cloud Computing*. <https://doi.org/10.6028/NIST.SP.800-145> — Definitions of SaaS, PaaS and IaaS.
2. ISO/IEC (2023). *ISO/IEC 22123-2 - Information technology - Cloud computing - Part 2 Concepts*. <https://www.iso.org/standard/80352.html> — Cloud capability types and service categories (paywalled; cited, not quoted).
3. Amazon Web Services. *Shared Responsibility Model*. <https://aws.amazon.com/compliance/shared-responsibility-model/> — How responsibility shifts between provider and customer across service types.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
