---
type: Concept
title: Aprendizado não supervisionado e auto-supervisionado
description: Machine learning a partir de dados sem rótulos fornecidos por humanos - o aprendizado não supervisionado encontra estrutura como agrupamentos ou representações compactas, e o auto-supervisionado cria seus próprios rótulos a partir dos dados, por exemplo prevendo uma palavra oculta.
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
- title: Hastie, T., Tibshirani, R. and Friedman, J. (2009). The Elements of Statistical Learning, 2nd ed., ch. 14 Unsupervised Learning. Springer
  resource: 'https://hastie.su.domains/ElemStatLearn/'
  kind: book
- title: 'Balestriero, R. et al. (2023). A Cookbook of Self-Supervised Learning. arXiv:2304.12210'
  resource: 'https://arxiv.org/abs/2304.12210'
  kind: paper
- title: Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning, ch. 5.8. MIT Press
  resource: 'https://www.deeplearningbook.org/'
  kind: book
foundation_candidate: true
lang: pt-br
translation_of: /concepts/ai-ml/unsupervised-learning.md
---

# Aprendizado não supervisionado e auto-supervisionado

**Definição.** O aprendizado não supervisionado modela a estrutura de dados sem rótulos: agrupamento (k-means), redução de dimensionalidade (PCA), estimação de densidade e detecção de anomalias. O aprendizado auto-supervisionado é uma variante moderna que transforma dados sem rótulos numa tarefa supervisionada, escondendo parte da entrada e treinando o modelo para prevê-la.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O aprendizado auto-supervisionado é como grandes modelos de linguagem são pré-treinados (prever o próximo token em trilhões de palavras) e como modelos de embedding e de visão como a JEPA aprendem representações. Como não precisa de rótulos, escala para dados do tamanho da internet; o modelo aprendido é depois adaptado a tarefas específicas com fine-tuning ou prompts.

## Exemplo

Não supervisionado - agrupar clientes em segmentos pelo comportamento de compra. Auto-supervisionado - mascarar 15% das palavras de uma frase e treinar um modelo para preenchê-las.

## Conceitos mais específicos

- [Arquitetura preditiva de embedding conjunto (JEPA)](/pt-br/concepts/ai-ml/jepa.md)
- [Busca semântica](/pt-br/concepts/ai-ml/semantic-search.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. Hastie, T., Tibshirani, R. and Friedman, J. (2009). *The Elements of Statistical Learning*, 2nd ed., ch. 14 Unsupervised Learning. Springer. <https://hastie.su.domains/ElemStatLearn/> — Agrupamento, PCA e outros métodos não supervisionados.
2. Balestriero, R. et al. (2023). *A Cookbook of Self-Supervised Learning*. arXiv:2304.12210. <https://arxiv.org/abs/2304.12210> — Revisão de métodos de aprendizado auto-supervisionado e por que funcionam.
3. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*, ch. 5.8. MIT Press. <https://www.deeplearningbook.org/> — Aprendizado não supervisionado no contexto do deep learning.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
