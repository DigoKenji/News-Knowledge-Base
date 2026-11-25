---
type: Concept
title: 'Regularization and the bias-variance trade-off'
description: 'Techniques that reduce overfitting by limiting how closely a model can fit its training data (penalties, dropout, early stopping, augmentation), framed by the trade-off between bias (too simple) and variance (too sensitive to the sample).'
tags:
- ai-ml
- classical-ml
domain: ai-ml
subdomain: classical-ml
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press'
  resource: 'https://www.deeplearningbook.org/'
  kind: book
  locator: '§5.2 Capacity, Overfitting and Underfitting, p. 110; §5.4 Estimators, Bias and Variance, p. 122; ch. 7 Regularization for Deep Learning, pp. 228-273 (early stopping §7.8 p. 246, dropout §7.12 p. 258)'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 4 L1 and L2 regularization, pp. 122-127; ch. 6 Diagnosing bias and variance problems with learning curves, pp. 180-185; k-fold cross-validation p. 175'
- title: 'Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). Mathematics for Machine Learning. Cambridge University Press'
  resource: 'https://mml-book.github.io/'
  kind: book
  locator: '§8.2 Empirical Risk Minimization (regularization), p. 258; §8.6 Model Selection, p. 283'
- title: 'Srivastava, N. et al. (2014). Dropout: A Simple Way to Prevent Neural Networks from Overfitting. JMLR 15'
  resource: 'https://jmlr.org/papers/v15/srivastava14a.html'
  kind: paper
---

# Regularization and the bias-variance trade-off

**Definition.** Goodfellow et al. define regularization as any modification to a learning algorithm intended to reduce its generalisation error but not its training error. The bias-variance view explains why it helps: expected error splits into bias (systematic error from a model too simple to capture the pattern), variance (error from the model changing a lot with each training sample) and irreducible noise. More capacity lowers bias and raises variance; regularization trades a little bias for a large cut in variance. Common techniques are L2 (weight decay) and L1 penalties on weight size (L1 also drives weights to exactly zero, giving sparse models), dropout (randomly switching off units during training), early stopping (halt when validation error starts rising), data augmentation and ensembling.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `classical-ml`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

This is the practical toolkit for the problem described in Overfitting and generalisation. Learning and validation curves (Raschka, ch. 6) are the diagnostic: a large gap between training and validation scores signals high variance, two low scores signal high bias. Regularization strength is a hyperparameter, so it is tuned with a validation set or k-fold cross-validation, never with the test set.

## Example

A credit-risk model scores 99% on training data and 78% on validation. Adding an L2 penalty and early stopping brings training to 91% and validation to 88%: slightly more bias, much less variance, better real-world performance.

## Broader concepts

- [Overfitting and generalisation](/concepts/ai-ml/overfitting.md)

## Related concepts

- [Model evaluation and metrics](/concepts/ai-ml/model-evaluation.md)

## Sources

1. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*. MIT Press. <https://www.deeplearningbook.org/> — §5.2 Capacity, Overfitting and Underfitting, p. 110; §5.4 Estimators, Bias and Variance, p. 122; ch. 7 Regularization for Deep Learning, pp. 228-273 (early stopping §7.8 p. 246, dropout §7.12 p. 258). Definition of regularization and the full catalogue of techniques for deep networks.
2. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 4 L1 and L2 regularization, pp. 122-127; ch. 6 Diagnosing bias and variance problems with learning curves, pp. 180-185; k-fold cross-validation p. 175. Practical diagnosis with learning and validation curves, and tuning with cross-validation.
3. Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). *Mathematics for Machine Learning*. Cambridge University Press. <https://mml-book.github.io/> — §8.2 Empirical Risk Minimization (regularization), p. 258; §8.6 Model Selection, p. 283. Regularization as a penalty term in empirical risk minimisation, and model selection.
4. Srivastava, N. et al. (2014). *Dropout: A Simple Way to Prevent Neural Networks from Overfitting*. JMLR 15. <https://jmlr.org/papers/v15/srivastava14a.html> — Original dropout paper.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
