---
type: Concept
title: 'Modelos generativos (VAE, GAN, difusão, autorregressivos)'
description: 'Modelos treinados para produzir dados novos parecidos com os de treino, aprendendo sua distribuição de probabilidade - as principais famílias são autoencoders variacionais, GANs, modelos autorregressivos (incluindo LLMs), normalizing flows, modelos baseados em energia e modelos de difusão.'
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
lang: pt-br
translation_of: /concepts/ai-ml/generative-models.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Modelos generativos (VAE, GAN, difusão, autorregressivos)

**Definição.** Foster define modelagem generativa como o ramo do aprendizado de máquina que treina um modelo para produzir dados novos semelhantes a um conjunto de dados (p. 4). Um modelo discriminativo aprende p(rótulo | dados); um generativo aprende p(dados), ou como amostrar dela. As famílias diferem no modo de fazer isso: um autoencoder variacional (VAE) aprende um espaço latente suave e decodifica amostras dele; uma rede adversária generativa (GAN) treina um gerador contra um discriminador que tenta identificar falsificações; um modelo autorregressivo gera um elemento por vez condicionado aos anteriores (é o que um LLM no estilo GPT faz com tokens); normalizing flows usam transformações inversíveis; modelos de difusão aprendem a reverter um processo gradual de adição de ruído e hoje dominam a geração de imagens.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `deep-learning`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

"IA generativa" é este campo em escala. LLMs são transformers autorregressivos; sistemas texto-para-imagem combinam difusão com embeddings de texto; modelos multimodais juntam várias famílias. Conhecer as famílias ajuda a avaliar trade-offs: GANs amostram rápido mas são instáveis de treinar, difusão é estável e de alta qualidade mas precisa de muitos passos de remoção de ruído, a geração autorregressiva é sequencial, e por isso existem otimizações de inferência de LLM (KV cache, decodificação especulativa).

## Exemplo

Um modelo de imagem parte de ruído puro e, em 50 passos de remoção de ruído guiados pelo embedding do prompt "um farol ao entardecer", produz uma imagem nova que não está no seu conjunto de treino.

## Conceitos mais amplos

- [Redes neurais e deep learning](/pt-br/concepts/ai-ml/deep-learning.md)

## Conceitos mais específicos

- [Grande modelo de linguagem (LLM) e modelo de fundação](/pt-br/concepts/ai-ml/large-language-model.md)

## Conceitos relacionados

- [Aprendizado não supervisionado e auto-supervisionado](/pt-br/concepts/ai-ml/unsupervised-learning.md)

## Fontes

1. Foster, D. (2023). *Generative Deep Learning, 2nd edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098134181> — ch. 1 Generative Modeling, pp. 3-22; ch. 3 VAEs p. 59; ch. 4 GANs p. 95; ch. 5 Autoregressive Models p. 129; ch. 6 Normalizing Flows p. 167; ch. 7 Energy-Based Models p. 189; ch. 8 Diffusion Models p. 205; ch. 9 Transformers p. 235. Organizado em torno das seis famílias, cada uma com um modelo funcional.
2. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*. MIT Press. <https://www.deeplearningbook.org/> — ch. 20 Deep Generative Models, pp. 654-720 (evaluating generative models §20.14 p. 717). Teoria dos modelos generativos profundos até 2016, incluindo as dificuldades de avaliação.
3. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 17 Generative Adversarial Networks, pp. 589-630. Constrói e treina uma GAN em PyTorch.
4. Kingma, D. P. and Welling, M. (2014). *Auto-Encoding Variational Bayes*. ICLR 2014. <https://arxiv.org/abs/1312.6114> — Artigo do VAE.
5. Goodfellow, I. et al. (2014). *Generative Adversarial Networks*. NeurIPS 2014. <https://arxiv.org/abs/1406.2661> — Artigo da GAN.
6. Ho, J., Jain, A. and Abbeel, P. (2020). *Denoising Diffusion Probabilistic Models*. NeurIPS 2020. <https://arxiv.org/abs/2006.11239> — DDPM, o artigo dos modelos de difusão.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
