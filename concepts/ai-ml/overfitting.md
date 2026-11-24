---
type: Concept
title: Overfitting and generalisation
description: Generalisation is a model's ability to perform well on new data it was not trained on; overfitting is when a model learns its training data too closely, including noise, so it scores well in training but poorly on new data.
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
- title: Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning, ch. 5.2 Capacity, Overfitting and Underfitting. MIT Press
  resource: 'https://www.deeplearningbook.org/'
  kind: book
- title: Hastie, T., Tibshirani, R. and Friedman, J. (2009). The Elements of Statistical Learning, 2nd ed., ch. 7 Model Assessment and Selection. Springer
  resource: 'https://hastie.su.domains/ElemStatLearn/'
  kind: book
- title: 'Zhang, C. et al. (2017). Understanding deep learning requires rethinking generalization. ICLR 2017, arXiv:1611.03530'
  resource: 'https://arxiv.org/abs/1611.03530'
  kind: paper
---

# Overfitting and generalisation

**Definition.** The goal of ML is low error on unseen data, not on the training set. Overfitting shows up as a gap between training and validation performance; underfitting is the opposite problem, a model too simple to capture the pattern. The classic framing is the bias-variance trade-off.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Standard defences: split data into train, validation and test sets; cross-validation; regularisation (weight penalties, dropout); early stopping; more data. Deep networks complicate the classic story, since they can memorise random labels yet still generalise on real data (Zhang et al.). Generalisation also fails over time when the world changes, which is data and concept drift.

## Example

A model reaches 99% accuracy on training emails but 70% on next month's emails because it memorised specific sender addresses instead of learning what spam looks like.

## Narrower concepts

- [Concept drift](/concepts/ai-ml/concept-drift.md)
- [Data drift](/concepts/ai-ml/data-drift.md)

## Sources

1. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*, ch. 5.2 Capacity, Overfitting and Underfitting. MIT Press. <https://www.deeplearningbook.org/> — Definitions and capacity view.
2. Hastie, T., Tibshirani, R. and Friedman, J. (2009). *The Elements of Statistical Learning*, 2nd ed., ch. 7 Model Assessment and Selection. Springer. <https://hastie.su.domains/ElemStatLearn/> — Bias-variance trade-off and cross-validation.
3. Zhang, C. et al. (2017). *Understanding deep learning requires rethinking generalization*. ICLR 2017, arXiv:1611.03530. <https://arxiv.org/abs/1611.03530> — Deep networks can memorise random labels; limits of the classic view.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
