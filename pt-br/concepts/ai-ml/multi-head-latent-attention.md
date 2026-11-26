---
type: Concept
title: Atenção latente multi-cabeça
description: Uma variante de atenção introduzida no DeepSeek-V2 que comprime as chaves e valores de cada token num pequeno vetor latente compartilhado, reduzindo o KV cache várias vezes e mantendo a qualidade próxima da atenção multi-head padrão.
tags:
- ai-ml
- deep-learning
domain: ai-ml
subdomain: deep-learning
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'DeepSeek-AI (2024). DeepSeek-V2 - A Strong, Economical, and Efficient Mixture-of-Experts Language Model. arXiv:2405.04434'
  resource: 'https://arxiv.org/abs/2405.04434'
  kind: paper
- title: 'Shazeer, N. (2019). Fast Transformer Decoding - One Write-Head is All You Need. arXiv:1911.02150'
  resource: 'https://arxiv.org/abs/1911.02150'
  kind: paper
- title: 'Ainslie, J. et al. (2023). GQA - Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints. EMNLP 2023, arXiv:2305.13245'
  resource: 'https://arxiv.org/abs/2305.13245'
  kind: paper
- title: 'Meng, F. et al. (2025). TransMLA - Multi-Head Latent Attention Is All You Need. arXiv:2502.07864'
  resource: 'https://arxiv.org/abs/2502.07864'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/multi-head-latent-attention.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Atenção latente multi-cabeça

**Definição.** Multi-head latent attention (MLA) guarda, para cada token anterior, um único vetor latente de baixa dimensão em vez de vetores de chave e valor separados para cada cabeça de atenção. Na hora da atenção, as chaves e valores de cada cabeça são reconstruídos (ou incorporados às projeções de consulta e saída) a partir desse latente.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md). Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Durante a geração, as chaves e valores de cada token ficam em cache para não serem recalculados; em contextos longos esse KV cache, e não os pesos, limita o tamanho do lote e a velocidade. Soluções anteriores compartilhavam chaves e valores entre cabeças: multi-query attention (MQA, um K/V compartilhado) e grouped-query attention (GQA, alguns grupos), trocando um pouco de qualidade por memória. A MLA, em vez disso, aplica uma projeção aprendida de posto baixo para um latente de algumas centenas de dimensões e projeta de volta para cada cabeça quando necessário. Como a rotação de posição do RoPE não comuta com essa compressão, a DeepSeek mantém uma pequena parte desacoplada da chave que carrega a posição. O DeepSeek-V2 relatou um KV cache 93% menor que o do antecessor denso e qualidade maior que a do GQA.

## Por que importa e trade-offs

KV cache menor significa contextos mais longos, lotes maiores e serviço mais barato. O custo é cálculo extra de projeções, uma implementação mais complexa (os kernels precisam suportar o layout latente) e o fato de que precisa ser previsto no pré-treino; converter modelos GQA existentes é uma área de pesquisa ativa.

## Exemplo

No DeepSeek-V3, cada token guarda em cache um latente de 512 dimensões mais uma chave de posição de 64 dimensões, em vez de 128 cabeças × 128 dimensões para chaves e valores.

## Fontes

1. DeepSeek-AI (2024). *DeepSeek-V2 - A Strong, Economical, and Efficient Mixture-of-Experts Language Model*. arXiv:2405.04434. <https://arxiv.org/abs/2405.04434> — Introdução da MLA, RoPE desacoplado, números de redução do KV cache.
2. Shazeer, N. (2019). *Fast Transformer Decoding - One Write-Head is All You Need*. arXiv:1911.02150. <https://arxiv.org/abs/1911.02150> — Multi-query attention, a forma anterior de reduzir o KV cache.
3. Ainslie, J. et al. (2023). *GQA - Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints*. EMNLP 2023, arXiv:2305.13245. <https://arxiv.org/abs/2305.13245> — Grouped-query attention, a referência com que a MLA é comparada.
4. Meng, F. et al. (2025). *TransMLA - Multi-Head Latent Attention Is All You Need*. arXiv:2502.07864. <https://arxiv.org/abs/2502.07864> — Análise independente mostrando que a MLA é mais expressiva que o GQA com o mesmo tamanho de cache.

## Relações
- (nenhuma ainda)

## Notas
- [O ciclo de realimentação de modelos de pesos abertos EUA-China](/pt-br/notes/ai-ml/2026-11-22-the-us-china-open-weight-feedback-loop.md)
