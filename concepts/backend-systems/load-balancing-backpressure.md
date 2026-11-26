---
type: Concept
title: Load balancing and backpressure
description: Load balancing spreads incoming requests across many servers; backpressure lets an overloaded component signal upstream to slow down, so excess load is rejected or delayed early instead of causing collapse.
tags:
- backend-systems
- reliability
- core
domain: backend-systems
subdomain: reliability
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 20 "Load Balancing in the Datacenter" and ch. 21 "Handling Overload"
  resource: 'https://sre.google/sre-book/load-balancing-datacenter/'
  kind: book
- title: Reactive Streams. Reactive Streams Specification for the JVM (1.0.4)
  resource: 'https://www.reactive-streams.org/'
  kind: standard
- title: Yanacek, D. Using load shedding to avoid overload. Amazon Builders' Library
  resource: 'https://builder.aws.com/content/3Eun1EEyX6p2e3VYNyRLSJzLuMV/using-load-shedding-to-avoid-overload'
  kind: blog
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Load balancing and backpressure

**Definition.** A load balancer (DNS, layer-4 or layer-7 proxy, or client-side library) chooses a backend for each request using policies such as round robin, least connections or weighted choice, and removes unhealthy backends using health checks. Backpressure is flow control in the other direction: a consumer that cannot keep up bounds its queue and makes producers wait or receive an error (load shedding). The Reactive Streams specification formalises demand-based backpressure for asynchronous streams.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `reliability`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Together they keep a system near its useful capacity: balancing uses all servers evenly, and backpressure plus load shedding stops unbounded queues from turning a spike into a cascading failure. Google SRE stresses that a server should reject cheaply what it cannot serve, and clients should limit retries.

## Example

An API gateway sends traffic to 10 instances using least-outstanding-requests. During a spike, each instance caps its in-flight requests at 200 and returns HTTP 503 with Retry-After beyond that, so latency for accepted requests stays at 80 ms instead of every request timing out.

## Narrower concepts

- [Adaptive concurrency limit](/concepts/backend-systems/adaptive-concurrency-limit.md)
- [Retry storm](/concepts/backend-systems/retry-storm.md)
- [Cascading failure](/concepts/backend-systems/cascading-failure.md)
- [Goodput](/concepts/ai-ml/goodput.md)

## Sources

1. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 20 "Load Balancing in the Datacenter" and ch. 21 "Handling Overload". <https://sre.google/sre-book/load-balancing-datacenter/> — Load-balancing policies, subsetting, load shedding and client-side throttling.
2. Reactive Streams. *Reactive Streams Specification for the JVM* (1.0.4). <https://www.reactive-streams.org/> — Formal definition of non-blocking backpressure via demand signalling.
3. Yanacek, D. *Using load shedding to avoid overload*. Amazon Builders' Library. <https://builder.aws.com/content/3Eun1EEyX6p2e3VYNyRLSJzLuMV/using-load-shedding-to-avoid-overload> — Practitioner account of load shedding and its relation to goodput.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
