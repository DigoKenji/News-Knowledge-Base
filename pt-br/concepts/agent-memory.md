---
type: Concept
title: Memória de agente
description: Informação que um agente de IA guarda fora do modelo e recupera em passos ou sessões posteriores - fatos, eventos passados e procedimentos aprendidos - porque a janela de contexto sozinha esquece tudo entre chamadas.
tags:
- ai-ml
- ai-agents
domain: ai-ml
subdomain: ai-agents
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
relations:
- type: part-of
  target: /concepts/ai-agent.md
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Sumers, T. et al. (2024). Cognitive Architectures for Language Agents (CoALA). TMLR, arXiv:2309.02427'
  resource: 'https://arxiv.org/abs/2309.02427'
  kind: paper
- title: 'Park, J. S. et al. (2023). Generative Agents - Interactive Simulacra of Human Behavior. UIST 2023, arXiv:2304.03442'
  resource: 'https://arxiv.org/abs/2304.03442'
  kind: paper
- title: 'Packer, C. et al. (2023). MemGPT - Towards LLMs as Operating Systems. arXiv:2310.08560'
  resource: 'https://arxiv.org/abs/2310.08560'
  kind: paper
lang: pt-br
translation_of: /concepts/agent-memory.md
---

# Memória de agente

**Definição.** Memória de agente é o conjunto de repositórios em que um agente escreve e dos quais lê para usar informação além da sua janela de contexto atual. Uma chamada ao modelo não guarda estado; a memória é como o harness dá ao agente continuidade, personalização e a capacidade de aprender com a experiência sem retreinar.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `ai-agents`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

O framework CoALA, emprestando da ciência cognitiva, separa a memória de trabalho (o que está na janela de contexto agora) da memória de longo prazo, que tem três tipos: episódica (registros de eventos e conversas passadas), semântica (fatos sobre o usuário ou o mundo) e procedural (habilidades, regras e código que o agente pode reutilizar). As implementações guardam a memória de longo prazo como arquivos, registros chave-valor, índices vetoriais ou grafos de conhecimento. O agente (ou o harness) decide o que escrever, muitas vezes resumindo ou refletindo sobre eventos recentes, e recupera por recência, relevância e importância, como no artigo Generative Agents. O MemGPT trata a janela de contexto como RAM e os repositórios de longo prazo como disco, paginando informação para dentro e para fora.

## Por que importa e trade-offs

A memória permite que agentes levem preferências e lições entre sessões e lidem com tarefas mais longas que uma janela de contexto. Os custos: a recuperação pode trazer registros desatualizados ou errados, os repositórios crescem sem limite se não forem consolidados, dados privados persistem (um problema de privacidade e exclusão) e tudo que é escrito na memória vira superfície de ataque, veja envenenamento de memória.

## Exemplo

A própria pasta de memória deste projeto é memória de agente: um arquivo por assunto que threads posteriores leem antes de começar, para que Kenji não precise repetir seu orçamento ou os nomes dos repositórios.

## Não confundir com

RAG sobre um corpus fixo de documentos é recuperação, não memória: o agente não escreve nele. Fine-tuning muda os pesos; a memória não os toca.

## Fontes

1. Sumers, T. et al. (2024). *Cognitive Architectures for Language Agents (CoALA)*. TMLR, arXiv:2309.02427. <https://arxiv.org/abs/2309.02427> — Memória de trabalho vs episódica, semântica e procedural para agentes de linguagem.
2. Park, J. S. et al. (2023). *Generative Agents - Interactive Simulacra of Human Behavior*. UIST 2023, arXiv:2304.03442. <https://arxiv.org/abs/2304.03442> — Fluxo de memória com recuperação por recência, importância e relevância, mais reflexão.
3. Packer, C. et al. (2023). *MemGPT - Towards LLMs as Operating Systems*. arXiv:2310.08560. <https://arxiv.org/abs/2310.08560> — Janela de contexto como memória principal, repositórios externos como disco, paginação entre eles.

## Relações
- **part-of** [Agente de IA](/pt-br/concepts/ai-agent.md)

## Notas
- [MemFolder: Dobramento de memória guiado por estrutura para gerenciamento eficiente de memória de agentes](/pt-br/notes/ai-ml/2026-11-21-memfolder-structure-guided-memory-folding-for-efficient-agen.md)
