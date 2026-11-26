---
type: Concept
title: Machine learning
description: The branch of AI in which systems improve at a task by learning patterns from data, instead of being explicitly programmed with rules.
tags:
- ai-ml
- ml-fundamentals
- core
domain: ai-ml
subdomain: ml-fundamentals
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: Mitchell, T. (1997). Machine Learning, ch. 1. McGraw-Hill
  resource: 'https://www.cs.cmu.edu/~tom/mlbook.html'
  kind: book
- title: ISO/IEC (2022). ISO/IEC 23053 - Framework for AI systems using machine learning
  resource: 'https://www.iso.org/standard/74438.html'
  kind: standard
- title: Google. Machine Learning Crash Course
  resource: 'https://developers.google.com/machine-learning/crash-course'
  kind: docs
- title: 'Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). Mathematics for Machine Learning. Cambridge University Press'
  resource: 'https://mml-book.github.io/'
  kind: book
  locator: 'Part II, ch. 8 When Models Meet Data, pp. 251-288'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 1 Giving Computers the Ability to Learn from Data, pp. 1-17'
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Machine learning

**Definition.** Tom Mitchell's classic definition: a program learns from experience E with respect to a task T and performance measure P if its performance at T, measured by P, improves with E. In practice, ML fits the parameters of a model to training data so it can make predictions or decisions on new data. ISO/IEC 23053 describes the framework for ML-based AI systems.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `ml-fundamentals`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

The main families are supervised, unsupervised (including self-supervised) and reinforcement learning. Every ML system has a training phase (learning the parameters) and an inference phase (using them). The core risk is generalisation: doing well on training data but badly on new data (overfitting), or degrading when the data changes (data and concept drift).

## Example

Instead of writing rules for which emails are spam, give a model 100,000 emails labelled spam or not and let it learn which features predict spam.

## Narrower concepts

- [Supervised learning](/concepts/ai-ml/supervised-learning.md)
- [Unsupervised and self-supervised learning](/concepts/ai-ml/unsupervised-learning.md)
- [Reinforcement learning](/concepts/ai-ml/reinforcement-learning.md)
- [Neural networks and deep learning](/concepts/ai-ml/deep-learning.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020)**, *Mathematics for Machine Learning*, Part II, ch. 8 When Models Meet Data, pp. 251-288: Frames learning as data + model + learning: empirical risk minimisation, parameter estimation and model selection.
- **Raschka, S., Liu, Y. and Mirjalili, V. (2022)**, *Machine Learning with PyTorch and Scikit-Learn*, ch. 1 Giving Computers the Ability to Learn from Data, pp. 1-17: The three types of learning and the end-to-end workflow (preprocessing, training, evaluation, prediction).

## Sources

1. Mitchell, T. (1997). *Machine Learning*, ch. 1. McGraw-Hill. <https://www.cs.cmu.edu/~tom/mlbook.html> — The E-T-P definition of learning.
2. ISO/IEC (2022). *ISO/IEC 23053 - Framework for AI systems using machine learning*. <https://www.iso.org/standard/74438.html> — Standard framework and terms for ML systems (paywalled; cited, not quoted).
3. Google. *Machine Learning Crash Course*. <https://developers.google.com/machine-learning/crash-course> — Practical introduction - models, training, loss, generalisation.
4. Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). *Mathematics for Machine Learning*. Cambridge University Press. <https://mml-book.github.io/> — Part II, ch. 8 When Models Meet Data, pp. 251-288. Frames learning as data + model + learning: empirical risk minimisation, parameter estimation and model selection.
5. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 1 Giving Computers the Ability to Learn from Data, pp. 1-17. The three types of learning and the end-to-end workflow (preprocessing, training, evaluation, prediction).

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
