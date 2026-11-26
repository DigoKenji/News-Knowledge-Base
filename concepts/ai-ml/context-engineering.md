---
type: Concept
title: Context engineering
description: Designing and managing everything a model sees at each call - instructions, retrieved documents, memory, tool definitions and results, history - so it has exactly the information it needs, in a form it can use, within a finite window.
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
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 5 Context Length and Context Efficiency, p. 218; ch. 10 Step 1. Enhance Context, p. 450'
relations:
- type: part-of
  target: ai-agent
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Context engineering

**Definition.** Context engineering is the discipline of curating the full set of tokens given to an LLM at inference time. Prompt engineering is the part concerned with wording the instructions; context engineering also covers what to retrieve, what to remember, which tools to expose, what to drop or summarise, and how to order it all. It is the broader idea, and the main lever for agent quality once the model is fixed.

Domain: [ai-ml](/domains/ai-ml.md). Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Core techniques: retrieval (RAG) to bring in only relevant documents; writing notes or memory to files outside the window and reading them back when needed; compaction, summarising old history when the window fills; selecting a small, well-described tool set; isolating sub-tasks in sub-agents with their own clean context; and structuring content clearly (headings, XML tags, examples). The reason it matters is that more context is not free: models attend unevenly across long inputs ("lost in the middle") and quality degrades as irrelevant tokens accumulate, which Anthropic calls context rot.

## Why it matters and trade-offs

Most agent failures are context failures: the model lacked a fact, saw a contradictory one, or was drowned in noise. Good context engineering raises quality and cuts cost. The trade-off is engineering effort and the risk of dropping something important when you summarise or filter.

## Example

Netflix (2026-11-24 note) gives coding agents curated repository docs, conventions and ownership data so they behave like an engineer who knows the codebase, instead of dumping the whole repository into the prompt.

## Review notes for Kenji

The draft definition was tied to coding agents; it now covers any LLM application.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 5 Context Length and Context Efficiency, p. 218; ch. 10 Step 1. Enhance Context, p. 450: Where to place information in long contexts and why context is the first architecture step.

## Sources

1. Anthropic (2025). *Effective context engineering for AI agents*. <https://www.anthropic.com/research/effective-context-engineering-for-ai-agents> — Definition, context rot, compaction, note-taking and sub-agent techniques.
2. Mei, L. et al. (2025). *A Survey of Context Engineering for Large Language Models*. arXiv:2507.13334. <https://arxiv.org/abs/2507.13334> — Academic survey framing context engineering as a discipline beyond prompt design.
3. Liu, N. F. et al. (2024). *Lost in the Middle - How Language Models Use Long Contexts*. TACL, arXiv:2307.03172. <https://arxiv.org/abs/2307.03172> — Evidence that models use information unevenly across long contexts.
4. LangChain (2025). *The rise of "context engineering"*. <https://www.langchain.com/blog/the-rise-of-context-engineering> — Independent practitioner framing - the right information, tools and format for the task.
5. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 5 Context Length and Context Efficiency, p. 218; ch. 10 Step 1. Enhance Context, p. 450. Where to place information in long contexts and why context is the first architecture step.

## Relations
- **part-of** [ai-agent](/concepts/ai-ml/ai-agent.md)

## Notes
- [Netflix at QConSF 2026: Retrofitting the SDLC for AI Coding Agents](/notes/software-engineering/2026-11-24-netflix-at-qconsf-2026-retrofitting-the-sdlc-for-ai-coding-a.md)
