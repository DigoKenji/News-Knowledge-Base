---
type: Concept
title: Overfitting e generalização
description: Generalização é a capacidade de um modelo de ir bem em dados novos com os quais não foi treinado; overfitting é quando um modelo aprende os dados de treino de perto demais, incluindo o ruído, e por isso vai bem no treino mas mal em dados novos.
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
- title: Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning, ch. 5.2 Capacity, Overfitting and Underfitting. MIT Press
  resource: 'https://www.deeplearningbook.org/'
  kind: book
- title: Hastie, T., Tibshirani, R. and Friedman, J. (2009). The Elements of Statistical Learning, 2nd ed., ch. 7 Model Assessment and Selection. Springer
  resource: 'https://hastie.su.domains/ElemStatLearn/'
  kind: book
- title: 'Zhang, C. et al. (2017). Understanding deep learning requires rethinking generalization. ICLR 2017, arXiv:1611.03530'
  resource: 'https://arxiv.org/abs/1611.03530'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/overfitting.md
---

# Overfitting e generalização

**Definição.** O objetivo do ML é ter erro baixo em dados não vistos, não no conjunto de treino. O overfitting aparece como uma distância entre o desempenho no treino e na validação; underfitting é o problema oposto, um modelo simples demais para captar o padrão. A visão clássica é o trade-off entre viés e variância.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Defesas padrão: dividir os dados em treino, validação e teste; validação cruzada; regularização (penalidade nos pesos, dropout); parada antecipada; mais dados. Redes profundas complicam a história clássica, já que conseguem memorizar rótulos aleatórios e ainda assim generalizar em dados reais (Zhang et al.). A generalização também falha com o tempo quando o mundo muda, que é o data drift e o concept drift.

## Exemplo

Um modelo chega a 99% de acurácia nos e-mails de treino mas 70% nos e-mails do mês seguinte porque memorizou endereços de remetentes específicos em vez de aprender como é um spam.

## Conceitos mais específicos

- [Drift de conceito](/pt-br/concepts/ai-ml/concept-drift.md)
- [Drift de dados](/pt-br/concepts/ai-ml/data-drift.md)

## Fontes

1. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*, ch. 5.2 Capacity, Overfitting and Underfitting. MIT Press. <https://www.deeplearningbook.org/> — Definições e a visão por capacidade.
2. Hastie, T., Tibshirani, R. and Friedman, J. (2009). *The Elements of Statistical Learning*, 2nd ed., ch. 7 Model Assessment and Selection. Springer. <https://hastie.su.domains/ElemStatLearn/> — Trade-off viés-variância e validação cruzada.
3. Zhang, C. et al. (2017). *Understanding deep learning requires rethinking generalization*. ICLR 2017, arXiv:1611.03530. <https://arxiv.org/abs/1611.03530> — Redes profundas conseguem memorizar rótulos aleatórios; limites da visão clássica.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
