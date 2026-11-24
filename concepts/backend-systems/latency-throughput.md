---
type: Concept
title: Latency, throughput and tail latency
description: Latency is how long one request takes; throughput is how many requests a system completes per unit of time; tail latency is the slow end of the latency distribution (p99, p99.9), which users of large systems hit far more often than averages suggest.
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
- title: Dean, J. and Barroso, L. A. (2013). The Tail at Scale. Communications of the ACM 56(2)
  resource: 'https://doi.org/10.1145/2408776.2408794'
  kind: paper
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 6 "Monitoring Distributed Systems" (the four golden signals)
  resource: 'https://sre.google/sre-book/monitoring-distributed-systems/'
  kind: book
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 1 "Describing Performance"
  resource: 'https://dataintensive.net/'
  kind: book
---

# Latency, throughput and tail latency

**Definition.** Latency is the time between sending a request and receiving its response; throughput is the rate of completed work (requests per second, tokens per second). Because latency varies, it is described by percentiles: p50 is the median, p99 is the value 99% of requests beat. Dean and Barroso showed that when one user request fans out to many servers, the slowest server decides the response time, so rare slowness becomes common at scale.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

These are the basic performance vocabulary for every backend and for LLM serving. They trade off: batching and queueing raise throughput but add latency; near full utilisation, queueing theory says latency rises sharply. SLOs are usually written on tail latency, not averages, because averages hide the users having a bad time.

## Example

A search page calls 100 backend shards. Each shard answers in 10 ms 99% of the time but takes 1 s for 1% of calls. The chance that at least one of the 100 is slow is about 63%, so most page loads take a second even though each shard looks fast "on average".

## Narrower concepts

- [Goodput](/concepts/ai-ml/goodput.md)
- [GC pressure](/concepts/backend-systems/gc-pressure.md)
- [KV cache](/concepts/ai-ml/kv-cache.md)

## Sources

1. Dean, J. and Barroso, L. A. (2013). *The Tail at Scale*. Communications of the ACM 56(2). <https://doi.org/10.1145/2408776.2408794> — Tail latency, fan-out amplification and tail-tolerant techniques.
2. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 6 "Monitoring Distributed Systems" (the four golden signals). <https://sre.google/sre-book/monitoring-distributed-systems/> — Latency and traffic as golden signals; why percentiles beat averages.
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 1 "Describing Performance". <https://dataintensive.net/> — Latency vs response time, percentiles and throughput definitions.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
