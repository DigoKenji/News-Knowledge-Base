---
type: Concept
title: 'Storage engines: LSM-trees and B-trees'
description: 'The two main ways databases organise data on disk - log-structured merge trees (append, then merge sorted files in the background) and B-trees (update fixed-size pages in place) - with LSM-trees usually faster for writes and B-trees for reads.'
tags:
- data
- databases
domain: data
subdomain: databases
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 3 Storage and Retrieval: SSTables and LSM-Trees; B-Trees; Comparing B-Trees and LSM-Trees; Column-Oriented Storage'
- title: 'O''Neil, P. et al. (1996). The Log-Structured Merge-Tree (LSM-Tree). Acta Informatica 33'
  resource: 'https://doi.org/10.1007/s002360050048'
  kind: paper
- title: 'Bayer, R. and McCreight, E. (1972). Organization and Maintenance of Large Ordered Indices. Acta Informatica 1'
  resource: 'https://doi.org/10.1007/BF00288683'
  kind: paper
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Storage engines: LSM-trees and B-trees

**Definition.** A storage engine is the part of a database that stores data on disk and finds it again. Kleppmann describes two families. A log-structured merge tree (LSM-tree) buffers writes in a sorted in-memory table, flushes it to disk as an immutable sorted file (SSTable), and merges and compacts those files in the background; it is used by LevelDB, RocksDB, Cassandra and HBase. A B-tree splits the database into fixed-size pages arranged as a tree and overwrites pages in place, with a write-ahead log for crash recovery; it is the standard index in relational databases. As a rule of thumb, LSM-trees are faster for writes and B-trees faster for reads, but Kleppmann warns that benchmarks are sensitive to the workload and you must test your own.

Domain: [data](/domains/data.md) · subdomain `databases`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-26); draft waiting for Kenji to certify.

## How it fits

The choice explains why databases behave differently under load: LSM-trees turn random writes into sequential ones and compress well, but compaction can interfere with foreground requests and reads may check several files; B-trees give predictable reads and keep each key in one place, which makes transactional locking simpler. Column-oriented storage for analytics (ch. 3) is a third design aimed at scanning a few columns over many rows.

## Example

A metrics service ingesting millions of points per second picks an LSM-based store; the orders database that mostly serves point lookups and transactions stays on PostgreSQL's B-tree indexes.

## Broader concepts

- [Database and DBMS](/concepts/data/database-dbms.md)

## Related concepts

- [OLTP vs OLAP](/concepts/data/oltp-olap.md)

## Sources

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 3 Storage and Retrieval: SSTables and LSM-Trees; B-Trees; Comparing B-Trees and LSM-Trees; Column-Oriented Storage. How both engine families work, their write amplification and the trade-offs, with the warning to benchmark your own workload.
2. O'Neil, P. et al. (1996). *The Log-Structured Merge-Tree (LSM-Tree)*. Acta Informatica 33. <https://doi.org/10.1007/s002360050048> — Original LSM-tree paper.
3. Bayer, R. and McCreight, E. (1972). *Organization and Maintenance of Large Ordered Indices*. Acta Informatica 1. <https://doi.org/10.1007/BF00288683> — Original B-tree paper.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
