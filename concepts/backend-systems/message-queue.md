---
type: Concept
title: Message queue and asynchronous messaging
description: A component that lets services communicate by sending messages to a queue or topic instead of calling each other directly, so senders and receivers are decoupled in time, rate and location.
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
- title: Hohpe, G. and Woolf, B. (2003). Enterprise Integration Patterns (Messaging patterns catalogue)
  resource: 'https://www.enterpriseintegrationpatterns.com/patterns/messaging/'
  kind: book
- title: Apache Software Foundation. Apache Kafka documentation - Introduction and Design
  resource: 'https://kafka.apache.org/documentation/'
  kind: docs
- title: Amazon Web Services. Amazon SQS Developer Guide - What is Amazon Simple Queue Service?
  resource: 'https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html'
  kind: docs
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 11 Stream Processing: Messaging Systems (message brokers vs log-based brokers); Transmitting Event Streams'
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Message queue and asynchronous messaging

**Definition.** Hohpe and Woolf describe messaging as asynchronous communication through channels: a point-to-point queue delivers each message to one consumer (work distribution), while a publish-subscribe topic delivers it to every subscriber. Brokers such as RabbitMQ or Amazon SQS hold messages until they are consumed; log-based systems such as Apache Kafka keep an ordered, replayable log per partition. Delivery is typically at-least-once, so consumers must be idempotent.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Queues absorb bursts, let slow work happen in the background and keep a failure in one service from immediately failing its callers. The costs are eventual consistency, harder debugging across hops, poison messages (handled by dead-letter queues) and the need to monitor queue depth as a sign of trouble.

## Example

When a user uploads a video, the API stores it and publishes a "video.uploaded" message. A transcoding worker pool consumes from the queue at its own pace; during a launch-day spike the queue grows to 50,000 messages and drains over the next hour instead of crashing the API.

## Narrower concepts

- [Dead-letter queue](/concepts/backend-systems/dead-letter-queue.md)
- [Retry storm](/concepts/backend-systems/retry-storm.md)
- [Change data capture](/concepts/data/change-data-capture.md)
- [Saga pattern](/concepts/backend-systems/saga-pattern.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (DDIA added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 11 Stream Processing: Messaging Systems (message brokers vs log-based brokers); Transmitting Event Streams: AMQP-style brokers versus log-based brokers such as Kafka, and what each guarantees.

## Sources

1. Hohpe, G. and Woolf, B. (2003). *Enterprise Integration Patterns* (Messaging patterns catalogue). <https://www.enterpriseintegrationpatterns.com/patterns/messaging/> — Message channels, point-to-point vs publish-subscribe, messaging patterns.
2. Apache Software Foundation. *Apache Kafka documentation - Introduction and Design*. <https://kafka.apache.org/documentation/> — Log-based messaging, partitions, consumer groups and delivery semantics.
3. Amazon Web Services. *Amazon SQS Developer Guide - What is Amazon Simple Queue Service?*. <https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html> — Managed queue semantics, at-least-once delivery and dead-letter queues.
4. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 11 Stream Processing: Messaging Systems (message brokers vs log-based brokers); Transmitting Event Streams. AMQP-style brokers versus log-based brokers such as Kafka, and what each guarantees.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
