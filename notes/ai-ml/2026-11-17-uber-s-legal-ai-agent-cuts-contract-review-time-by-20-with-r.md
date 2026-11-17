---
type: Article Summary
title: Uber's Legal AI Agent Cuts Contract Review Time by 20% with Rules and RAG
description: Uber's Legal Redlining Agent cuts contract review time over 20% by pairing deterministic rules with RAG over lawyer feedback, keeping lawyers in control.
resource: https://www.infoq.com/news/2026/11/uber-legal-redlining-agent/
tags:
- ai-ml
- ai-agents
- rag
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/uber-legal-redlining-agent/
  title: Uber's Legal AI Agent Cuts Contract Review Time by 20% with Rules and RAG
  author: Leela Kumili
status: stable
source_url: https://www.infoq.com/news/2026/11/uber-legal-redlining-agent/
source: InfoQ
source_type: blog
published: '2026-11-16'
domain: ai-ml
subdomain: ai-agents
concepts:
- rag
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d817c9a4bf174b09ef2f1
---

# Uber's Legal AI Agent Cuts Contract Review Time by 20% with Rules and RAG

**Source:** https://www.infoq.com/news/2026/11/uber-legal-redlining-agent/
**Author:** Leela Kumili
**Date:** 2026-11-16
**Concepts:** [Retrieval-augmented generation](/concepts/rag.md)

---

## 🎯 Core thesis
Uber's Legal Redlining Agent cuts average contract review time by more than 20% by combining retrieval over past lawyer decisions, deterministic legal rules and language models, while lawyers remain responsible for approving every recommendation.

## 🧭 Why the author wrote it
Contract review is repetitive: outside parties propose edits, and lawyers must judge each change against company policy. The article describes how Uber built an agent for that step, how the design changed after a first attempt fell short, and what the company reports. If the thesis holds, routine legal review can be accelerated when precedent and hard rules shape the model's output and lawyers stay in control.

## What the agent does
The agent is delivered as an add-in inside Microsoft Word. It reads edits proposed by external parties, interprets their intent, compares each change with legal policies, and suggests whether to accept, reject or modify the clause. It also writes comments explaining each recommendation and flags changes that need closer scrutiny. Uber began the first generation in early 2024 and piloted it with its Legal team in 2025. According to the company, average contract review time dropped by more than 20%, with 91% accuracy in AI-generated decisions. The article does not explain how those figures were measured.

> 🔎 Added context: Redlining means marking up proposed changes to a contract. In Word this is usually done with tracked edits and comments.

### Redlining agent
The agent treats each external edit as a question: what is this change trying to achieve, and does it fit policy? Its output is a suggestion (accept, reject or modify), an explanation in a Word comment, and a flag where a human should look more closely. The agent does not finalize the contract. It produces a proposal that a lawyer reviews inside the document they already use.

### Human-in-the-loop review
Lawyers retain responsibility for reviewing and approving every recommendation. The article presents this as central to the design rather than a formality. The legal team also sets the desired tone and style, so the people accountable for the outcome shape how the agent speaks.

## From a first RAG pipeline to a feedback loop
The first version used retrieval-augmented generation. It pulled relevant sections from legal playbooks to guide the model's responses. The system struggled with semantic retrieval accuracy, with negotiation tone, and with scenarios that no existing example covered. Uber then added a feedback loop that captures each lawyer's decision, the reasoning behind it, and the final edited text.

### Retrieval-augmented generation (RAG)
> 🔎 Added context: RAG pairs a search step with a language model. Relevant text is retrieved and placed in the prompt so the answer draws on it.

In this article, RAG meant retrieving playbook sections to steer the model's wording. The weakness was retrieval quality: the system did not reliably find the right passage, and written playbooks did not cover every negotiation case. The article gives no retrieval metric for this version. The lesson is that grounding a model in documents is only as good as the retrieval step, and that a policy corpus alone may not capture practical judgment.

### Lawyer feedback loop
The loop records three things for each decision: what the lawyer chose, why, and the text they finally used. Those records become material the agent can retrieve during later reviews. The key shift is that the agent retrieves examples of how similar clauses were handled, not only whether a previous recommendation was accepted or rejected.

> ❌ **Anti-pattern — storing only accept/reject outcomes:** keeping just whether a past recommendation was accepted or rejected. **Why people do it:** not stated in the article. **Consequence:** not spelled out; the article reports only that the first RAG version struggled with retrieval accuracy and tone, and that the richer loop replaced reliance on accept/reject signals. **Fix:** capture the reasoning and final edited text, so the system can show how a similar clause was actually handled.

## Retrieving precedent at runtime
At query time, the agent narrows the history in stages. Similarity search and metadata filtering reduce thousands of historical interactions to about 20 candidates. A second, language-model-based stage then evaluates their relevance. Exponential decay weighting, with a configurable half-life currently set to 365 days, favours recent decisions while keeping older ones. Domain experts also control the language and negotiation style.

### Similarity search and metadata filtering
Similarity search finds stored records whose embeddings are close to the embedding of the current clause. Metadata filtering restricts that search using attributes attached to each record; the article does not list which attributes Uber uses. Together they make the first cut, from thousands of interactions to roughly 20 candidates. This matters because the second stage can only judge what the first stage passes through.

### LLM-based relevance filtering
The second stage asks a language model to judge whether each of the roughly 20 candidates is relevant to the current edit. The article does not describe the prompt or criteria. It gives this stage a narrower job than the first: precision over recall, applied to a small set.

> 🔎 Added context: This resembles the LLM-as-a-judge pattern, where a model scores candidates against stated criteria. The article does not use that term.

### Exponential decay weighting
Exponential decay weighting gives each record a weight that halves for every half-life that passes. With the article's 365-day half-life, a decision from a year ago counts half as much as one made today.

> 🔎 Added context: The weight is 0.5 raised to the power of (age in days divided by 365). The half-life is configurable, so the article's 365 days is a setting, not a universal value.

## Team split and architecture
Uber Engineering framed the problem in a LinkedIn post: "How can AI help streamline repetitive contract review while keeping lawyers in control?" The division of work is explicit. The legal team defines the desired tone and style, while engineers maintain the prompt structure and application workflow. The system combines a React and TypeScript Word add-in with a Python backend.

### LangGraph orchestration
The Python backend is orchestrated through LangGraph. The article does not say why this framework was chosen over alternatives.

> 🔎 Added context: LangGraph lets developers define an LLM workflow as a graph of steps that share state, which suits multi-step review logic.

### Parallel workflow nodes
Several workflow nodes run in parallel: intent detection, risk assessment and policy lookup. Running them concurrently means the agent does not wait for one analysis before starting another. The article does not give latency figures.

### OpenSearch
OpenSearch stores the vectorized legal rules and the historical feedback. It is the retrieval layer behind the similarity search described above.

> 🔎 Added context: OpenSearch is an open-source search and analytics engine that can index vectors for similarity queries.

### Redis session state
Redis maintains session state so users can ask follow-up questions about the same review without restarting the context.

> 🔎 Added context: Redis is an in-memory data store often used for short-lived state such as sessions.

## Deterministic rules and verification
A deterministic rules engine enforces non-negotiable legal requirements and overrides model recommendations when a matching rule applies. Language models handle the tasks that need contextual interpretation, including clause recommendations and negotiation tone. A final verification step checks generated responses against the retrieved rules.

### Deterministic rules engine
A rules engine applies fixed conditions, so the same input always yields the same result. Placing non-negotiable requirements here means they hold even when a model's suggestion would differ. The article makes the split explicit: rules for what must be true, models for what needs judgment. The article does not list the rules or how they are authored.

> ❌ **Anti-pattern — letting the model decide non-negotiables:** leaving required clauses to the model's judgment. **Why people do it:** not stated in the article. **Consequence:** not stated in the article; the design implies the model could otherwise override a legal requirement. **Fix:** encode non-negotiable requirements as deterministic rules that override model output.

### Final verification step
Before a response is returned, the system checks it against the retrieved rules. The article does not describe the checks, their failure handling, or how often they override output.

## Next steps
Uber is exploring agentic harnesses, including Claude Code and OpenCode, along with skills that structure and automate workflows across its agentic AI efforts. It also plans to investigate legal ontologies and knowledge graphs to represent legal concepts and relationships.

### Agentic harnesses
A harness is the runtime that lets a model use tools and follow a workflow. The article names Claude Code and OpenCode as options under evaluation, and skills as a way to structure repeatable work.

### Legal ontologies and knowledge graphs
An ontology defines the concepts in a domain and how they relate. A knowledge graph stores those relationships as data. The article presents them as a future semantic foundation for more consistent interpretation and reasoning, not as a current component.

## 🧠 Mental models
### Precedent over verdicts
**The model:** a stored decision is most useful as a worked example (what was asked, what was decided, why, and the final text), not as a binary accept or reject label.
**How to apply it:** when designing any feedback capture for AI-assisted work, record the reasoning and the final output alongside the verdict.

### Rules above the model
**The model:** non-negotiable requirements belong in deterministic code that overrides the model, and the model handles interpretation and tone.
**How to apply it:** list each requirement that must never be violated, implement it as a rule, and check model output against those rules before it reaches a user.

### The expert owns the outcome
**The model:** the AI proposes, the domain expert approves, and the expert also sets the style the AI uses.
**How to apply it:** give domain experts direct control over the parts of the system that reflect their judgment, and keep an explicit approval step.

## 🔑 Key takeaways
- Uber reports that its Legal Redlining Agent cuts average contract review time by more than 20%, with 91% accuracy in AI-generated decisions; both figures are company claims.
- A plain RAG system over playbooks struggled with retrieval accuracy, negotiation tone and uncovered cases, which led to the feedback loop.
- Recording reasoning and final edited text, not just accept or reject outcomes, lets the agent retrieve how similar clauses were handled.
- Runtime retrieval narrows thousands of interactions to about 20 candidates with similarity search and metadata filtering, then an LLM stage judges relevance.
- Exponential decay weighting with a 365-day half-life favours recent decisions while keeping older precedent.
- A deterministic rules engine enforces non-negotiable requirements and overrides model recommendations, with a final verification step.
- Lawyers stay responsible for approval, and the legal team defines tone while engineers own the prompt structure and workflow.

## 🏛️ For Kenji
This is your interpretation, not the author's. The pattern applies to any Staff-level review workflow where expert decisions are logged only as outcomes, which makes them weak precedent. The article also shows a clean split between deterministic rules and model judgment, which maps directly to governance controls. The question it raises: in your own AI systems, which requirements are enforced by rules that cannot be overridden, and which are left to a model that can drift?

## 📚 Reading plan
### From the article
- [Uber blog post: Building Uber's redlining agent](https://www.uber.com/us/en/blog/building-ubers-redlining-agent/) — the primary source, with the architecture diagrams this summary omits.
- [Uber Engineering LinkedIn post](https://www.linkedin.com/posts/how-can-ai-help-streamline-repetitive-contract-share-7513997846849732610-vbbo/) — the framing question and the team's own summary.
- [LangGraph](https://www.langchain.com/langgraph) — the orchestration framework used for the backend.
- [OpenSearch](https://opensearch.org/) — the search and vector store holding rules and feedback.
### To go further
- Retrieval-augmented generation — how retrieval quality limits grounded answers.
- Exponential decay weighting — how recency-weighted retrieval is tuned in practice.
- Legal ontologies and knowledge graphs — the representation layer Uber plans to explore.
