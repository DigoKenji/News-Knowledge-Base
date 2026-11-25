---
type: Concept
title: Supervised learning
description: Machine learning from labelled examples - each input comes with the correct output - so the model learns to predict the output for new inputs; the two main tasks are classification (categories) and regression (numbers).
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
- title: Hastie, T., Tibshirani, R. and Friedman, J. (2009). The Elements of Statistical Learning, 2nd ed., ch. 2. Springer
  resource: 'https://hastie.su.domains/ElemStatLearn/'
  kind: book
- title: Google. Machine Learning Glossary - supervised machine learning
  resource: 'https://developers.google.com/machine-learning/glossary'
  kind: docs
- title: ISO/IEC (2022). ISO/IEC 22989 - Artificial intelligence concepts and terminology, ML section
  resource: 'https://www.iso.org/standard/74296.html'
  kind: standard
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 2-3, pp. 19-104 (perceptron, Adaline, logistic regression, SVM, trees, k-NN)'
- title: 'Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press'
  resource: 'https://www.deeplearningbook.org/'
  kind: book
  locator: '§5.7 Supervised Learning Algorithms, p. 140'
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Supervised learning

**Definition.** In supervised learning the training data consists of input-label pairs (x, y). The model learns a function from x to y by minimising a loss that measures how far its predictions are from the true labels, then is evaluated on held-out data it has not seen.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

It is the most widely used ML family in industry: fraud detection, churn prediction, image classification, demand forecasting. Algorithms range from linear models, decision trees and gradient boosting to SVMs and neural networks. Its main cost is labels: they are expensive, can be noisy or biased, and the model can only be as good as they are.

## Example

Classification - predict whether a transaction is fraud (yes/no). Regression - predict tomorrow's sales in reais.

## Narrower concepts

- [Support vector machine](/concepts/ai-ml/svm.md)
- [Calibration](/concepts/ai-ml/calibration.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Raschka, S., Liu, Y. and Mirjalili, V. (2022)**, *Machine Learning with PyTorch and Scikit-Learn*, ch. 2-3, pp. 19-104 (perceptron, Adaline, logistic regression, SVM, trees, k-NN): Builds the main supervised classifiers step by step and compares them in scikit-learn.
- **Goodfellow, I., Bengio, Y. and Courville, A. (2016)**, *Deep Learning*, §5.7 Supervised Learning Algorithms, p. 140: Short formal overview of supervised learning as estimating p(y | x).

## Sources

1. Hastie, T., Tibshirani, R. and Friedman, J. (2009). *The Elements of Statistical Learning*, 2nd ed., ch. 2. Springer. <https://hastie.su.domains/ElemStatLearn/> — Formal treatment of supervised learning, classification and regression.
2. Google. *Machine Learning Glossary* - supervised machine learning. <https://developers.google.com/machine-learning/glossary> — Practitioner definitions of labels, features, classification and regression.
3. ISO/IEC (2022). *ISO/IEC 22989 - Artificial intelligence concepts and terminology*, ML section. <https://www.iso.org/standard/74296.html> — Standard term (paywalled; cited, not quoted).
4. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 2-3, pp. 19-104 (perceptron, Adaline, logistic regression, SVM, trees, k-NN). Builds the main supervised classifiers step by step and compares them in scikit-learn.
5. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*. MIT Press. <https://www.deeplearningbook.org/> — §5.7 Supervised Learning Algorithms, p. 140. Short formal overview of supervised learning as estimating p(y | x).

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
