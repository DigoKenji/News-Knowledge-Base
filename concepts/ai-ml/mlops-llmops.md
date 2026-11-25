---
type: Concept
title: 'MLOps and LLMOps'
description: 'The engineering practices that take ML and LLM systems from notebook to reliable production - versioned data and models, automated pipelines (feature, training, inference), deployment, monitoring and feedback - with LLMOps adding prompts, guardrails and LLM-specific evaluation.'
tags:
- ai-ml
- mlops
domain: ai-ml
subdomain: mlops
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 1 Building ML systems with feature/training/inference pipelines, pp. 8-22; ch. 11 MLOps and LLMOps, pp. 401-440 (MLOps principles p. 407; LLMOps p. 410; prompt monitoring p. 413)'
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 1 AI Engineering Versus ML Engineering, p. 39; ch. 10 AI Engineering Architecture, pp. 449-474 (monitoring and observability p. 465; pipeline orchestration p. 472); user feedback pp. 474-492'
- title: 'Sculley, D. et al. (2015). Hidden Technical Debt in Machine Learning Systems. NeurIPS 2015'
  resource: 'https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems'
  kind: paper
---

# MLOps and LLMOps

**Definition.** MLOps applies DevOps ideas (automation, versioning, CI/CD, monitoring) to systems whose behaviour depends on data and trained models as much as on code. Iusztin and Labonne frame an ML system as three independent pipelines (FTI): a feature pipeline turns raw data into features or embeddings stored in a feature store or vector database; a training pipeline turns features into a versioned model in a registry; an inference pipeline loads both to serve predictions. LLMOps adds what is new with foundation models: prompt management and monitoring, guardrails on inputs and outputs, LLM-as-a-judge and RAG evaluation, cost and latency tracking, and handling huge models that are usually fine-tuned rather than trained from scratch. Huyen's AI engineering architecture builds up the same production stack step by step: context, guardrails, router and gateway, caches, agent patterns, then monitoring and observability and user feedback.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `mlops`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Sculley et al. showed that the model is a small part of a real ML system; the surrounding data, configuration and serving code is where technical debt piles up, and MLOps exists to manage that. Drift (data and concept) is why monitoring never ends. Project_News itself is a small LLMOps system: scheduled pipelines, versioned prompts Kenji approves, a validator, and digests as the feedback loop.

## Example

A RAG assistant's feature pipeline re-embeds new documents nightly, a training pipeline fine-tunes a small model monthly and registers it only if it beats the current one on the eval set, and the inference pipeline logs every prompt, answer, latency and user rating.

## Broader concepts

- [DevOps](/concepts/software-engineering/devops.md)

## Narrower concepts

- [Feature store](/concepts/ai-ml/feature-store.md)
- [AI gateway](/concepts/ai-ml/ai-gateway.md)

## Related concepts

- [Model evaluation and metrics](/concepts/ai-ml/model-evaluation.md)
- [Data drift](/concepts/ai-ml/data-drift.md)
- [Observability (logs, metrics, traces)](/concepts/cloud-infra/observability.md)

## Sources

1. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 1 Building ML systems with feature/training/inference pipelines, pp. 8-22; ch. 11 MLOps and LLMOps, pp. 401-440 (MLOps principles p. 407; LLMOps p. 410; prompt monitoring p. 413). The FTI pipeline architecture and the path from DevOps to MLOps to LLMOps.
2. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 1 AI Engineering Versus ML Engineering, p. 39; ch. 10 AI Engineering Architecture, pp. 449-474 (monitoring and observability p. 465; pipeline orchestration p. 472); user feedback pp. 474-492. How the production stack for foundation-model apps differs from classic ML engineering.
3. Sculley, D. et al. (2015). *Hidden Technical Debt in Machine Learning Systems*. NeurIPS 2015. <https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems> — Hidden technical debt in ML systems.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
