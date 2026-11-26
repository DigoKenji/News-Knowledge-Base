---
type: Concept
title: Concept drift
description: 'A change over time in the relationship between a model''s inputs and the correct output, so a model trained on past data becomes wrong even if the inputs look the same.'
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
- title: 'Gama, J. et al. (2014). A Survey on Concept Drift Adaptation. ACM Computing Surveys 46(4)'
  resource: 'https://doi.org/10.1145/2523813'
  kind: paper
- title: 'Lu, J. et al. (2019). Learning under Concept Drift: A Review. IEEE TKDE 31(12)'
  resource: 'https://doi.org/10.1109/TKDE.2018.2876857'
  kind: paper
relations:
- type: alternative-to
  target: /concepts/ai-ml/data-drift.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Concept drift

**Definition.** Concept drift happens when the relationship between inputs and the correct answer changes after a model is trained: P(y|x) moves, so the same input should now get a different prediction. Fraud patterns adapting to a detector, or customer preferences shifting after a price change, are typical cases. Surveys distinguish sudden, gradual, incremental and recurring drift, and detection usually relies on watching model error once true labels arrive.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `ml-governance`. Expanded on 2026-11-26 with independent sources.

## How it fits

It is the harder twin of data drift: input statistics can look unchanged while accuracy falls. The usual responses are monitoring error on labelled samples, scheduled or triggered retraining, and windowed or weighted training data that favours recent examples.

## Example

A churn model trained before a competitor launched a cheaper plan keeps receiving the same customer features, but customers with those features now leave more often; precision drops until it is retrained on recent outcomes.

## Sources

1. Iusztin, P. and Labonne, M. (2024). LLM Engineer's Handbook. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 11 MLOps and LLMOps: Drifts, p. 469. Drift types and why monitoring must catch them in production.
2. Gama, J. et al. (2014). A Survey on Concept Drift Adaptation. ACM Computing Surveys 46(4). <https://doi.org/10.1145/2523813> — Standard survey: drift types and adaptation strategies.
3. Lu, J. et al. (2019). Learning under Concept Drift: A Review. IEEE TKDE 31(12). <https://doi.org/10.1109/TKDE.2018.2876857> — Detection, understanding and adaptation framework.

## Relations
- **alternative-to** [Data drift](/concepts/ai-ml/data-drift.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
