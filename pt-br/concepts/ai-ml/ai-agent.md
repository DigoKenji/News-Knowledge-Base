---
type: Concept
title: Agente de IA
description: Um sistema em que um modelo de linguagem decide, passo a passo, que ações executar com ferramentas para atingir um objetivo, observando os resultados e se adaptando, com supervisão humana limitada.
tags:
- ai-ml
- ai-agents
domain: ai-ml
subdomain: ai-agents
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Russell, S. and Norvig, P. (2021). Artificial Intelligence - A Modern Approach, 4th ed., ch. 2 Intelligent Agents. Pearson
  resource: 'https://aima.cs.berkeley.edu/'
  kind: book
- title: 'Yao, S. et al. (2023). ReAct - Synergizing Reasoning and Acting in Language Models. ICLR 2023, arXiv:2210.03629'
  resource: 'https://arxiv.org/abs/2210.03629'
  kind: paper
- title: 'Wang, L. et al. (2024). A Survey on Large Language Model based Autonomous Agents. Frontiers of Computer Science, arXiv:2308.11432'
  resource: 'https://arxiv.org/abs/2308.11432'
  kind: paper
- title: Anthropic (2024). Building effective agents
  resource: 'https://www.anthropic.com/research/building-effective-agents'
  kind: practitioner
lang: pt-br
translation_of: /concepts/ai-ml/ai-agent.md
---

# Agente de IA

**Definição.** Na IA clássica, um agente é qualquer coisa que percebe seu ambiente e age sobre ele para atingir objetivos. Um agente de IA baseado em LLM é um sistema em que o próprio modelo dirige o processo: planeja, escolhe ferramentas, lê os resultados e decide o próximo passo até atingir o objetivo ou devolver a um humano. A Anthropic separa workflows, em que o código fixa a sequência de chamadas ao LLM, de agentes, em que o modelo controla a sequência.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `ai-agents`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

O padrão central é o ReAct: o modelo alterna raciocínio ("preciso do log do teste que falhou") com ações (chamar uma ferramenta) e observações (a saída da ferramenta). Em volta fica um harness que fornece ferramentas, memória e guardrails. Agentes podem ser únicos ou multiagente (um orquestrador delegando a subagentes especializados). Autonomia é um espectro: de um chatbot que chama uma API, a agentes de código que rodam por horas, a sistemas que agem sem nenhum checkpoint humano.

## Por que importa e trade-offs

Agentes lidam com tarefas abertas cujos passos não podem ser roteirizados de antemão. Em troca custam mais (muitas chamadas ao modelo), são menos previsíveis, acumulam erros em cadeias longas e ampliam a superfície de segurança porque agem sobre entrada não confiável com permissões reais. Use um workflow fixo quando os passos são conhecidos; use um agente quando não são.

## Exemplo

Um agente de código que recebe "corrija o job de CI que está falhando" lê o log, abre o arquivo, edita, roda o teste de novo e abre um pull request, escolhendo cada passo sozinho.

## Fontes

1. Russell, S. and Norvig, P. (2021). *Artificial Intelligence - A Modern Approach*, 4th ed., ch. 2 Intelligent Agents. Pearson. <https://aima.cs.berkeley.edu/> — A definição clássica de agente como algo que percebe e age num ambiente.
2. Yao, S. et al. (2023). *ReAct - Synergizing Reasoning and Acting in Language Models*. ICLR 2023, arXiv:2210.03629. <https://arxiv.org/abs/2210.03629> — O loop raciocinar-agir-observar por trás da maioria dos agentes de LLM.
3. Wang, L. et al. (2024). *A Survey on Large Language Model based Autonomous Agents*. Frontiers of Computer Science, arXiv:2308.11432. <https://arxiv.org/abs/2308.11432> — Revisão dos componentes de agentes (perfil, memória, planejamento, ação).
4. Anthropic (2024). *Building effective agents*. <https://www.anthropic.com/research/building-effective-agents> — Workflows vs agentes, e quando cada um é apropriado.

## Relações
- (nenhuma ainda)

## Notas
- [Você acha que a IA está superestimada? Espere até ver o que vem a seguir.](/pt-br/notes/ai-ml/2026-11-20-you-think-ai-is-overhyped-just-wait-until-you-see-what-comes.md)
