---
type: Concept
title: KV cache
description: KV cache (llm-engineering).
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
relations:
- type: part-of
  target: /concepts/ai-ml/inference-tradeoffs.md
sources:
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 8 KV cache, pp. 291-295'
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 9 Model Optimization (attention and KV cache), pp. 426-440'
lang: pt-br
translation_of: /concepts/ai-ml/kv-cache.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição curta acrescentada'
---

# KV cache

**Definição.** Na decodificação autorregressiva, cada token novo presta atenção a todos os anteriores. O KV cache guarda os vetores de chave e valor já calculados para os tokens anteriores, para não recalculá-los a cada passo. O preço é uma memória que cresce com o tamanho da sequência, o tamanho do lote e o número de camadas e cabeças, o que muitas vezes limita quantas requisições uma GPU consegue atender (Iusztin e Labonne, cap. 8; Huyen, cap. 9).

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 8 KV cache, pp. 291-295: Por que o cache acelera a decodificação e quanta memória custa.
- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 9 Model Optimization (attention and KV cache), pp. 426-440: O tamanho do KV cache como gargalo principal da inferência.

## Fontes

1. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 8 KV cache, pp. 291-295. Por que o cache acelera a decodificação e quanta memória custa.
2. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 9 Model Optimization (attention and KV cache), pp. 426-440. O tamanho do KV cache como gargalo principal da inferência.

## Relações
- **parte-de** [Throughput/latência/custo de inferência](/pt-br/concepts/ai-ml/inference-tradeoffs.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
