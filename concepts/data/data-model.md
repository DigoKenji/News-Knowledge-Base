---
type: Concept
title: Data model (relational, document, graph)
description: The way data is structured and related so it can be stored and queried - most commonly as relational tables, nested documents or graphs of nodes and edges - each fitting different shapes of data and queries.
tags:
- data
- databases
- core
domain: data
subdomain: databases
also_in:
- architecture/data-architecture
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Codd, E. F. (1970). A Relational Model of Data for Large Shared Data Banks. Communications of the ACM 13(6)
  resource: 'https://doi.org/10.1145/362384.362685'
  kind: paper
- title: ISO/IEC (2023). ISO/IEC 9075-1 - Information technology - Database languages SQL - Part 1 Framework
  resource: 'https://www.iso.org/standard/76583.html'
  kind: standard
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 2 "Data Models and Query Languages"
  resource: 'https://dataintensive.net/'
  kind: book
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Data model (relational, document, graph)

**Definition.** A data model defines the structures, operations and integrity rules used to represent data. Codd's relational model (1970) represents data as relations (tables of rows) manipulated by set operations, independent of physical storage; SQL, standardised as ISO/IEC 9075, is its dominant language. The document model stores self-contained nested records (JSON); the graph model stores entities as nodes and relationships as edges (property graphs, RDF). DAMA-DMBOK distinguishes conceptual, logical and physical levels of modelling.

Domain: [data](/domains/data.md) · subdomain `databases`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

The data model is the first and most lasting design decision: it shapes what is easy to query and what needs joins or duplication. Relational fits many-to-many relationships and ad-hoc queries; document fits data read as a whole unit; graph fits highly connected data and path queries. This KB itself is a graph of concepts stored as documents.

## Example

An e-commerce site keeps orders and customers in relational tables (joins, reports), stores each product's variable attributes as a JSON document, and models "customers who bought X also bought Y" as a graph.

## Narrower concepts

- [Knowledge graph](/concepts/data/knowledge-graph.md)
- [Database and DBMS](/concepts/data/database-dbms.md)
- [OLTP vs OLAP](/concepts/data/oltp-olap.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. Codd, E. F. (1970). *A Relational Model of Data for Large Shared Data Banks*. Communications of the ACM 13(6). <https://doi.org/10.1145/362384.362685> — Original definition of the relational model and data independence.
2. ISO/IEC (2023). *ISO/IEC 9075-1 - Information technology - Database languages SQL - Part 1 Framework*. <https://www.iso.org/standard/76583.html> — SQL standard framework (paywalled; cited, not quoted).
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 2 "Data Models and Query Languages". <https://dataintensive.net/> — Relational vs document vs graph trade-offs.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
