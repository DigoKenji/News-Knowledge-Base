---
type: Concept
title: Machine learning (aprendizado de máquina)
description: O ramo da IA em que sistemas melhoram numa tarefa aprendendo padrões a partir de dados, em vez de serem programados explicitamente com regras.
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
- title: Mitchell, T. (1997). Machine Learning, ch. 1. McGraw-Hill
  resource: 'https://www.cs.cmu.edu/~tom/mlbook.html'
  kind: book
- title: ISO/IEC (2022). ISO/IEC 23053 - Framework for AI systems using machine learning
  resource: 'https://www.iso.org/standard/74438.html'
  kind: standard
- title: Google. Machine Learning Crash Course
  resource: 'https://developers.google.com/machine-learning/crash-course'
  kind: docs
foundation_candidate: true
lang: pt-br
translation_of: /concepts/ai-ml/machine-learning.md
---

# Machine learning (aprendizado de máquina)

**Definição.** A definição clássica de Tom Mitchell: um programa aprende com a experiência E em relação a uma tarefa T e uma medida de desempenho P se seu desempenho em T, medido por P, melhora com E. Na prática, ML ajusta os parâmetros de um modelo a dados de treino para que ele faça previsões ou decisões sobre dados novos. A ISO/IEC 23053 descreve o framework para sistemas de IA baseados em ML.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

As principais famílias são aprendizado supervisionado, não supervisionado (incluindo auto-supervisionado) e por reforço. Todo sistema de ML tem uma fase de treino (aprender os parâmetros) e uma fase de inferência (usá-los). O risco central é a generalização: ir bem nos dados de treino e mal em dados novos (overfitting), ou piorar quando os dados mudam (data drift e concept drift).

## Exemplo

Em vez de escrever regras sobre quais e-mails são spam, dar a um modelo 100.000 e-mails rotulados como spam ou não e deixá-lo aprender quais características preveem spam.

## Conceitos mais específicos

- [Aprendizado supervisionado](/pt-br/concepts/ai-ml/supervised-learning.md)
- [Aprendizado não supervisionado e auto-supervisionado](/pt-br/concepts/ai-ml/unsupervised-learning.md)
- [Aprendizado por reforço](/pt-br/concepts/ai-ml/reinforcement-learning.md)
- [Redes neurais e deep learning](/pt-br/concepts/ai-ml/deep-learning.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. Mitchell, T. (1997). *Machine Learning*, ch. 1. McGraw-Hill. <https://www.cs.cmu.edu/~tom/mlbook.html> — A definição de aprendizado por E-T-P.
2. ISO/IEC (2022). *ISO/IEC 23053 - Framework for AI systems using machine learning*. <https://www.iso.org/standard/74438.html> — Framework e termos padronizados para sistemas de ML (pago; citado, não transcrito).
3. Google. *Machine Learning Crash Course*. <https://developers.google.com/machine-learning/crash-course> — Introdução prática - modelos, treino, função de perda, generalização.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
