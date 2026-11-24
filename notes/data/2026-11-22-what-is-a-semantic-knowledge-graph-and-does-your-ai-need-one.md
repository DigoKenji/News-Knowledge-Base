---
type: Article Summary
title: What is a semantic knowledge graph, and does your AI need one?
description: Agentic AI needs a semantic layer and knowledge graphs that encode business context, not just data, to act reliably.
resource: https://www.cio.com/article/4249101/what-is-a-semantic-knowledge-graph-and-does-your-ai-need-one.html
tags:
- data
- knowledge-graph
- rag
- ai-agent
- multi-agent-orchestration
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://www.cio.com/article/4249101/what-is-a-semantic-knowledge-graph-and-does-your-ai-need-one.html
  title: What is a semantic knowledge graph, and does your AI need one?
  author: CIO.com
status: stable
source_url: https://www.cio.com/article/4249101/what-is-a-semantic-knowledge-graph-and-does-your-ai-need-one.html
source: CIO.com
source_type: blog
published: '2026-11-18'
domain: data
concepts:
- knowledge-graph
- rag
- ai-agent
- multi-agent-orchestration
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d811d888bfd346469d409
---

## 🎯 Core thesis
AI agents that do real work need a semantic layer that encodes business meaning and context over enterprise data, and knowledge graphs are the structure that holds that context.

## 🧭 Why the author wrote it
Enterprises have already spent heavily on data lakes, lakehouses, ERP and CRM systems, yet the article argues that raw data does not explain what is happening in a business. If that is right, encoding business context becomes an architecture requirement for agentic AI rather than an optional analytics project.

## Introduction: from tables to semantic graphs
### Graph database
A traditional database organizes information into tables, with one column per attribute, such as a name or an employer. A graph database stores the same information as nodes, such as a person and a company, joined by edges that represent the connection between them. Because any node can connect to any other, the model is flexible, and finding related entities means following edges. The article lists supply chain dependencies, fraud detection and mapping user preferences for recommendations as typical uses.

### Knowledge graph
A knowledge graph sits on top of a graph database as a map of relationships that explains what the data is. In the article's example, a node with a person's name connects to a node with company information because the person is an employee of that company. The graph therefore carries meaning about the data, not only the data itself. For agents, this is what lets them follow relationships to the facts they need.

### Semantic knowledge graph
A semantic graph, or semantic knowledge graph, adds a deeper level of meaning on top of the relationships. The article's example: a list of companies can be understood as clients or potential clients, and a list of people can be potential contacts for sales calls, with job titles indicating their relative importance. The added meaning is specific to how the enterprise uses its data.

### Semantic layer
The semantic layer is about what the data means for the enterprise and how it is actually used in practice. When AI agents are assigned real work tasks, they need this information. Cosentino, a manufacturing company whose global CIO Rafael Domene is quoted, had already invested heavily in a massive data lake and lakehouse, plus ERP and CRM systems. Domene's point: "You can collect all the data, but that's not enough to explain what's going on." The semantic layer supplies the business context behind the data.

### Agentic AI
Domene defines it this way: "Agentic AI is AI that can do things, not just tell you things." He adds that providing "not just data, but information" is what lets AI do the job the right way, meaning business knowledge and an understanding of what is going on.

## Justified layering
### Semantic layer construction
Building a semantic layer has historically been hard. It requires subject matter experts, and it is not a one-and-done project because the way a company does things changes constantly. Analysts often skipped it, since their own knowledge of the company supplied the context. AI agents are different: they understand how companies work poorly in general, and they know even less about how any one company works in particular. Cosentino began its layer three years ago and aims to finish by the end of the year, with the main processes and flows already in place and the remaining work mostly on the financial side, including logistics, manufacturing, operations, customer service and sales, procurement and HR.

### Delayed orders
Domene describes an order process involving 15 departments, each with its own dashboards, where the company was "having a lot of issues finding out last minute that a specific order wasn't going to be served." Now an AI agent can examine any order for any customer, predict whether it will arrive on time, and identify where a delay is. The value is not only information: if a salesperson learns of a problem as it happens, they can return to the customer, explain what happened and offer solutions. Domene: "Without context, I can't link things."

### Credit-limit blocks
When a customer exceeded a credit limit and an order was blocked, the average delay used to be 22 days. The article reports that the maximum delay today is one day. Domene calls it "a service and performance improvement," and says the finance team that handled this work is now free for tasks more vital to customers.

### Savings and sales time
Domene says the company saw up to $10 million in savings in the first year of the project and expects $100 million in savings by the end of the year. Sales employees used to spend more than a third of their time on administrative work, and AI agents built last year and tested this year are expected to assist them soon. Domene cautions that "not all of that saved time is going to be turned into revenue."

### Human-led build
The project began as a largely manual effort run by the company's experts together with engineers from the vendor, process mining company Celonis. Domene says SAP's fixed flows made the work relatively easy, but it still required human effort, and AI was added over time to augment people. David Hoelzer, a SANS Institute fellow and chief of operations at Enclave Forensics, adds that humans do not always create documents in ways AI can use to understand semantic relationships.

> ❌ **Anti-pattern — fully autonomous semantic layer:** letting AI build the knowledge graph on its own. **Why people do it:** the article does not say, but the appeal is that AI accelerates the work.
> 🔎 Added context: the tempting reading is that AI could finish the job alone, which the article argues against. **Consequence:** Hoelzer warns the AI "may end up with relationships that don't make sense." **Fix:** use AI to accelerate building the graph and keep it up to date, while people with "a very particular set of skills" stay in the loop.

## Knowledge graphs versus vector databases
### Retrieval-augmented generation (RAG)
About a year ago, the standard way to feed new information into an LLM was RAG combined with vector databases. The article says that is still a good approach for unstructured data in older RAG pipelines. Relevant context from a prompt is included in the LLM input, which is how the model gets the information it needs for the task at hand.

> ❌ **Anti-pattern — wordy retrieved context:** passing large amounts of retrieved material into every LLM call. **Why people do it:** the article does not say; more context looks like more safety. **Consequence:** material "can confuse the LLM or be dated," and excess content means more tokens and higher costs, which matters because agents make many LLM calls. **Fix:** let knowledge graphs return the exact information needed, with the semantic layer supplying what to do with it.

### Vector database
A vector database takes unstructured information and organizes it so similar topics are grouped together. That grouping is what allows the right material to be pulled into a prompt. The article's point is that this approach tends to produce a lot of material, which is useful for unstructured content but less precise than a graph for agent work.

### Knowledge graph for agents
Knowledge graphs let agents find the exact information they need, while the semantic layer gives them the knowledge about what to do with it. New York Life Group Benefits Solutions uses both: its knowledge graph is built on the LangGraph agent framework from LangChain. CIO Matt Marze: "The knowledge graphs work for multi-agent orchestration and the vector database for gen AI RAG applications." He describes the graph starting from basic business information, data, signals and external sources such as the broker community, with the semantic layer adding business understanding, relationships, common definitions of products and plans, and the current situation, history, workflow and priorities. "All that drives into insights, predictions, and recommendations, and allows us to coordinate the abilities of the AI agents."

### Multi-agent orchestration
In Marze's usage, the knowledge graph is what coordinates the abilities of several agents.
> 🔎 Added context: orchestration here means deciding which agent handles which part of a task, and in what order.

### Growing interest
James Kaplan, distinguished partner at McKinsey & Company, says that a year and a half ago people "looked at me like I had three heads" when he discussed knowledge graphs. In the past year he has seen a real uptick, and he says institutions can use knowledge graphs for more complicated decision making and, with agents, to enable more capable agents.

## The flip side
### Hallucination and compounding inaccuracy
Accuracy and hallucinations are a big problem for AI models in general, and in agentic systems with multiple LLM calls, inaccuracies can compound quickly. EY's Zakir Hussain, partner and Americas data and AI leader for industrials and energy, explains that all these models are probabilistic. When used for enterprise-level questions or decisions, "they start hallucinating because in many cases they don't understand or misinterpret the context." Generic training makes company-specific questions harder: "As soon as it becomes specific to a company, that's where things start going sideways."

### Accuracy result
According to a paper released at the end of 2025 by researchers at UC Santa Barbara and JP Morgan AI research, knowledge graphs improve accuracy by 26% or more over standard chain-of-thought baselines. The article gives no further detail on the benchmark, the tasks or the baselines, so treat the figure as the article reports it.

### Codifying company context
Hussain says that every organization is trying to use AI for growth, efficiency and productivity, and that "to be able to unlock these three layers — growth, efficiency, and productivity — you have to codify the context of the company." He calls the ontology, semantic layer and knowledge graph conversation "pretty important right now."

## 🧠 Mental models
### Data is not information
**The model:** Collecting data does not explain what happened. Information needs business context, meaning and an understanding of how the company works.
**How to apply it:** When a metric turns red, ask which processes and context explain it before adding more data sources.

### Agentic AI does work, not just reports
**The model:** Agentic AI is AI that can do things, not just tell you things (Domene).
**How to apply it:** Judge an agent by the actions it can complete reliably, and supply the business context those actions require.

### Probabilistic models need codified context
**The model:** Probabilistic models hallucinate on enterprise questions when they misread context, so company context must be made explicit (Hussain).
**How to apply it:** Before trusting agent answers on company-specific questions, write down the company's definitions, relationships and priorities in a form the system can use.

## 🔑 Key takeaways
- Agents that act on business data need a semantic layer that supplies company-specific meaning, processes and context; the article argues raw data is not enough.
- At Cosentino, the average 22-day delay for credit-limit-blocked orders is now at most one day, with up to $10 million saved in year one and $100 million expected by year end.
- Building the semantic layer is ongoing, expert-driven work; AI can accelerate and maintain the knowledge graph but should not build it alone.
- Knowledge graphs let agents find exact information, while wordy RAG context adds tokens, cost and potential confusion.
- New York Life Group Benefits Solutions pairs knowledge graphs for multi-agent orchestration with vector databases for generative AI RAG.
- A paper released at the end of 2025 by UC Santa Barbara and JP Morgan AI research reports knowledge graphs improving accuracy by 26% or more over standard chain-of-thought baselines.
- Probabilistic models hallucinate on company-specific questions and inaccuracies compound in multi-call agentic systems, so company context has to be codified.

## 🏛️ For Kenji
For a Staff Architect deciding how agents get enterprise context, the semantic layer looks like a governed architectural component with owners, business definitions and ongoing maintenance, sitting alongside vector retrieval rather than replacing it. The question it raises is which business definitions and relationships should be codified first, and who owns them as processes change. This is your interpretation, not the author's.

## 📚 Reading plan
### From the article
- The article body contains no links, so there are none to list.

### To go further
- UC Santa Barbara and JP Morgan AI research paper, released end of 2025 — the source of the 26% accuracy figure; check its benchmark and baselines before relying on the number.
- LangGraph, the LangChain agent framework New York Life builds its knowledge graph on — a concrete look at a graph-based agent framework.
- Celonis process mining — the vendor Cosentino worked with to build its knowledge graph and semantic layer.
- Retrieval-augmented generation with vector databases — the older approach the article contrasts with knowledge graphs for agent work.

## Concepts
- [Knowledge graph](/concepts/data/knowledge-graph.md)
- [Retrieval-augmented generation](/concepts/ai-ml/rag.md)
- [AI agent](/concepts/ai-ml/ai-agent.md)
- [Multi-agent orchestration](/concepts/ai-ml/multi-agent-orchestration.md)
