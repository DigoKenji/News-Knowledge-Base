---
type: Concept
title: RAG sem vetores
description: 'RAG que recupera sem índice de embeddings - por exemplo, deixando o LLM navegar pela estrutura ou sumário do documento, ou com busca por palavras-chave - para manter o contexto e evitar erros de fragmentação.'
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
  locator: 'ch. 6 RAG and Agents: Retrieval algorithms (term-based versus embedding-based retrieval)'
- title: 'Robertson, S. and Zaragoza, H. (2009). The Probabilistic Relevance Framework: BM25 and Beyond. Foundations and Trends in IR 3(4)'
  resource: 'https://doi.org/10.1561/1500000019'
  kind: paper
- title: 'PageIndex (Vectify AI). PageIndex documentation - vectorless, reasoning-based RAG'
  resource: 'https://docs.pageindex.ai/'
  kind: docs
relations:
- type: alternative-to
  target: /concepts/ai-ml/rag.md
lang: pt-br
translation_of: /concepts/ai-ml/vectorless-rag.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# RAG sem vetores

**Definição.** O RAG sem vetores recupera contexto sem um índice de embeddings. Uma variante usa recuperação clássica por termos (BM25); outra, mais nova, deixa o LLM navegar pela própria estrutura do documento - uma árvore ou sumário gerado - e raciocinar sobre quais seções abrir, como faz o PageIndex. O objetivo é uma recuperação que siga a forma como uma pessoa lê documentos longos e estruturados e que indique exatamente de onde veio a resposta.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `rag`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

É uma estratégia alternativa de recuperação dentro do RAG, não um substituto da recuperação. Evita a fragmentação que separa tabelas ou seções e a similaridade de embeddings que perde termos exatos, mas a navegação em árvore custa mais chamadas ao LLM por pergunta e funciona melhor em documentos bem estruturados, como relatórios, manuais e documentos regulatórios.

## Exemplo

Para um relatório anual de 300 páginas, o sistema monta uma árvore de seções uma vez; ao ser perguntado sobre passivos de arrendamento, o LLM percorre Notas explicativas → Arrendamentos e cita aquela página.

## Fontes

1. Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 6 RAG and Agents: Retrieval algorithms (term-based versus embedding-based retrieval). Recuperação por termos como alternativa forte e barata aos embeddings.
2. Robertson, S. and Zaragoza, H. (2009). The Probabilistic Relevance Framework: BM25 and Beyond. Foundations and Trends in IR 3(4). <https://doi.org/10.1561/1500000019> — BM25, a função padrão de ranqueamento por termos.
3. PageIndex (Vectify AI). PageIndex documentation - vectorless, reasoning-based RAG. <https://docs.pageindex.ai/> — Recuperação por busca em árvore sobre a estrutura do documento.

## Relações
- **alternativa a** [Geração aumentada por recuperação (RAG)](/pt-br/concepts/ai-ml/rag.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
