---
type: Concept
title: Orquestração multiagente
description: 'Coordenar vários agentes de IA com papéis ou ferramentas diferentes - quem age quando, como passam o trabalho adiante e compartilham estado - para concluir uma tarefa que nenhum faria tão bem sozinho.'
tags:
- ai-ml
- ai-agents
domain: ai-ml
subdomain: ai-agents
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 6 RAG and Agents: Agents (planning, multi-agent systems), p. 281'
- title: 'Wu, Q. et al. (2023). AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation. arXiv:2308.08155'
  resource: 'https://arxiv.org/abs/2308.08155'
  kind: paper
- title: 'Anthropic (2025). How we built our multi-agent research system'
  resource: 'https://www.anthropic.com/engineering/multi-agent-research-system'
  kind: practitioner
lang: pt-br
translation_of: /concepts/ai-ml/multi-agent-orchestration.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Orquestração multiagente

**Definição.** A orquestração multiagente divide uma tarefa entre vários agentes LLM - por exemplo, um planejador, trabalhadores com ferramentas diferentes e um crítico - e define como são coordenados: um orquestrador central que delega e junta resultados, um pipeline fixo, ou agentes que conversam até terminar. Huyen observa que até o roteamento de intenção ou a reflexão podem ser vistos como um sistema multiagente.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `ai-agents`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Ganha paralelismo, separação de contexto e especialização ao custo de mais tokens, depuração mais difícil e novos modos de falha (agentes que não se entendem, trabalho duplicado). A Anthropic relata grandes ganhos em tarefas amplas de pesquisa com o desenho orquestrador-trabalhadores, e também consumo de tokens bem maior.

## Exemplo

O agente principal de um assistente de pesquisa divide 'comparar três bancos vetoriais' em três subperguntas, roda um agente de busca para cada uma em paralelo e escreve a comparação com o que eles encontraram.

## Fontes

1. Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 6 RAG and Agents: Agents (planning, multi-agent systems), p. 281. Visão multiagente de planejamento, roteamento e reflexão.
2. Wu, Q. et al. (2023). AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation. arXiv:2308.08155. <https://arxiv.org/abs/2308.08155> — Framework de agentes que se coordenam por conversa.
3. Anthropic (2025). How we built our multi-agent research system. <https://www.anthropic.com/engineering/multi-agent-research-system> — Desenho orquestrador-trabalhadores, ganhos e custo em tokens em produção.

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
