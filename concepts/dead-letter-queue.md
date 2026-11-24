---
type: Concept
title: Dead-letter queue
description: A separate queue where a messaging system puts messages it could not deliver or process after a set number of attempts, so they stop blocking or looping in the main queue and can be inspected, fixed and replayed.
tags:
- backend-systems
- microservices
domain: backend-systems
subdomain: microservices
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Hohpe, G. and Woolf, B. (2003). Enterprise Integration Patterns - Dead Letter Channel. Addison-Wesley
  resource: 'https://www.enterpriseintegrationpatterns.com/patterns/messaging/DeadLetterChannel.html'
  kind: book
- title: AWS. Using dead-letter queues in Amazon SQS
  resource: 'https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html'
  kind: docs
- title: RabbitMQ. Dead Letter Exchanges
  resource: 'https://www.rabbitmq.com/docs/dlx'
  kind: docs
---

# Dead-letter queue

**Definition.** A dead-letter queue (DLQ), or dead letter channel in Enterprise Integration Patterns, isolates "poison" messages: ones that are malformed, expired, rejected, or that keep failing in the consumer. Moving them aside keeps the main flow healthy and preserves the failed messages instead of dropping them.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `microservices`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

In Amazon SQS, a redrive policy moves a message to the DLQ after maxReceiveCount failed receives; a redrive later moves it back. RabbitMQ uses dead-letter exchanges for rejected, expired or overflowed messages. Kafka has no built-in DLQ; frameworks such as Kafka Connect and Spring Kafka write failed records to a separate topic. Good practice: keep the original payload plus error metadata (exception, attempt count, timestamps), alarm on DLQ depth, set retention longer than the main queue, and make reprocessing idempotent.

## Why it matters and trade-offs

Without a DLQ, a single bad message can block an ordered queue forever or burn resources retrying. With one, you trade that for operational work: someone must watch the DLQ, and messages sitting there are silently not processed. A DLQ nobody monitors becomes a silent failure.

## Example

An order event with a missing customer ID fails validation three times and goes to the DLQ; an alarm fires, the team fixes the producer bug, patches the event and redrives it.

## Sources

1. Hohpe, G. and Woolf, B. (2003). *Enterprise Integration Patterns* - Dead Letter Channel. Addison-Wesley. <https://www.enterpriseintegrationpatterns.com/patterns/messaging/DeadLetterChannel.html> — The pattern definition.
2. AWS. *Using dead-letter queues in Amazon SQS*. <https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html> — maxReceiveCount, redrive policy, retention guidance.
3. RabbitMQ. *Dead Letter Exchanges*. <https://www.rabbitmq.com/docs/dlx> — Independent broker - rejection, expiry and length-limit triggers.

## Relations
- (none yet)

## Notes
- [How Country Road Group built a pattern-driven integration solution on AWS](/notes/architecture/2026-11-20-how-country-road-group-built-a-pattern-driven-integration-so.md)
