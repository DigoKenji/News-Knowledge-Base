---
type: Concept
title: Aprendizado supervisionado
description: 'Aprendizado de máquina a partir de exemplos rotulados - cada entrada vem com a saída correta - para que o modelo aprenda a prever a saída de novas entradas.'
tags:
- ai-ml
- ml-fundamentals
- core
domain: ai-ml
subdomain: ml-fundamentals
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
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
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 2-3, pp. 19-104 (perceptron, Adaline, logistic regression, SVM, trees, k-NN)'
- title: 'Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press'
  resource: 'https://www.deeplearningbook.org/'
  kind: book
  locator: '§5.7 Supervised Learning Algorithms, p. 140'
foundation_candidate: true
lang: pt-br
translation_of: /concepts/ai-ml/supervised-learning.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
---

# Aprendizado supervisionado

**Definição.** No aprendizado supervisionado os dados de treino são pares entrada-rótulo (x, y). O modelo aprende uma função de x para y minimizando uma função de perda que mede o quanto suas previsões se afastam dos rótulos verdadeiros, e depois é avaliado em dados separados que não viu.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `ml-fundamentals`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

É a família de ML mais usada na indústria: detecção de fraude, previsão de churn, classificação de imagens, previsão de demanda. Os algoritmos vão de modelos lineares, árvores de decisão e gradient boosting a SVMs e redes neurais. Seu principal custo são os rótulos: são caros, podem ter ruído ou viés, e o modelo só é tão bom quanto eles.

## Exemplo

Classificação - prever se uma transação é fraude (sim/não). Regressão - prever as vendas de amanhã em reais.

## Conceitos mais específicos

- [Máquina de vetores de suporte (SVM)](/pt-br/concepts/ai-ml/svm.md)
- [Calibração](/pt-br/concepts/ai-ml/calibration.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Raschka, S., Liu, Y. and Mirjalili, V. (2022)**, *Machine Learning with PyTorch and Scikit-Learn*, ch. 2-3, pp. 19-104 (perceptron, Adaline, logistic regression, SVM, trees, k-NN): Constrói passo a passo os principais classificadores supervisionados e os compara no scikit-learn.
- **Goodfellow, I., Bengio, Y. and Courville, A. (2016)**, *Deep Learning*, §5.7 Supervised Learning Algorithms, p. 140: Visão formal curta do aprendizado supervisionado como estimativa de p(y | x).

## Fontes

1. Hastie, T., Tibshirani, R. and Friedman, J. (2009). *The Elements of Statistical Learning*, 2nd ed., ch. 2. Springer. <https://hastie.su.domains/ElemStatLearn/> — Tratamento formal de aprendizado supervisionado, classificação e regressão.
2. Google. *Machine Learning Glossary* - supervised machine learning. <https://developers.google.com/machine-learning/glossary> — Definições práticas de rótulos, features, classificação e regressão.
3. ISO/IEC (2022). *ISO/IEC 22989 - Artificial intelligence concepts and terminology*, ML section. <https://www.iso.org/standard/74296.html> — Termo padronizado (pago; citado, não transcrito).
4. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 2-3, pp. 19-104 (perceptron, Adaline, logistic regression, SVM, trees, k-NN). Constrói passo a passo os principais classificadores supervisionados e os compara no scikit-learn.
5. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*. MIT Press. <https://www.deeplearningbook.org/> — §5.7 Supervised Learning Algorithms, p. 140. Visão formal curta do aprendizado supervisionado como estimativa de p(y | x).

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
