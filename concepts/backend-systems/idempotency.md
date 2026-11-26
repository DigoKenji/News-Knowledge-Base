---
type: Concept
title: Idempotency
description: 'The property that repeating an operation has the same effect as doing it once, which makes retries and duplicate messages safe.'
tags:
- backend-systems
- messaging-and-workflows
domain: backend-systems
subdomain: messaging-and-workflows
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 11 Stream Processing: Idempotence (Fault Tolerance); ch. 12 Exactly-once execution of an operation'
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 17 Your Coffee Shop Doesn''t Use Two-Phase Commit (Retry)'
- title: 'IETF (2022). RFC 9110 HTTP Semantics, §9.2.2 Idempotent Methods'
  resource: 'https://www.rfc-editor.org/rfc/rfc9110#section-9.2.2'
  kind: standard
- title: 'Stripe. Idempotent requests (API reference)'
  resource: 'https://docs.stripe.com/api/idempotent_requests'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Idempotency

**Definition.** An operation is idempotent when performing it several times has the same effect as performing it once. Some operations are naturally idempotent (setting a value, deleting by id; HTTP defines PUT and DELETE as idempotent methods); others are made so by giving each request a unique idempotency key that the server stores with the result and checks before doing the work again.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `messaging-and-workflows`. Expanded on 2026-11-26 with independent sources.

## How it fits

Networks and message brokers give at-least-once delivery, so retries and duplicates are normal; idempotent receivers turn that into effectively-once processing without distributed transactions. It underpins safe retries, sagas and exactly-once stream processing.

## Example

A client sends a payment with Idempotency-Key 7f3a; the connection drops and it retries; the payment API finds the key and returns the first charge instead of charging twice.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (DDIA added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 11 Stream Processing: Idempotence (Fault Tolerance); ch. 12 Exactly-once execution of an operation: Idempotence as a cheaper alternative to distributed transactions, e.g. storing the message offset with each write.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (Architect Elevator added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Retry): Retrying a group of operations becomes simple when every receiver is idempotent and ignores duplicates.

## Sources

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 11 Stream Processing: Idempotence (Fault Tolerance); ch. 12 Exactly-once execution of an operation. Idempotence as a cheaper alternative to distributed transactions, e.g. storing the message offset with each write.
2. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Retry). Retrying a group of operations becomes simple when every receiver is idempotent and ignores duplicates.
3. IETF (2022). RFC 9110 HTTP Semantics, §9.2.2 Idempotent Methods. <https://www.rfc-editor.org/rfc/rfc9110#section-9.2.2> — Definition of idempotent HTTP methods.
4. Stripe. Idempotent requests (API reference). <https://docs.stripe.com/api/idempotent_requests> — Idempotency keys in a production payments API.

## Notes
Notes tagged with this concept are listed in the concept index after each run.
