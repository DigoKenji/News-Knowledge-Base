---
type: Concept
title: 'Tokenização'
description: 'Dividir o texto nas unidades que um modelo de linguagem lê e escreve - geralmente pedaços de subpalavras aprendidos dos dados (BPE, WordPiece, SentencePiece) - o que define o vocabulário do modelo, seus limites de contexto e seu custo por requisição.'
tags:
- ai-ml
- nlp
domain: ai-ml
subdomain: nlp
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 2 Character, Word and Subword Tokenization, pp. 29-35; ch. 4 The Tokenizer Pipeline and SentencePiece, pp. 94-95; ch. 10 Building a Tokenizer, pp. 310-322'
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 1 From Language Models to Large Language Models, pp. 2-3'
- title: 'Sennrich, R., Haddow, B. and Birch, A. (2016). Neural Machine Translation of Rare Words with Subword Units. ACL 2016'
  resource: 'https://arxiv.org/abs/1508.07909'
  kind: paper
- title: 'Kudo, T. and Richardson, J. (2018). SentencePiece: A simple and language independent subword tokenizer. EMNLP 2018'
  resource: 'https://arxiv.org/abs/1808.06226'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/tokenization.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Tokenização

**Definição.** Huyen descreve o token como a unidade básica de um modelo de linguagem: um caractere, uma palavra ou parte de uma palavra (como "-ção"), conforme o modelo. A tokenização por caractere tem um vocabulário mínimo, mas sequências muito longas; a por palavra tem vocabulários enormes e não lida com palavras desconhecidas. A tokenização por subpalavras é o meio-termo dos modelos modernos: algoritmos como byte-pair encoding (BPE), WordPiece e Unigram aprendem com um corpus quais sequências frequentes de caracteres juntar, de modo que palavras comuns ficam inteiras e as raras se dividem em pedaços conhecidos. O SentencePiece trabalha sobre o texto bruto, incluindo espaços, o que o torna independente de idioma. Cada token vira um id e depois um embedding.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `nlp`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

A tokenização explica vários fatos do dia a dia com LLMs: preços, limites de taxa e janelas de contexto são contados em tokens, não em palavras; o mesmo texto custa mais tokens em idiomas pouco representados quando o tokenizador foi treinado (português costuma precisar de mais tokens que inglês para o mesmo conteúdo); e falhas estranhas, como contar letras errado, vêm do modelo nunca ver caracteres individuais. Um tokenizador é treinado uma vez e congelado junto com o modelo, então não pode ser trocado livremente (Tunstall et al., cap. 10).

## Exemplo

"Tokenização" pode virar ["Token", "ização"]: dois ids de um vocabulário de ~100 mil, cada um convertido em vetor antes de o transformer vê-lo.

## Conceitos mais amplos

- [Grande modelo de linguagem (LLM) e modelo de fundação](/pt-br/concepts/ai-ml/large-language-model.md)

## Conceitos relacionados

- [Embedding](/pt-br/concepts/ai-ml/embedding.md)

## Fontes

1. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 2 Character, Word and Subword Tokenization, pp. 29-35; ch. 4 The Tokenizer Pipeline and SentencePiece, pp. 94-95; ch. 10 Building a Tokenizer, pp. 310-322. Compara os três níveis de tokenização e mostra como treinar um tokenizador de subpalavras próprio.
2. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 1 From Language Models to Large Language Models, pp. 2-3. O token como unidade básica de um modelo de linguagem, e por que o tamanho do vocabulário importa.
3. Sennrich, R., Haddow, B. and Birch, A. (2016). *Neural Machine Translation of Rare Words with Subword Units*. ACL 2016. <https://arxiv.org/abs/1508.07909> — Introduziu as subpalavras BPE em modelos neurais.
4. Kudo, T. and Richardson, J. (2018). *SentencePiece: A simple and language independent subword tokenizer*. EMNLP 2018. <https://arxiv.org/abs/1808.06226> — SentencePiece: tokenizador de subpalavras independente de idioma.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
