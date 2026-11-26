---
type: Concept
title: Modelo de decisão estruturado
description: Um modelo usado para tomar uma decisão a partir de um conjunto fixo de saídas tipadas - um rótulo, uma nota, um sim/não, uma probabilidade - em vez de escrever texto livre; na prática, um classificador ou uma chamada de LLM com saída restrita.
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
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Willard, B. T. and Louf, R. (2023). Efficient Guided Generation for Large Language Models. arXiv:2307.09702'
  resource: 'https://arxiv.org/abs/2307.09702'
  kind: paper
- title: OpenAI. Structured model outputs (API guide)
  resource: 'https://developers.openai.com/api/docs/guides/structured-outputs'
  kind: docs
- title: Anthropic. Structured outputs (Claude API docs)
  resource: 'https://platform.claude.com/docs/en/build-with-claude/structured-outputs'
  kind: docs
lang: pt-br
translation_of: /concepts/ai-ml/structured-decision-model.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Modelo de decisão estruturado

**Definição.** Um modelo de decisão estruturado devolve uma resposta legível por máquina que segue um schema (por exemplo {"route": "billing", "confidence": 0.92}) para que o código possa agir sobre ela diretamente. Pode ser um pequeno classificador treinado para a tarefa, ou um LLM geral forçado ao schema com structured outputs ou decodificação restrita.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Dois caminhos. (1) Restringir um modelo generativo: o provedor (ou uma biblioteca como Outlines) mascara tokens durante a decodificação para que a saída sempre siga um JSON schema ou gramática, o que garante saída que pode ser lida por código. (2) Usar um modelo discriminativo: treinar ou destilar um pequeno classificador que devolve diretamente as probabilidades das classes, muito mais barato e com notas calibradas se bem treinado. As notas de 2026-11-17 descrevem fornecedores vendendo (2) como substituto barato de chamadas de LLM em roteamento, moderação e correção.

## Por que importa e trade-offs

Para decisões de alto volume, uma resposta tipada é mais fácil de validar, registrar, limitar e testar do que prosa. Um classificador pequeno é barato mas precisa de dados rotulados e não lida com categorias novas; um LLM restrito é flexível mas caro, e um schema garante formato, não acerto.

## Exemplo

Em vez de perguntar a um LLM "este artigo deve entrar no digest? explique", chamar um modelo que devolve {"select": true, "domain": "security", "score": 0.81}.

## Notas de revisão para Kenji

"Modelo de decisão estruturado" não é um termo estabelecido; vem de artigos de fornecedor (TypeSafe Jev). Considere rejeitá-lo, ou renomear para "structured outputs" ou "classificador no lugar de chamada de LLM". A definição acima descreve a ideia real por trás.

## Fontes

1. Willard, B. T. and Louf, R. (2023). *Efficient Guided Generation for Large Language Models*. arXiv:2307.09702. <https://arxiv.org/abs/2307.09702> — Decodificação restrita que garante saída de acordo com uma regex ou JSON schema.
2. OpenAI. *Structured model outputs* (API guide). <https://developers.openai.com/api/docs/guides/structured-outputs> — Saídas restritas por schema oferecidas por um grande provedor.
3. Anthropic. *Structured outputs* (Claude API docs). <https://platform.claude.com/docs/en/build-with-claude/structured-outputs> — A mesma capacidade num provedor independente.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
