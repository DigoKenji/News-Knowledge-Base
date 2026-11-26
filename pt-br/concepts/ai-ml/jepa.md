---
type: Concept
title: Arquitetura preditiva de embedding conjunto (JEPA)
description: Joint-Embedding Predictive Architecture - um desenho auto-supervisionado que aprende prevendo a representação abstrata de uma parte oculta da entrada a partir de uma parte visível, em vez de reconstruir pixels ou tokens brutos.
tags:
- ai-ml
- deep-learning
domain: ai-ml
subdomain: deep-learning
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude (Project_News daily run)
  at: '2026-11-16T06:50:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: LeCun, Y. (2022). A Path Towards Autonomous Machine Intelligence. OpenReview
  resource: 'https://openreview.net/forum?id=BZ5a1r-kVsf'
  kind: paper
- title: 'Assran, M. et al. (2023). Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture (I-JEPA). CVPR 2023, arXiv:2301.08243'
  resource: 'https://arxiv.org/abs/2301.08243'
  kind: paper
- title: 'Bardes, A. et al. (2024). Revisiting Feature Prediction for Learning Visual Representations from Video (V-JEPA). arXiv:2404.08471'
  resource: 'https://arxiv.org/abs/2404.08471'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/jepa.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Arquitetura preditiva de embedding conjunto (JEPA)

**Definição.** Uma JEPA tem dois encoders e um preditor. Um encoder gera o embedding de um "contexto" visível (parte de uma imagem ou vídeo); o outro gera o de um "alvo" oculto; o preditor aprende a prever o embedding do alvo a partir do embedding do contexto. Como a perda é calculada no espaço de embeddings, o modelo pode ignorar detalhes imprevisíveis (textura exata, ruído) e se concentrar em estrutura e semântica. Yann LeCun a propôs em 2022 como bloco de construção para modelos de mundo.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `deep-learning`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

O I-JEPA (imagens) mascara grandes blocos de uma imagem e prevê suas representações; o V-JEPA faz o mesmo no espaço e no tempo em vídeo. O encoder do alvo é uma média móvel exponencial do encoder do contexto, um truque que, junto com o preditor, evita o "colapso" (todas as entradas mapeadas para o mesmo vetor) sem precisar de exemplos negativos como no aprendizado contrastivo. Após o pré-treino, o encoder é reutilizado em tarefas posteriores, muitas vezes com um probe leve por cima.

## Por que importa e trade-offs

JEPAs aprendem características semânticas de forma mais eficiente que métodos de reconstrução de pixels como masked autoencoders, e são a aposta da Meta para modelos que entendem o mundo físico. Não são generativas: não dá para amostrar uma imagem ou texto delas diretamente, e ainda são principalmente uma direção de pesquisa, não um padrão de produção.

## Exemplo

Mostre ao V-JEPA um vídeo com uma região mascarada por vários quadros; ele prevê um embedding que significa "uma mão alcançando a xícara", não os pixels exatos.

## Notas de revisão para Kenji

As três fontes são de pesquisadores da Meta; existem replicações independentes, mas menos conhecidas. Suficiente para um conceito, não para um fundamento.

## Fontes

1. LeCun, Y. (2022). *A Path Towards Autonomous Machine Intelligence*. OpenReview. <https://openreview.net/forum?id=BZ5a1r-kVsf> — Proposta original da JEPA e por que prever no espaço de representações importa.
2. Assran, M. et al. (2023). *Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture (I-JEPA)*. CVPR 2023, arXiv:2301.08243. <https://arxiv.org/abs/2301.08243> — JEPA para imagens, estratégia de mascaramento, comparação com reconstrução de pixels.
3. Bardes, A. et al. (2024). *Revisiting Feature Prediction for Learning Visual Representations from Video (V-JEPA)*. arXiv:2404.08471. <https://arxiv.org/abs/2404.08471> — JEPA para vídeo treinada só com previsão de características.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
