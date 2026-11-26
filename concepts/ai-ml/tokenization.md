---
type: Concept
title: 'Tokenization'
description: 'Splitting text into the units a language model reads and writes - usually subword pieces learned from data (BPE, WordPiece, SentencePiece) - which sets the model''s vocabulary, its context limits and its cost per request.'
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
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Tokenization

**Definition.** Huyen describes the token as the basic unit of a language model: a character, a word or part of a word (like "-tion"), depending on the model. Character tokenization gives a tiny vocabulary but very long sequences; word tokenization gives huge vocabularies and cannot handle unseen words. Subword tokenization is the compromise used by modern models: algorithms such as byte-pair encoding (BPE), WordPiece and Unigram learn from a corpus which frequent character sequences to merge, so common words stay whole and rare ones split into known pieces. SentencePiece works on raw text including spaces, which makes it language independent. Each token maps to an id and then to an embedding.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `nlp`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Tokenization explains several everyday LLM facts: prices, rate limits and context windows are counted in tokens, not words; the same text costs more tokens in languages that were under-represented when the tokenizer was trained (Portuguese usually needs more tokens than English for the same content); and odd failures such as miscounting letters come from the model never seeing individual characters. A tokenizer is trained once and frozen with the model, so it cannot be swapped freely (Tunstall et al., ch. 10).

## Example

"Tokenization" might become ["Token", "ization"]: two ids from a vocabulary of ~100k, each looked up as a vector before the transformer sees it.

## Broader concepts

- [Large language model and foundation model](/concepts/ai-ml/large-language-model.md)

## Related concepts

- [Embedding](/concepts/ai-ml/embedding.md)

## Sources

1. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 2 Character, Word and Subword Tokenization, pp. 29-35; ch. 4 The Tokenizer Pipeline and SentencePiece, pp. 94-95; ch. 10 Building a Tokenizer, pp. 310-322. Compares the three tokenization levels and walks through training a custom subword tokenizer.
2. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 1 From Language Models to Large Language Models, pp. 2-3. Token as the basic unit of a language model, and why vocabulary size matters.
3. Sennrich, R., Haddow, B. and Birch, A. (2016). *Neural Machine Translation of Rare Words with Subword Units*. ACL 2016. <https://arxiv.org/abs/1508.07909> — Introduced BPE subword units for neural models.
4. Kudo, T. and Richardson, J. (2018). *SentencePiece: A simple and language independent subword tokenizer*. EMNLP 2018. <https://arxiv.org/abs/1808.06226> — SentencePiece: language-independent subword tokenizer.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
