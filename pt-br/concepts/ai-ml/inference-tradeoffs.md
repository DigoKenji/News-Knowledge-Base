---
type: Concept
title: Throughput/latência/custo de inferência
description: Throughput/latência/custo de inferência (llm-engineering).
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 9 Inference Performance Metrics p. 412; AI Accelerators p. 419; Inference Service Optimization (batching, parallelism) p. 440'
lang: pt-br
translation_of: /concepts/ai-ml/inference-tradeoffs.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição curta acrescentada'
---

# Throughput/latência/custo de inferência

**Definição.** Servir um modelo envolve trocar latência (tempo até o primeiro token, tempo por token de saída), throughput (tokens ou requisições por segundo) e custo. O batching aumenta o throughput e reduz o custo por token, mas pode adicionar latência; modelos menores ou quantizados são mais baratos e rápidos, mas podem perder qualidade. Huyen explica que o prefill é limitado por computação e a decodificação pela largura de banda de memória, por isso são otimizados de formas diferentes (cap. 9).

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 9 Inference Performance Metrics p. 412; AI Accelerators p. 419; Inference Service Optimization (batching, parallelism) p. 440: Métricas de latência, throughput, utilização e custo, e as alavancas no nível do serviço.

## Fontes

1. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 9 Inference Performance Metrics p. 412; AI Accelerators p. 419; Inference Service Optimization (batching, parallelism) p. 440. Métricas de latência, throughput, utilização e custo, e as alavancas no nível do serviço.

## Relações
- [KV cache](/pt-br/concepts/ai-ml/kv-cache.md) **parte-de** este

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
