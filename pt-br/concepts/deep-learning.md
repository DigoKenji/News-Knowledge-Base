---
type: Concept
title: Redes neurais e deep learning
description: Machine learning com redes neurais - camadas de unidades simples conectadas cujos pesos são aprendidos com dados - em que "profundo" significa muitas camadas que aprendem representações cada vez mais abstratas automaticamente.
tags:
- ai-ml
- core
domain: ai-ml
subdomain: core
status: draft
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
lang: pt-br
translation_of: /concepts/deep-learning.md
---

# Redes neurais e deep learning

**Definição.** Uma rede neural calcula sua saída passando a entrada por camadas; cada unidade faz uma soma ponderada das entradas e aplica uma função não linear. O treino ajusta os pesos com gradiente descendente, usando backpropagation para calcular quanto cada peso contribuiu para o erro. Deep learning usa redes com muitas camadas, que aprendem características diretamente dos dados brutos em vez de depender de características feitas à mão.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O deep learning dominou visão (redes convolucionais, 2012), fala e linguagem (redes recorrentes, depois transformers a partir de 2017). O sucesso veio de mais dados, GPUs e arquiteturas melhores. Exige muitos dados e muito cálculo, e os modelos são difíceis de interpretar, o que motiva o trabalho em avaliação, calibração e segurança.

## Exemplo

As primeiras camadas de um classificador de imagens detectam bordas, as camadas do meio detectam formas como olhos e rodas, e as últimas reconhecem "gato" ou "carro".

## Conceitos mais específicos

- [Transformer e atenção](/pt-br/concepts/transformer.md)
- [Mistura de especialistas](/pt-br/concepts/mixture-of-experts.md)
- [Rede RBF](/pt-br/concepts/rbf-network.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. LeCun, Y., Bengio, Y. and Hinton, G. (2015). *Deep learning*. Nature 521. <https://doi.org/10.1038/nature14539> — Definição e visão geral do deep learning pelos seus pioneiros.
2. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*. MIT Press. <https://www.deeplearningbook.org/> — Livro-texto padrão - redes feedforward, backpropagation, otimização.
3. Krizhevsky, A., Sutskever, I. and Hinton, G. (2012). *ImageNet Classification with Deep Convolutional Neural Networks*. NeurIPS 2012. <https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks> — O resultado de 2012 que iniciou a era do deep learning em visão.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
