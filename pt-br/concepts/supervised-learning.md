---
type: Concept
title: Aprendizado supervisionado
description: Machine learning a partir de exemplos rotulados - cada entrada vem com a saída correta - para que o modelo aprenda a prever a saída para entradas novas; as duas tarefas principais são classificação (categorias) e regressão (números).
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
- title: Hastie, T., Tibshirani, R. and Friedman, J. (2009). The Elements of Statistical Learning, 2nd ed., ch. 2. Springer
  resource: 'https://hastie.su.domains/ElemStatLearn/'
  kind: book
- title: Google. Machine Learning Glossary - supervised machine learning
  resource: 'https://developers.google.com/machine-learning/glossary'
  kind: docs
- title: ISO/IEC (2022). ISO/IEC 22989 - Artificial intelligence concepts and terminology, ML section
  resource: 'https://www.iso.org/standard/74296.html'
  kind: standard
foundation_candidate: true
lang: pt-br
translation_of: /concepts/supervised-learning.md
---

# Aprendizado supervisionado

**Definição.** No aprendizado supervisionado os dados de treino são pares entrada-rótulo (x, y). O modelo aprende uma função de x para y minimizando uma função de perda que mede o quanto suas previsões se afastam dos rótulos verdadeiros, e depois é avaliado em dados separados que não viu.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

É a família de ML mais usada na indústria: detecção de fraude, previsão de churn, classificação de imagens, previsão de demanda. Os algoritmos vão de modelos lineares, árvores de decisão e gradient boosting a SVMs e redes neurais. Seu principal custo são os rótulos: são caros, podem ter ruído ou viés, e o modelo só é tão bom quanto eles.

## Exemplo

Classificação - prever se uma transação é fraude (sim/não). Regressão - prever as vendas de amanhã em reais.

## Conceitos mais específicos

- [Máquina de vetores de suporte (SVM)](/pt-br/concepts/svm.md)
- [Calibração](/pt-br/concepts/calibration.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. Hastie, T., Tibshirani, R. and Friedman, J. (2009). *The Elements of Statistical Learning*, 2nd ed., ch. 2. Springer. <https://hastie.su.domains/ElemStatLearn/> — Tratamento formal de aprendizado supervisionado, classificação e regressão.
2. Google. *Machine Learning Glossary* - supervised machine learning. <https://developers.google.com/machine-learning/glossary> — Definições práticas de rótulos, features, classificação e regressão.
3. ISO/IEC (2022). *ISO/IEC 22989 - Artificial intelligence concepts and terminology*, ML section. <https://www.iso.org/standard/74296.html> — Termo padronizado (pago; citado, não transcrito).

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
