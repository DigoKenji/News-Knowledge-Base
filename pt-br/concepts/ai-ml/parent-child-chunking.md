---
type: Concept
title: Chunking pai-filho
description: Um padrão de recuperação em RAG que indexa pequenos trechos filhos para uma correspondência precisa, mas devolve a seção pai maior a que pertencem, para que o modelo receba contexto suficiente ao redor.
tags:
- ai-ml
- rag
domain: ai-ml
subdomain: rag
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: LangChain. ParentDocumentRetriever (API reference)
  resource: 'https://reference.langchain.com/python/langchain-classic/retrievers/parent_document_retriever/ParentDocumentRetriever'
  kind: docs
- title: LlamaIndex. Auto Merging Retriever
  resource: 'https://developers.llamaindex.ai/python/framework/integrations/retrievers/auto_merging_retriever/'
  kind: docs
- title: 'Lewis, P. et al. (2020). Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. NeurIPS 2020, arXiv:2005.11401'
  resource: 'https://arxiv.org/abs/2005.11401'
  kind: paper
relations:
- type: part-of
  target: /concepts/ai-ml/rag.md
lang: pt-br
translation_of: /concepts/ai-ml/parent-child-chunking.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Chunking pai-filho

**Definição.** Parent-child chunking (também chamado de recuperação small-to-big, ou parent document retriever) divide cada documento duas vezes: em trechos pais grandes (seções, páginas) e, dentro de cada um, trechos filhos pequenos (frases, parágrafos curtos). Só os filhos recebem embedding e são buscados; quando um filho corresponde, o sistema devolve o pai ao LLM.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `rag`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Resolve uma tensão no tamanho dos trechos. Trechos pequenos geram embeddings precisos, porque uma ideia por vetor corresponde bem às consultas, mas faltam contexto. Trechos grandes carregam contexto mas seus embeddings misturam vários temas, então correspondem pior. Guardar um mapeamento filho-pai permite buscar pequeno e ler grande. Variantes: o auto-merging retriever do LlamaIndex devolve o pai só quando filhos suficientes correspondem; a recuperação por janela de frases devolve uma janela fixa de frases em volta do acerto; elimine duplicatas quando vários filhos compartilham o pai.

## Por que importa e trade-offs

Geralmente melhora a qualidade das respostas em documentos longos e estruturados (manuais, normas, contratos) a baixo custo. Consome mais tokens de contexto por acerto, precisa de um repositório de documentos ao lado do índice vetorial e depende de os pais serem unidades com sentido; uma má divisão em seções gera pais ruins.

## Exemplo

Indexar cada frase do PDF do TOGAF, mas quando "O que é um building block?" corresponder a uma frase, mandar ao modelo a subseção inteira a que ela pertence.

## Notas de revisão para Kenji

É um padrão de engenharia documentado por frameworks, não um resultado de pesquisa; as fontes são documentações de dois frameworks independentes mais o artigo original de RAG para a ideia de recuperação por trás.

## Fontes

1. LangChain. *ParentDocumentRetriever* (API reference). <https://reference.langchain.com/python/langchain-classic/retrievers/parent_document_retriever/ParentDocumentRetriever> — Trechos pequenos para embedding, documentos pais devolvidos; repositório filho-pai.
2. LlamaIndex. *Auto Merging Retriever*. <https://developers.llamaindex.ai/python/framework/integrations/retrievers/auto_merging_retriever/> — Trechos hierárquicos e fusão dos filhos nos pais.
3. Lewis, P. et al. (2020). *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*. NeurIPS 2020, arXiv:2005.11401. <https://arxiv.org/abs/2005.11401> — A arquitetura de RAG que este padrão refina.

## Relações
- **parte de** [Geração aumentada por recuperação (RAG)](/pt-br/concepts/ai-ml/rag.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
