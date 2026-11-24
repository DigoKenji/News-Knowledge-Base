---
type: Concept
title: Engenharia de contexto
description: Projetar e gerenciar tudo o que um modelo vê em cada chamada - instruções, documentos recuperados, memória, definições e resultados de ferramentas, histórico - para que tenha exatamente a informação de que precisa, numa forma utilizável, dentro de uma janela finita.
tags:
- ai-ml
domain: ai-ml
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Anthropic (2025). Effective context engineering for AI agents
  resource: 'https://www.anthropic.com/research/effective-context-engineering-for-ai-agents'
  kind: practitioner
- title: 'Mei, L. et al. (2025). A Survey of Context Engineering for Large Language Models. arXiv:2507.13334'
  resource: 'https://arxiv.org/abs/2507.13334'
  kind: paper
- title: 'Liu, N. F. et al. (2024). Lost in the Middle - How Language Models Use Long Contexts. TACL, arXiv:2307.03172'
  resource: 'https://arxiv.org/abs/2307.03172'
  kind: paper
- title: LangChain (2025). The rise of "context engineering"
  resource: 'https://www.langchain.com/blog/the-rise-of-context-engineering'
  kind: practitioner
relations:
- type: part-of
  target: ai-agent
lang: pt-br
translation_of: /concepts/context-engineering.md
---

# Engenharia de contexto

**Definição.** Engenharia de contexto é a disciplina de selecionar o conjunto completo de tokens entregue a um LLM no momento da inferência. Engenharia de prompt é a parte que cuida da redação das instruções; engenharia de contexto também cobre o que recuperar, o que lembrar, quais ferramentas expor, o que descartar ou resumir e em que ordem colocar tudo. É a ideia mais ampla, e a principal alavanca de qualidade de um agente quando o modelo está fixo.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md). Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Técnicas centrais: recuperação (RAG) para trazer só documentos relevantes; escrever notas ou memória em arquivos fora da janela e lê-las de volta quando necessário; compactação, resumindo o histórico antigo quando a janela enche; selecionar um conjunto pequeno e bem descrito de ferramentas; isolar subtarefas em subagentes com seu próprio contexto limpo; e estruturar o conteúdo com clareza (títulos, tags XML, exemplos). Isso importa porque mais contexto não é de graça: os modelos prestam atenção de forma desigual em entradas longas ("lost in the middle") e a qualidade cai à medida que tokens irrelevantes se acumulam, o que a Anthropic chama de context rot.

## Por que importa e trade-offs

A maioria das falhas de agentes são falhas de contexto: faltou um fato ao modelo, ele viu um contraditório ou foi afogado em ruído. Boa engenharia de contexto aumenta a qualidade e reduz custo. O trade-off é o esforço de engenharia e o risco de descartar algo importante ao resumir ou filtrar.

## Exemplo

A Netflix (nota de 2026-11-24) dá aos agentes de código documentação selecionada do repositório, convenções e dados de responsabilidade para que se comportem como um engenheiro que conhece o código, em vez de despejar o repositório inteiro no prompt.

## Notas de revisão para Kenji

A definição do rascunho estava presa a agentes de código; agora cobre qualquer aplicação de LLM.

## Fontes

1. Anthropic (2025). *Effective context engineering for AI agents*. <https://www.anthropic.com/research/effective-context-engineering-for-ai-agents> — Definição, context rot, compactação, anotações e técnicas de subagentes.
2. Mei, L. et al. (2025). *A Survey of Context Engineering for Large Language Models*. arXiv:2507.13334. <https://arxiv.org/abs/2507.13334> — Revisão acadêmica que trata engenharia de contexto como disciplina além do design de prompts.
3. Liu, N. F. et al. (2024). *Lost in the Middle - How Language Models Use Long Contexts*. TACL, arXiv:2307.03172. <https://arxiv.org/abs/2307.03172> — Evidência de que os modelos usam a informação de forma desigual em contextos longos.
4. LangChain (2025). *The rise of "context engineering"*. <https://www.langchain.com/blog/the-rise-of-context-engineering> — Visão independente de praticantes - a informação, as ferramentas e o formato certos para a tarefa.

## Relações
- **part-of** [ai-agent](/pt-br/concepts/ai-agent.md)

## Notas
- [Netflix no QConSF 2026: Adaptando o SDLC para agentes de codificação com IA](/pt-br/notes/software-engineering/2026-11-24-netflix-at-qconsf-2026-retrofitting-the-sdlc-for-ai-coding-a.md)
