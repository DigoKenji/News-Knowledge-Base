---
type: Concept
title: Harness de agente
description: O software em volta de um modelo de linguagem que o transforma em agente - monta o contexto, expõe ferramentas, executa o loop, aplica regras e guarda estado entre passos.
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
  at: '2026-11-17T21:40:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Anthropic (2024). Building effective agents
  resource: 'https://www.anthropic.com/research/building-effective-agents'
  kind: practitioner
- title: Anthropic (2025). Effective harnesses for long-running agents
  resource: 'https://www.anthropic.com/research/effective-harnesses-for-long-running-agents'
  kind: practitioner
- title: 'Yang, J. et al. (2024). SWE-agent - Agent-Computer Interfaces Enable Automated Software Engineering. NeurIPS 2024, arXiv:2405.15793'
  resource: 'https://arxiv.org/abs/2405.15793'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/agent-harness.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Harness de agente

**Definição.** Um harness de agente é tudo num agente de IA que não são os pesos do modelo: o código que monta o prompt e o contexto de cada passo, oferece ferramentas e executa as chamadas que o modelo pede, roda o loop planejar-agir-observar, decide quando parar e guarda memória, logs e checkpoints. O mesmo modelo pode se comportar de forma muito diferente em harnesses diferentes.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `ai-agents`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Um loop típico de harness: (1) monta a janela de contexto com o prompt de sistema, a tarefa, arquivos relevantes, memória e os resultados de chamadas de ferramenta anteriores; (2) chama o modelo; (3) interpreta a resposta e, se ela pede uma ferramenta, valida os argumentos, verifica permissões e executa (shell, navegador, API, busca); (4) anexa o resultado e repete até o modelo sinalizar que terminou ou um orçamento, limite de passos ou guardrail interromper. Em volta do loop o harness adiciona sandbox, retentativas, compactação de contexto quando a janela enche e checkpoints para que tarefas longas possam ser retomadas. O SWE-agent chamou a camada de ferramentas de "agent-computer interface" e mostrou que projetá-la bem aumentou o sucesso nas tarefas com o mesmo modelo.

## Por que importa e trade-offs

Regras que vivem só no prompt são sugestões; regras aplicadas no código do harness (ferramentas permitidas, limites de gasto, etapas de aprovação, validação de saída) são garantias. O harness também determina o custo, porque decide quanto contexto é reenviado a cada passo. O trade-off é a complexidade: a orientação da Anthropic é começar com o loop mais simples e acrescentar estrutura (workflows, subagentes, memória) só quando uma falha medida exigir.

## Exemplo

Claude Code, Codex CLI e SWE-agent rodam modelos por meio de harnesses que leem e editam arquivos, executam testes em sandbox e perguntam ao usuário antes de comandos arriscados. A execução diária do Project_News é ela mesma um pequeno harness: uma tarefa agendada com prompts fixos, uma lista de fontes e um validador.

## Não confundir com

Um framework de agentes (LangGraph, CrewAI) é uma biblioteca para construir harnesses; o harness é o sistema em execução. Um harness de avaliação (por exemplo lm-evaluation-harness) é ferramental de teste, outro uso da palavra.

## Fontes

1. Anthropic (2024). *Building effective agents*. <https://www.anthropic.com/research/building-effective-agents> — Agente = modelo usando ferramentas em loop; começar simples e só adicionar estrutura de workflow quando necessário.
2. Anthropic (2025). *Effective harnesses for long-running agents*. <https://www.anthropic.com/research/effective-harnesses-for-long-running-agents> — O termo "harness" e o que ele gerencia em tarefas longas (arquivos de progresso, checkpoints, retomada).
3. Yang, J. et al. (2024). *SWE-agent - Agent-Computer Interfaces Enable Automated Software Engineering*. NeurIPS 2024, arXiv:2405.15793. <https://arxiv.org/abs/2405.15793> — Evidência revisada por pares de que a interface em volta de um modelo fixo muda o sucesso nas tarefas.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
