---
type: Concept
title: Adaptive concurrency limit
description: A limit on how many requests a service handles at once that adjusts itself from observed latency or errors - shrinking quickly when the service shows congestion and growing back cautiously when it recovers - instead of a fixed, hand-tuned number.
tags:
- backend-systems
- reliability
domain: backend-systems
subdomain: reliability
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Jacobson, V. (1988). Congestion Avoidance and Control. SIGCOMM 1988
  resource: 'https://ee.lbl.gov/papers/congavoid.pdf'
  kind: paper
- title: Netflix (2018). Performance Under Load. Netflix Technology Blog
  resource: 'https://netflixtechblog.medium.com/performance-under-load-3e6fa9a60581'
  kind: practitioner
- title: Envoy Proxy. Adaptive Concurrency filter
  resource: 'https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/adaptive_concurrency_filter'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Adaptive concurrency limit

**Definition.** An adaptive concurrency limit applies TCP congestion-control ideas to services. The server (or client) keeps a limit on in-flight requests; excess requests are rejected fast or queued briefly. The limit is continuously re-estimated from signals such as round-trip latency compared to its no-load minimum, or from timeouts and drops.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `reliability`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

By Little's law, concurrency = throughput × latency. When a service is past its capacity, latency rises before throughput falls, so rising latency is an early congestion signal. Algorithms borrowed from networking: AIMD (additive increase, multiplicative decrease, from Jacobson's 1988 TCP work) and delay-based schemes in the style of TCP Vegas or Google's Gradient approach, which compare current latency to the minimum seen. Netflix's concurrency-limits library and Envoy's adaptive concurrency filter implement these; requests over the limit get an immediate 429/503 so clients can back off or go elsewhere.

## Why it matters and trade-offs

Fixed limits are wrong most of the time: too high and the service overloads, too low and capacity is wasted, and the right value moves with deploys, hardware and traffic mix. Adaptive limits keep latency bounded and protect against cascading failure without manual tuning. They can oscillate, can misread latency caused by a slow downstream as local overload, and need sensible minimums so they do not throttle to zero.

## Example

A service normally answers in 20 ms with 50 requests in flight. When latency climbs to 60 ms, the limiter cuts the limit to 35 and sheds the rest with 503s until latency returns near 20 ms, then raises it one step at a time.

## Sources

1. Jacobson, V. (1988). *Congestion Avoidance and Control*. SIGCOMM 1988. <https://ee.lbl.gov/papers/congavoid.pdf> — AIMD congestion control, the origin of the technique.
2. Netflix (2018). *Performance Under Load*. Netflix Technology Blog. <https://netflixtechblog.medium.com/performance-under-load-3e6fa9a60581> — Applying TCP congestion ideas to service concurrency; latency-gradient limits.
3. Envoy Proxy. *Adaptive Concurrency filter*. <https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/adaptive_concurrency_filter> — Independent implementation using a gradient controller over sampled minimum latency.

## Relations
- (none yet)

## Notes
- [How we raised Databricks cluster rate limits by 7.5×](/notes/backend-systems/2026-11-18-how-we-raised-databricks-cluster-rate-limits-by-7-5.md)
