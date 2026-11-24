---
type: Concept
title: Embedding posicional rotacional (RoPE)
description: Rotary Position Embedding - uma forma de codificar a posição dos tokens rotacionando os vetores de consulta e chave por ângulos que dependem da posição, para que os scores de atenção dependam da distância relativa entre tokens.
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: draft
generated:
  by: claude (Project_News daily run)
  at: '2026-11-16T06:50:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Su, J. et al. (2021). RoFormer - Enhanced Transformer with Rotary Position Embedding. arXiv:2104.09864 (Neurocomputing 2024)'
  resource: 'https://arxiv.org/abs/2104.09864'
  kind: paper
- title: Biderman, S. et al. (2021). Rotary Embeddings - A Relative Revolution. EleutherAI blog
  resource: 'https://blog.eleuther.ai/rotary-embeddings/'
  kind: practitioner
- title: 'Peng, B. et al. (2024). YaRN - Efficient Context Window Extension of Large Language Models. ICLR 2024, arXiv:2309.00071'
  resource: 'https://arxiv.org/abs/2309.00071'
  kind: paper
- title: 'Vaswani, A. et al. (2017). Attention Is All You Need. NeurIPS 2017, arXiv:1706.03762'
  resource: 'https://arxiv.org/abs/1706.03762'
  kind: paper
lang: pt-br
translation_of: /concepts/rope.md
---

# Embedding posicional rotacional (RoPE)

**Definição.** Transformers não têm noção própria de ordem, então a posição precisa ser injetada. O RoPE faz isso dividindo cada vetor de consulta e chave em pares de dimensões e rotacionando cada par por um ângulo proporcional à posição do token, com uma frequência diferente por par. O produto escalar entre consulta e chave rotacionadas passa a depender só da diferença entre suas posições.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Expandido em 2026-11-24 com fontes independentes; ainda um rascunho aguardando a certificação de Kenji.

## Como funciona

O Transformer original somava vetores senoidais fixos aos embeddings (posição absoluta); modelos posteriores aprenderam vetores de posição. O RoPE (Su et al., 2021) aplica a rotação dentro da atenção, o que dá comportamento de posição relativa sem parâmetros extras e funciona com kernels de atenção eficientes. Pares de baixa frequência acompanham posição de longo alcance, pares de alta frequência de curto alcance. Virou padrão no LLaMA, Mistral, Qwen, DeepSeek e na maioria dos LLMs abertos. Estender o contexto além do comprimento de treino é feito reescalando as frequências: position interpolation, NTK-aware scaling e YaRN.

## Por que importa e trade-offs

O RoPE é simples, sem parâmetros e bom em posição relativa, por isso venceu. Os modelos ainda pioram além do comprimento treinado sem reescala, e o RoPE complica a compressão do KV cache (veja multi-head latent attention, que precisa de uma parte de RoPE desacoplada).

## Exemplo

As palavras 5 e 8 de uma frase têm a mesma relação de atenção que as palavras 105 e 108, porque só a distância de 3 posições entra no score.

## Fontes

1. Su, J. et al. (2021). *RoFormer - Enhanced Transformer with Rotary Position Embedding*. arXiv:2104.09864 (Neurocomputing 2024). <https://arxiv.org/abs/2104.09864> — Método original e sua propriedade de posição relativa.
2. Biderman, S. et al. (2021). *Rotary Embeddings - A Relative Revolution*. EleutherAI blog. <https://blog.eleuther.ai/rotary-embeddings/> — Explicação independente e experimentos comparando RoPE com outros métodos de posição.
3. Peng, B. et al. (2024). *YaRN - Efficient Context Window Extension of Large Language Models*. ICLR 2024, arXiv:2309.00071. <https://arxiv.org/abs/2309.00071> — Extensão de contexto reescalando as frequências do RoPE.
4. Vaswani, A. et al. (2017). *Attention Is All You Need*. NeurIPS 2017, arXiv:1706.03762. <https://arxiv.org/abs/1706.03762> — A referência de posição absoluta senoidal que o RoPE substituiu.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
