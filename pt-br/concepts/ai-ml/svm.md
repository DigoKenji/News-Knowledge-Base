---
type: Concept
title: Máquina de vetores de suporte (SVM)
description: 'Um classificador que encontra a fronteira com a maior margem entre as classes, usando funções de kernel para separar dados que não são linearmente separáveis.'
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
- title: 'Cortes, C. and Vapnik, V. (1995). Support-Vector Networks. Machine Learning 20'
  resource: 'https://doi.org/10.1007/BF00994018'
  kind: paper
- title: 'Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press'
  resource: 'https://www.deeplearningbook.org/'
  kind: book
  locator: '§5.7.2 Support Vector Machines'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 3: Maximum margin classification with support vector machines; RBF kernel SVM, p. 83'
- title: 'Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). Mathematics for Machine Learning. Cambridge University Press'
  resource: 'https://mml-book.github.io/'
  kind: book
  locator: 'ch. 12 Classification with Support Vector Machines'
lang: pt-br
translation_of: /concepts/ai-ml/svm.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Máquina de vetores de suporte (SVM)

**Definição.** Uma máquina de vetores de suporte separa duas classes com o hiperplano de maior margem em relação aos pontos de treino mais próximos, os vetores de suporte. Uma margem suave com penalidade C permite alguns pontos do lado errado, trocando largura de margem por erros. Pelo truque do kernel (por exemplo, o kernel RBF), a mesma otimização encontra fronteiras não lineares trabalhando com similaridades entre pontos em vez de features explícitas.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `classical-ml`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

As SVMs foram o classificador de ponta antes do deep learning para conjuntos tabulares ou de texto pequenos e médios, e continuam sendo um baseline forte. Elas produzem um escore de decisão e não uma probabilidade, e SVMs com kernel escalam mal para conjuntos muito grandes.

## Exemplo

Um filtro de spam com features TF-IDF treina uma SVM linear; apenas as poucas centenas de mensagens mais próximas da fronteira acabam definindo-a.

## Fontes

1. Cortes, C. and Vapnik, V. (1995). Support-Vector Networks. Machine Learning 20. <https://doi.org/10.1007/BF00994018> — SVM de margem suave, a formulação padrão.
2. Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press. <https://www.deeplearningbook.org/> — §5.7.2 Support Vector Machines. Margem, truque do kernel e vetores de suporte.
3. Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 3: Maximum margin classification with support vector machines; RBF kernel SVM, p. 83. SVMs na prática com scikit-learn, C e gamma.
4. Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). Mathematics for Machine Learning. Cambridge University Press. <https://mml-book.github.io/> — ch. 12 Classification with Support Vector Machines. Derivação dos problemas primal e dual.

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
