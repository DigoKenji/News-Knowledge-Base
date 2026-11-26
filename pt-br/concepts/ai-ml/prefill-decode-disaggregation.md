---
type: Concept
title: Desagregação prefill-decode
description: Executar as duas fases da inferência de LLM - prefill (processar o prompt) e decode (gerar tokens um a um) - em pools de GPU separados, transferindo o KV cache entre eles, para que cada um seja dimensionado e ajustado para seu próprio gargalo.
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude (Project_News daily run)
  at: '2026-11-16T06:50:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Zhong, Y. et al. (2024). DistServe - Disaggregating Prefill and Decoding for Goodput-optimized LLM Serving. OSDI 2024, arXiv:2401.09670'
  resource: 'https://arxiv.org/abs/2401.09670'
  kind: paper
- title: 'Patel, P. et al. (2024). Splitwise - Efficient Generative LLM Inference Using Phase Splitting. ISCA 2024, arXiv:2311.18677'
  resource: 'https://arxiv.org/abs/2311.18677'
  kind: paper
- title: 'Qin, R. et al. (2024). Mooncake - A KVCache-centric Disaggregated Architecture for LLM Serving. arXiv:2407.00079'
  resource: 'https://arxiv.org/abs/2407.00079'
  kind: paper
- title: vLLM. Disaggregated Prefilling (experimental)
  resource: 'https://docs.vllm.ai/en/latest/features/disagg_prefill/'
  kind: docs
relations:
- type: uses
  target: /concepts/ai-ml/kv-cache.md
- type: part-of
  target: /concepts/ai-ml/inference-tradeoffs.md
lang: pt-br
translation_of: /concepts/ai-ml/prefill-decode-disaggregation.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Desagregação prefill-decode

**Definição.** No serviço desagregado, uma requisição vai primeiro a uma instância de prefill que processa o prompt inteiro numa passagem paralela e constrói o KV cache. O cache é então enviado por uma interconexão rápida a uma instância de decode, que gera a saída token a token.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

As fases exigem o hardware de formas diferentes. O prefill é limitado por cálculo (grandes multiplicações de matrizes sobre muitos tokens) e define o tempo até o primeiro token (TTFT). O decode é limitado por banda de memória (um token por vez, lendo todos os pesos e o cache) e define o tempo por token de saída (TPOT). Em GPUs compartilhadas um prefill longo trava a geração de todos, então as metas de latência entram em conflito. Separá-las elimina a interferência e permite escolher paralelismo, tamanho de lote e até tipo de GPU por fase. O DistServe relatou até 7,4× mais requisições dentro do SLO; Splitwise e o Mooncake da Moonshot mostraram ganhos parecidos em cenários próximos de produção; vLLM, SGLang e NVIDIA Dynamo já suportam.

## Por que importa e trade-offs

Aumenta o goodput sob SLOs de latência rígidos em larga escala. Custos: mover o KV cache exige rede de alta banda (NVLink, RDMA), o escalonador fica mais complexo, e em implantações pequenas ou com prompts curtos o custo da transferência pode superar o benefício; chunked prefill em GPUs compartilhadas é a alternativa mais simples.

## Exemplo

Um serviço de chat com prompts de RAG de 20.000 tokens coloca o prefill em algumas GPUs fortes em cálculo e o decode em muitas GPUs com bastante memória, mantendo o streaming fluido enquanto prompts grandes chegam.

## Fontes

1. Zhong, Y. et al. (2024). *DistServe - Disaggregating Prefill and Decoding for Goodput-optimized LLM Serving*. OSDI 2024, arXiv:2401.09670. <https://arxiv.org/abs/2401.09670> — Interferência entre fases, desagregação, ganhos de goodput.
2. Patel, P. et al. (2024). *Splitwise - Efficient Generative LLM Inference Using Phase Splitting*. ISCA 2024, arXiv:2311.18677. <https://arxiv.org/abs/2311.18677> — Trabalho independente (Microsoft) sobre divisão de fases, incluindo hardware diferente por fase.
3. Qin, R. et al. (2024). *Mooncake - A KVCache-centric Disaggregated Architecture for LLM Serving*. arXiv:2407.00079. <https://arxiv.org/abs/2407.00079> — Implantação em produção (Kimi) construída em volta de um KV cache desagregado.
4. vLLM. *Disaggregated Prefilling (experimental)*. <https://docs.vllm.ai/en/latest/features/disagg_prefill/> — Suporte num motor de serviço open source muito usado.

## Relações
- **usa** [KV cache](/pt-br/concepts/ai-ml/kv-cache.md)
- **parte de** [Throughput/latência/custo de inferência](/pt-br/concepts/ai-ml/inference-tradeoffs.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
