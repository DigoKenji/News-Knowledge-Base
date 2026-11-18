---
type: Article Summary
title: Built on object storage, served from cache
description: Lakebase Postgres serves 99.99999% of page reads from local caches, so object storage stays off the query path.
resource: https://www.databricks.com/blog/built-object-storage-served-cache
tags:
- data
- databases
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://www.databricks.com/blog/built-object-storage-served-cache
  title: Built on object storage, served from cache
  author: Databricks
status: stable
source_url: https://www.databricks.com/blog/built-object-storage-served-cache
source: Databricks
source_type: blog
published: '2026-11-17'
domain: data
subdomain: databases
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d81bba857cafc95625f28
---

## 🎯 Core thesis
A database built on object storage does not have to read from object storage to answer queries. In Lakebase Postgres, 99.99999% of page accesses are served from local caches on the compute node or the pageservers.

## 🧭 Why the author wrote it
The common objection is that Postgres issues many small, latency-sensitive reads, object storage takes hundreds of milliseconds per read, and so a Postgres built on object storage must be slow. The article accepts the premise and rejects the conclusion, which only follows if every read goes to object storage. If the thesis holds, object storage can supply durability, bottomless storage, instant branches and restores, and one copy of data shared with analytics, without taxing query latency.

## The objection and where it breaks
### The common argument
Postgres processes many small reads that are sensitive to latency. Object storage is built for larger requests at higher latency, and one read from it can take hundreds of milliseconds. The objection concludes that the design must be slow. The article's counter is that this assumes a query has to touch object storage, and Lakebase Postgres almost never does.

## Measuring where reads actually land
The authors sampled page-read statistics across a slice of their production fleet. Reads are split into tiers, and every page read is served by the first tier that holds the page.

### Tier 1: the compute node's local cache
This tier served about 1.1 billion page accesses per second, roughly 99.46% of the total. These reads never leave the compute node, so they cost memory or microsecond-level latency, the same as a warm cache in traditional Postgres.

### Tier 2: pageserver local NVMe
On a local miss, the read crosses the network to a pageserver. This tier served about 6 million reads per second, roughly 0.54% of the total. The pageserver answers directly from its own NVMe disk.

### Tier 3: object storage
Only about 39 requests per second reached object storage. That is roughly 1 in 28 million page accesses, so more than 99.99999% never need it. The article says these rare reads are special cases, not the normal path.

## What building on object storage buys
### Durability
Object storage such as S3 is designed for 99.999999999% durability. Durable state therefore does not depend on any single machine.

### Storage without provisioning
Storage grows with the data. There are no disks to size and no risk of running out of space.

### Immutable history makes branches and restores cheap
History is kept as immutable layer files, so branching and instant restores are metadata operations that need no data copies. A branch is a pointer to an LSN. Point-in-time restore and snapshot restore create a new branch at an LSN instead of rewriting the current one.

### Stateless compute
Durable state lives outside the compute node, so compute can scale to zero and start again without moving data.

### One copy for transactions and analytics
Once operational data lives in the lake, analytical engines can read the same copy through LTAP.

## The design constraint: keep object storage off the query path
The article states that speed is the part you must design around when building on object storage. The storage layer is arranged so object storage never sits in front of a query. Hot data is held in local caches on the compute node and on the pageservers, and object storage only backs those tiers.

> ❌ **Anti-pattern — object storage on the query path:** every page read goes straight to object storage, so each query waits on a remote request. **Why people do it:** the article does not say; a single tier is the simplest design. **Consequence:** each read takes hundreds of milliseconds, which is the article's premise for why such a database would be slow. **Fix:** place caches on the compute node and on the pageservers, so object storage is reached only on a rare miss.

## How the storage layer is split
The article notes that this architecture differs enough from conventional Postgres to be its own category, the lakebase. The design decouples compute from storage.

### Compute layer
The compute layer runs standard Postgres and keeps hot pages in a local cache. It holds no durable state, so it can start, stop, scale or fail without putting data at risk.

### Safekeepers
Safekeepers replicate the WAL. A transaction is committed once a quorum of safekeepers acknowledges its record. A Paxos-based protocol handles leader election and failover.

> 🔎 Added context: Paxos is a consensus protocol that lets a group of nodes agree on one value even when some nodes fail. WAL is the write-ahead log, the ordered record of changes that Postgres writes before applying them.

### Pageservers
Pageservers turn WAL into pages. They build an LSM tree on local NVMe and persist materialized history to object storage asynchronously.

> 🔎 Added context: an LSM tree is a write-optimized structure that buffers writes in memory and merges them into sorted files on disk. Materialized history here means page versions already computed from the WAL.

### Object storage
Object storage holds the immutable, long-term history and backs the pageservers. Postgres never reads from it directly.

## The read path, step by step
### GetPage@LSN
When Lakebase Postgres needs a page, it first checks the compute node's local cache. On a miss, it sends GetPage@LSN to a pageserver. The request names a page and an LSN, and the pageserver returns the page as it existed at that LSN.

> 🔎 Added context: an LSN (log sequence number) is a position in the WAL. Asking for a page at an LSN means asking for the version of that page as of that point in the log.

In almost every case the pageserver already has the layers it needs on its own NVMe. Only if it does not does it fetch layers from object storage.

### Caching at every tier
Each tier caches what it fetches from the tier below. A page returned by a pageserver lands in the compute node's cache. Layers fetched from object storage land on pageserver NVMe. The cost of a deeper tier is paid once, and the next read of the same data stays local.

## Image and delta layers keep replay short
The pageserver organizes data into two kinds of layer files.

### Image layers
An image layer holds a snapshot of every page in a key range at one LSN.

### Delta layers
A delta layer holds the WAL records for a key range across a range of LSNs.

### Replaying deltas on read
To answer GetPage@LSN, the pageserver starts at the requested page and LSN and walks down through the layers, collecting the WAL records for that page. It stops at the first image of the page, then replays the collected deltas on top of that image. The number of deltas is the measure of how much work a read takes. With zero deltas the page comes straight from an image. With many, the pageserver replays a long WAL chain before it can return.

### Fleet numbers for replay
Across the us-east-1 fleet, p90 stayed under 1 delta per GetPage, so most requests need no replay at all. p99 was around 6. It dropped to that level after a change that creates base pages (images) more eagerly. With images closer to the LSNs that reads ask for, fewer reads have a long chain to replay.

## Why reads do reach object storage
Only a tiny fraction of pageserver reads go further. The roughly 39 requests per second that reach object storage share one trait: they ask for data nobody has read recently. In practice that means one of two cases.

### Old branches and restores
A branch is a pointer to an LSN. When a restore point is far in the past, that history may not be on pageserver NVMe, so the first reads fetch it from object storage.

### Dormant databases waking up
A database that has scaled to zero for weeks no longer has its data cached. Its first reads after waking go to object storage.

### Only the first reads pay
In both cases only the first reads pay the cost. After that the data is cached in the fastest tiers and follows the same local path as any active workload. During active query execution, object storage is not on the critical path.

## 🧠 Mental models
### Every tier caches the tier below
**The model:** A deeper tier's cost is paid once, because each tier keeps what it fetches from the one beneath it.
**How to apply it:** When sizing a tiered store, ask whether a miss fills the cache above it. If it does, judge the tail by first-touch cost only.

### Judge a tiered system by fall-through rate
**The model:** The useful metric is the share of requests that fall through to each deeper tier, not the worst-case latency of the slowest tier.
**How to apply it:** Sample per-tier serve counts from production, as the authors did, and check whether the slow tier's share is tiny.

### Replay cost is the delta chain length
**The model:** In a log-structured page store, read cost is the number of WAL deltas replayed on top of the nearest image.
**How to apply it:** Track deltas-per-read percentiles such as p90 and p99. If the tail grows, create base images more eagerly.

## 🔑 Key takeaways
- The compute node's local cache answers about 99.46% of page accesses. Pageserver NVMe answers about 0.54%. About 1 in 28 million reaches object storage.
- Lakebase Postgres is designed so object storage never sits in front of a query. It only backs the pageservers.
- Compute holds no durable state, so it can start, stop, scale or fail without risking data.
- A safekeeper quorum acknowledgment is what commits a transaction.
- Because history is kept as immutable layer files, branches and instant restores are metadata operations with no data copies.
- Image layers keep replay short. p90 is under one delta per GetPage and p99 is around six.
- Object-storage reads come mostly from old-point-in-time branches and from databases waking after scale-to-zero, and only the first reads pay that cost.

## 🏛️ For Kenji
This is your interpretation, not the author's. For an architect deciding whether an object-storage-backed data layer fits a workload, the useful check is to measure the fall-through rate and the cold-start path separately. Steady-state latency follows the top tiers, while restores and wake-ups follow the rare tail. The question it raises: at what fall-through rate would cold starts become visible to your users, and how would you measure it?

## 📚 Reading plan
### From the article
- [lakebase architecture](https://docs.databricks.com/aws/en/oltp/projects/architecture#how-a-read-works) — the read-path tiers, with the exact sequence a page read follows.
- [lakebase architecture](https://docs.databricks.com/aws/en/oltp/projects/architecture) — the decoupled compute and storage components in full.
- [branches](https://docs.databricks.com/aws/en/oltp/projects/branches) — how a branch maps to an LSN.
- [point-in-time restore](https://docs.databricks.com/aws/en/oltp/projects/point-in-time-restore) — the restore mechanism the article describes.
- [scale to zero](https://docs.databricks.com/aws/en/oltp/projects/scale-to-zero) — the wake-up path that sends first reads to object storage.
- [Building on object storage](https://www.databricks.com/product/lakebase) — the product context for the design.
- [From monolith to Lakebase to LTAP](https://www.databricks.com/blog/lakebase-ltap-rethinking-database-storage) — the shared-copy argument for analytics.
- [WAL + S3: Lakebase storage for the era of agents](https://neon.com/blog/wal-s3-lakebase-storage-for-the-era-of-agents) — a companion piece on WAL and object storage.
- [A New Era of Databases: Lakebase](https://www.databricks.com/blog/what-is-a-lakebase) — the definition of the lakebase category.

### To go further
- Write-ahead logging in PostgreSQL — the background for WAL records, LSNs and replay.
- Log-structured merge trees — the write path behind pageserver storage.
- Paxos and quorum commit — the consensus behind safekeeper failover.
