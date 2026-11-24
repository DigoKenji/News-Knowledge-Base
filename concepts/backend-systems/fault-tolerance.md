---
type: Concept
title: Fault tolerance and redundancy
description: The ability of a system to keep delivering correct service when some of its components fail, usually achieved by redundancy (spare copies of components or data), error detection and recovery.
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
- title: Avizienis, A., Laprie, J.-C., Randell, B. and Landwehr, C. (2004). Basic Concepts and Taxonomy of Dependable and Secure Computing. IEEE Transactions on Dependable and Secure Computing 1(1)
  resource: 'https://doi.org/10.1109/TDSC.2004.2'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 1 "Reliability" and ch. 8
  resource: 'https://dataintensive.net/'
  kind: book
- title: Brooker, M. Timeouts, retries, and backoff with jitter. Amazon Builders' Library
  resource: 'https://builder.aws.com/content/3EumjoZascWd1oZiEgL8ORlv3qE/timeouts-retries-and-backoff-with-jitter'
  kind: blog
---

# Fault tolerance and redundancy

**Definition.** Avizienis, Laprie, Randell and Landwehr give the standard chain: a fault (a defect or cause) can activate into an error (a wrong internal state), which can propagate into a failure (service deviates from what is required). Fault tolerance is the set of means that avoid service failure in the presence of faults: error detection, recovery (rollback, rollforward, failover) and fault masking through redundancy.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Redundancy only helps when copies fail independently: correlated failures (same bug, same region, same bad config) defeat it. Practical techniques include replicas across zones, retries with backoff and idempotency, timeouts, circuit breakers and graceful degradation. The vocabulary of fault, error and failure keeps incident analysis precise.

## Example

A database runs a primary and two replicas in three availability zones. When the primary's zone loses power, a replica is promoted automatically within 30 seconds; clients retry their failed writes with the same idempotency key, so no order is lost or duplicated.

## Narrower concepts

- [Cascading failure](/concepts/backend-systems/cascading-failure.md)
- [Silent failure](/concepts/backend-systems/silent-failure.md)
- [Correlated failure](/concepts/backend-systems/correlated-failure.md)
- [Retry storm](/concepts/backend-systems/retry-storm.md)
- [Chaos engineering](/concepts/backend-systems/chaos-engineering.md)
- [Idempotency](/concepts/backend-systems/idempotency.md)

## Sources

1. Avizienis, A., Laprie, J.-C., Randell, B. and Landwehr, C. (2004). *Basic Concepts and Taxonomy of Dependable and Secure Computing*. IEEE Transactions on Dependable and Secure Computing 1(1). <https://doi.org/10.1109/TDSC.2004.2> — Fault-error-failure chain and definition of fault tolerance.
2. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 1 "Reliability" and ch. 8. <https://dataintensive.net/> — Faults vs failures, hardware/software/human faults in practice.
3. Brooker, M. *Timeouts, retries, and backoff with jitter*. Amazon Builders' Library. <https://builder.aws.com/content/3EumjoZascWd1oZiEgL8ORlv3qE/timeouts-retries-and-backoff-with-jitter> — Practitioner techniques for tolerating faults without amplifying them.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
