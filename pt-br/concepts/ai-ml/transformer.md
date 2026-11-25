---
type: Concept
title: Transformer e atenção
description: A arquitetura de rede neural por trás dos LLMs modernos, construída sobre self-attention - um mecanismo que permite a cada token ponderar todos os outros tokens da entrada para decidir o que é relevante - processando sequências em paralelo em vez de palavra por palavra.
tags:
- ai-ml
- core
domain: ai-ml
subdomain: core
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
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
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 3 Transformer Anatomy, pp. 57-86 (self-attention p. 61; positional embeddings p. 73)'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 16 Transformers, pp. 539-588 (self-attention p. 544; original architecture p. 552)'
- title: 'Foster, D. (2023). Generative Deep Learning, 2nd edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098134181'
  kind: book
  locator: 'ch. 9 Transformers, pp. 235-265'
lang: pt-br
translation_of: /concepts/ai-ml/transformer.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Transformer e atenção

**Definição.** Apresentado em "Attention Is All You Need" (2017), o transformer empilha camadas de self-attention multi-head e redes feed-forward. Na atenção, cada token produz uma consulta (query), uma chave (key) e um valor (value); a consulta é comparada com todas as chaves para obter pesos, e a saída é a mistura ponderada dos valores. A informação de posição é adicionada à parte (por exemplo com RoPE).

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

A atenção já existia (Bahdanau et al. a usaram com redes recorrentes para tradução); o transformer eliminou totalmente a recorrência, o que tornou o treino altamente paralelo em GPUs e permitiu escalar para bilhões de parâmetros. Variantes: só encoder (BERT, usado para embeddings), só decoder (GPT, Claude, Llama, usados para geração) e encoder-decoder (T5). O custo da atenção cresce com o quadrado do comprimento da sequência, o que motiva o trabalho em contexto longo, tamanho do KV cache e atenção eficiente.

## Exemplo

Em "o animal não atravessou a rua porque estava cansado", a atenção permite que o token de "estava cansado" ponha a maior parte do peso em "animal" e não em "rua".

## Conceitos mais específicos

- [Embedding posicional rotacional (RoPE)](/pt-br/concepts/ai-ml/rope.md)
- [Atenção latente multi-cabeça](/pt-br/concepts/ai-ml/multi-head-latent-attention.md)
- [KV cache](/pt-br/concepts/ai-ml/kv-cache.md)
- [Mistura de especialistas](/pt-br/concepts/ai-ml/mixture-of-experts.md)

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Tunstall, L., von Werra, L. and Wolf, T. (2022)**, *Natural Language Processing with Transformers, revised edition*, ch. 3 Transformer Anatomy, pp. 57-86 (self-attention p. 61; positional embeddings p. 73): Os internos de encoder e decoder explicados por autores da Hugging Face.
- **Raschka, S., Liu, Y. and Mirjalili, V. (2022)**, *Machine Learning with PyTorch and Scikit-Learn*, ch. 16 Transformers, pp. 539-588 (self-attention p. 544; original architecture p. 552): Da atenção em RNNs à atenção por produto escalar escalado e multi-head.
- **Foster, D. (2023)**, *Generative Deep Learning, 2nd edition*, ch. 9 Transformers, pp. 235-265: O transformer como modelo generativo (autorregressivo).

## Fontes

1. Vaswani, A. et al. (2017). *Attention Is All You Need*. NeurIPS 2017, arXiv:1706.03762. <https://arxiv.org/abs/1706.03762> — Arquitetura original do transformer e scaled dot-product attention.
2. Bahdanau, D., Cho, K. and Bengio, Y. (2015). *Neural Machine Translation by Jointly Learning to Align and Translate*. ICLR 2015, arXiv:1409.0473. <https://arxiv.org/abs/1409.0473> — O mecanismo de atenção antes dos transformers.
3. Alammar, J. (2018). *The Illustrated Transformer*. <https://jalammar.github.io/illustrated-transformer/> — Explicação visual muito usada de queries, keys, values e atenção multi-head.
4. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 3 Transformer Anatomy, pp. 57-86 (self-attention p. 61; positional embeddings p. 73). Os internos de encoder e decoder explicados por autores da Hugging Face.
5. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 16 Transformers, pp. 539-588 (self-attention p. 544; original architecture p. 552). Da atenção em RNNs à atenção por produto escalar escalado e multi-head.
6. Foster, D. (2023). *Generative Deep Learning, 2nd edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098134181> — ch. 9 Transformers, pp. 235-265. O transformer como modelo generativo (autorregressivo).

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
