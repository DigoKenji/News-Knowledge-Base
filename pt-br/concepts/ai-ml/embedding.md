---
type: Concept
title: Embedding
description: Uma lista de números aprendida (um vetor) que representa um item - uma palavra, frase, imagem ou usuário - de modo que itens com significado parecido fiquem próximos, permitindo que o software compare significados com simples cálculos de distância.
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
- title: 'Mikolov, T. et al. (2013). Efficient Estimation of Word Representations in Vector Space. arXiv:1301.3781'
  resource: 'https://arxiv.org/abs/1301.3781'
  kind: paper
- title: 'Reimers, N. and Gurevych, I. (2019). Sentence-BERT - Sentence Embeddings using Siamese BERT-Networks. EMNLP 2019, arXiv:1908.10084'
  resource: 'https://arxiv.org/abs/1908.10084'
  kind: paper
- title: Google. Machine Learning Crash Course - Embeddings
  resource: 'https://developers.google.com/machine-learning/crash-course/embeddings'
  kind: docs
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 3 Introduction to Embedding, pp. 134-136; ch. 6 embedding-based retrieval, p. 257'
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 9 Using Embeddings as a Lookup Table, p. 275'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 15 Embedding layers for sentence encoding, p. 517'
lang: pt-br
translation_of: /concepts/ai-ml/embedding.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Embedding

**Definição.** Um embedding mapeia um objeto discreto ou complexo num espaço vetorial contínuo de algumas centenas a alguns milhares de dimensões. O mapeamento é aprendido, então a geometria carrega significado: a distância (muitas vezes similaridade de cosseno) reflete semelhança, e direções podem codificar relações. O word2vec (2013) popularizou os embeddings de palavras; hoje encoders transformer produzem embeddings para frases e documentos inteiros.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

Embeddings são a ponte entre dados não estruturados e computação: alimentam busca semântica, recuperação em RAG, recomendação, agrupamento e deduplicação, e são a representação interna de toda rede neural. A qualidade depende do modelo de embedding e do domínio; embeddings de modelos diferentes não são comparáveis, então trocar de modelo significa refazer todos os embeddings.

## Exemplo

"Carro" e "automóvel" recebem vetores próximos, longe de "banana"; no word2vec, vetor("rei") − vetor("homem") + vetor("mulher") fica perto de vetor("rainha").

## Conceitos mais específicos

- [Busca semântica](/pt-br/concepts/ai-ml/semantic-search.md)
- [Geração aumentada por recuperação (RAG)](/pt-br/concepts/ai-ml/rag.md)
- [GraphRAG](/pt-br/concepts/ai-ml/graph-rag.md)
- [Grafo de conhecimento](/pt-br/concepts/data/knowledge-graph.md)

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 3 Introduction to Embedding, pp. 134-136; ch. 6 embedding-based retrieval, p. 257: Embeddings para medir similaridade e para recuperação.
- **Tunstall, L., von Werra, L. and Wolf, T. (2022)**, *Natural Language Processing with Transformers, revised edition*, ch. 9 Using Embeddings as a Lookup Table, p. 275: Embeddings como truque de classificação sem rótulos.
- **Raschka, S., Liu, Y. and Mirjalili, V. (2022)**, *Machine Learning with PyTorch and Scikit-Learn*, ch. 15 Embedding layers for sentence encoding, p. 517: Como uma camada de embedding é aprendida dentro de uma rede.

## Fontes

1. Mikolov, T. et al. (2013). *Efficient Estimation of Word Representations in Vector Space*. arXiv:1301.3781. <https://arxiv.org/abs/1301.3781> — Word2vec e aritmética vetorial em embeddings de palavras.
2. Reimers, N. and Gurevych, I. (2019). *Sentence-BERT - Sentence Embeddings using Siamese BERT-Networks*. EMNLP 2019, arXiv:1908.10084. <https://arxiv.org/abs/1908.10084> — Embeddings no nível de frase para similaridade.
3. Google. *Machine Learning Crash Course - Embeddings*. <https://developers.google.com/machine-learning/crash-course/embeddings> — Introdução prática a espaços de embedding.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 3 Introduction to Embedding, pp. 134-136; ch. 6 embedding-based retrieval, p. 257. Embeddings para medir similaridade e para recuperação.
5. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 9 Using Embeddings as a Lookup Table, p. 275. Embeddings como truque de classificação sem rótulos.
6. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 15 Embedding layers for sentence encoding, p. 517. Como uma camada de embedding é aprendida dentro de uma rede.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
