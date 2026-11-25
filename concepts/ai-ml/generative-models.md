---
type: Concept
title: 'Generative models (VAE, GAN, diffusion, autoregressive)'
description: 'Models trained to produce new data that resembles their training data by learning its probability distribution - the main families are variational autoencoders, GANs, autoregressive models (including LLMs), normalizing flows, energy-based and diffusion models.'
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
- title: 'Foster, D. (2023). Generative Deep Learning, 2nd edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098134181'
  kind: book
  locator: 'ch. 1 Generative Modeling, pp. 3-22; ch. 3 VAEs p. 59; ch. 4 GANs p. 95; ch. 5 Autoregressive Models p. 129; ch. 6 Normalizing Flows p. 167; ch. 7 Energy-Based Models p. 189; ch. 8 Diffusion Models p. 205; ch. 9 Transformers p. 235'
- title: 'Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press'
  resource: 'https://www.deeplearningbook.org/'
  kind: book
  locator: 'ch. 20 Deep Generative Models, pp. 654-720 (evaluating generative models §20.14 p. 717)'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 17 Generative Adversarial Networks, pp. 589-630'
- title: 'Kingma, D. P. and Welling, M. (2014). Auto-Encoding Variational Bayes. ICLR 2014'
  resource: 'https://arxiv.org/abs/1312.6114'
  kind: paper
- title: 'Goodfellow, I. et al. (2014). Generative Adversarial Networks. NeurIPS 2014'
  resource: 'https://arxiv.org/abs/1406.2661'
  kind: paper
- title: 'Ho, J., Jain, A. and Abbeel, P. (2020). Denoising Diffusion Probabilistic Models. NeurIPS 2020'
  resource: 'https://arxiv.org/abs/2006.11239'
  kind: paper
---

# Generative models (VAE, GAN, diffusion, autoregressive)

**Definition.** Foster defines generative modeling as the branch of machine learning that trains a model to produce new data similar to a given dataset (p. 4). A discriminative model learns p(label | data); a generative model learns p(data), or how to sample from it. The families differ in how they do that: a variational autoencoder (VAE) learns a smooth latent space and decodes samples from it; a generative adversarial network (GAN) trains a generator against a discriminator that tries to spot fakes; an autoregressive model generates one element at a time conditioned on the previous ones (this is what a GPT-style LLM does with tokens); normalizing flows use invertible transformations; diffusion models learn to reverse a gradual noising process and now dominate image generation.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `deep-learning`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

"Generative AI" is this field at scale. LLMs are autoregressive transformers; text-to-image systems combine diffusion with text embeddings; multimodal models join several families. Knowing the families helps judge trade-offs: GANs are fast to sample but unstable to train, diffusion is stable and high quality but needs many denoising steps, autoregressive generation is sequential, which is why LLM inference optimisations (KV cache, speculative decoding) exist.

## Example

An image model starts from pure noise and, over 50 denoising steps guided by the prompt embedding for "a lighthouse at dusk", produces a new picture that is not in its training set.

## Broader concepts

- [Neural networks and deep learning](/concepts/ai-ml/deep-learning.md)

## Narrower concepts

- [Large language model and foundation model](/concepts/ai-ml/large-language-model.md)

## Related concepts

- [Unsupervised and self-supervised learning](/concepts/ai-ml/unsupervised-learning.md)

## Sources

1. Foster, D. (2023). *Generative Deep Learning, 2nd edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098134181> — ch. 1 Generative Modeling, pp. 3-22; ch. 3 VAEs p. 59; ch. 4 GANs p. 95; ch. 5 Autoregressive Models p. 129; ch. 6 Normalizing Flows p. 167; ch. 7 Energy-Based Models p. 189; ch. 8 Diffusion Models p. 205; ch. 9 Transformers p. 235. Organised around the six families, each with a working model.
2. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*. MIT Press. <https://www.deeplearningbook.org/> — ch. 20 Deep Generative Models, pp. 654-720 (evaluating generative models §20.14 p. 717). Theory of deep generative models up to 2016, including evaluation difficulties.
3. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 17 Generative Adversarial Networks, pp. 589-630. Builds and trains a GAN in PyTorch.
4. Kingma, D. P. and Welling, M. (2014). *Auto-Encoding Variational Bayes*. ICLR 2014. <https://arxiv.org/abs/1312.6114> — VAE paper.
5. Goodfellow, I. et al. (2014). *Generative Adversarial Networks*. NeurIPS 2014. <https://arxiv.org/abs/1406.2661> — GAN paper.
6. Ho, J., Jain, A. and Abbeel, P. (2020). *Denoising Diffusion Probabilistic Models*. NeurIPS 2020. <https://arxiv.org/abs/2006.11239> — DDPM, the diffusion model paper.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
