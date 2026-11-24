---
type: Concept
title: ETL / ELT and data pipeline
description: A data pipeline is a chain of automated steps that moves data from sources to destinations; ETL transforms data before loading it into the target, ELT loads raw data first and transforms it inside the target.
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
- title: DAMA International (2017). DAMA-DMBOK - Data Management Body of Knowledge, 2nd edition, ch. 8 "Data Integration and Interoperability"
  resource: 'https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/'
  kind: book
- title: Apache Software Foundation. Apache Airflow documentation - Core Concepts (DAGs, tasks)
  resource: 'https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/index.html'
  kind: docs
- title: dbt Labs. What is dbt? (ELT transformations in the warehouse)
  resource: 'https://docs.getdbt.com/docs/introduction'
  kind: docs
---

# ETL / ELT and data pipeline

**Definition.** Extract, transform, load (ETL) pulls data from operational systems, cleans and reshapes it in a separate processing layer, then loads it into a warehouse. With scalable cloud warehouses, ELT became common: raw data is loaded as-is and transformed with SQL inside the warehouse (for example with dbt). A pipeline is usually orchestrated as a directed acyclic graph (DAG) of tasks with schedules, dependencies, retries and monitoring (for example Apache Airflow).

Domain: [data](/domains/data.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Pipelines are the plumbing between OLTP systems and analytics or ML. Their typical failures are silent: a schema change upstream, a late file or a duplicated load produces wrong numbers rather than errors, which is why data quality checks, lineage and idempotent loads matter. This project's daily digest is itself a small pipeline.

## Example

Every night an Airflow DAG extracts yesterday's orders from PostgreSQL, loads them raw into the warehouse, runs dbt models that join them with customers and currencies, tests that no order id is duplicated, and refreshes the sales dashboard.

## Narrower concepts

- [Change data capture](/concepts/data/change-data-capture.md)
- [Batch vs stream processing](/concepts/data/batch-stream-processing.md)
- [Silent failure](/concepts/backend-systems/silent-failure.md)

## Sources

1. DAMA International (2017). *DAMA-DMBOK - Data Management Body of Knowledge*, 2nd edition, ch. 8 "Data Integration and Interoperability". <https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/> — ETL/ELT as data-integration patterns.
2. Apache Software Foundation. *Apache Airflow documentation - Core Concepts (DAGs, tasks)*. <https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/index.html> — Pipelines as DAGs of tasks with scheduling and retries.
3. dbt Labs. *What is dbt?* (ELT transformations in the warehouse). <https://docs.getdbt.com/docs/introduction> — In-warehouse transformation (the T of ELT) with tests.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
