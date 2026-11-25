---
type: Concept
title: Unsupervised and self-supervised learning
description: Machine learning from data without human-provided labels - unsupervised learning finds structure such as clusters or compact representations, and self-supervised learning creates its own labels from the data, for example by predicting a hidden word.
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
- title: Hastie, T., Tibshirani, R. and Friedman, J. (2009). The Elements of Statistical Learning, 2nd ed., ch. 14 Unsupervised Learning. Springer
  resource: 'https://hastie.su.domains/ElemStatLearn/'
  kind: book
- title: 'Balestriero, R. et al. (2023). A Cookbook of Self-Supervised Learning. arXiv:2304.12210'
  resource: 'https://arxiv.org/abs/2304.12210'
  kind: paper
- title: Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning, ch. 5.8. MIT Press
  resource: 'https://www.deeplearningbook.org/'
  kind: book
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 5 PCA and dimensionality reduction, p. 139; ch. 10 Clustering Analysis, pp. 305-334'
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Unsupervised and self-supervised learning

**Definition.** Unsupervised learning models the structure of unlabelled data: clustering (k-means), dimensionality reduction (PCA), density estimation and anomaly detection. Self-supervised learning is a modern variant that turns unlabelled data into a supervised task by hiding part of the input and training the model to predict it.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Self-supervised learning is how large language models are pre-trained (predict the next token over trillions of words) and how embedding and vision models such as JEPA learn representations. Because it needs no labels, it scales to internet-sized data; the learned model is then adapted to specific tasks with fine-tuning or prompting.

## Example

Unsupervised - group customers into segments by purchase behaviour. Self-supervised - mask 15% of the words in a sentence and train a model to fill them in.

## Narrower concepts

- [Joint-embedding predictive architecture (JEPA)](/concepts/ai-ml/jepa.md)
- [Semantic search](/concepts/ai-ml/semantic-search.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Raschka, S., Liu, Y. and Mirjalili, V. (2022)**, *Machine Learning with PyTorch and Scikit-Learn*, ch. 5 PCA and dimensionality reduction, p. 139; ch. 10 Clustering Analysis, pp. 305-334: Hands-on PCA, t-SNE, k-means and hierarchical clustering.
- **Goodfellow, I., Bengio, Y. and Courville, A. (2016)**, *Deep Learning*, §5.8 Unsupervised Learning Algorithms, p. 146; ch. 14 Autoencoders, p. 502; ch. 15 Representation Learning, p. 526: Autoencoders and representation learning, the roots of self-supervised pre-training.

## Sources

1. Hastie, T., Tibshirani, R. and Friedman, J. (2009). *The Elements of Statistical Learning*, 2nd ed., ch. 14 Unsupervised Learning. Springer. <https://hastie.su.domains/ElemStatLearn/> — Clustering, PCA and other unsupervised methods.
2. Balestriero, R. et al. (2023). *A Cookbook of Self-Supervised Learning*. arXiv:2304.12210. <https://arxiv.org/abs/2304.12210> — Survey of self-supervised learning methods and why they work.
3. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*, ch. 5.8. MIT Press. <https://www.deeplearningbook.org/> — Unsupervised learning in the context of deep learning.
4. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 5 PCA and dimensionality reduction, p. 139; ch. 10 Clustering Analysis, pp. 305-334. Hands-on PCA, t-SNE, k-means and hierarchical clustering.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
