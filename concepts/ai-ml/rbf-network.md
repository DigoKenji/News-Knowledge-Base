---
type: Concept
title: RBF network
description: 'A neural network with one hidden layer of radial basis functions (usually Gaussians centred on prototype points) and a linear output layer, used for function approximation and classification.'
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
- title: 'Broomhead, D. S. and Lowe, D. (1988). Multivariable Functional Interpolation and Adaptive Networks. Complex Systems 2(3)'
  resource: 'https://www.complex-systems.com/abstracts/v02_i03_a05/'
  kind: paper
- title: 'Moody, J. and Darken, C. J. (1989). Fast Learning in Networks of Locally-Tuned Processing Units. Neural Computation 1(2)'
  resource: 'https://doi.org/10.1162/neco.1989.1.2.281'
  kind: paper
- title: 'Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press'
  resource: 'https://www.deeplearningbook.org/'
  kind: book
  locator: '§6.3.3 Other Hidden Units (RBF units); §5.7.2 (RBF kernel)'
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# RBF network

**Definition.** A radial basis function network has one hidden layer whose units respond to how close the input is to a centre: each computes a radial function, usually a Gaussian exp(-||x - c||^2 / 2s^2), and the output is a weighted sum of these responses. Centres are often chosen by clustering, after which the output weights can be fitted by linear least squares, which makes training fast.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `classical-ml`. Expanded on 2026-11-26 with independent sources.

## How it fits

RBF networks are universal approximators with local units, closely related to kernel methods: the RBF kernel used in SVMs measures the same kind of similarity. Goodfellow et al. list RBF units among hidden-unit types but note they saturate to zero far from the centres, which makes them hard to train in deep networks.

## Example

To model a sensor's calibration curve, 20 Gaussian units are centred on k-means clusters of the readings and a linear layer maps their activations to the corrected value.

## Sources

1. Broomhead, D. S. and Lowe, D. (1988). Multivariable Functional Interpolation and Adaptive Networks. Complex Systems 2(3). <https://www.complex-systems.com/abstracts/v02_i03_a05/> — Original formulation of RBF networks.
2. Moody, J. and Darken, C. J. (1989). Fast Learning in Networks of Locally-Tuned Processing Units. Neural Computation 1(2). <https://doi.org/10.1162/neco.1989.1.2.281> — Clustering for centres plus fast linear training.
3. Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press. <https://www.deeplearningbook.org/> — §6.3.3 Other Hidden Units (RBF units); §5.7.2 (RBF kernel). RBF units and why they are rarely used in deep nets.

## Notes
Notes tagged with this concept are listed in the concept index after each run.
