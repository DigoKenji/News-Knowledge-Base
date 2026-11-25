---
type: Concept
title: Geração aumentada por recuperação (RAG)
description: Geração aumentada por recuperação (rag).
tags:
- ai-ml
- rag
domain: ai-ml
subdomain: rag
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 6 RAG, pp. 253-275 (architecture p. 256; retrieval algorithms p. 257; retrieval optimization p. 267)'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 4 RAG Feature Pipeline, pp. 99-176; ch. 9 RAG Inference Pipeline, pp. 317-354 (query expansion, self-querying, filtered vector search, reranking)'
lang: pt-br
translation_of: /concepts/ai-ml/rag.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição curta acrescentada'
---

# Geração aumentada por recuperação (RAG)

**Definição.** O RAG dá ao modelo conhecimento externo na hora da consulta: um recuperador encontra os trechos mais relevantes em um repositório de documentos (por termos, como BM25, por embeddings, ou híbrido), e o modelo responde com esses trechos no contexto. Huyen o enquadra como construção de contexto: reduz alucinações e permite usar dados privados ou recentes sem retreinar; nas palavras dela, "fine-tuning é para forma, RAG é para fatos" (p. 317).

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `rag`.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 6 RAG, pp. 253-275 (architecture p. 256; retrieval algorithms p. 257; retrieval optimization p. 267): Recuperadores por termos vs por embeddings, busca híbrida, chunking e reranking.
- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 4 RAG Feature Pipeline, pp. 99-176; ch. 9 RAG Inference Pipeline, pp. 317-354 (query expansion, self-querying, filtered vector search, reranking): RAG avançado dividido em pipeline de ingestão (features) e pipeline de inferência.

## Fontes

1. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 6 RAG, pp. 253-275 (architecture p. 256; retrieval algorithms p. 257; retrieval optimization p. 267). Recuperadores por termos vs por embeddings, busca híbrida, chunking e reranking.
2. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 4 RAG Feature Pipeline, pp. 99-176; ch. 9 RAG Inference Pipeline, pp. 317-354 (query expansion, self-querying, filtered vector search, reranking). RAG avançado dividido em pipeline de ingestão (features) e pipeline de inferência.

## Relações
- [GraphRAG](/pt-br/concepts/ai-ml/graph-rag.md) **estende** este conceito
- [RAG sem vetores](/pt-br/concepts/ai-ml/vectorless-rag.md) **alternativa a** este conceito

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
