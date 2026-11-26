---
type: Concept
title: 'Prompt engineering'
description: 'Designing the instructions, examples and context given to a model so it produces the wanted output without changing its weights - including versioning and evaluating prompts, and defending them against injection and extraction.'
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 5 Prompt Engineering, pp. 211-251 (best practices p. 220; organize and version prompts p. 233; defensive prompt engineering p. 235)'
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 9 In-Context and Few-Shot Learning with Prompts, p. 288'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 2 Opik: prompt monitoring, p. 46; ch. 11 Prompt monitoring, p. 413'
- title: 'Wei, J. et al. (2022). Chain-of-Thought Prompting Elicits Reasoning in Large Language Models. NeurIPS 2022'
  resource: 'https://arxiv.org/abs/2201.11903'
  kind: paper
- title: 'Brown, T. et al. (2020). Language Models are Few-Shot Learners. NeurIPS 2020'
  resource: 'https://arxiv.org/abs/2005.14165'
  kind: paper
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Prompt engineering

**Definition.** A prompt is everything sent to the model: a system prompt (role and rules set by the application), the user prompt, and any examples or retrieved context. Huyen's best practices are to write clear and explicit instructions, provide sufficient context, break complex tasks into simpler subtasks, give the model time to think (chain-of-thought), and iterate. Zero-shot and few-shot prompting rely on in-context learning. Because prompts are code, she recommends organising and versioning them separately from application logic and evaluating every change. Defensive prompt engineering covers jailbreaking, prompt injection and information extraction, and the layered defences against them.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Prompting is the cheapest lever and comes first in the adaptation ladder (prompt, then RAG, then fine-tuning). It overlaps with context engineering, which widens the focus from the wording to everything placed in the context window. In this project the prompts in News-Automation/prompts/ are versioned and approved by Kenji, which is exactly the practice Huyen describes.

## Example

Instead of "Summarise this", a prompt states the audience, the output sections, two short example summaries and "if the article has no architecture content, say so" - and is stored as a versioned file with an evaluation set.

## Broader concepts

- [Large language model and foundation model](/concepts/ai-ml/large-language-model.md)

## Narrower concepts

- [In-context learning](/concepts/ai-ml/in-context-learning.md)

## Related concepts

- [Context engineering](/concepts/ai-ml/context-engineering.md)
- [Indirect prompt injection](/concepts/security/indirect-prompt-injection.md)

## Sources

1. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 5 Prompt Engineering, pp. 211-251 (best practices p. 220; organize and version prompts p. 233; defensive prompt engineering p. 235). The most complete treatment: anatomy of a prompt, best practices, prompt versioning and attacks/defences.
2. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 9 In-Context and Few-Shot Learning with Prompts, p. 288. Prompting as an alternative to labelled data.
3. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 2 Opik: prompt monitoring, p. 46; ch. 11 Prompt monitoring, p. 413. Tracing and monitoring prompts in production.
4. Wei, J. et al. (2022). *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models*. NeurIPS 2022. <https://arxiv.org/abs/2201.11903> — Chain-of-thought prompting.
5. Brown, T. et al. (2020). *Language Models are Few-Shot Learners*. NeurIPS 2020. <https://arxiv.org/abs/2005.14165> — Few-shot prompting at scale.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
