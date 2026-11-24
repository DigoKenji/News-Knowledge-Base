---
type: Concept
title: Transformer e atenção
description: A arquitetura de rede neural por trás dos LLMs modernos, construída sobre self-attention - um mecanismo que permite a cada token ponderar todos os outros tokens da entrada para decidir o que é relevante - processando sequências em paralelo em vez de palavra por palavra.
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
- title: 'Vaswani, A. et al. (2017). Attention Is All You Need. NeurIPS 2017, arXiv:1706.03762'
  resource: 'https://arxiv.org/abs/1706.03762'
  kind: paper
- title: 'Bahdanau, D., Cho, K. and Bengio, Y. (2015). Neural Machine Translation by Jointly Learning to Align and Translate. ICLR 2015, arXiv:1409.0473'
  resource: 'https://arxiv.org/abs/1409.0473'
  kind: paper
- title: Alammar, J. (2018). The Illustrated Transformer
  resource: 'https://jalammar.github.io/illustrated-transformer/'
  kind: practitioner
lang: pt-br
translation_of: /concepts/transformer.md
---

# Transformer e atenção

**Definição.** Apresentado em "Attention Is All You Need" (2017), o transformer empilha camadas de self-attention multi-head e redes feed-forward. Na atenção, cada token produz uma consulta (query), uma chave (key) e um valor (value); a consulta é comparada com todas as chaves para obter pesos, e a saída é a mistura ponderada dos valores. A informação de posição é adicionada à parte (por exemplo com RoPE).

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A atenção já existia (Bahdanau et al. a usaram com redes recorrentes para tradução); o transformer eliminou totalmente a recorrência, o que tornou o treino altamente paralelo em GPUs e permitiu escalar para bilhões de parâmetros. Variantes: só encoder (BERT, usado para embeddings), só decoder (GPT, Claude, Llama, usados para geração) e encoder-decoder (T5). O custo da atenção cresce com o quadrado do comprimento da sequência, o que motiva o trabalho em contexto longo, tamanho do KV cache e atenção eficiente.

## Exemplo

Em "o animal não atravessou a rua porque estava cansado", a atenção permite que o token de "estava cansado" ponha a maior parte do peso em "animal" e não em "rua".

## Conceitos mais específicos

- [Embedding posicional rotacional (RoPE)](/pt-br/concepts/rope.md)
- [Atenção latente multi-cabeça](/pt-br/concepts/multi-head-latent-attention.md)
- [KV cache](/pt-br/concepts/kv-cache.md)
- [Mistura de especialistas](/pt-br/concepts/mixture-of-experts.md)

## Fontes

1. Vaswani, A. et al. (2017). *Attention Is All You Need*. NeurIPS 2017, arXiv:1706.03762. <https://arxiv.org/abs/1706.03762> — Arquitetura original do transformer e scaled dot-product attention.
2. Bahdanau, D., Cho, K. and Bengio, Y. (2015). *Neural Machine Translation by Jointly Learning to Align and Translate*. ICLR 2015, arXiv:1409.0473. <https://arxiv.org/abs/1409.0473> — O mecanismo de atenção antes dos transformers.
3. Alammar, J. (2018). *The Illustrated Transformer*. <https://jalammar.github.io/illustrated-transformer/> — Explicação visual muito usada de queries, keys, values e atenção multi-head.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
