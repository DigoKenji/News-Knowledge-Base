---
type: Concept
title: Support vector machine
description: 'A classifier that finds the boundary with the widest margin between classes, using kernel functions to separate data that is not linearly separable.'
tags:
- ai-ml
- classical-ml
domain: ai-ml
subdomain: classical-ml
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Cortes, C. and Vapnik, V. (1995). Support-Vector Networks. Machine Learning 20'
  resource: 'https://doi.org/10.1007/BF00994018'
  kind: paper
- title: 'Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press'
  resource: 'https://www.deeplearningbook.org/'
  kind: book
  locator: '§5.7.2 Support Vector Machines'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 3: Maximum margin classification with support vector machines; RBF kernel SVM, p. 83'
- title: 'Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). Mathematics for Machine Learning. Cambridge University Press'
  resource: 'https://mml-book.github.io/'
  kind: book
  locator: 'ch. 12 Classification with Support Vector Machines'
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Support vector machine

**Definition.** A support vector machine separates two classes with the hyperplane that has the largest margin to the nearest training points, the support vectors. A soft margin with penalty C allows some points on the wrong side, trading margin width against errors. Through the kernel trick (for example the RBF kernel) the same optimisation finds non-linear boundaries by working with similarities between points instead of explicit features.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `classical-ml`. Expanded on 2026-11-26 with independent sources.

## How it fits

SVMs were the leading classifier before deep learning for small and medium tabular or text datasets and are still a strong baseline. They output a decision score rather than a probability, and kernel SVMs scale poorly to very large datasets.

## Example

A spam filter on TF-IDF features trains a linear SVM; only the few hundred messages closest to the boundary end up defining it.

## Sources

1. Cortes, C. and Vapnik, V. (1995). Support-Vector Networks. Machine Learning 20. <https://doi.org/10.1007/BF00994018> — Soft-margin SVM, the standard formulation.
2. Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press. <https://www.deeplearningbook.org/> — §5.7.2 Support Vector Machines. Margin, kernel trick and support vectors.
3. Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 3: Maximum margin classification with support vector machines; RBF kernel SVM, p. 83. Practical SVMs in scikit-learn, C and gamma.
4. Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). Mathematics for Machine Learning. Cambridge University Press. <https://mml-book.github.io/> — ch. 12 Classification with Support Vector Machines. Derivation of the primal and dual problems.

## Notes
Notes tagged with this concept are listed in the concept index after each run.
