---
type: Concept
title: Metadata and data catalog
description: Metadata is data about data - what a dataset contains, where it came from, who owns it and how it may be used; a data catalog is the searchable inventory of an organisation's datasets built from that metadata, often with lineage.
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
- title: DAMA International (2017). DAMA-DMBOK - Data Management Body of Knowledge, 2nd edition, ch. 12 "Metadata Management"
  resource: 'https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/'
  kind: book
- title: W3C (2024). Data Catalog Vocabulary (DCAT) - Version 3. W3C Recommendation
  resource: 'https://www.w3.org/TR/vocab-dcat-3/'
  kind: standard
- title: OpenLineage. OpenLineage specification and documentation
  resource: 'https://openlineage.io/docs/'
  kind: docs
---

# Metadata and data catalog

**Definition.** DAMA-DMBOK groups metadata into business (definitions, owners, rules), technical (schemas, types, locations, lineage) and operational (run times, volumes, quality results). A data catalog collects these so people can discover, understand and trust data. W3C's DCAT vocabulary standardises how catalogs and datasets are described for exchange, and OpenLineage standardises lineage events emitted by pipelines.

Domain: [data](/domains/data.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Metadata is what turns a data lake from a swamp into something usable and what makes governance enforceable. Lineage answers "if this source breaks, which dashboards and models are affected?". This KB's frontmatter (type, status, sources, verified) is a small metadata scheme for knowledge instead of tables.

## Example

An analyst searches the catalog for "churn", finds the certified table customer_churn_monthly, reads its owner, refresh schedule and column definitions, and follows its lineage back to the CRM and billing sources before using it.

## Narrower concepts

- [Feature store](/concepts/ai-ml/feature-store.md)
- [Knowledge graph](/concepts/data/knowledge-graph.md)

## Sources

1. DAMA International (2017). *DAMA-DMBOK - Data Management Body of Knowledge*, 2nd edition, ch. 12 "Metadata Management". <https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/> — Types of metadata and metadata management.
2. W3C (2024). *Data Catalog Vocabulary (DCAT) - Version 3*. W3C Recommendation. <https://www.w3.org/TR/vocab-dcat-3/> — Standard vocabulary for describing catalogs and datasets.
3. OpenLineage. *OpenLineage specification and documentation*. <https://openlineage.io/docs/> — Open standard for lineage metadata emitted by pipelines.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
