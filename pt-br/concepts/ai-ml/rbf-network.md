---
type: Concept
title: Rede RBF
description: 'Uma rede neural com uma camada oculta de funções de base radial (geralmente gaussianas centradas em pontos protótipo) e uma camada de saída linear, usada para aproximação de funções e classificação.'
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
lang: pt-br
translation_of: /concepts/ai-ml/rbf-network.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Rede RBF

**Definição.** Uma rede de funções de base radial tem uma camada oculta cujas unidades respondem a quão perto a entrada está de um centro: cada uma calcula uma função radial, geralmente uma gaussiana exp(-||x - c||^2 / 2s^2), e a saída é uma soma ponderada dessas respostas. Os centros costumam ser escolhidos por clustering, e então os pesos de saída podem ser ajustados por mínimos quadrados lineares, o que torna o treino rápido.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `classical-ml`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Redes RBF são aproximadores universais com unidades locais, muito ligadas aos métodos de kernel: o kernel RBF usado em SVMs mede o mesmo tipo de similaridade. Goodfellow et al. listam unidades RBF entre os tipos de unidade oculta, mas observam que elas saturam em zero longe dos centros, o que dificulta treiná-las em redes profundas.

## Exemplo

Para modelar a curva de calibração de um sensor, 20 unidades gaussianas são centradas em clusters k-means das leituras e uma camada linear mapeia suas ativações para o valor corrigido.

## Fontes

1. Broomhead, D. S. and Lowe, D. (1988). Multivariable Functional Interpolation and Adaptive Networks. Complex Systems 2(3). <https://www.complex-systems.com/abstracts/v02_i03_a05/> — Formulação original das redes RBF.
2. Moody, J. and Darken, C. J. (1989). Fast Learning in Networks of Locally-Tuned Processing Units. Neural Computation 1(2). <https://doi.org/10.1162/neco.1989.1.2.281> — Clustering para os centros e treino linear rápido.
3. Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press. <https://www.deeplearningbook.org/> — §6.3.3 Other Hidden Units (RBF units); §5.7.2 (RBF kernel). Unidades RBF e por que são pouco usadas em redes profundas.

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
