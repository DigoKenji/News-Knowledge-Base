# Concepts — Backend & Systems Engineering

Grouped by subdomain, following the taxonomy on the [domain page](/domains/backend-systems.md) (Kleppmann, Designing Data-Intensive Applications (2017) and the SWEBOK v4 Software Design area). **core** marks core vocabulary; ↗ marks a concept whose home is another theme.

## Distributed systems

- [Consensus](/concepts/backend-systems/consensus.md) — core · **draft**
- [Consistency models](/concepts/backend-systems/consistency-models.md) — core · **draft**
- [Distributed system](/concepts/backend-systems/distributed-system.md) — core · **draft**
- [Replication and partitioning (sharding)](/concepts/backend-systems/replication-partitioning.md) — core · **draft**
- [CAP theorem](/concepts/backend-systems/cap-theorem.md)
- [Deterministic simulation testing](/concepts/backend-systems/deterministic-simulation-testing.md)
- [Linearizability](/concepts/backend-systems/linearizability.md)
- ↗ [Control plane](/concepts/cloud-infra/control-plane.md) — home: Cloud & Infrastructure

## Reliability and resilience

- [Availability and reliability](/concepts/backend-systems/availability-reliability.md) — core · **draft**
- [Fault tolerance and redundancy](/concepts/backend-systems/fault-tolerance.md) — core · **draft**
- [Load balancing and backpressure](/concepts/backend-systems/load-balancing-backpressure.md) — core · **draft**
- [Adaptive concurrency limit](/concepts/backend-systems/adaptive-concurrency-limit.md)
- [Cascading failure](/concepts/backend-systems/cascading-failure.md)
- [Chaos engineering](/concepts/backend-systems/chaos-engineering.md)
- [Correlated failure](/concepts/backend-systems/correlated-failure.md)
- [Retry storm](/concepts/backend-systems/retry-storm.md)
- [Silent failure](/concepts/backend-systems/silent-failure.md)
- [Circuit breaker](/concepts/backend-systems/circuit-breaker.md) — **draft**
- ↗ [Observability (logs, metrics, traces)](/concepts/cloud-infra/observability.md) — home: Cloud & Infrastructure
- ↗ [SLO error budget](/concepts/cloud-infra/slo-error-budget.md) — home: Cloud & Infrastructure

## Performance and scalability

- [Caching](/concepts/backend-systems/caching.md) — core · **draft**
- [Latency, throughput and tail latency](/concepts/backend-systems/latency-throughput.md) — core · **draft**
- [Scalability (vertical and horizontal)](/concepts/backend-systems/scalability.md) — core · **draft**
- [GC pressure](/concepts/backend-systems/gc-pressure.md)
- [Hybrid memory-disk cache](/concepts/backend-systems/hybrid-memory-disk-cache.md)

## Messaging and workflows

- [Message queue and asynchronous messaging](/concepts/backend-systems/message-queue.md) — core · **draft**
- [Dead-letter queue](/concepts/backend-systems/dead-letter-queue.md)
- [Durable Execution](/concepts/backend-systems/durable-execution.md)
- [Event sourcing](/concepts/backend-systems/event-sourcing.md) — **draft**
- [Idempotency](/concepts/backend-systems/idempotency.md)
- [Saga pattern](/concepts/backend-systems/saga-pattern.md)
- [Visibility timeout](/concepts/backend-systems/visibility-timeout.md) — **draft**
- ↗ [Change data capture](/concepts/data/change-data-capture.md) — home: Data & Data Science
- ↗ [Event-driven architecture](/concepts/architecture/event-driven-architecture.md) — home: Architecture

## APIs and protocols

- [API versioning](/concepts/backend-systems/api-versioning.md)
- [gRPC](/concepts/backend-systems/grpc.md)
- ↗ [Schema evolution and data compatibility](/concepts/data/schema-evolution.md) — home: Data & Data Science

## Web platform

- [Reverse Proxy](/concepts/backend-systems/reverse-proxy.md)
