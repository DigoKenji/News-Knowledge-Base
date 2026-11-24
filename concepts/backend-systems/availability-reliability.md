---
type: Concept
title: Availability and reliability
description: Reliability is a system's ability to perform its required function without failure over time; availability is the fraction of time it is ready to be used when needed, often written as "nines" (99.9% = about 8.8 hours down a year).
tags:
- backend-systems
- core
domain: backend-systems
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: ISO/IEC (2023). ISO/IEC 25010 - Systems and software Quality Requirements and Evaluation (SQuaRE) - Product quality model
  resource: 'https://www.iso.org/standard/78176.html'
  kind: standard
- title: IEC. IEC 60050-192 - International Electrotechnical Vocabulary - Dependability (Electropedia)
  resource: 'https://www.electropedia.org/iev/iev.nsf/index?openform&part=192'
  kind: standard
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 3 "Embracing Risk"
  resource: 'https://sre.google/sre-book/embracing-risk/'
  kind: book
foundation_candidate: true
---

# Availability and reliability

**Definition.** ISO/IEC 25010 lists reliability as a quality characteristic with sub-characteristics including availability, fault tolerance and recoverability; the IEC 60050-192 dependability vocabulary defines availability as the ability to be in a state to perform as required. Avizienis et al. place both inside dependability, alongside safety, integrity and maintainability. In operations, availability is often measured as successful requests divided by total requests.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

These are the targets that fault tolerance, redundancy and SRE practices serve. Google SRE argues that 100% is the wrong target: each extra nine costs much more, and users cannot tell the difference beyond the reliability of their own network. The gap between the target and 100% becomes the error budget that funds releases and experiments.

## Example

A payments API has a 99.95% monthly availability target, which allows about 22 minutes of failed requests a month. A 15-minute outage uses most of that budget, so the team freezes risky deploys until the month resets.

## Narrower concepts

- [SLO error budget](/concepts/cloud-infra/slo-error-budget.md)
- [Fault tolerance and redundancy](/concepts/backend-systems/fault-tolerance.md)
- [Correlated failure](/concepts/backend-systems/correlated-failure.md)
- [Cascading failure](/concepts/backend-systems/cascading-failure.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. ISO/IEC (2023). *ISO/IEC 25010 - Systems and software Quality Requirements and Evaluation (SQuaRE) - Product quality model*. <https://www.iso.org/standard/78176.html> — Reliability as quality characteristic with availability, fault tolerance, recoverability (paywalled; cited, not quoted).
2. IEC. *IEC 60050-192 - International Electrotechnical Vocabulary - Dependability* (Electropedia). <https://www.electropedia.org/iev/iev.nsf/index?openform&part=192> — Standard definitions of availability and reliability.
3. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 3 "Embracing Risk". <https://sre.google/sre-book/embracing-risk/> — Measuring availability, why not 100%, error budgets.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
