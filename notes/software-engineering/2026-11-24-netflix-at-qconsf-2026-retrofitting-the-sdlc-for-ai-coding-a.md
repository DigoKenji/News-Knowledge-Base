---
type: Article Summary
title: 'Netflix at QConSF 2026: Retrofitting the SDLC for AI Coding Agents'
description: Netflix Commerce made AI coding agents safe to ship by turning architecture decisions, docs and review rules
  into context both agents and engineers read.
resource: https://www.infoq.com/news/2026/11/netflix-sdlc-ai-coding-agents
tags:
- software-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/netflix-sdlc-ai-coding-agents
  title: 'Netflix at QConSF 2026: Retrofitting the SDLC for AI Coding Agents'
  author: Andrew Hoblitzell
status: stable
source_url: https://www.infoq.com/news/2026/11/netflix-sdlc-ai-coding-agents
source: InfoQ
source_type: blog
published: '2026-11-23'
domain: software-engineering
concepts:
- ai-agent
- adr
- graph-rag
- model-context-protocol
- context-engineering
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d81e5ab58c4d2ff5685e4
---

## 🎯 Core thesis
Letting AI coding agents and engineers ship together safely is mostly a problem of documentation, consistency and review, not a problem with the agents themselves.

## 🧭 Why the author wrote it
Khuzam starts from a gap: agents promised speed, but Netflix's Commerce team had spent 20 years building its code, docs and tests for human readers. The talk describes how the team retrofitted its software development lifecycle so agents and engineers could ship together. If the thesis holds, the fixes are ordinary good engineering aimed at a new kind of reader.

> 🔎 Added context: The software development lifecycle (SDLC) is the path code takes from planning through building, testing, review and release.

## Setting: Commerce and server-driven UI
Mark Khuzam, a senior software engineer at Netflix, presented this at QCon San Francisco 2026. Commerce runs much of its user interface as server-driven UI, an approach Netflix has discussed at QCon before. A single screen-composition service behind the API gateway builds screens for iOS, Android, web and TV.

### Server-driven UI
> 🔎 Added context: In server-driven UI, the server describes what a screen contains and each client renders that description, instead of each app defining its own screens.

Commerce uses this pattern at scale. Because one composition service feeds four platforms, a change to that service reaches all of them. The article does not explain the mechanics further and points back to earlier QCon talks on the subject.

## Opening premise: agents promised speed
Khuzam opened with a mock company email announcing that every engineer now had Claude Code and asking how much faster everyone was shipping. The line he used to frame the problem was: "Agents promised speed. But we spent 20 years building for humans." The rest of the talk works on that mismatch. The code, docs and conventions were built for human readers, and agents had to work with them as they were.

### Context window
The context window is the amount of text a model can consider at one time. In the article it is the constraint that broke the team's first attempt: the agent's context window ran out before it found the right documentation page. The agent therefore has to reach the relevant material early, because it cannot read everything. The documentation has to be findable, not just present.

## First attempt: an agent migrates tests
The team's first attempt was to have an agent migrate tests, and it took four tries. Each try added guidance: first "migrate this file," then instructions, then the APIs to use, then how to mock data. Two more problems showed up. The docs were hard to find, and they were inconsistent, because tests were not written the same way across the codebase.

> ❌ **Anti-pattern — Inconsistent conventions:** the same kind of test is written a different way in each part of the codebase. **Why people do it:** the article gives no reason; this is an interpretation, since no written standard is being followed. **Consequence:** the agent has no single pattern to copy, so each task needs more guidance. **Fix:** write the conventions down as decision records, which the team did.

### Net Promoter Score
Developers rated the internal framework at a Net Promoter Score (NPS) of -3. Moving from one test to a whole feature, from screen to data to route to test, meant teaching the agent how the team builds. The low score is the baseline the rest of the story improves on.

> 🔎 Added context: NPS is calculated from a "would you recommend" question and ranges from -100 to +100. Scores below zero mean more detractors than promoters.

## Context engineering and decision records
Khuzam described the fix as context engineering, "getting what engineers know into what agents see." The team wrote 45 architecture decision records (ADRs), each "written for people and for agents." Each record covers the context, the decision, a pattern and an anti-pattern, plus AI usage notes on what an agent should do. It also links to related docs and code, and the records now ship inside almost every repository.

### Context engineering
> 🔎 Added context: Context engineering here means deciding what information an agent sees, and in what form, when it works in your codebase.

The phrase captures two halves. The knowledge must exist in writing, and it must reach the agent at the moment it is needed. Engineers already know how the team builds; the work is making that knowledge available to an agent. The article offers this phrase as its definition and gives no formal one.

### Architecture decision record
An ADR records one architectural decision along with the context that led to it. The team's version adds a pattern to follow, an anti-pattern to avoid and notes on what an agent should do. The article presents these fields as what lets an agent act on a record, and the links tie each rule to the code it governs. The 45 records are the base that the review agent later checks against.

## Findability: AGENTS.md as robots.txt
To keep the records and more than 300 other documents findable, the team borrowed from how search engines read a site. An AGENTS.md file, the project instructions file that coding agents read before anything else, plays the part of robots.txt.

### AGENTS.md
> 🔎 Added context: AGENTS.md is a plain file at a repository's root that tells coding agents how to work there. The article describes it as the first thing a coding agent reads.

An agent that starts at this file can find the 45 ADRs and the other 300-plus documents, rather than relying on search inside a limited context window. The article does not show the file's contents, so no specific layout should be assumed.

### robots.txt
> 🔎 Added context: robots.txt is a small file at a website's root that tells search engine crawlers where they may go.

The analogy works because robots.txt is a well-known entry point that crawlers read first. The team applied the same logic so that agents find the important material first.

## After the code: deterministic CI and a review agent
Khuzam noted that the CI pipeline is still deterministic, running builds, tests, screenshot diffing, merge queues and canaries. What changed is how agents plug into each phase. A review agent pre-reviews each pull request against the docs and ADRs, flags anti-patterns and looks at what depends on the change. Engineers can add a revise label so an agent reads the review and pushes a fix to the same PR. Changes are not yet auto-merged, and engineers own the code.

### Continuous integration (CI)
> 🔎 Added context: CI is an automated pipeline that builds and tests every change before it merges.

The team left this pipeline as it was. Agents are added around its checks, not in place of them. The article treats the deterministic checks as the foundation that the agent work sits on.

### Review agent
The first version of the review agent left 23 comments on a single PR, starting with "Overall this PR looks great!"

> ❌ **Anti-pattern — Comment on everything:** the review agent posts every observation, including praise. **Why people do it:** the article gives no reason; the first version's output suggests it tried to cover everything. **Consequence:** 23 comments bury the few that matter. **Fix:** post only verified blockers, each with evidence, a fix and the ADR rule it breaks.

The current version posts only those verified blockers.

### Revise label
Adding a `revise` label asks an agent to read the review and push a fix to the same PR. The article describes the mechanism but reports no results for it, and it does not say how often it is used.

## Results: what Khuzam reported
Khuzam reported no Sev 1 incidents in 2026 on a core service, 32% more pull requests year over year, and developer NPS rising from -3 to +25 over three quarters. The earlier -3 was the score for the internal framework. The article gives no detail on how these figures were measured.

Ahead of the session, Khuzam framed the stakes on LinkedIn: "Agents can write code, but what gives us the confidence to ship Tier 0 services to 300M+ members without everything falling apart?"

> 🔎 Added context: Tier 0 usually denotes the most critical class of service in a company's classification scheme. The article does not define it.

## Cross-service context: what did not hold up
Work that crosses services raised a harder question. A product engineer building one feature touches the payment API, billing, the gateway and every client, so the team had to decide who owns the context and how it follows the work. Khuzam listed four approaches that did not hold up.

### Loading every repository
> ❌ **Anti-pattern — Load everything:** put every repository in the agent's context. **Why people do it:** the article gives no reason; it is the simplest way to show an agent the whole system (interpretation). **Consequence:** the article says it did not fit. **Fix:** a graph scoped to one domain, pinned to commits.

### Git submodules
Git submodules nest one repository inside another. The article calls them brittle and gives no detail. For cross-service context, that brittleness is a problem because the agent's view depends on many nested repositories staying in sync (interpretation).

### Git MCP server
A Git MCP server, which exposes repository contents to an agent through the Model Context Protocol, "sees one file, not the system." Reading files one at a time does not reveal how services connect.

> 🔎 Added context: MCP is the open standard used to connect agents to tools and data; here it is the way a repository is exposed to the agent.

### Shared copies
Shared copies of context went stale. A copy taken at one moment stops matching the code and docs it came from. The team's fix is the opposite: graphs pinned to specific commits and kept fresh.

## GraphRAG per domain
The team needed context that was accurate, portable across agents, owned per domain and kept fresh. It settled on GraphRAG per domain, taking structure from the code and meaning from the docs and ADRs. Each graph is pinned to specific commits and served through a single Model Context Protocol tool. Claude Code, Cursor, Codex, a Slack bot and CI review all query the same graph, and every answer cites a file and a line.

### GraphRAG
> 🔎 Added context: GraphRAG combines a graph of entities and relationships with retrieval, so an agent receives connected facts as well as text snippets.

Splitting the work this way means structure comes from the code and meaning comes from the written decisions. Pinning each graph to commits ties answers to a known state of the code. Owning each graph per domain means the team responsible for a domain maintains its context. Citing a file and a line for every answer lets a reviewer check it.

### Model Context Protocol
> 🔎 Added context: Model Context Protocol is an open standard that lets AI applications connect to tools and data sources through one common interface.

One tool in front of the graph lets five different clients, from coding agents to a Slack bot and CI review, query the same source. Each client does not need its own copy of the context.

## Closing rules and human judgement
Khuzam closed with three rules: decide what agents should do and tell them, make it easy to find how to do it, and remember that human judgement still matters. He summed up: "None of this is new. It's good engineering, built for a new reader." The article says InfoQ will publish video of the session in the coming weeks.

### Human judgement
The last rule carries the most weight in the article's own practice. Changes are not auto-merged, and engineers own the code. The agents propose and check, and people decide what ships.

## 🧠 Mental models
### Write for people and for agents
**The model:** A decision record serves two readers at once: the engineer who needs the rationale and the agent that needs something it can act on. **How to apply it:** give each record a context, a decision, a pattern, an anti-pattern and notes on what an agent should do, and link it to the code it governs.

### An entry file as robots.txt
**The model:** One well-known file at the root tells any reader, human or agent, where the important material is and how to read it. **How to apply it:** keep an AGENTS.md in each repository that points to that domain's decision records and docs.

### Verified blockers only
**The model:** An automated reviewer earns trust by posting few comments, each one checked. **How to apply it:** require every automated review comment to include evidence, a suggested fix and the rule it breaks, and drop comments that are praise or unverified.

## 🔑 Key takeaways
- Agents inherited a codebase built for humans. The team's first test-migration agent needed four tries, adding guidance from "migrate this file" to APIs and mock data.
- The team wrote 45 ADRs "written for people and for agents," each with context, decision, pattern, anti-pattern and AI usage notes, linked to docs and code.
- An AGENTS.md file acts as the entry point agents read first, pointing to the decision records and more than 300 other documents, as robots.txt does for crawlers.
- The CI pipeline stays deterministic. The review agent posts only verified blockers, each with evidence, a fix and the ADR rule it breaks.
- Cross-service context should be accurate, portable, owned per domain and kept fresh. The team chose GraphRAG graphs pinned to commits, served through one MCP tool.
- Agents do not yet auto-merge changes, and engineers still own the code.
- Khuzam reported no Sev 1 incidents in 2026 on a core service, 32% more pull requests year over year, and developer NPS rising from -3 to +25 over three quarters.

## 🏛️ For Kenji
*Your interpretation, not the author's:* Architecture governance becomes practical when decisions are written in a form that reviewers and tools can both apply, which is what the ADR-plus-AGENTS.md pattern attempts. In your architecture work, the question it raises is which of your team's decisions could a tool check automatically. A second question is who owns the context when a change crosses domains, which the article answers with per-domain ownership.

## 📚 Reading plan
### From the article
- No links are cited in the article text. It says session details are available through a link, but gives no URL, so none is given here. The article also promises InfoQ video of the session in the coming weeks.

### To go further
- Architecture decision records (ADRs): the format behind the 45 records. Read it to see what the context, decision, pattern and anti-pattern fields should contain.
- The AGENTS.md convention for coding agents: the entry-point file the team compares to robots.txt.
- GraphRAG: the retrieval approach behind the per-domain context graphs.
- Model Context Protocol: the standard that let one tool serve Claude Code, Cursor, Codex, a Slack bot and CI review.
- The Robots Exclusion Protocol (robots.txt): the search-engine convention behind the team's findability analogy.

## 🔗 Concepts
- [ai-agent](/concepts/ai-agent.md)
- [adr](/concepts/adr.md)
- [graph-rag](/concepts/graph-rag.md)
- [model-context-protocol](/concepts/model-context-protocol.md)
- [Context engineering](/concepts/context-engineering.md)
