---
type: Concept
title: Scalability (vertical and horizontal)
description: A system's ability to handle growing load by adding resources - either a bigger machine (vertical, scale up) or more machines (horizontal, scale out) - while keeping performance acceptable and cost proportionate.
tags:
- backend-systems
- performance
- core
domain: backend-systems
subdomain: performance
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Bondi, A. B. (2000). Characteristics of Scalability and Their Impact on Performance. Proceedings of WOSP 2000 (ACM)
  resource: 'https://doi.org/10.1145/350391.350432'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 1 "Scalability"
  resource: 'https://dataintensive.net/'
  kind: book
- title: Amazon Web Services. Reliability Pillar - AWS Well-Architected Framework ("Use horizontal scaling")
  resource: 'https://docs.aws.amazon.com/wellarchitected/latest/framework/reliability.html'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Scalability (vertical and horizontal)

**Definition.** Bondi characterises scalability as the ability of a system to accommodate an increasing number of elements or load, process growing volumes of work gracefully, and be susceptible to enlargement. Vertical scaling adds CPU, memory or disk to one machine; horizontal scaling adds machines and splits work among them, which requires stateless services or partitioned data.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `performance`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Kleppmann's advice is to first describe the load (requests per second, read/write ratio, fan-out) and then ask what happens to performance when it grows. Vertical scaling is simple but has a ceiling and a single point of failure; horizontal scaling has no fixed ceiling but needs load balancing, partitioning and dealing with distributed-system problems. Elastic cloud capacity made horizontal scaling the default.

## Example

A web API at 500 requests/s moves from a 4-core to a 32-core server (vertical). At 20,000 requests/s it runs 40 identical stateless instances behind a load balancer and partitions its database by customer id (horizontal).

## Narrower concepts

- [Replication and partitioning (sharding)](/concepts/backend-systems/replication-partitioning.md)
- [Load balancing and backpressure](/concepts/backend-systems/load-balancing-backpressure.md)
- [Caching](/concepts/backend-systems/caching.md)
- [Adaptive concurrency limit](/concepts/backend-systems/adaptive-concurrency-limit.md)

## Sources

1. Bondi, A. B. (2000). *Characteristics of Scalability and Their Impact on Performance*. Proceedings of WOSP 2000 (ACM). <https://doi.org/10.1145/350391.350432> — Definition and types of scalability (load, space, structural).
2. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 1 "Scalability". <https://dataintensive.net/> — Describing load, scaling up vs scaling out.
3. Amazon Web Services. *Reliability Pillar - AWS Well-Architected Framework* ("Use horizontal scaling"). <https://docs.aws.amazon.com/wellarchitected/latest/framework/reliability.html> — Practitioner guidance to scale horizontally to avoid single points of failure.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
