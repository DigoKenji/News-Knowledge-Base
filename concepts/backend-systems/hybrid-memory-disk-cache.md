---
type: Concept
title: Hybrid memory-disk cache
description: A cache that keeps its index and hottest items in RAM and the larger, colder part on SSD, so it holds far more data per server than memory alone at a cost of slower reads for items that live on disk.
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
- title: Berg, B. et al. (2020). The CacheLib Caching Engine - Design and Experiences at Scale. OSDI 2020
  resource: 'https://www.usenix.org/conference/osdi20/presentation/berg'
  kind: paper
- title: McAllister, S. et al. (2021). Kangaroo - Caching Billions of Tiny Objects on Flash. SOSP 2021
  resource: 'https://dl.acm.org/doi/10.1145/3477132.3483568'
  kind: paper
- title: Memcached. Extstore (wiki)
  resource: 'https://github.com/memcached/memcached/wiki/Extstore'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Hybrid memory-disk cache

**Definition.** A hybrid (tiered) cache layers DRAM over flash. Lookups check memory first; misses in memory may still hit on SSD, which is much cheaper per gigabyte and much faster than going to the backing database. Items are promoted, demoted or evicted between tiers by admission and eviction policies.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `performance`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

The key design problem is metadata: keeping a full in-memory index for billions of small objects costs too much RAM, and naive small writes wear out flash. Meta's CacheLib uses separate engines for large objects (log-structured regions) and small objects (set-associative buckets), with admission policies that skip writing one-hit items to flash. Kangaroo combined a small log with a large set-associative cache to cut both DRAM and write amplification. Memcached's extstore keeps keys and metadata in RAM and moves values to disk. Aerospike's hybrid memory architecture keeps the primary index in RAM and records on SSD.

## Why it matters and trade-offs

Much higher hit rates and capacity per dollar than RAM-only caches, which can mean fewer servers and less database load. Costs: higher and more variable latency for SSD hits, flash endurance limits, more complex tuning (admission, segment sizes), and slower warm-up after restarts unless the SSD contents survive.

## Example

A server with 64 GB RAM and 2 TB NVMe caches 30× more items than RAM alone; 80% of hits are served from RAM in microseconds, the rest from SSD in about 100 microseconds, still far faster than a 5 ms database query.

## Sources

1. Berg, B. et al. (2020). *The CacheLib Caching Engine - Design and Experiences at Scale*. OSDI 2020. <https://www.usenix.org/conference/osdi20/presentation/berg> — DRAM plus flash hybrid caching at Meta, small vs large object engines, admission policies.
2. McAllister, S. et al. (2021). *Kangaroo - Caching Billions of Tiny Objects on Flash*. SOSP 2021. <https://dl.acm.org/doi/10.1145/3477132.3483568> — Independent academic design reducing DRAM metadata and flash writes.
3. Memcached. *Extstore* (wiki). <https://github.com/memcached/memcached/wiki/Extstore> — Keys in RAM, values on disk in a widely used open-source cache.

## Relations
- (none yet)

## Notes
- [Uber Cuts Translation Cache Memory by up to 90% While Keeping P99 Latency Below 1 MS](/notes/backend-systems/2026-11-21-uber-cuts-translation-cache-memory-by-up-to-90-while-keeping.md)
