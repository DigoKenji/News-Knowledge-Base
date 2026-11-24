---
type: Concept
title: Busca semântica
description: Busca que corresponde ao significado da consulta e não às palavras exatas, geralmente transformando consultas e documentos em vetores de embedding e devolvendo os mais próximos.
tags:
- ai-ml
- rag
domain: ai-ml
subdomain: rag
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Reimers, N. and Gurevych, I. (2019). Sentence-BERT - Sentence Embeddings using Siamese BERT-Networks. EMNLP 2019, arXiv:1908.10084'
  resource: 'https://arxiv.org/abs/1908.10084'
  kind: paper
- title: 'Karpukhin, V. et al. (2020). Dense Passage Retrieval for Open-Domain Question Answering. EMNLP 2020, arXiv:2004.04906'
  resource: 'https://arxiv.org/abs/2004.04906'
  kind: paper
- title: 'Thakur, N. et al. (2021). BEIR - A Heterogeneous Benchmark for Zero-shot Evaluation of Information Retrieval Models. NeurIPS 2021, arXiv:2104.08663'
  resource: 'https://arxiv.org/abs/2104.08663'
  kind: paper
- title: Manning, C., Raghavan, P. and Schütze, H. (2008). Introduction to Information Retrieval. Cambridge University Press
  resource: 'https://nlp.stanford.edu/IR-book/'
  kind: book
lang: pt-br
translation_of: /concepts/semantic-search.md
---

# Busca semântica

**Definição.** Busca semântica recupera documentos que tratam do mesmo assunto da consulta mesmo quando não compartilham nenhuma palavra com ela. Sistemas modernos usam recuperação densa: um encoder neural mapeia texto para vetores de modo que significados parecidos fiquem próximos, e um índice de vizinhos mais próximos aproximado encontra os documentos mais perto.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `rag`. Expandido em 2026-11-24 com fontes independentes; ainda um rascunho aguardando a certificação de Kenji.

## Como funciona

Pipeline: dividir os documentos em trechos, gerar o embedding de cada trecho com um bi-encoder (estilo Sentence-BERT), guardar os vetores num índice (HNSW, IVF) e, na consulta, gerar o embedding da consulta e buscar os top-k por similaridade de cosseno. A busca lexical (BM25), ao contrário, pontua a sobreposição exata de termos. Cada uma falha de um jeito: a recuperação densa lida com sinônimos e paráfrases mas pode perder códigos exatos, nomes e termos raros; o BM25 é o inverso. O benchmark BEIR mostrou que o BM25 continua uma referência forte fora do domínio. Por isso sistemas em produção costumam usar busca híbrida (as duas, fundidas com reciprocal rank fusion) mais um reranker cross-encoder.

## Por que importa e trade-offs

É a camada de recuperação por trás da maioria dos sistemas de RAG e faz a busca funcionar para perguntas em linguagem natural. Custos: infraestrutura de embeddings e índice, sensibilidade ao modelo de embedding e à divisão em trechos, e resultados mais difíceis de explicar que correspondências por palavra-chave.

## Exemplo

A consulta "como evitar que retentativas piorem uma queda" encontra a nota sobre retry storms mesmo que ela nunca diga "evitar" ou "queda".

## Fontes

1. Reimers, N. and Gurevych, I. (2019). *Sentence-BERT - Sentence Embeddings using Siamese BERT-Networks*. EMNLP 2019, arXiv:1908.10084. <https://arxiv.org/abs/1908.10084> — Embeddings de frases com bi-encoder para busca por similaridade semântica.
2. Karpukhin, V. et al. (2020). *Dense Passage Retrieval for Open-Domain Question Answering*. EMNLP 2020, arXiv:2004.04906. <https://arxiv.org/abs/2004.04906> — Recuperação densa superando o BM25 em QA dentro do domínio.
3. Thakur, N. et al. (2021). *BEIR - A Heterogeneous Benchmark for Zero-shot Evaluation of Information Retrieval Models*. NeurIPS 2021, arXiv:2104.08663. <https://arxiv.org/abs/2104.08663> — Força do BM25 fora do domínio; o argumento para busca híbrida.
4. Manning, C., Raghavan, P. and Schütze, H. (2008). *Introduction to Information Retrieval*. Cambridge University Press. <https://nlp.stanford.edu/IR-book/> — Fundamentos de recuperação lexical (ponderação de termos, modelo de espaço vetorial) para contraste.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
