---
type: Concept
title: Database and DBMS
description: A database is an organised, persistent collection of data; a database management system (DBMS) is the software that stores it and lets many users define, query, update and protect it safely and concurrently.
tags:
- data
- core
domain: data
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: ISO/IEC (2015). ISO/IEC 2382 - Information technology - Vocabulary (entry "database")
  resource: 'https://www.iso.org/standard/63598.html'
  kind: standard
- title: Hellerstein, J. M., Stonebraker, M. and Hamilton, J. (2007). Architecture of a Database System. Foundations and Trends in Databases 1(2)
  resource: 'https://doi.org/10.1561/1900000002'
  kind: paper
- title: DAMA International (2017). DAMA-DMBOK - Data Management Body of Knowledge, 2nd edition, ch. 6 "Data Storage and Operations"
  resource: 'https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/'
  kind: book
foundation_candidate: true
---

# Database and DBMS

**Definition.** ISO/IEC 2382 defines a database as a collection of data organised according to a conceptual structure describing the characteristics of the data and their relationships. A DBMS provides that structure plus a query language, a query optimiser, storage and indexing, concurrency control, recovery after crashes and access control. Hellerstein, Stonebraker and Hamilton describe the canonical architecture: process manager, query processor, transactional storage manager and shared utilities.

Domain: [data](/domains/data.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

The DBMS is why applications do not have to reinvent safe concurrent storage. Families include relational (PostgreSQL, MySQL), key-value, document, wide-column, graph, time-series and vector databases, each optimised for a data model and access pattern. Picking one is mostly about the queries and consistency you need, not fashion.

## Example

A clinic stores patients and appointments in PostgreSQL. Two receptionists booking the same slot at the same moment are serialised by the DBMS; after a power cut it replays its write-ahead log so no confirmed booking is lost.

## Narrower concepts

- [Transaction and ACID](/concepts/data/transactions-acid.md)
- [OLTP vs OLAP](/concepts/data/oltp-olap.md)
- [Data model (relational, document, graph)](/concepts/data/data-model.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. ISO/IEC (2015). *ISO/IEC 2382 - Information technology - Vocabulary* (entry "database"). <https://www.iso.org/standard/63598.html> — Standard vocabulary definition of database (paywalled; cited, not quoted).
2. Hellerstein, J. M., Stonebraker, M. and Hamilton, J. (2007). *Architecture of a Database System*. Foundations and Trends in Databases 1(2). <https://doi.org/10.1561/1900000002> — Components of a DBMS and how they fit together.
3. DAMA International (2017). *DAMA-DMBOK - Data Management Body of Knowledge*, 2nd edition, ch. 6 "Data Storage and Operations". <https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/> — Database management as a data-management knowledge area.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
