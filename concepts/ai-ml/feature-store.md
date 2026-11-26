---
type: Concept
title: Feature store
description: 'A shared system that computes, stores and serves ML features consistently for training and for online inference, so both use the same definitions.'
tags:
- ai-ml
- mlops
domain: ai-ml
subdomain: mlops
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 4 RAG Feature Pipeline: The feature store, p. 128'
- title: 'Feast. Feast documentation (open-source feature store)'
  resource: 'https://docs.feast.dev/'
  kind: docs
- title: 'Hopsworks. Feature Store (MLOps dictionary)'
  resource: 'https://www.hopsworks.ai/dictionary/feature-store'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Feature store

**Definition.** A feature store is the data system between raw data and models: it computes features with versioned definitions, stores them in an offline store for training (point-in-time correct history) and an online store for low-latency inference, and lets teams discover and reuse them. Its main job is to prevent training-serving skew, where a feature is computed one way in a notebook and another way in production.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `mlops`. Expanded on 2026-11-26 with independent sources.

## How it fits

It is the feature layer of an MLOps platform, fed by batch and streaming pipelines. In LLM systems the same role is played by the store that keeps cleaned, chunked and embedded documents for RAG, which the LLM Engineer's Handbook calls its feature store.

## Example

'Orders in the last 7 days' is defined once; the training job reads its historical values as of each label date, and the checkout service reads the current value in milliseconds from the online store.

## Sources

1. Iusztin, P. and Labonne, M. (2024). LLM Engineer's Handbook. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 4 RAG Feature Pipeline: The feature store, p. 128. Feature store as the versioned, shared state of features (and of RAG data).
2. Feast. Feast documentation (open-source feature store). <https://docs.feast.dev/> — Offline/online stores and point-in-time joins in an open-source implementation.
3. Hopsworks. Feature Store (MLOps dictionary). <https://www.hopsworks.ai/dictionary/feature-store> — Vendor-neutral definition and the training-serving skew problem.

## Notes
Notes tagged with this concept are listed in the concept index after each run.
