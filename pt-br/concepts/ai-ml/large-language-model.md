---
type: Concept
title: Grande modelo de linguagem (LLM) e modelo de fundação
description: Um modelo transformer com bilhões de parâmetros, pré-treinado em enormes quantidades de texto para prever o próximo token e depois ajustado para seguir instruções; modelo de fundação é a categoria mais ampla de grandes modelos pré-treinados adaptados a muitas tarefas.
tags:
- ai-ml
- core
domain: ai-ml
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: 'Bommasani, R. et al. (2021). On the Opportunities and Risks of Foundation Models. Stanford CRFM, arXiv:2108.07258'
  resource: 'https://arxiv.org/abs/2108.07258'
  kind: paper
- title: 'Zhao, W. X. et al. (2023). A Survey of Large Language Models. arXiv:2303.18223'
  resource: 'https://arxiv.org/abs/2303.18223'
  kind: paper
- title: NIST (2024). AI 600-1 - Artificial Intelligence Risk Management Framework - Generative Artificial Intelligence Profile
  resource: 'https://doi.org/10.6028/NIST.AI.600-1'
  kind: standard
lang: pt-br
translation_of: /concepts/ai-ml/large-language-model.md
---

# Grande modelo de linguagem (LLM) e modelo de fundação

**Definição.** Um LLM aprende linguagem, fatos e padrões de raciocínio como subproduto de prever o próximo token em trilhões de tokens (pré-treino auto-supervisionado). O pós-treino (fine-tuning supervisionado com instruções, RLHF e similares) o transforma num assistente. O CRFM de Stanford cunhou "foundation model" (2021) para modelos treinados em dados amplos e em escala que podem ser adaptados a muitas tarefas; os LLMs são o principal exemplo, e muitos já são multimodais.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

LLMs são adaptados sem retreino por meio de prompts e aprendizado em contexto, ancorados com recuperação (RAG), customizados de forma barata com LoRA ou destilação, e recebem ferramentas para agir como agentes. Suas fraquezas conhecidas são alucinação (saída fluente mas falsa), sensibilidade ao prompt, data de corte do conhecimento, custo e problemas de segurança como prompt injection; o perfil de IA generativa do NIST lista esses riscos.

## Exemplo

Claude, GPT, Gemini e Llama são LLMs; o mesmo modelo base pode escrever um resumo, traduzir para português ou classificar um artigo dependendo só do prompt.

## Conceitos mais específicos

- [Aprendizado em contexto](/pt-br/concepts/ai-ml/in-context-learning.md)
- [Adaptação de baixo posto (LoRA)](/pt-br/concepts/ai-ml/lora.md)
- [Destilação](/pt-br/concepts/ai-ml/distillation.md)
- [Geração aumentada por recuperação (RAG)](/pt-br/concepts/ai-ml/rag.md)
- [Agente de IA](/pt-br/concepts/ai-ml/ai-agent.md)
- [LLM como juiz](/pt-br/concepts/ai-ml/llm-as-a-judge.md)

## Fontes

1. Bommasani, R. et al. (2021). *On the Opportunities and Risks of Foundation Models*. Stanford CRFM, arXiv:2108.07258. <https://arxiv.org/abs/2108.07258> — Definição de modelos de fundação, adaptação, riscos.
2. Zhao, W. X. et al. (2023). *A Survey of Large Language Models*. arXiv:2303.18223. <https://arxiv.org/abs/2303.18223> — Pré-treino, pós-treino, capacidades e avaliação de LLMs.
3. NIST (2024). *AI 600-1 - Artificial Intelligence Risk Management Framework - Generative Artificial Intelligence Profile*. <https://doi.org/10.6028/NIST.AI.600-1> — Descrição em padrão governamental da IA generativa e de seus riscos.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
