---
type: Concept
title: 'Event sourcing'
description: 'Storing an application''s state as an append-only log of immutable domain events ("order placed", "item shipped") and deriving current state by replaying them, instead of overwriting records in place.'
tags:
- backend-systems
- messaging-and-workflows
domain: backend-systems
subdomain: messaging-and-workflows
also_in:
- architecture/software-architecture
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 11 Stream Processing: Change Data Capture; Event Sourcing; State, Streams, and Immutability; ch. 12 The Future of Data Systems'
- title: 'Fowler, M. (2005). Event Sourcing'
  resource: 'https://martinfowler.com/eaaDev/EventSourcing.html'
  kind: practitioner
- title: 'Microsoft. Event Sourcing pattern (Azure Architecture Center)'
  resource: 'https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Event sourcing

**Definition.** Kleppmann places event sourcing next to change data capture: both store all changes to application state as a log of events, but at different levels. In CDC the application updates a database freely and the change log is extracted underneath; in event sourcing the application is explicitly built on immutable events written to an append-only event store, where updates and deletes are discouraged or prohibited, and each event records something that happened at the application level rather than a low-level state change. Current state is a derived view, computed by replaying events (often with snapshots for speed). The technique came from the domain-driven design community and is usually paired with CQRS, which separates the write model (events) from read models.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `messaging-and-workflows`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-26); draft waiting for Kenji to certify.

## How it fits

Event sourcing gives a full audit trail, makes it possible to rebuild or add new read models later, and fits event-driven architecture naturally. The costs are real: events must stay readable as schemas evolve, deleting personal data needs special handling, and querying current state needs projections. Kleppmann's wider point (ch. 11-12) is that a log of immutable events can be the system of record from which databases, caches and indexes are derived.

## Example

A shopping cart is stored as events (item added, item removed, checkout started). A bug in the price calculation is fixed and the cart totals are recomputed by replaying the events, with nothing lost.

## Broader concepts

- [Event-driven architecture](/concepts/architecture/event-driven-architecture.md)

## Related concepts

- [Change data capture](/concepts/data/change-data-capture.md)
- [Schema evolution and data compatibility](/concepts/data/schema-evolution.md)
- [Domain-driven design (DDD)](/concepts/architecture/domain-driven-design.md)

## Sources

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 11 Stream Processing: Change Data Capture; Event Sourcing; State, Streams, and Immutability; ch. 12 The Future of Data Systems. Event sourcing compared with CDC, and the log as the source of derived data.
2. Fowler, M. (2005). *Event Sourcing*. <https://martinfowler.com/eaaDev/EventSourcing.html> — Fowler's original description of the pattern.
3. Microsoft. Event Sourcing pattern (Azure Architecture Center). <https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing> — Microsoft Azure Architecture Center pattern page, with trade-offs.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
