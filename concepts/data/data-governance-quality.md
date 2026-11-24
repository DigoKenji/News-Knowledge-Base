---
type: Concept
title: Data governance and data quality
description: Data governance is the exercise of authority and control over how data is managed - ownership, policies, standards and accountability; data quality is how fit the data is for its intended use, measured by dimensions such as accuracy, completeness, consistency and timeliness.
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
- title: DAMA International (2017). DAMA-DMBOK - Data Management Body of Knowledge, 2nd edition, ch. 3 "Data Governance" and ch. 13 "Data Quality"
  resource: 'https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/'
  kind: book
- title: ISO/IEC (2008). ISO/IEC 25012 - Software engineering - SQuaRE - Data quality model
  resource: 'https://www.iso.org/standard/35736.html'
  kind: standard
- title: Wang, R. Y. and Strong, D. M. (1996). Beyond Accuracy - What Data Quality Means to Data Consumers. Journal of Management Information Systems 12(4)
  resource: 'https://doi.org/10.1080/07421222.1996.11518099'
  kind: paper
foundation_candidate: true
---

# Data governance and data quality

**Definition.** DAMA-DMBOK defines data governance as the exercise of authority, control and shared decision-making (planning, monitoring and enforcement) over the management of data assets, and places it at the centre of its framework. ISO/IEC 25012 defines a data quality model with inherent characteristics (accuracy, completeness, consistency, credibility, currentness) and system-dependent ones (availability, portability, recoverability); ISO 8000 covers data quality for master data exchange.

Domain: [data](/domains/data.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Governance answers who owns a dataset, who may use it and under which rules (including privacy laws such as LGPD and GDPR); quality practices check that the data is right before decisions or models depend on it. Without them, analytics and ML inherit silent errors - "garbage in, garbage out".

## Example

A company names a data owner for "customer", defines that a valid record needs a verified e-mail and a tax id, runs nightly checks that flag records failing those rules, and restricts personal fields to approved roles under LGPD.

## Narrower concepts

- [Metadata and data catalog](/concepts/data/metadata-catalog.md)
- [Silent failure](/concepts/backend-systems/silent-failure.md)
- [Data drift](/concepts/ai-ml/data-drift.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. DAMA International (2017). *DAMA-DMBOK - Data Management Body of Knowledge*, 2nd edition, ch. 3 "Data Governance" and ch. 13 "Data Quality". <https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/> — Definitions of data governance and data quality management.
2. ISO/IEC (2008). *ISO/IEC 25012 - Software engineering - SQuaRE - Data quality model*. <https://www.iso.org/standard/35736.html> — Data quality characteristics (paywalled; cited, not quoted).
3. Wang, R. Y. and Strong, D. M. (1996). *Beyond Accuracy - What Data Quality Means to Data Consumers*. Journal of Management Information Systems 12(4). <https://doi.org/10.1080/07421222.1996.11518099> — Fitness-for-use view and empirically derived quality dimensions.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
