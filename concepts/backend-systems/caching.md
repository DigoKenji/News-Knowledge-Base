---
type: Concept
title: Caching
description: Keeping a copy of data in a faster or closer store so repeated reads avoid the slow original source - trading memory and the risk of serving stale data for lower latency and less load.
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
- title: Fielding, R., Nottingham, M. and Reschke, J. (eds.) (2022). RFC 9111 - HTTP Caching. IETF
  resource: 'https://datatracker.ietf.org/doc/html/rfc9111'
  kind: standard
- title: Nishtala, R. et al. (2013). Scaling Memcache at Facebook. NSDI 2013
  resource: 'https://www.usenix.org/conference/nsdi13/technical-sessions/presentation/nishtala'
  kind: paper
- title: Hennessy, J. L. and Patterson, D. A. (2017). Computer Architecture - A Quantitative Approach, 6th edition, ch. 2 "Memory Hierarchy Design"
  resource: 'https://shop.elsevier.com/books/computer-architecture/hennessy/978-0-12-811905-1'
  kind: book
---

# Caching

**Definition.** A cache stores the results of earlier reads or computations so later requests can be served from it. It works because access patterns show locality: recently or frequently used data is likely to be used again. Key design choices are what to cache, where (CPU, process memory, a shared cache such as Redis or memcached, a CDN, the browser), how to evict (LRU, LFU, TTL) and how to keep it fresh (expiry, invalidation, write-through or write-back). HTTP caching is standardised in RFC 9111.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Caching appears at every layer, from CPU caches to LLM KV caches and CDNs. Its classic difficulties are invalidation (knowing when the copy is stale), thundering herds when a hot entry expires, and cold starts after a restart. A cache that the system cannot survive without has become a dependency, not an optimisation.

## Example

A product page reads prices from a database in 20 ms. The service caches each product in memcached for 60 seconds; 95% of reads now take 1 ms, and when a price changes the update also deletes the cached entry.

## Narrower concepts

- [Hybrid memory-disk cache](/concepts/backend-systems/hybrid-memory-disk-cache.md)
- [KV cache](/concepts/ai-ml/kv-cache.md)

## Sources

1. Fielding, R., Nottingham, M. and Reschke, J. (eds.) (2022). *RFC 9111 - HTTP Caching*. IETF. <https://datatracker.ietf.org/doc/html/rfc9111> — Standard model of freshness, validation and cache control for HTTP.
2. Nishtala, R. et al. (2013). *Scaling Memcache at Facebook*. NSDI 2013. <https://www.usenix.org/conference/nsdi13/technical-sessions/presentation/nishtala> — Look-aside caching at scale, invalidation and thundering-herd mitigation.
3. Hennessy, J. L. and Patterson, D. A. (2017). *Computer Architecture - A Quantitative Approach*, 6th edition, ch. 2 "Memory Hierarchy Design". <https://shop.elsevier.com/books/computer-architecture/hennessy/978-0-12-811905-1> — Locality principle and cache fundamentals.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
