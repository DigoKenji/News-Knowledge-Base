---
type: Concept
title: Event-driven architecture
description: An architectural style in which components communicate by producing and reacting to events - records that something happened - usually through a broker, so producers do not need to know who consumes them.
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
- title: Fowler, M. (2017). What do you mean by "Event-Driven"?
  resource: 'https://martinfowler.com/articles/201701-event-driven.html'
  kind: practitioner
- title: Hohpe, G. and Woolf, B. (2003). Enterprise Integration Patterns
  resource: 'https://www.enterpriseintegrationpatterns.com/'
  kind: book
- title: CNCF. CloudEvents specification
  resource: 'https://cloudevents.io/'
  kind: standard
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 11 Stream Processing: Messaging Systems; Change Data Capture; Event Sourcing'
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 17 Your Coffee Shop Doesn''t Use Two-Phase Commit'
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'added MBA library citations; definition unchanged'
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Event-driven architecture

**Definition.** An event is an immutable fact ("OrderPlaced at 10:42"). Fowler distinguishes four patterns often lumped together: event notification, event-carried state transfer, event sourcing (storing state as the log of events) and CQRS. Brokers such as Kafka, RabbitMQ or cloud event buses deliver events; CloudEvents standardises their format.

Domain: [architecture](/domains/architecture.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Event-driven designs decouple services in time and knowledge, absorb traffic spikes and make it easy to add new consumers (analytics, notifications). The price: harder end-to-end reasoning and debugging, eventual consistency, duplicate and out-of-order delivery (consumers must be idempotent), and the need for dead-letter handling and tracing.

## Example

When an order is placed, the order service publishes OrderPlaced; inventory, billing and email services each react on their own, and adding a fraud check later needs no change to the order service.

## Narrower concepts

- [Dead-letter queue](/concepts/backend-systems/dead-letter-queue.md)
- [Change data capture](/concepts/data/change-data-capture.md)
- [Message queue and asynchronous messaging](/concepts/backend-systems/message-queue.md)
- [Saga pattern](/concepts/backend-systems/saga-pattern.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (DDIA added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 11 Stream Processing: Messaging Systems; Change Data Capture; Event Sourcing: Event streams, brokers, CDC and event sourcing as the data foundation of event-driven systems.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (Architect Elevator added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit: The coffee-shop queue as an asynchronous, throughput-optimised design: correlation of out-of-order results, competing consumers and error handling without distributed transactions.

## Sources

1. Fowler, M. (2017). *What do you mean by "Event-Driven"?*. <https://martinfowler.com/articles/201701-event-driven.html> — The four event-driven patterns and their trade-offs.
2. Hohpe, G. and Woolf, B. (2003). *Enterprise Integration Patterns*. <https://www.enterpriseintegrationpatterns.com/> — Messaging channels, publish-subscribe, event messages.
3. CNCF. *CloudEvents specification*. <https://cloudevents.io/> — Vendor-neutral standard format for events.
4. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 11 Stream Processing: Messaging Systems; Change Data Capture; Event Sourcing. Event streams, brokers, CDC and event sourcing as the data foundation of event-driven systems.
5. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit. The coffee-shop queue as an asynchronous, throughput-optimised design: correlation of out-of-order results, competing consumers and error handling without distributed transactions.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
