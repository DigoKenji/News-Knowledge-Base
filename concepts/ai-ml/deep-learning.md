---
type: Concept
title: Neural networks and deep learning
description: Machine learning with neural networks - layers of simple connected units whose weights are learned from data - where "deep" means many layers that learn increasingly abstract representations automatically.
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
- title: LeCun, Y., Bengio, Y. and Hinton, G. (2015). Deep learning. Nature 521
  resource: 'https://doi.org/10.1038/nature14539'
  kind: paper
- title: Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press
  resource: 'https://www.deeplearningbook.org/'
  kind: book
- title: Krizhevsky, A., Sutskever, I. and Hinton, G. (2012). ImageNet Classification with Deep Convolutional Neural Networks. NeurIPS 2012
  resource: 'https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks'
  kind: paper
foundation_candidate: true
---

# Neural networks and deep learning

**Definition.** A neural network computes its output by passing the input through layers; each unit takes a weighted sum of its inputs and applies a non-linear function. Training adjusts the weights with gradient descent, using backpropagation to compute how much each weight contributed to the error. Deep learning uses networks with many layers, which learn features directly from raw data instead of relying on hand-crafted ones.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Deep learning took over vision (convolutional networks, 2012), speech and language (recurrent networks, then transformers from 2017). Its success came from more data, GPUs and better architectures. It needs large data and compute, and the models are hard to interpret, which drives the work on evaluation, calibration and safety.

## Example

An image classifier's first layers detect edges, middle layers detect shapes like eyes and wheels, and the last layers recognise "cat" or "car".

## Narrower concepts

- [Transformer and attention](/concepts/ai-ml/transformer.md)
- [Mixture of experts](/concepts/ai-ml/mixture-of-experts.md)
- [RBF network](/concepts/ai-ml/rbf-network.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. LeCun, Y., Bengio, Y. and Hinton, G. (2015). *Deep learning*. Nature 521. <https://doi.org/10.1038/nature14539> — Definition and overview of deep learning by its pioneers.
2. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*. MIT Press. <https://www.deeplearningbook.org/> — Standard textbook - feedforward networks, backpropagation, optimisation.
3. Krizhevsky, A., Sutskever, I. and Hinton, G. (2012). *ImageNet Classification with Deep Convolutional Neural Networks*. NeurIPS 2012. <https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks> — The 2012 result that started the deep learning era in vision.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
