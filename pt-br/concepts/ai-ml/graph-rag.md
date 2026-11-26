---
type: Concept
title: GraphRAG
description: 'Geração aumentada por recuperação que busca em um grafo de conhecimento de entidades e relações (muitas vezes construído por um LLM a partir dos documentos) em vez de, ou junto com, trechos de texto.'
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
- title: 'Edge, D. et al. (2024). From Local to Global: A Graph RAG Approach to Query-Focused Summarization. arXiv:2404.16130'
  resource: 'https://arxiv.org/abs/2404.16130'
  kind: paper
- title: 'Microsoft. GraphRAG documentation'
  resource: 'https://microsoft.github.io/graphrag/'
  kind: docs
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 6 RAG and Agents: RAG architecture and retrieval algorithms'
relations:
- type: extends
  target: /concepts/ai-ml/rag.md
lang: pt-br
translation_of: /concepts/ai-ml/graph-rag.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# GraphRAG

**Definição.** O GraphRAG constrói um grafo de conhecimento a partir do corpus - um LLM extrai entidades e relações, o grafo é agrupado em comunidades e cada comunidade é resumida - e responde perguntas recuperando dessa estrutura. O artigo da Microsoft de 2024 mostra que ele ajuda em perguntas globais ('quais são os temas principais?') que a recuperação de trechos simples responde mal, porque nenhum trecho isolado contém a resposta.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `rag`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Estende o RAG comum e troca uma indexação cara (muitas chamadas ao LLM) por respostas melhores em múltiplos saltos e em resumos. A busca local parte de entidades que casam com a pergunta; a busca global percorre os resumos das comunidades e combina as respostas parciais.

## Exemplo

Sobre um ano de relatórios de incidentes, 'quais serviços aparecem com mais frequência em falhas em cascata?' é respondida a partir das comunidades de serviços e dependências do grafo, e não dos poucos relatórios que por acaso contêm as palavras.

## Fontes

1. Edge, D. et al. (2024). From Local to Global: A Graph RAG Approach to Query-Focused Summarization. arXiv:2404.16130. <https://arxiv.org/abs/2404.16130> — O método GraphRAG e sua avaliação em perguntas globais.
2. Microsoft. GraphRAG documentation. <https://microsoft.github.io/graphrag/> — Pipeline de indexação, busca local e global.
3. Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 6 RAG and Agents: RAG architecture and retrieval algorithms. Onde a recuperação baseada em grafos se encaixa entre as opções de recuperação do RAG.

## Relações
- **estende** [Geração aumentada por recuperação](/pt-br/concepts/ai-ml/rag.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
