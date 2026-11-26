---
type: Concept
title: OLTP vs OLAP
description: OLTP (online transaction processing) serves many small, fast reads and writes for running the business; OLAP (online analytical processing) serves fewer, large queries that scan and aggregate history to analyse it.
tags:
- data
- databases
- core
domain: data
subdomain: databases
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Kimball, R. and Ross, M. (2013). The Data Warehouse Toolkit, 3rd edition. Wiley
  resource: 'https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/books/data-warehouse-dw-toolkit/'
  kind: book
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 3 "Transaction Processing or Analytics?"
  resource: 'https://dataintensive.net/'
  kind: book
- title: Abadi, D. J., Madden, S. R. and Hachem, N. (2008). Column-Stores vs. Row-Stores - How Different Are They Really? SIGMOD 2008
  resource: 'https://doi.org/10.1145/1376616.1376712'
  kind: paper
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# OLTP vs OLAP

**Definition.** OLTP workloads touch a few rows per request by key, need low latency and strong transactional guarantees, and are usually stored row by row. OLAP workloads read millions of rows but few columns, aggregate them (sums, counts, group-by), tolerate seconds of latency and are best served by column-oriented storage and star or snowflake schemas. Codd introduced the term OLAP in 1993; Kimball's dimensional modelling is the classic way to design for it.

Domain: [data](/domains/data.md) · subdomain `databases`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Running analytics on the operational database slows down customers, so organisations copy data from OLTP systems into an analytical store (warehouse or lakehouse) through ETL/ELT or change data capture. Hybrid (HTAP) systems try to serve both, but the access patterns still differ.

## Example

"Insert this order and decrement stock" is OLTP, done thousands of times a second. "Revenue by region and month for the last three years" is OLAP, run in a columnar warehouse overnight or on demand.

## Narrower concepts

- [Data warehouse, data lake and lakehouse](/concepts/data/data-warehouse-lake-lakehouse.md)
- [Change data capture](/concepts/data/change-data-capture.md)

## Sources

1. Kimball, R. and Ross, M. (2013). *The Data Warehouse Toolkit*, 3rd edition. Wiley. <https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/books/data-warehouse-dw-toolkit/> — Dimensional modelling for analytics and the operational vs analytical split.
2. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 3 "Transaction Processing or Analytics?". <https://dataintensive.net/> — Contrast of OLTP and OLAP access patterns and column storage.
3. Abadi, D. J., Madden, S. R. and Hachem, N. (2008). *Column-Stores vs. Row-Stores - How Different Are They Really?* SIGMOD 2008. <https://doi.org/10.1145/1376616.1376712> — Why column-oriented storage suits analytical queries.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
