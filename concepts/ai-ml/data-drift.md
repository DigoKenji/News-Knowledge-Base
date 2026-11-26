---
type: Concept
title: Data drift
description: 'A change over time in the distribution of a model''s input data compared with its training data, which can degrade predictions and is monitored in production.'
tags:
- ai-ml
- ml-governance
domain: ai-ml
subdomain: ml-governance
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 11 MLOps and LLMOps: Drifts, p. 469'
- title: 'Rabanser, S., Günnemann, S. and Lipton, Z. C. (2019). Failing Loudly: An Empirical Study of Methods for Detecting Dataset Shift. NeurIPS 2019'
  resource: 'https://arxiv.org/abs/1810.11953'
  kind: paper
- title: 'Quiñonero-Candela, J. et al. (eds.) (2009). Dataset Shift in Machine Learning. MIT Press'
  resource: 'https://books.google.com/books?vid=ISBN9780262170055'
  kind: book
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Data drift

**Definition.** Data drift (also covariate shift) is a change in the distribution of a model's inputs, P(x), compared with the data it was trained on - new user segments, a changed sensor, a different upstream format. The model may still be right for the inputs it knows, but it is now asked about regions it saw rarely, so quality can degrade. Drift is detected without labels by comparing feature statistics between a reference window and live traffic.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `ml-governance`. Expanded on 2026-11-26 with independent sources.

## How it fits

Because it needs no labels it is the first alarm in ML monitoring, typically using statistical tests or distances (KS test, population stability index, maximum mean discrepancy) per feature or on embeddings. It is a trigger to investigate, not proof of failure; concept drift is what actually breaks the mapping.

## Example

After a mobile app redesign, the share of sessions with a missing referrer jumps from 2% to 30%; the monitoring job flags the feature before any accuracy metric is available.

## Sources

1. Iusztin, P. and Labonne, M. (2024). LLM Engineer's Handbook. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 11 MLOps and LLMOps: Drifts, p. 469. Data drift versus concept drift and how they are monitored.
2. Rabanser, S., Günnemann, S. and Lipton, Z. C. (2019). Failing Loudly: An Empirical Study of Methods for Detecting Dataset Shift. NeurIPS 2019. <https://arxiv.org/abs/1810.11953> — Compares shift-detection methods on real data.
3. Quiñonero-Candela, J. et al. (eds.) (2009). Dataset Shift in Machine Learning. MIT Press. <https://books.google.com/books?vid=ISBN9780262170055> — Reference book on covariate and dataset shift.

## Relations
- [Concept drift](/concepts/ai-ml/concept-drift.md) **alternative-to** this

## Notes
Notes tagged with this concept are listed in the concept index after each run.
