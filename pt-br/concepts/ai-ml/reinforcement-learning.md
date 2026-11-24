---
type: Concept
title: Aprendizado por reforço
description: Machine learning em que um agente aprende por tentativa e erro quais ações tomar num ambiente para maximizar uma recompensa acumulada, em vez de aprender com exemplos rotulados.
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
- title: Sutton, R. S. and Barto, A. G. (2018). Reinforcement Learning - An Introduction, 2nd ed. MIT Press
  resource: 'http://incompleteideas.net/book/the-book-2nd.html'
  kind: book
- title: 'Ouyang, L. et al. (2022). Training language models to follow instructions with human feedback. NeurIPS 2022, arXiv:2203.02155'
  resource: 'https://arxiv.org/abs/2203.02155'
  kind: paper
- title: Silver, D. et al. (2016). Mastering the game of Go with deep neural networks and tree search. Nature 529
  resource: 'https://doi.org/10.1038/nature16961'
  kind: paper
foundation_candidate: true
lang: pt-br
translation_of: /concepts/ai-ml/reinforcement-learning.md
---

# Aprendizado por reforço

**Definição.** No aprendizado por reforço (RL) um agente observa um estado, executa uma ação, recebe uma recompensa e um novo estado, e aprende uma política (uma regra para escolher ações) que maximiza a recompensa de longo prazo. A dificuldade central é o equilíbrio entre explorar ações novas e aproveitar o que já funciona, e atribuir recompensas a ações tomadas muito antes.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O RL impulsionou sistemas que jogam (AlphaGo), robótica e recomendação. Para LLMs importa via RLHF (reinforcement learning from human feedback) e RL mais recente em tarefas verificáveis, que transformam modelos pré-treinados em assistentes úteis e raciocinadores mais fortes. O RL consome muitos dados e é sensível a recompensas mal desenhadas, que o modelo pode "burlar".

## Exemplo

Um modelo escreve duas respostas, humanos escolhem a melhor, um modelo de recompensa aprende essas preferências e o RL ajusta o LLM para produzir respostas que o modelo de recompensa pontua bem.

## Conceitos mais específicos

- [Agente de IA](/pt-br/concepts/ai-ml/ai-agent.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. Sutton, R. S. and Barto, A. G. (2018). *Reinforcement Learning - An Introduction*, 2nd ed. MIT Press. <http://incompleteideas.net/book/the-book-2nd.html> — Definição clássica de livro-texto - agente, ambiente, recompensa, política, exploração.
2. Ouyang, L. et al. (2022). *Training language models to follow instructions with human feedback*. NeurIPS 2022, arXiv:2203.02155. <https://arxiv.org/abs/2203.02155> — RLHF aplicado a LLMs (InstructGPT).
3. Silver, D. et al. (2016). *Mastering the game of Go with deep neural networks and tree search*. Nature 529. <https://doi.org/10.1038/nature16961> — Resultado marcante de RL profundo (AlphaGo), independente do contexto de LLMs.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
