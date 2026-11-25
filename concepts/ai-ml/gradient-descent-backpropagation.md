---
type: Concept
title: 'Gradient descent and backpropagation'
description: 'How neural networks learn - gradient descent repeatedly nudges the weights against the gradient of the loss, and backpropagation computes that gradient efficiently with the chain rule, layer by layer from the output back.'
tags:
- ai-ml
- deep-learning
domain: ai-ml
subdomain: deep-learning
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press'
  resource: 'https://www.deeplearningbook.org/'
  kind: book
  locator: '§4.3 Gradient-Based Optimization, p. 82; §6.5 Back-Propagation, pp. 204-224; §8.3 Basic Algorithms (SGD, momentum), p. 294'
- title: 'Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). Mathematics for Machine Learning. Cambridge University Press'
  resource: 'https://mml-book.github.io/'
  kind: book
  locator: '§5.6 Backpropagation and Automatic Differentiation, p. 159; §7.1 Optimization Using Gradient Descent, p. 227'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 2 Minimizing loss functions with gradient descent, p. 37 (SGD p. 45); ch. 11 Training neural networks via backpropagation, pp. 362-366'
- title: 'Rumelhart, D. E., Hinton, G. E. and Williams, R. J. (1986). Learning representations by back-propagating errors. Nature 323'
  resource: 'https://doi.org/10.1038/323533a0'
  kind: paper
---

# Gradient descent and backpropagation

**Definition.** Training a model means minimising a loss function over its parameters. Gradient descent does this iteratively: compute the gradient of the loss with respect to every parameter and take a small step (the learning rate) in the opposite direction. Stochastic gradient descent (SGD) estimates the gradient on a small random mini-batch instead of the full dataset, which is what makes training on large data feasible. Backpropagation is the algorithm that computes those gradients in a network: a forward pass stores intermediate values, then a backward pass applies the chain rule from the loss back to the first layer, reusing work so the cost is about the same as one forward pass. It is a special case of reverse-mode automatic differentiation, which is what PyTorch and similar frameworks implement.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `deep-learning`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Every deep learning model, from a small classifier to an LLM, is trained this way; fine-tuning and LoRA are the same loop over fewer or different parameters. Most practical trouble lives here: a learning rate that is too high diverges, one too low stalls; very deep or recurrent networks suffer vanishing or exploding gradients, which motivated ReLU activations, careful initialisation, normalisation layers, residual connections and adaptive optimisers such as Adam. Memory for training is dominated by what backpropagation must keep: activations, gradients and optimiser state, which is why fine-tuning needs far more memory than inference (Huyen, ch. 7).

## Example

A spam classifier predicts 0.9 for a legitimate email. The loss is high, backpropagation tells each weight how much it pushed the prediction towards "spam", and SGD shifts each weight slightly the other way; after many mini-batches the errors shrink.

## Broader concepts

- [Neural networks and deep learning](/concepts/ai-ml/deep-learning.md)

## Narrower concepts

- [Low-rank adaptation (LoRA)](/concepts/ai-ml/lora.md)

## Related concepts

- [Fine-tuning](/concepts/ai-ml/fine-tuning.md)

## Sources

1. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*. MIT Press. <https://www.deeplearningbook.org/> — §4.3 Gradient-Based Optimization, p. 82; §6.5 Back-Propagation, pp. 204-224; §8.3 Basic Algorithms (SGD, momentum), p. 294. Formal treatment of gradient-based learning, back-propagation and the optimisers built on SGD.
2. Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). *Mathematics for Machine Learning*. Cambridge University Press. <https://mml-book.github.io/> — §5.6 Backpropagation and Automatic Differentiation, p. 159; §7.1 Optimization Using Gradient Descent, p. 227. The calculus behind it: gradients, the chain rule, and why reverse-mode autodiff is efficient.
3. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 2 Minimizing loss functions with gradient descent, p. 37 (SGD p. 45); ch. 11 Training neural networks via backpropagation, pp. 362-366. Hands-on version: gradient descent from scratch, then backpropagation coded by hand for a multilayer network.
4. Rumelhart, D. E., Hinton, G. E. and Williams, R. J. (1986). *Learning representations by back-propagating errors*. Nature 323. <https://doi.org/10.1038/323533a0> — The 1986 paper that popularised backpropagation for multilayer networks.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
