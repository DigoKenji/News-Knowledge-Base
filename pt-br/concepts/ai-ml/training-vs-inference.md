---
type: Concept
title: Treino vs inferência
description: As duas fases da vida de um modelo - treino, em que seus parâmetros são aprendidos com dados (lento, caro, feito uma vez ou de vez em quando), e inferência, em que o modelo treinado é usado para produzir saídas para entradas novas (feita a cada requisição).
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
- title: Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning, ch. 6 and 8. MIT Press
  resource: 'https://www.deeplearningbook.org/'
  kind: book
- title: 'Pope, R. et al. (2023). Efficiently Scaling Transformer Inference. MLSys 2023, arXiv:2211.05102'
  resource: 'https://arxiv.org/abs/2211.05102'
  kind: paper
- title: Google. Machine Learning Glossary - training, inference
  resource: 'https://developers.google.com/machine-learning/glossary'
  kind: docs
lang: pt-br
translation_of: /concepts/ai-ml/training-vs-inference.md
---

# Treino vs inferência

**Definição.** O treino passa os dados pelo modelo, mede o erro e atualiza os pesos de trás para frente (backpropagation), em muitas passagens. A inferência só executa a passagem direta com pesos fixos. Em LLMs, o treino se divide em pré-treino (auto-supervisionado, em corpora enormes) e pós-treino (fine-tuning, RLHF); a inferência se divide em prefill (ler o prompt) e decode (gerar tokens).

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

As duas fases têm economia e engenharia diferentes. O treino é um grande job em lote, limitado por cálculo, medido em semanas de GPU. A inferência é um serviço sensível a latência cujo custo cresce com o uso e muitas vezes supera o custo de treino ao longo da vida do modelo. Muitos conceitos da KB são otimizações de inferência (KV cache, speculative decoding, desagregação) ou variantes baratas de treino (LoRA, destilação).

## Exemplo

Um laboratório gasta meses e milhões de dólares treinando um modelo uma vez; cada pergunta que Kenji manda ao Hermes pelo Telegram são alguns segundos de inferência.

## Conceitos mais específicos

- [Throughput/latência/custo de inferência](/pt-br/concepts/ai-ml/inference-tradeoffs.md)
- [KV cache](/pt-br/concepts/ai-ml/kv-cache.md)
- [Decodificação especulativa](/pt-br/concepts/ai-ml/speculative-decoding.md)
- [Desagregação prefill-decode](/pt-br/concepts/ai-ml/prefill-decode-disaggregation.md)
- [Adaptação de baixo posto (LoRA)](/pt-br/concepts/ai-ml/lora.md)
- [Destilação](/pt-br/concepts/ai-ml/distillation.md)

## Fontes

1. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*, ch. 6 and 8. MIT Press. <https://www.deeplearningbook.org/> — Passagem direta, backpropagation e otimização no treino.
2. Pope, R. et al. (2023). *Efficiently Scaling Transformer Inference*. MLSys 2023, arXiv:2211.05102. <https://arxiv.org/abs/2211.05102> — Custo e latência de inferência e a distinção prefill/decode.
3. Google. *Machine Learning Glossary* - training, inference. <https://developers.google.com/machine-learning/glossary> — Definições práticas dos dois termos.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
