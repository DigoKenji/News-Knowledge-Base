---
type: Article Summary
title: 'OKF + RAG: The Ultimate AI Agent Architecture'
description: Curated Markdown knowledge plus RAG search gives agents deterministic facts and broad reach, avoiding hallucinated
  answers on critical data.
resource: https://medium.com/@ravishkhullar/okf-rag-the-ultimate-ai-agent-architecture-26b9ceed44f1
tags:
- ai-ml
- rag
- embedding
- semantic-search
- ai-agent
- large-language-model
- version-control-code-review
- hallucination
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-28T06:47:00-03:00'
sources:
- resource: https://medium.com/@ravishkhullar/okf-rag-the-ultimate-ai-agent-architecture-26b9ceed44f1
  title: 'OKF + RAG: The Ultimate AI Agent Architecture'
  author: Ravish Khullar
status: stable
source_url: https://medium.com/@ravishkhullar/okf-rag-the-ultimate-ai-agent-architecture-26b9ceed44f1
source: Medium save
source_type: medium
published: '2026-11-20'
domain: ai-ml
concepts:
- rag
- embedding
- semantic-search
- ai-agent
- large-language-model
- version-control-code-review
- hallucination
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/426baf74172d81dda71dfd3a5709695a
---

## 🎯 Core thesis
Enterprise agents need two memory systems: a curated, version-controlled set of authoritative facts (OKF) for exact answers, and RAG search over unstructured material for reach, routed between them.

## 🧭 Why the author wrote it
Agents answer well-known facts well but confidently invent answers about company-specific facts such as a churn formula, an internal API schema or a database failover runbook, and this blocks enterprises from trusting them with real work. If the thesis is right, exact facts should come from a reviewed, deterministic source, and retrieval should cover everything else.

## The Problem and the Proposal

### Hallucination
Hallucination is when a model produces an answer that sounds fluent and certain but is factually wrong. The author's examples are specific, private facts: a company's churn-rate formula, its internal API schema, its production failover runbook. A model cannot know these from its training, so it fills the gap with plausible text. The article calls hallucination "the single biggest blocker preventing enterprises from trusting AI agents with real work."

### Retrieval-augmented generation (RAG)
For the past two years, the industry's answer has been RAG: connect the model to a vector database of company documents, search it, and paste the results into the prompt. The author grants that RAG works well for large unstructured corpora. Its flaw is that it is probabilistic: it finds documents similar to the question, not documents that are authoritative. For a revenue formula or a payment API contract, the author says "similar" is not good enough.

### Open Knowledge Format (OKF)
In June 2026, Google Cloud introduced OKF as a vendor-neutral, open specification for storing curated, authoritative knowledge as simple Markdown files. The author's proposal is to combine OKF's deterministic precision with RAG's probabilistic reach. The rest of the article explains each half and how they join.

## The Two Memory Systems
The article opens with an analogy: a human expert joining a company on Day 1 is handed two things.

### The company wiki (OKF)
A curated, well-organised internal wiki containing canonical definitions, API docs, metric formulas and runbooks. Everything in it has been reviewed, approved and version-controlled. When the expert needs the official answer, this is where they go.

### The search engine for everything else (RAG)
Access to thousands of Slack messages, email threads, support tickets and meeting transcripts. When the question is what people said about a topic, or the information is buried in unstructured archives, the expert searches here.

The author's point is that neither system is sufficient alone; together they cover the full range of organisational knowledge.

*[Image: diagram, no caption. Not captured in the source text.]*

## What is OKF? (Open Knowledge Format)
OKF is deliberately simple. An OKF "knowledge bundle" is a directory of Markdown files stored in Git. Each file describes one concept, such as a database table, a metric, an API endpoint or a runbook, and begins with a YAML frontmatter block of machine-readable metadata. The article gives one example, for a business metric.

### OKF knowledge bundle file
```markdown
---
type: metric
title: "Monthly Churn Rate"
description: "Official formula for calculating monthly customer churn."
owner: "data-engineering"
tags: [revenue, kpi, board-report]
timestamp: 2026-06-20T10:00:00Z
---

# Monthly Churn Rate

The official churn rate formula used in all board reports and investor decks:

    Churn Rate = (Customers Lost During Month / Customers at Start of Month) × 100

### Rules
- **Do NOT** use trial accounts in the denominator.
- **Do NOT** count plan downgrades as churn.
- Source of truth: `analytics.monthly_churn_summary` table.

### Related
- [Monthly Active Users](mau.md)
- [Revenue Dashboard](revenue_dashboard.md)
```

**What this does:**
- The frontmatter gives tools typed metadata (`type`, `title`, `description`, `owner`, `tags`, `timestamp`) that can be read without parsing the prose.
- The body restates the formula as a heading and a formula line, so people and the agent read the same definition.
- The `### Rules` list records exclusions that a generic churn definition would get wrong: trial accounts stay out of the denominator, and plan downgrades are not churn.
- The `Source of truth` line names the table the metric comes from, so the definition points back to the data.
- The `### Related` links use relative file paths, so the bundle forms a navigable set of linked files.

> 🔎 Added context: YAML is a plain-text format for key-value data and lists. A frontmatter block is the metadata section at the top of a file, delimited by `---` lines.

### Why This Matters
#### No vector math, no hallucination
When the agent needs the churn formula, it reads this file directly. There is no embedding, no cosine similarity and no "top-k nearest neighbors." It gets the exact, approved answer every time.

#### Git-ops native
Because the bundle lives in Git, every change goes through pull requests, code reviews and commit history. The result is an audit trail of who changed the churn formula, when, and why.

> 🔎 Added context: Git records each change as a commit with an author, a timestamp and a message. Pull requests add a review step before a change is merged.

#### Zero infrastructure
No vector database, no embedding pipeline, no chunking strategy and no re-indexing jobs. The knowledge is just files in a folder.

## What is RAG? (Retrieval-Augmented Generation)
The article presents RAG as the standard pattern for connecting an LLM to large unstructured data. It has six steps: ingest documents such as PDFs, emails, Confluence pages and Slack messages; chunk them into smaller segments; embed each chunk into a vector; store the vectors in a vector database such as Pinecone, Weaviate or pgvector; at query time, embed the question into the same vector space and retrieve the most similar chunks; then augment the LLM's prompt with those chunks so it can answer.

### Chunking
Chunking splits each source document into smaller segments so that each one can be embedded and retrieved on its own. The article lists it as step 2 and gives no chunk size or method. Its OKF comparison treats chunking as a cost: the file-based approach needs no chunking strategy at all.

### Embedding
An embedding is a list of numbers that represents a chunk's meaning, so that software can compare meanings with a simple distance calculation. The article embeds both the stored chunks and the incoming question into the same vector space, which is what makes the comparison possible.

> 🔎 Added context: "Vector space" means the common coordinate system all embeddings share; distance in that space stands in for closeness of meaning.

### Vector database
The vector database stores the embedded chunks and returns the nearest ones for a query. The article names Pinecone, Weaviate and pgvector as examples. It ranks results by semantic similarity, not by authority.

### Where RAG struggles
RAG relies on vector math, so it can return text that is related to the question without being authoritative. The article's example is three Slack threads that define churn rate slightly differently; RAG might retrieve the wrong one. RAG excels at reach across thousands of documents and struggles with precision.

> ❌ **Anti-pattern — RAG as the source of truth for exact facts:** the agent answers a formula, contract or runbook question from whichever chunks rank highest. **Why people do it:** the article does not say; it describes RAG as the default answer of the past two years. **Consequence:** a wrong or conflicting definition can be retrieved and presented as the answer, because retrieval does not separate authoritative text from merely similar text. **Fix:** store the approved definition in OKF and route exact-fact questions there, keeping RAG for discovery.

## The Hybrid Architecture: OKF + RAG
The ultimate agent architecture uses an intelligent router that classifies each incoming query and directs it to the appropriate memory system.

### Intelligent router
The router decides, query by query, whether the answer should come from OKF or from RAG. The article describes this only as classification and routing. The code sketch below implements it as a fallback order: try the deterministic source first, then the probabilistic one.

### How the Router Decides
The article's router decision table is an image, and its content was not captured in the source text. The article gives no rule beyond the code below.

### Implementation Example
The article gives a simplified Python sketch of the router. It is a sketch: the RAG search is a placeholder, and the OKF lookup is keyword-based.

```py
from openai import OpenAI
import os
import glob
import yaml

client = OpenAI()

# 1. Load the OKF knowledge bundle from the local directory
def load_okf_bundle(bundle_path: str) -> dict:
    """Reads all Markdown files in the OKF directory into a searchable dict."""
    knowledge = {}
    for filepath in glob.glob(f"{bundle_path}/**/*.md", recursive=True):
        with open(filepath) as f:
            content = f.read()
            # Extract the title from YAML frontmatter
            if content.startswith("---"):
                _, frontmatter, body = content.split("---", 2)
                meta = yaml.safe_load(frontmatter)
                title = meta.get("title", os.path.basename(filepath))
                knowledge[title.lower()] = body.strip()
    return knowledge

# 2. Search OKF (deterministic, keyword-based)
def search_okf(query: str, okf_knowledge: dict) -> str | None:
    """Simple keyword match against OKF titles."""
    for title, content in okf_knowledge.items():
        if title in query.lower():
            return content
    return None

# 3. Search RAG (probabilistic, vector-based)
def search_rag(query: str) -> str:
    """Placeholder for your vector DB search (Pinecone, Weaviate, etc.)."""
    # results = vector_db.similarity_search(query, top_k=5)
    return "RAG context: [retrieved chunks would appear here]"

# 4. The Intelligent Router
def answer_query(query: str, okf_bundle_path: str) -> str:
    okf_knowledge = load_okf_bundle(okf_bundle_path)
    
    # Try OKF first (deterministic path)
    okf_result = search_okf(query, okf_knowledge)
    
    if okf_result:
        context = f"[SOURCE: Official Knowledge Base (OKF)]\n{okf_result}"
    else:
        # Fall back to RAG (probabilistic path)
        context = f"[SOURCE: Document Search (RAG)]\n{search_rag(query)}"
    
    # Send to LLM with the retrieved context
    response = client.chat.completions.create(
        model="gpt-4o",
        messages=[
            {"role": "system", "content": "Answer using ONLY the provided context."},
            {"role": "user", "content": f"Context:\n{context}\n\nQuestion: {query}"}
        ]
    )
    return response.choices[0].message.content
```

**What this does:**
- `load_okf_bundle` walks the bundle directory recursively, reads each Markdown file, and keys its body by the lowercased `title` from the frontmatter, falling back to the filename. The bundle is re-read from disk on every call.
- `search_okf` returns a body only when a stored title appears verbatim inside the lowercased query, so a paraphrased question misses and falls through to RAG.
- `search_rag` is a placeholder. The commented line shows the intended call, `similarity_search(query, top_k=5)`, which would return five results.
- `answer_query` tries OKF first and labels the context with its source, either `[SOURCE: Official Knowledge Base (OKF)]` or `[SOURCE: Document Search (RAG)]`, so the reader can see which memory answered.
- The LLM call uses `gpt-4o` with a system message telling it to answer using only the provided context.

*[Image: diagram, no caption. Not captured in the source text.]*

## Conclusion
The article's conclusion is that the future of enterprise agents is a hybrid, not a choice between OKF and RAG.

### Precision and reach together
OKF gives the agent a curated, hallucination-proof memory for the facts that matter most: KPIs, API contracts and operational runbooks. RAG lets it explore large archives of unstructured knowledge that no person could organise by hand. The combined result is a deterministic, Git-controlled knowledge base paired with probabilistic vector search, so the agent is precise when it must be and exploratory when it needs to discover.

## 🧠 Mental models
### Day-one expert with two resources
**The model:** A new hire is given a curated company wiki for official answers and a search engine for everything else. Neither alone covers the organisation's knowledge.
**How to apply it:** When designing an agent, decide which questions need the approved answer and which need discovery, and give each its own source.

### Deterministic first, probabilistic fallback
**The model:** Check the source of truth first, and use fuzzy retrieval only when it has no answer.
**How to apply it:** Route each query to an exact lookup, and call similarity search only when the lookup misses, as the router code does.

### Similar is not authoritative
**The model:** Semantic similarity finds related documents, not authoritative ones.
**How to apply it:** For mission-critical facts, require an approved, version-controlled source, and never let the top-ranked chunk stand in for it.

## 🔑 Key takeaways
- Hallucination on company-specific facts, such as KPIs, API contracts and runbooks, is what blocks enterprises from trusting agents with real work.
- RAG retrieves documents semantically similar to a question, which is not the same as retrieving authoritative ones.
- OKF stores curated knowledge as a directory of Markdown files in Git, one concept per file, each with YAML frontmatter.
- Git history gives OKF an audit trail of every change: who changed a formula, when, and why.
- The router tries the OKF lookup first and falls back to vector search, and the LLM is told to answer using only the context it receives.
- Infrastructure is the trade-off the author emphasises: the file-based source needs no vector database, embedding pipeline, chunking or re-indexing.
- Use both: OKF for exact, reviewed facts, and RAG for exploring unstructured archives.

## 🏛️ For Kenji
This is your interpretation, not the author's. Architecture governance could treat the knowledge an agent may cite as an authoritative source with an owner, a review step and a change history, alongside data governance and metadata cataloguing. The question it raises: which facts in your organisation would an agent need as reviewed, versioned records rather than retrievable text, and who would own them?

## 📚 Reading plan
### From the article
- The article cites no external links.

### To go further
- Open Knowledge Format (OKF) specification from Google Cloud, the source of the format described here; check the original for details the article summarises.
- Vector databases and chunking strategies, since the article's RAG side is only a placeholder in its code and the real retrieval design is left open.
- Data governance and metadata catalogues, for deciding who approves the facts an agent may treat as authoritative.

## Concepts
- [Retrieval-augmented generation](/concepts/ai-ml/rag.md)
- [Embedding](/concepts/ai-ml/embedding.md)
- [Semantic search](/concepts/ai-ml/semantic-search.md)
- [AI agent](/concepts/ai-ml/ai-agent.md)
- [Large language model and foundation model](/concepts/ai-ml/large-language-model.md)
- [Version control and code review](/concepts/software-engineering/version-control-code-review.md)
- [Hallucination](/concepts/ai-ml/hallucination.md)
