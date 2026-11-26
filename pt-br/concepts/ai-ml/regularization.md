---
type: Concept
title: 'Regularização e o trade-off viés-variância'
description: 'Técnicas que reduzem o overfitting limitando o quanto um modelo pode se ajustar aos dados de treino (penalidades, dropout, early stopping, aumento de dados), dentro do trade-off entre viés (simples demais) e variância (sensível demais à amostra).'
tags:
- ai-ml
- ml-fundamentals
domain: ai-ml
subdomain: ml-fundamentals
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press'
  resource: 'https://www.deeplearningbook.org/'
  kind: book
  locator: '§5.2 Capacity, Overfitting and Underfitting, p. 110; §5.4 Estimators, Bias and Variance, p. 122; ch. 7 Regularization for Deep Learning, pp. 228-273 (early stopping §7.8 p. 246, dropout §7.12 p. 258)'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 4 L1 and L2 regularization, pp. 122-127; ch. 6 Diagnosing bias and variance problems with learning curves, pp. 180-185; k-fold cross-validation p. 175'
- title: 'Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). Mathematics for Machine Learning. Cambridge University Press'
  resource: 'https://mml-book.github.io/'
  kind: book
  locator: '§8.2 Empirical Risk Minimization (regularization), p. 258; §8.6 Model Selection, p. 283'
- title: 'Srivastava, N. et al. (2014). Dropout: A Simple Way to Prevent Neural Networks from Overfitting. JMLR 15'
  resource: 'https://jmlr.org/papers/v15/srivastava14a.html'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/regularization.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Regularização e o trade-off viés-variância

**Definição.** Goodfellow et al. definem regularização como qualquer modificação no algoritmo de aprendizado que pretende reduzir o erro de generalização, mas não o erro de treino. A visão viés-variância explica por que ajuda: o erro esperado se divide em viés (erro sistemático de um modelo simples demais para captar o padrão), variância (erro de um modelo que muda muito a cada amostra de treino) e ruído irredutível. Mais capacidade reduz o viés e aumenta a variância; a regularização troca um pouco de viés por um corte grande de variância. As técnicas comuns são penalidades L2 (weight decay) e L1 sobre o tamanho dos pesos (a L1 também zera pesos, gerando modelos esparsos), dropout (desligar unidades aleatoriamente no treino), early stopping (parar quando o erro de validação começa a subir), aumento de dados e ensembles.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `ml-fundamentals`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

Esta é a caixa de ferramentas prática para o problema descrito em Overfitting e generalização. Curvas de aprendizado e de validação (Raschka, cap. 6) são o diagnóstico: uma distância grande entre os escores de treino e validação indica variância alta, e dois escores baixos indicam viés alto. A força da regularização é um hiperparâmetro, então é ajustada com um conjunto de validação ou validação cruzada k-fold, nunca com o conjunto de teste.

## Exemplo

Um modelo de risco de crédito acerta 99% nos dados de treino e 78% na validação. Com penalidade L2 e early stopping, o treino cai para 91% e a validação sobe para 88%: um pouco mais de viés, muito menos variância, melhor desempenho no mundo real.

## Conceitos mais amplos

- [Overfitting e generalização](/pt-br/concepts/ai-ml/overfitting.md)

## Conceitos relacionados

- [Avaliação de modelos e métricas](/pt-br/concepts/ai-ml/model-evaluation.md)

## Fontes

1. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*. MIT Press. <https://www.deeplearningbook.org/> — §5.2 Capacity, Overfitting and Underfitting, p. 110; §5.4 Estimators, Bias and Variance, p. 122; ch. 7 Regularization for Deep Learning, pp. 228-273 (early stopping §7.8 p. 246, dropout §7.12 p. 258). Definição de regularização e o catálogo completo de técnicas para redes profundas.
2. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 4 L1 and L2 regularization, pp. 122-127; ch. 6 Diagnosing bias and variance problems with learning curves, pp. 180-185; k-fold cross-validation p. 175. Diagnóstico prático com curvas de aprendizado e validação, e ajuste com validação cruzada.
3. Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). *Mathematics for Machine Learning*. Cambridge University Press. <https://mml-book.github.io/> — §8.2 Empirical Risk Minimization (regularization), p. 258; §8.6 Model Selection, p. 283. Regularização como termo de penalidade na minimização do risco empírico, e seleção de modelos.
4. Srivastava, N. et al. (2014). *Dropout: A Simple Way to Prevent Neural Networks from Overfitting*. JMLR 15. <https://jmlr.org/papers/v15/srivastava14a.html> — Artigo original do dropout.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
