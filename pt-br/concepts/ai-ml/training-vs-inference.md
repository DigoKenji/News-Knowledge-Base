---
type: Concept
title: Treino vs inferência
description: As duas fases da vida de um modelo - treino, em que seus parâmetros são aprendidos com dados (lento, caro, feito uma vez ou de vez em quando), e inferência, em que o modelo treinado é usado para produzir saídas para entradas novas (feita a cada requisição).
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
- title: Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning, ch. 6 and 8. MIT Press
  resource: 'https://www.deeplearningbook.org/'
  kind: book
- title: 'Pope, R. et al. (2023). Efficiently Scaling Transformer Inference. MLSys 2023, arXiv:2211.05102'
  resource: 'https://arxiv.org/abs/2211.05102'
  kind: paper
- title: Google. Machine Learning Glossary - training, inference
  resource: 'https://developers.google.com/machine-learning/glossary'
  kind: docs
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 9 Inference Optimization, pp. 405-447 (inference overview p. 406; performance metrics p. 412)'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 10 Inference Pipeline Deployment, pp. 355-400'
lang: pt-br
translation_of: /concepts/ai-ml/training-vs-inference.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Treino vs inferência

**Definição.** O treino passa os dados pelo modelo, mede o erro e atualiza os pesos de trás para frente (backpropagation), em muitas passagens. A inferência só executa a passagem direta com pesos fixos. Em LLMs, o treino se divide em pré-treino (auto-supervisionado, em corpora enormes) e pós-treino (fine-tuning, RLHF); a inferência se divide em prefill (ler o prompt) e decode (gerar tokens).

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `ml-fundamentals`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

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

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 9 Inference Optimization, pp. 405-447 (inference overview p. 406; performance metrics p. 412): Inferência como problema de engenharia próprio: fases limitadas por computação ou memória, TTFT/TPOT, aceleradores.
- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 10 Inference Pipeline Deployment, pp. 355-400: Tipos de implantação online, assíncrona e em lote e seus trade-offs.

## Fontes

1. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*, ch. 6 and 8. MIT Press. <https://www.deeplearningbook.org/> — Passagem direta, backpropagation e otimização no treino.
2. Pope, R. et al. (2023). *Efficiently Scaling Transformer Inference*. MLSys 2023, arXiv:2211.05102. <https://arxiv.org/abs/2211.05102> — Custo e latência de inferência e a distinção prefill/decode.
3. Google. *Machine Learning Glossary* - training, inference. <https://developers.google.com/machine-learning/glossary> — Definições práticas dos dois termos.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 9 Inference Optimization, pp. 405-447 (inference overview p. 406; performance metrics p. 412). Inferência como problema de engenharia próprio: fases limitadas por computação ou memória, TTFT/TPOT, aceleradores.
5. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 10 Inference Pipeline Deployment, pp. 355-400. Tipos de implantação online, assíncrona e em lote e seus trade-offs.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
