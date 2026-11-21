---
type: Article Summary
title: Uber Cuts Translation Cache Memory by up to 90% While Keeping P99 Latency Below 1 MS
description: Uber cut its Go translation cache's initial memory over 90% by keeping keys and hot values in RAM, spilling the
  rest to disk.
resource: https://www.infoq.com/news/2026/11/uber-go-translation-cache
tags:
- backend-systems
- hybrid-memory-disk-cache
- gc-pressure
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/uber-go-translation-cache
  title: Uber Cuts Translation Cache Memory by up to 90% While Keeping P99 Latency Below 1 MS
  author: Leela Kumili
status: stable
source_url: https://www.infoq.com/news/2026/11/uber-go-translation-cache
source: InfoQ
source_type: blog
published: '2026-11-20'
domain: backend-systems
concepts:
- hybrid-memory-disk-cache
- gc-pressure
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d818d9407eb1b0a1f01ac
---

## 🎯 Core thesis
Uber cut the initial memory footprint of its Go translation cache by more than 90% by keeping translation keys and frequently accessed values in memory and storing the remaining values on disk, while keeping p99 cache-miss latency below 1 ms.

## 🧭 Why the author wrote it
Uber's Go services spent a large share of CPU on garbage collection, and translation datasets of millions of keys could push some services out of memory during cache refreshes. If the thesis holds, a hybrid memory-and-disk cache can keep the existing Get() and Put() API while cutting memory at Uber's scale.

## Opening: the problem and the redesign
Uber redesigned its Go-based translation cache. The article reports that the system serves millions of translations per second with a cache hit rate above 99.9%. The author quotes Uber Engineering: "At Uber's scale, even small inefficiencies in memory management can add up."

## Profiling: where the memory and CPU went
Memory profiling across Uber's Go services found two problems. Garbage collection consumed more than 20% of CPU utilization in some services, and translations accounted for about 70% of allocated objects.

### Garbage collection (GC) overhead
Garbage collection is the runtime work that finds and frees heap memory no longer in use. Its cost grows with the number of objects the heap holds and the pointers between them, so a cache built from millions of small objects spends CPU on collection instead of serving lookups. The article's numbers show the effect: translations were about 70% of allocated objects, and GC took over 20% of CPU in some services. The lesson is that object count drives cost, not only bytes.
> 🔎 Added context: The article does not describe Go's collector internals; the description above is general Go runtime behavior.

> ❌ **Anti-pattern — several objects per entry:** each translation entry was built from multiple objects and pointers. **Why people do it:** the article does not say; per-record object graphs are a common default. **Consequence:** GC overhead rises with entry count. **Fix:** store entries as plain values in a flat map keyed by string.

## First fix: from sync.Map to a plain map
### sync.Map versus map with RWMutex
Go's sync.Map is a concurrent-safe map type. The workload here was read-heavy, with infrequent updates and no individual deletions. Uber replaced it with a conventional map[string]string guarded by an RWMutex, which lets many readers proceed together while a writer takes exclusive access. This reduced object allocations and cut CPU by more than 30% in the affected service. The article gives no benchmark of sync.Map itself; the reported gain is the CPU change after the switch.

## Refresh spikes and rejected options
Translation datasets could contain two to three million keys, but each service typically read only a small subset. Refreshes were the next problem. The service built a replacement cache in the background and then swapped it in atomically, so both copies sat in memory at once, and some services ran out of memory during refreshes.

### Build-then-swap refresh
The pattern builds a full replacement structure, then switches readers to it in one step. The swap is atomic, but during the build both the old and new copies are resident. The article reports the result directly: some services ran out of memory on refresh.

> ❌ **Anti-pattern — full shadow copy on refresh:** the whole replacement cache is built before the swap. **Why people do it:** the article describes the swap as atomic but gives no reason. **Consequence:** peak memory is roughly double during refresh, and some services ran out of memory. **Fix:** keep only keys in memory and page values from disk, as Uber did.

Uber also considered external caching and generating service-specific translation subsets. Both added operational or maintenance costs, so the team rejected them.

## Hybrid design: keys in memory, values on disk
The team adopted a hybrid storage design that keeps the existing Get() and Put() API. All keys stay in memory for fast lookups, and translation values live in disk-backed files. The article includes a diagram of the cache, which is not in the extracted text.

### Hybrid memory and disk storage
Lookups never touch disk for the key, so a miss is the only path that reads a value from disk. Because the key index is small compared with the values, memory stays low even as the dataset grows. Frequently accessed values are kept in memory through an LRU policy. The trade is an occasional disk read on a cold value in exchange for a much smaller heap. The article reports that disk-backed misses keep p99 latency below 100 microseconds.

### Sharding
The cache uses 256 shards, each with its own read-write mutex, its own in-memory entries and its own disk file. A lock is therefore shared only within a shard, not across the whole cache. The article does not report a contention measurement.

### Six-byte hashed keys and LRU eviction
Six-byte hash-based keys reduce metadata overhead, and the system falls back to the original key format to handle collisions. A time-based global least recently used (LRU) policy manages the in-memory entries.
> 🔎 Added context: LRU evicts the entries that have gone longest without access, so the hot set stays in RAM.

### Warm-up preload after refresh
During a refresh, the system uses access history to preload translations accessed in the previous 15 minutes. This avoids a cold cache right after the swap.

## Reported results
In one example, a translation dataset that used about 800 MB in memory needed about 40 MB for metadata after the redesign. The configured limit was 50,000 in-memory entries, and actual memory use typically ranged from 80 MB to 120 MB. In another workload, about 30,000 of more than four million entries were in memory, and the hit rate stayed above 99.9%. The article also reports memory falling from about 2.7 GB to 750 MB. That is about a 72% cut, below the 90% headline, and the article does not say which workload it measures or how the 90% is calculated.

## Lessons and recommendations
The team recommends minimizing object references to reduce GC overhead, and measuring how often cached values are accessed. When only a small share of cached data is active, a hybrid approach keeps the hot values in memory and moves the rest to disk.

## 🧠 Mental models
### Keys in memory, values on disk
**The model:** Index data is small and needed on every lookup, while value data is large and mostly cold, so only the hot values need RAM.
**How to apply:** Measure what fraction of cached values is actually read. If it is small, keep the index in memory and read the values from disk.

### Fewer objects, less GC
**The model:** GC cost tracks the number of heap objects and references, not only the bytes stored.
**How to apply:** Store entries as plain values in flat maps, and count allocations in a profile before tuning anything else.

### Small inefficiencies at scale
**The model:** A cost too small to notice in one process becomes large when multiplied across a fleet, as the author's Uber Engineering quote suggests.
**How to apply:** Profile the hot path in the services with the largest footprint first.

## 🔑 Key takeaways
- Profiling showed GC above 20% of CPU in some services, with translations about 70% of allocated objects.
- Replacing sync.Map with map[string]string under an RWMutex cut CPU by more than 30% in the affected service.
- Build-then-swap refreshes held both copies in memory, and some services ran out of memory during refreshes.
- The hybrid design keeps all keys and frequently accessed values in memory across 256 shards, with the rest in disk-backed files.
- One dataset dropped from about 800 MB to about 40 MB of metadata, with actual memory of 80 to 120 MB at a 50,000-entry limit.
- Refreshes preload translations accessed in the previous 15 minutes, which avoids a cold cache after the swap.
- One reported workload fell from about 2.7 GB to 750 MB, a cut of about 72% rather than the 90% headline.

## 🏛️ For Kenji
This is your interpretation, not the author's. For an architect, memory layout and GC cost are capacity decisions, so they belong in design review next to the storage choice. The hybrid design also trades an occasional disk read on a cold miss for lower memory, which is a latency-budget decision. The question it raises: which of our read-heavy datasets have a small hot set that would justify keys in memory and values on disk?

## 📚 Reading plan
### From the article
- Uber Blog Post (cited twice as the source of the cache design and the memory figures) — the original write-up with the diagram and details. The extracted text has no URL for it, so none is given here.

### To go further
- Go sync.Map and its trade-offs versus a plain map with a mutex — to see when the first fix applies.
- Go garbage collector pacing and allocation cost — to see how object count drives GC CPU.
- LRU eviction policies, time-based versus size-based — to choose the policy for in-memory entries.
- Cache warm-up after atomic swaps — to avoid cold caches on refresh.
- Hybrid memory and disk caches — related designs that keep an index in RAM.

## Concepts
- [hybrid-memory-disk-cache](/concepts/hybrid-memory-disk-cache.md)
- [gc-pressure](/concepts/gc-pressure.md)
