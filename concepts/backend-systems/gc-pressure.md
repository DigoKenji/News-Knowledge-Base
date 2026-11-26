---
type: Concept
title: GC pressure
description: The load a program puts on its garbage collector - mainly through allocation rate and the size and pointer-density of the live heap - which shows up as CPU spent collecting, longer or more frequent pauses, and higher tail latency.
tags:
- backend-systems
- performance
domain: backend-systems
subdomain: performance
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Jones, R., Hosking, A. and Moss, E. (2023). The Garbage Collection Handbook, 2nd ed. CRC Press
  resource: 'https://gchandbook.org/'
  kind: book
- title: The Go Authors. A Guide to the Go Garbage Collector
  resource: 'https://go.dev/doc/gc-guide'
  kind: docs
- title: Oracle. Java SE 21 HotSpot Virtual Machine Garbage Collection Tuning Guide
  resource: 'https://docs.oracle.com/en/java/javase/21/gctuning/'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# GC pressure

**Definition.** GC pressure is how hard an application makes the garbage collector work. High allocation rates make collections more frequent; a large live heap with many objects and references makes each collection (marking, copying) more expensive. The result is less CPU for the application and latency spikes during pauses.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `performance`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Most runtimes (JVM, .NET, Go, JavaScript) use tracing collectors whose cost is roughly proportional to live data traced plus allocation-driven frequency. Generational collectors exploit the observation that most objects die young, making short-lived garbage cheap; trouble comes from medium-lived objects promoted to the old generation, huge caches of small objects, and many pointers. Go's GC guide frames it as a trade-off controlled by GOGC: more memory means fewer collections. Remedies: allocate less (reuse buffers, object pools, value types), keep fewer, larger objects or off-heap storage for caches, tune heap size and collector (G1, ZGC, Shenandoah).

## Why it matters and trade-offs

GC pressure is a common hidden cause of p99 latency spikes and of high CPU in managed-language services. Reducing it often makes code less idiomatic (pooling, manual memory layout) and can trade GC cost for more memory use.

## Example

A Java service that caches 50 million small objects spends 30% of CPU in GC; storing the cache in a few large off-heap byte arrays drops it under 5%.

## Review notes for Kenji

The draft defined it only as "CPU cost of GC"; it now covers allocation rate, live heap and pause latency.

## Sources

1. Jones, R., Hosking, A. and Moss, E. (2023). *The Garbage Collection Handbook*, 2nd ed. CRC Press. <https://gchandbook.org/> — Tracing and generational collection theory and costs.
2. The Go Authors. *A Guide to the Go Garbage Collector*. <https://go.dev/doc/gc-guide> — GC cost model - CPU vs memory trade-off, allocation rate, GOGC.
3. Oracle. *Java SE 21 HotSpot Virtual Machine Garbage Collection Tuning Guide*. <https://docs.oracle.com/en/java/javase/21/gctuning/> — Generations, promotion, pause vs throughput goals, collector choice.

## Relations
- (none yet)

## Notes
- [Uber Cuts Translation Cache Memory by up to 90% While Keeping P99 Latency Below 1 MS](/notes/backend-systems/2026-11-21-uber-cuts-translation-cache-memory-by-up-to-90-while-keeping.md)
