---
type: Concept
title: Data warehouse, data lake and lakehouse
description: A data warehouse stores cleaned, structured data modelled for analysis; a data lake stores raw data of any format cheaply in object storage; a lakehouse adds warehouse features (transactions, schemas, fast SQL) on top of open files in a lake.
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
- title: Inmon, W. H. (2005). Building the Data Warehouse, 4th edition. Wiley
  resource: 'https://www.wiley.com/en-us/Building+the+Data+Warehouse%2C+4th+Edition-p-9780764599446'
  kind: book
- title: Armbrust, M., Ghodsi, A., Xin, R. and Zaharia, M. (2021). Lakehouse - A New Generation of Open Platforms that Unify Data Warehousing and Advanced Analytics. CIDR 2021
  resource: 'https://www.cidrdb.org/cidr2021/papers/cidr2021_paper17.pdf'
  kind: paper
- title: Apache Software Foundation. Apache Iceberg documentation
  resource: 'https://iceberg.apache.org/docs/latest/'
  kind: docs
---

# Data warehouse, data lake and lakehouse

**Definition.** Inmon defined the data warehouse as a subject-oriented, integrated, time-variant and non-volatile collection of data in support of management decisions. Data lakes, popularised in the 2010s, keep raw files (CSV, JSON, Parquet, images) in cheap storage with schema applied on read. Armbrust et al. proposed the lakehouse: open table formats (Delta Lake, Apache Iceberg, Apache Hudi) add ACID transactions, schema enforcement and indexing to lake files so one copy can serve BI, data science and ML.

Domain: [data](/domains/data.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

These are the main destinations of analytical data. Warehouses give governed, fast SQL but cost more and handle unstructured data poorly; lakes are flexible but easily become "data swamps" without governance and metadata; lakehouses try to combine both. ML feature stores and RAG corpora often read from the same platform.

## Example

A retailer lands raw clickstream JSON and supplier files in object storage (lake), converts them into Iceberg tables with schemas and time travel (lakehouse), and publishes a curated sales star schema that finance queries from their BI tool (warehouse layer).

## Narrower concepts

- [Change data capture](/concepts/data/change-data-capture.md)
- [Feature store](/concepts/ai-ml/feature-store.md)
- [ETL / ELT and data pipeline](/concepts/data/etl-elt-pipeline.md)

## Sources

1. Inmon, W. H. (2005). *Building the Data Warehouse*, 4th edition. Wiley. <https://www.wiley.com/en-us/Building+the+Data+Warehouse%2C+4th+Edition-p-9780764599446> — Classic definition of the data warehouse.
2. Armbrust, M., Ghodsi, A., Xin, R. and Zaharia, M. (2021). *Lakehouse - A New Generation of Open Platforms that Unify Data Warehousing and Advanced Analytics*. CIDR 2021. <https://www.cidrdb.org/cidr2021/papers/cidr2021_paper17.pdf> — Lakehouse definition and its relation to warehouses and lakes.
3. Apache Software Foundation. *Apache Iceberg documentation*. <https://iceberg.apache.org/docs/latest/> — Open table format providing transactions and schema evolution on lake files.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
