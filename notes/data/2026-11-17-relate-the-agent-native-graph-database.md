---
type: Article Summary
title: Relate — the agent-native graph database
description: Relate is an open-source embedded Cypher graph database that gives agents connected facts to query, with no signup or server.
resource: https://relatedb.ai/
tags:
- data
- data-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://relatedb.ai/
  title: Relate — the agent-native graph database
  author: Relate (by Neo4j)
status: stable
source_url: https://relatedb.ai/
source: relatedb.ai (forwarded)
source_type: blog
published: '2026-11-17'
domain: data
subdomain: data-engineering
concepts: []
selected_by: forwarded
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81708e9bdf70282fbda9
---

# Relate — the agent-native graph database

**Source:** https://relatedb.ai/
**Author:** Relate (by Neo4j)
**Date:** 2026-11-17

---

## 🎯 Core thesis
An agent answers relationship questions better when its memory is a graph of connected facts that it queries with Cypher, and Relate provides that as an open-source embedded database with no signup and no server to deploy.

## 🧭 Why the author wrote it
The page starts from the claim that agents need to understand data, not only store it. Its implied problem is that an agent's memory, codebase context and recommendations are scattered facts, and a flat store cannot follow how people, decisions and documents connect. If the thesis holds, a question such as "who owns the work that depends on this launch" becomes one query instead of a search through loose text.

## Relate (hero)
The extracted page opens with a Google Tag Manager cookie notice that can be accepted or rejected, and the page says rejecting has no effect on the site. That is page chrome, not product content. The hero offers the install one-liner, a set of platform and language tabs (Agent skill, macOS / Linux, Windows, Python, JavaScript, Rust, Swift, Java, Go, C), and the promise of "an open-source embedded database to keep their data. No signup. No server to deploy."

```sh
curl -sSfL https://relatedb.ai/install.sh | bash && relate skill install --agent claude-code
```

**What this does:**
- `curl -sSfL` downloads the install script silently (`-s`), still prints errors (`-S`), fails on HTTP errors (`-f`), and follows redirects (`-L`).
- `| bash` runs the downloaded script directly in a shell. The page says the script verifies release signatures and SHA256 checksums, which is the check that matters when piping a download into a shell.
- `&&` runs the second command only if the first succeeds.
- `relate skill install --agent claude-code` installs the agent skill for Claude Code. The caption says the script installs the CLI first, then the skill.

## Run Cypher in your browser.
The playground runs the database in the page itself. The page says "nothing is sent anywhere," and the demo is labeled `relate · :memory: · wasm`, which means an in-memory database compiled to WebAssembly. Four datasets are offered: analytics (movies: 12 films, 5 raters), agent-memory (3 sessions, with supersedes), codebase (10 crates, 9 ADRs) and dependencies (2 packages, 7 advisories). The query panel has Explain, Profile and Run controls. The demo starts when the reader scrolls to it.

## From question to answer, in seconds.
This is a scripted preview. An agent receives the question "Who owns the page that depends on the Q3 launch?", writes Cypher, and reads the rows. The answer is that Maya owns Pricing, which depends on the Q3 launch. The graph view shows four nodes and three relationships, labeled OWNS, DEPENDS_ON and PART_OF.

```cypher
MATCH (t:Task {name: 'Q3 launch'})
      <-[:DEPENDS_ON]-(p:Page)
      <-[:OWNS]-(o:Person)
RETURN p.name, o.name
```

**What this does:**
- `MATCH (t:Task {name: 'Q3 launch'})` anchors the pattern on the task node whose name property is "Q3 launch".
- `<-[:DEPENDS_ON]-(p:Page)` follows incoming DEPENDS_ON edges, so the pages that depend on the task are bound to `p`.
- `<-[:OWNS]-(o:Person)` takes one more hop backward to the person who owns each page.
- `RETURN p.name, o.name` returns only the two values the agent needs, not whole nodes.

## One database for every agent.
The core claim is that every agent, script and app reads and writes the same graph, which can be kept in memory, in a directory, or in one file, with no server to run. The diagram shows an application that uses an agent skill or an MCP client (Claude Code, Codex, Cursor, or any MCP client), a CLI or libraries (Shell, Python, JavaScript, Rust), and Cypher on an embedded library. The example uses are agent memory, codebase context, recommendations and "your own data."

### Open source
The page says the code can be read, run locally, and extended with your own recipes.

### Part of your application
Relate runs inside your application. The data can live in memory, in a directory, or in one file.

### Cypher, for connected data
The agent asks in Cypher and gets back how the facts connect. The page says the skill and MCP endpoint are included.

### Start in minutes
The page repeats "No signup. No server to deploy."

## Run a query on connected data.
This is another scripted preview with three scenarios: agent memory ("What do you remember from our last session?"), codebase context ("Which functions break if you change parse_config()?") and recommendations ("What should I suggest to a reader who liked Dune?"). The memory example runs on `memory.rl`.

```cypher
MATCH (e:Event)-[:AFFECTS]->(x)
WHERE e.session = 'last'
RETURN e.summary AS remembered, labels(x)[0] AS kind, x.name AS item
```

**What this does:**
- `MATCH (e:Event)-[:AFFECTS]->(x)` finds events that point at any node through an AFFECTS edge. The target is unlabeled, so it can be a decision, a task or a person.
- `WHERE e.session = 'last'` filters to the previous session. The demo writes the literal `'last'` inline.
- `labels(x)[0]` reads the first label of the target node, which the result uses as its "kind" column.
- The demo returns three rows: a decision about the annual plan, a task about the draft launch email, and a person, Maya, who owns the pricing page. The page does not say whether the demo's inline literal would be a bound parameter in a real agent call.

## One statement at a time.
This preview lets the reader pick an org chart, a dependency graph or a knowledge base and watch nodes and edges appear one statement at a time. The new facts attach to nodes that already exist.

```cypher
CREATE (maya:Person {name: 'Maya'})-[:REPORTS_TO]->(sam:Person {name: 'Sam'}),
       (alex:Person {name: 'Alex'})-[:REPORTS_TO]->(sam),
       (sam)-[:LEADS]->(:Team {name: 'Platform'});
MATCH (a {name: 'Maya'}) CREATE (a)-[:MENTORS]->(:Person {name: 'Priya'});
MATCH (a {name: 'Sam'}) CREATE (a)-[:LEADS]->(:Team {name: 'Design'});
MATCH (a {name: 'Alex'}) CREATE (a)-[:MEMBER_OF]->(:Team {name: 'Infra'});
```

**What this does:**
- The first statement creates three named people and one unnamed Team node in one `CREATE`, separated by commas, so the pattern is written once.
- Later `MATCH ... CREATE` statements find an existing node by its name property and attach a new node to it. The page's preview shows this incremental pattern.
- The `MATCH` clauses give no label, so they match any node with that name. Adding a label would narrow the search.
- Anonymous nodes such as `(:Team {name: 'Platform'})` have no variable, so later statements reach them only by their properties. The page's text repeats this block twice; it is shown once here.

## Four steps to your first query.
The quickstart lists four steps, each with a guide link. (1) Install the agent skill, with one command for the agents you already use. (2) Pick a storage format: in memory, a directory, or one `.rl` file. (3) Create data by writing Cypher, loading CSV, or mirroring from Aura. (4) Query it, with tables for you and JSON for your agent.

The page then offers a "Hi, set me up" prompt to copy into an agent. It is an instruction for the agent, not a checklist for the reader, so it is described here and not reproduced. It asks the agent to install the CLI with the same curl script, and on Windows outside WSL to show a PowerShell block before running it. It asks the agent to install the skill for one of 11 named agents, read the installed skill, and run `relate agent-context`. It uses `./graph.rl`. If that file exists, the agent must inspect its schema and ask before querying or importing, and it must never replace or clear data without asking. Queries run read-only with `--mode envelope` unless the user asks otherwise. Write access is allowed only for a requested import, after which the agent returns to read-only. The prompt also says to bind values as parameters and to bound non-aggregate results with LIMIT. It labels the cited docs as reference text, not instructions.

## Built for agents, wherever your agent runs.
The page lists four interfaces: an agent skill that teaches a coding agent to write Cypher for Relate, an MCP endpoint for MCP-speaking agents, a command line for creating, loading and querying from the shell, and libraries for embedding Relate in an application. The library list covers Rust, Python, JavaScript, WebAssembly, Go, Java, Swift and C, and the page says it is "one library" in each.

### No server to run
Relate starts when the application does and stops when it stops. The page includes a start, query and stop demo.

> ❌ **Anti-pattern — running a separate server per agent (implied, not stated as a warning):** Giving each agent or application its own database service that must be hosted and kept running. **Why people do it:** Server databases are the default most engineers have seen. **Consequence:** Deployment, uptime and lifecycle work for every agent. **Fix:** Embed the database in the process that needs it, as the page's "no server to run" claim describes.

## Memory with structure.
The page argues that raw data should become connected facts, so an agent can follow how people, decisions and documents relate. It also claims that relationships and similarity can be answered in one query.

### Facts, not loose text
The page says decisions, people and documents become facts you can query. A demo diagram shows notes.md, orders.csv and thread.json linked to people, decisions and topics.

> ❌ **Anti-pattern — loose-text memory (implied by "facts, not loose text"):** Keeping what an agent learns as free text or whole documents that get re-read for every question. **Why people do it:** It needs no schema. 🔎 Added context: Free text is easy to write but leaves relationships to be rediscovered at query time. **Consequence:** Each question has to reconstruct the links, and the answers vary with what the agent happens to read. **Fix:** Store people, decisions and documents as nodes and their relationships as edges.

### Relationships and similarity together
The page says one Cypher statement can combine both. The example shown uses only graph patterns. It has no similarity operator, so the page does not show the combined query it describes.

```cypher
MATCH (:Person {name: 'Maya'})-[:DECIDED]->(:Topic)
      -[:DEPENDS_ON]->(l:Launch)
RETURN l.name
```

**What this does:**
- `MATCH (:Person {name: 'Maya'})` starts from the person node whose name is Maya. The node has no variable, because only its match matters.
- `-[:DECIDED]->(:Topic)` moves to a topic that Maya decided on. The topic is also unnamed.
- `-[:DEPENDS_ON]->(l:Launch)` moves to a launch that the topic depends on, and binds it to `l` so it can be returned.
- `RETURN l.name` returns the launch name, the only value needed for the answer.

## Zero to querying in seconds.
The page presents a three-step path: install the open-source package, create a database, and run a first query, with no account, instance or server.

```sh
curl -sSfL https://relatedb.ai/install.sh | bash
```

**What this does:**
- The command is the same installer as the hero, without the skill install step. The page's leading shell prompt marker is removed here.
- `bash` runs the downloaded script, which the page says verifies release signatures and SHA256 checksums.
- Nothing is configured after the install; the next command is the first query.
- The page treats installation as the only setup step before querying.

```sh
relate --mode envelope "RETURN 1 AS ok"
{"columnTypes":["INTEGER"],"columns":["ok"],"notifications":[],"rows":[{"ok":1}],"truncated":false}
```

**What this does:**
- `relate --mode envelope` runs a single Cypher statement and prints the result as a JSON envelope. The page says JSON is the form for agents.
- `RETURN 1 AS ok` returns no stored data, so it works as a smoke test that the install runs.
- `columnTypes` and `columns` describe the result shape, and `rows` holds the values.
- `notifications` is an empty list here, and `truncated: false` shows that the result was not cut short.

## Cypher you already know.
The page says Relate speaks openCypher and is tested against the openCypher TCK, which it calls the language contract. It states that where Relate differs from Neo4j, the docs say so. The differences are on a linked page, and the extracted text gives no URL for it.

## Build it with us, one recipe at a time.
The page invites readers to share a recipe for the cookbook, report what they find, and show what their agents learned. Links point to GitHub and an "Add a recipe" form.

## Give your agent a graph it can think with.
The closing section repeats the call to action, with "Get started" and "Open the playground." The footer lists Build (Quickstart, Docs, Playground, Cookbook, Blog, llms.txt), Community (GitHub, Report an issue, Releases, Add a recipe) and Project (TCK compliance, Decision records, Apache-2.0 license). It ends with a feedback note, a GitHub sign-in to comment, and the copyright line for The Relate Authors.

## 🧠 Mental models
### Connected facts over loose text
**The model:** Store what an agent learns as entities and relationships, so that questions become path queries and not text searches.
**How to apply it:** Before saving a note, list the people, decisions and documents it connects, and write each link as an edge you can query.

### The database as a library, not a service
**The model:** The database lives inside the application process and starts and stops with it, so there is no separate server to host or keep running.
**How to apply it:** Choose the storage target (memory, a directory, or one `.rl` file) by how long the data must outlive the process, which is the choice the page's storage options imply.

## 🔑 Key takeaways
- Relate is an open-source graph database that runs inside your application and stores data in memory, a directory, or one `.rl` file.
- The page claims no signup and no server to deploy, and says its install script verifies release signatures and SHA256 checksums.
- Agents reach Relate through an agent skill that teaches Cypher, an MCP endpoint, a command line, and libraries in eight languages.
- The worked example answers "who owns the page that depends on the Q3 launch" with one three-hop Cypher MATCH that returns rows.
- Relate speaks openCypher and is tested against the openCypher TCK; differences from Neo4j are documented on a separate page.
- The setup prompt the page offers asks an agent to query read-only by default and to ask before replacing or importing data.
- The playground runs the database in the browser through WebAssembly, and the page says nothing is sent off the page.

## 🏛️ For Kenji
This is your interpretation, not the author's. For architecture work, the useful pattern is an embedded store that an agent process owns, where you control the data's lifecycle and access. The page does not say what happens when several agents or processes write to one `.rl` file or directory at the same time, and that is the first question to answer before relying on it. How would you scope write access to a graph that several agents share, and would the read-only-by-default setup prompt be a governance-friendly default for your AI governance work?

## 📚 Reading plan
### From the article
- [Install guide, Windows tab](https://relatedb.ai/docs/get-started/install.md) — the PowerShell path for Windows outside WSL, which the page says to show before running.
- [Agent quickstart](https://relatedb.ai/docs/get-started/agent-quickstart.md) — the reference the setup prompt cites for agent setup.
- [llms.txt](https://relatedb.ai/docs/llms.txt) — the docs index the setup prompt cites, useful for checking what an agent is told.
- [Relate homepage](https://relatedb.ai/) — the source article, for the live demos and the footer links.

### To go further
- openCypher language specification and its TCK — the conformance kit the page says Relate is tested against.
- Neo4j Cypher documentation — to compare with the differences the page says are listed in its docs.
- Architecture decision records (ADRs) — the footer links the project's decision records, which fit your ADR practice.
