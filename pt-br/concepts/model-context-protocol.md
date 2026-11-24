---
type: Concept
title: Model Context Protocol (MCP)
description: Um protocolo aberto (baseado em JSON-RPC) que padroniza como aplicações de IA se conectam a ferramentas, fontes de dados e prompts externos, para que qualquer cliente compatível use qualquer servidor compatível.
tags:
- ai-ml
- ai-agents
domain: ai-ml
subdomain: ai-agents
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Model Context Protocol. Specification 2025-06-18
  resource: 'https://modelcontextprotocol.io/specification/2025-06-18'
  kind: standard
- title: Anthropic (2024). Introducing the Model Context Protocol
  resource: 'https://www.anthropic.com/news/model-context-protocol'
  kind: practitioner
- title: 'Hou, X. et al. (2025). Model Context Protocol (MCP) - Landscape, Security Threats, and Future Research Directions. arXiv:2503.23278'
  resource: 'https://arxiv.org/abs/2503.23278'
  kind: paper
lang: pt-br
translation_of: /concepts/model-context-protocol.md
---

# Model Context Protocol (MCP)

**Definição.** O Model Context Protocol (MCP) define um contrato cliente-servidor entre uma aplicação de IA (o host, como o Claude Desktop ou uma IDE) e servidores que expõem capacidades. A Anthropic o lançou em novembro de 2024 como padrão aberto; é muitas vezes descrito como "USB-C para apps de IA" porque troca uma integração sob medida por par app-ferramenta por um único protocolo.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `ai-agents`. Expandido em 2026-11-24 com fontes independentes; ainda um rascunho aguardando a certificação de Kenji.

## Como funciona

As mensagens são JSON-RPC 2.0 sobre stdio (servidores locais) ou HTTP com streaming (servidores remotos). Servidores expõem três primitivas: tools (funções que o modelo pode chamar), resources (dados que o app pode ler para o contexto) e prompts (modelos reutilizáveis). Clientes podem oferecer sampling (permitir que um servidor peça uma resposta ao modelo do host) e elicitation (pedir informação ao usuário). Servidores remotos se autenticam com OAuth 2.1. Uma negociação de capacidades na conexão diz a cada lado o que o outro suporta.

## Por que importa e trade-offs

O MCP foi adotado rapidamente por OpenAI, Google, Microsoft e pela maioria das ferramentas de agentes, então um servidor funciona em muitos clientes. Segurança é a principal preocupação: descrições de ferramentas e conteúdo retornado são entrada não confiável que pode carregar prompt injection ("tool poisoning"), servidores podem pedir permissões demais, e um servidor malicioso ou comprometido é um risco de supply chain. Os hosts precisam mostrar e controlar o que as ferramentas fazem.

## Exemplo

Os conectores do Notion e do GitHub usados neste projeto são servidores MCP: o mesmo servidor funciona no Claude, numa IDE ou em outro agente.

## Fontes

1. Model Context Protocol. *Specification 2025-06-18*. <https://modelcontextprotocol.io/specification/2025-06-18> — Arquitetura, transportes, primitivas, autorização.
2. Anthropic (2024). *Introducing the Model Context Protocol*. <https://www.anthropic.com/news/model-context-protocol> — Origem e motivação (trocar conectores por integração por um padrão único).
3. Hou, X. et al. (2025). *Model Context Protocol (MCP) - Landscape, Security Threats, and Future Research Directions*. arXiv:2503.23278. <https://arxiv.org/abs/2503.23278> — Análise independente do ciclo de vida do protocolo e de suas ameaças de segurança.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
