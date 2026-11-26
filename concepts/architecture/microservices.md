---
type: Concept
title: Microservices
description: An architectural style that builds an application as a suite of small, independently deployable services, each owning its data and a business capability, communicating over the network.
tags:
- architecture
- software-architecture
- core
domain: architecture
subdomain: software-architecture
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Lewis, J. and Fowler, M. (2014). Microservices. martinfowler.com
  resource: 'https://martinfowler.com/articles/microservices.html'
  kind: practitioner
- title: 'Dragoni, N. et al. (2017). Microservices - yesterday, today, and tomorrow. arXiv:1606.04036'
  resource: 'https://arxiv.org/abs/1606.04036'
  kind: paper
- title: Newman, S. (2021). Building Microservices, 2nd ed. O'Reilly
  resource: 'https://samnewman.io/books/building_microservices_2nd_edition/'
  kind: book
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Microservices

**Definition.** Lewis and Fowler (2014) describe microservices as an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms, built around business capabilities and independently deployable by fully automated machinery. Each service owns its database ("decentralised data management").

Domain: [architecture](/domains/architecture.md) · subdomain `software-architecture`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Microservices let many teams deploy and scale independently, which is why large organisations adopt them. The costs are those of distributed systems: network failures, eventual consistency, distributed transactions (sagas), observability and operational overhead. Many teams now start with a modular monolith and extract services only when needed.

## Example

An online store splits into catalog, cart, payment and shipping services; the payment team can release three times a day without coordinating with the catalog team.

## Narrower concepts

- [Saga pattern](/concepts/backend-systems/saga-pattern.md)
- [Bounded context](/concepts/architecture/bounded-context.md)
- [API versioning](/concepts/backend-systems/api-versioning.md)
- [Strangler fig migration](/concepts/architecture/strangler-fig.md)

## Sources

1. Lewis, J. and Fowler, M. (2014). *Microservices*. martinfowler.com. <https://martinfowler.com/articles/microservices.html> — The defining description and characteristics.
2. Dragoni, N. et al. (2017). *Microservices - yesterday, today, and tomorrow*. arXiv:1606.04036. <https://arxiv.org/abs/1606.04036> — Academic review of the style and open problems.
3. Newman, S. (2021). *Building Microservices*, 2nd ed. O'Reilly. <https://samnewman.io/books/building_microservices_2nd_edition/> — Practical guidance, including when not to use microservices.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
