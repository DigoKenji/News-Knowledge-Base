---
type: Concept
title: Agent memory
description: Information an AI agent stores outside the model and retrieves in later steps or sessions - facts, past events and learned procedures - because the context window alone forgets everything between calls.
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
  target: /concepts/ai-ml/ai-agent.md
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
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 6 Memory, pp. 300-305'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Agent memory

**Definition.** Agent memory is the set of stores an agent writes to and reads from so it can use information beyond its current context window. A model call is stateless; memory is how the harness gives an agent continuity, personalization and the ability to learn from experience without retraining.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `ai-agents`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

The CoALA framework, borrowing from cognitive science, separates working memory (what is in the context window now) from long-term memory, which comes in three kinds: episodic (records of past events and conversations), semantic (facts about the user or the world) and procedural (skills, rules and code the agent can reuse). Implementations store long-term memory as files, key-value records, vector indexes or knowledge graphs. The agent (or the harness) decides what to write, often by summarizing or reflecting on recent events, and retrieves by recency, relevance and importance, as in the Generative Agents paper. MemGPT treats the context window like RAM and long-term stores like disk, paging information in and out.

## Why it matters and trade-offs

Memory lets agents carry preferences and lessons across sessions and handle tasks longer than one context window. The costs: retrieval can surface stale or wrong records, stores grow without bound unless consolidated, private data persists (a privacy and deletion problem), and anything written into memory becomes an attack surface, see memory poisoning.

## Example

This project's own memory folder is agent memory: one file per subject that later threads read before they start, so Kenji does not have to repeat his budget or repository names.

## Not to be confused with

RAG over a fixed document corpus is retrieval, not memory: the agent does not write to it. Fine-tuning changes the weights; memory leaves them untouched.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 6 Memory, pp. 300-305: Internal knowledge, short-term context and long-term external memory for agents.

## Sources

1. Sumers, T. et al. (2024). *Cognitive Architectures for Language Agents (CoALA)*. TMLR, arXiv:2309.02427. <https://arxiv.org/abs/2309.02427> — Working vs episodic, semantic and procedural memory for language agents.
2. Park, J. S. et al. (2023). *Generative Agents - Interactive Simulacra of Human Behavior*. UIST 2023, arXiv:2304.03442. <https://arxiv.org/abs/2304.03442> — Memory stream with retrieval by recency, importance and relevance, plus reflection.
3. Packer, C. et al. (2023). *MemGPT - Towards LLMs as Operating Systems*. arXiv:2310.08560. <https://arxiv.org/abs/2310.08560> — Context window as main memory, external stores as disk, paging between them.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 6 Memory, pp. 300-305. Internal knowledge, short-term context and long-term external memory for agents.

## Relations
- **part-of** [AI agent](/concepts/ai-ml/ai-agent.md)

## Notes
- [MemFolder: Structure-Guided Memory Folding for Efficient Agent Memory Management](/notes/ai-ml/2026-11-21-memfolder-structure-guided-memory-folding-for-efficient-agen.md)
