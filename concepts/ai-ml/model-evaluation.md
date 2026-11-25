---
type: Concept
title: Model evaluation and metrics
description: Measuring how well a model performs on data it has not seen, with metrics chosen for the task - accuracy, precision, recall, F1, AUC for classification; error measures for regression; benchmarks and human or model judges for generative AI.
tags:
- ai-ml
- core
domain: ai-ml
subdomain: core
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
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
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 3 Evaluation Methodology, pp. 113-157 (perplexity p. 121; exact evaluation p. 125; AI as a judge p. 136); ch. 4 Evaluate AI Systems, pp. 159-209'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 6 Model Evaluation and Hyperparameter Tuning, pp. 171-204 (k-fold CV p. 175; metrics p. 193)'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 7 Evaluating LLMs, pp. 261-288 (RAG evaluation and Ragas p. 271)'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Model evaluation and metrics

**Definition.** Evaluation estimates real-world performance by testing on held-out data and summarising results with metrics that reflect what matters for the use case. Precision is the share of predicted positives that are correct; recall is the share of actual positives found; F1 balances them. Regression uses MAE or RMSE. ISO/IEC TS 4213 standardises how to assess classification performance.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Choosing the metric is a business decision: in fraud detection missing fraud (low recall) and blocking good customers (low precision) have different costs, and accuracy is misleading when classes are imbalanced. Generative models are harder: there is no single right answer, so teams combine benchmarks (MMLU, HELM), task-specific test sets, human ratings and LLM-as-a-judge, and watch for benchmark contamination.

## Example

A fraud model flags 100 transactions, 80 really are fraud (precision 80%), and it caught 80 of the 200 frauds that happened (recall 40%), even though overall accuracy is 99.9%.

## Narrower concepts

- [LLM-as-a-judge](/concepts/ai-ml/llm-as-a-judge.md)
- [Position bias](/concepts/ai-ml/position-bias.md)
- [Calibration](/concepts/ai-ml/calibration.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 3 Evaluation Methodology, pp. 113-157 (perplexity p. 121; exact evaluation p. 125; AI as a judge p. 136); ch. 4 Evaluate AI Systems, pp. 159-209: Evaluation for open-ended generative output and how to design an evaluation pipeline.
- **Raschka, S., Liu, Y. and Mirjalili, V. (2022)**, *Machine Learning with PyTorch and Scikit-Learn*, ch. 6 Model Evaluation and Hyperparameter Tuning, pp. 171-204 (k-fold CV p. 175; metrics p. 193): Classic evaluation: cross-validation, confusion matrix, precision/recall, ROC.
- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 7 Evaluating LLMs, pp. 261-288 (RAG evaluation and Ragas p. 271): General, domain and task-specific LLM evaluations, plus RAG evaluation.

## Sources

1. scikit-learn. *Metrics and scoring - quantifying the quality of predictions*. <https://scikit-learn.org/stable/modules/model_evaluation.html> — Definitions and use of classification and regression metrics.
2. ISO/IEC (2022). *ISO/IEC TS 4213 - Assessment of machine learning classification performance*. <https://www.iso.org/standard/79799.html> — Standardised assessment of classification performance (paywalled; cited, not quoted).
3. Liang, P. et al. (2023). *Holistic Evaluation of Language Models (HELM)*. TMLR, arXiv:2211.09110. <https://arxiv.org/abs/2211.09110> — Multi-metric evaluation of language models beyond accuracy.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 3 Evaluation Methodology, pp. 113-157 (perplexity p. 121; exact evaluation p. 125; AI as a judge p. 136); ch. 4 Evaluate AI Systems, pp. 159-209. Evaluation for open-ended generative output and how to design an evaluation pipeline.
5. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 6 Model Evaluation and Hyperparameter Tuning, pp. 171-204 (k-fold CV p. 175; metrics p. 193). Classic evaluation: cross-validation, confusion matrix, precision/recall, ROC.
6. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 7 Evaluating LLMs, pp. 261-288 (RAG evaluation and Ragas p. 271). General, domain and task-specific LLM evaluations, plus RAG evaluation.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
