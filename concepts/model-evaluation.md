---
type: Concept
title: Model evaluation and metrics
description: Measuring how well a model performs on data it has not seen, with metrics chosen for the task - accuracy, precision, recall, F1, AUC for classification; error measures for regression; benchmarks and human or model judges for generative AI.
tags:
- ai-ml
- core
domain: ai-ml
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: scikit-learn. Metrics and scoring - quantifying the quality of predictions
  resource: 'https://scikit-learn.org/stable/modules/model_evaluation.html'
  kind: docs
- title: ISO/IEC (2022). ISO/IEC TS 4213 - Assessment of machine learning classification performance
  resource: 'https://www.iso.org/standard/79799.html'
  kind: standard
- title: 'Liang, P. et al. (2023). Holistic Evaluation of Language Models (HELM). TMLR, arXiv:2211.09110'
  resource: 'https://arxiv.org/abs/2211.09110'
  kind: paper
---

# Model evaluation and metrics

**Definition.** Evaluation estimates real-world performance by testing on held-out data and summarising results with metrics that reflect what matters for the use case. Precision is the share of predicted positives that are correct; recall is the share of actual positives found; F1 balances them. Regression uses MAE or RMSE. ISO/IEC TS 4213 standardises how to assess classification performance.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Choosing the metric is a business decision: in fraud detection missing fraud (low recall) and blocking good customers (low precision) have different costs, and accuracy is misleading when classes are imbalanced. Generative models are harder: there is no single right answer, so teams combine benchmarks (MMLU, HELM), task-specific test sets, human ratings and LLM-as-a-judge, and watch for benchmark contamination.

## Example

A fraud model flags 100 transactions, 80 really are fraud (precision 80%), and it caught 80 of the 200 frauds that happened (recall 40%), even though overall accuracy is 99.9%.

## Narrower concepts

- [LLM-as-a-judge](/concepts/llm-as-a-judge.md)
- [Position bias](/concepts/position-bias.md)
- [Calibration](/concepts/calibration.md)

## Sources

1. scikit-learn. *Metrics and scoring - quantifying the quality of predictions*. <https://scikit-learn.org/stable/modules/model_evaluation.html> — Definitions and use of classification and regression metrics.
2. ISO/IEC (2022). *ISO/IEC TS 4213 - Assessment of machine learning classification performance*. <https://www.iso.org/standard/79799.html> — Standardised assessment of classification performance (paywalled; cited, not quoted).
3. Liang, P. et al. (2023). *Holistic Evaluation of Language Models (HELM)*. TMLR, arXiv:2211.09110. <https://arxiv.org/abs/2211.09110> — Multi-metric evaluation of language models beyond accuracy.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
