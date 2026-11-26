---
type: Concept
title: Quality attributes and trade-offs
description: The measurable properties that say how well a system works - performance, availability, security, modifiability, scalability, cost and so on - which drive architecture and usually trade off against each other.
tags:
- architecture
- core
domain: architecture
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: ISO/IEC (2023). ISO/IEC 25010 - Product quality model
  resource: 'https://www.iso.org/standard/78176.html'
  kind: standard
- title: Bass, L., Clements, P. and Kazman, R. (2021). Software Architecture in Practice, 4th ed. Addison-Wesley
  resource: 'https://www.oreilly.com/library/view/software-architecture-in/9780136885979/'
  kind: book
- title: AWS. AWS Well-Architected Framework
  resource: 'https://aws.amazon.com/architecture/well-architected/'
  kind: practitioner
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 9 Architecture Is Selling Options (An Architecture Option: Elasticity; Strike Prices)'
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Quality attributes and trade-offs

**Definition.** ISO/IEC 25010 defines a product quality model with characteristics such as functional suitability, performance efficiency, compatibility, interaction capability, reliability, security, maintainability, flexibility and safety. Bass, Clements and Kazman make them concrete with quality attribute scenarios: stimulus, environment, response and a measurable response measure.

Domain: [architecture](/domains/architecture.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Architecture is mostly the art of balancing quality attributes: caching improves performance but risks consistency; encryption improves security but costs latency; more redundancy improves availability but costs money. Methods such as ATAM and frameworks such as AWS Well-Architected (organised by pillars) exist to make these trade-offs explicit.

## Example

Scenario - "when 500 users search at once during normal operation, 95% of results return in under 1 second." That is testable, unlike "the system must be fast".

## Narrower concepts

- [ATAM tradeoff analysis](/concepts/backend-systems/atam.md)
- [Well-Architected Framework](/concepts/cloud-infra/well-architected.md)
- [SLO error budget](/concepts/cloud-infra/slo-error-budget.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (Architect Elevator added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 9 Architecture Is Selling Options (An Architecture Option: Elasticity; Strike Prices): Quality attributes such as elasticity as options with an up-front price and a strike price, worth more when demand is uncertain.

## Sources

1. ISO/IEC (2023). *ISO/IEC 25010 - Product quality model*. <https://www.iso.org/standard/78176.html> — Standard quality characteristics (paywalled; cited).
2. Bass, L., Clements, P. and Kazman, R. (2021). *Software Architecture in Practice*, 4th ed. Addison-Wesley. <https://www.oreilly.com/library/view/software-architecture-in/9780136885979/> — Quality attribute scenarios and tactics.
3. AWS. *AWS Well-Architected Framework*. <https://aws.amazon.com/architecture/well-architected/> — Quality attributes organised as pillars with trade-off guidance.
4. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 9 Architecture Is Selling Options (An Architecture Option: Elasticity; Strike Prices). Quality attributes such as elasticity as options with an up-front price and a strike price, worth more when demand is uncertain.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
