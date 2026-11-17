---
type: Article Summary
title: How to Architect Semantic Chunking Pipelines for High-Performance RAG
description: Chunking strategy determines what RAG retrieves, so pick among five methods by document structure and retrieval needs.
resource: https://medium.com/@kuriko-iwai/how-to-architect-semantic-chunking-pipelines-for-high-performance-rag-c539ea658865
tags:
- ai-ml
- rag
- rag
- parent-child-chunking
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/@kuriko-iwai/how-to-architect-semantic-chunking-pipelines-for-high-performance-rag-c539ea658865
  title: How to Architect Semantic Chunking Pipelines for High-Performance RAG
  author: Kuriko Iwai
status: stable
source_url: https://medium.com/@kuriko-iwai/how-to-architect-semantic-chunking-pipelines-for-high-performance-rag-c539ea658865
source: Medium
source_type: medium
published: '2026-09-29'
domain: ai-ml
subdomain: rag
concepts:
- rag
- parent-child-chunking
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81448119c16027420c89
---

# How to Architect Semantic Chunking Pipelines for High-Performance RAG

**Source:** https://medium.com/@kuriko-iwai/how-to-architect-semantic-chunking-pipelines-for-high-performance-rag-c539ea658865
**Author:** Kuriko Iwai
**Date:** 2026-09-29
**Concepts:** [Retrieval-augmented generation](/concepts/rag.md) · [Parent-child chunking](/concepts/parent-child-chunking.md)

---

## 🎯 Core thesis
Chunking is a retrieval-architecture decision, not a preprocessing detail. How you split documents determines what a RAG system can find and how much context the model receives when it answers.

## 🧭 Why the author wrote it
RAG has become the default way to ground AI in private data, but its answers are only as good as what retrieval returns. The author argues the splitting step deserves the same design care as the retrieval pipeline itself, and compares five chunking strategies on one shared sample text so the trade-offs can be seen directly.

## Introduction
The introduction describes RAG as the standard for grounding a model in private data and states that output quality is capped by the information retrieved. It argues that teams have to go beyond simple ingestion and design retrieval pipelines deliberately. The lever it names is chunking: breaking large datasets into digestible, semantically meaningful segments. The article's plan is to explain the mechanism first and then evaluate the major strategies used in modern AI applications.

## What is Chunking
Chunking is the process of splitting large bodies of text into smaller, manageable pieces, called chunks, before they are converted into embeddings.

### Chunking in the ingestion pipeline
The article's Figure A shows the data ingestion flow. Raw, unstructured data arrives in forms such as text (PDFs, docs), images, audio and video. It is retrieved first, then chunked, then passed through an embedding model, and finally written to a vector store. Retrieval gets the data in; chunking is the second box, and it runs after retrieval. At query time, the system can return only the chunk that answers the question instead of the whole file.

### Embeddings and the vector store
An embedding model turns each chunk into a vector embedding, which is a long list of numbers (coordinates) representing the chunk's semantic meaning. The vector store holds these embeddings. Figure A uses three dimensions for demonstration, but real spaces are usually high-dimensional. Related chunks are stored close together. The author's example is a chunk about "Golden Retrievers" and one about "Labradors," which sit near each other, so the LLM can find relevant material almost instantly.

### Why Chunking Matters
The article gives three reasons:

- **Relevance:** small chunks let RAG retrieve the exact piece that answers a question from a large document.
- **Cost efficiency:** processing smaller, targeted snippets saves tokens and computation time.
- **Context retention:** well-chunked data keeps enough surrounding information for the LLM to understand why and how.

Taken together, chunking retrieves the most relevant context for a query while keeping input tokens low enough to fit the model's context window.

> 🔎 Added context: the three reasons pull in different directions. Small chunks favor relevance and cost, while context retention favors larger chunks. That tension is why the article treats strategy choice as the real decision.

## Comparative Analysis: 5 Industry-Standard Chunking Strategies
The article says no single strategy fits every case and lists five: fixed-size chunking, recursive character chunking, document-specific chunking, semantic chunking, and parent-child (hierarchical) chunking. Each is applied to the same sample text about solar energy infrastructure, so differences in output come from the splitter, not the input.

### Sample Text
The author uses this sample for all five strategies:

```py
text = f"""Solar panels, or photovoltaic cells, convert sunlight into
electricity. This process happens at the atomic level. Some materials exhibit
a property known as the photoelectric effect. This causes them to absorb
photons and release electrons. Beyond the cells, an inverter is required
to convert DC to AC. Large-scale solar farms also require battery storage
systems to manage peak load during non-sunny hours."""
```

**What this does:**
- Builds one multi-line string of six sentences about solar panels, the photoelectric effect, inverters and battery storage.
- Triple quotes allow line breaks without escape characters.
- Trailing spaces and line breaks inside the string are kept, so each splitter receives the same bytes.
- The `f` prefix has no placeholders, so it has no effect here.
- The shared input is what makes the five strategies comparable.

## 1. Fixed-Size Chunking
Fixed-size chunking is the simplest approach. It sets a fixed number of characters or tokens per chunk and optionally overlaps neighboring chunks, a method called a sliding window. The article's Figure B example uses a chunk size of 500 tokens with a 50-token overlap. The overlap ensures context is not lost if a key sentence falls across a boundary.

### Sliding-window overlap
The overlap repeats the end of one chunk at the start of the next. Without it, a sentence split in half would be unreadable in both chunks. The article's sample uses a 100-unit window with a 20-unit overlap.

### Pros
Computationally affordable and easy to implement.

### Best For
- Quick prototyping.
- Handling simple text.
- General use cases where speed matters more than granular semantic accuracy.

### Practical Implementation
The article uses the `CharacterTextSplitter` class from the `langchain_text_splitters` library:

```py
from langchain_text_splitters import CharacterTextSplitter

# fixed size chunking
fixed_splitter = CharacterTextSplitter(
    separator="",
    chunk_size=100,
    chunk_overlap=20
)
fixed_chunks = fixed_splitter.split_text(text)
```

**What this does:**
- Imports the character-based splitter from the LangChain text-splitting package.
- `separator=""` means there is no preferred break point, so the splitter cuts at exact lengths.
- `chunk_size=100` caps each chunk at 100 units.
- `chunk_overlap=20` carries the last 20 units of each chunk into the next one.
- `split_text(text)` applies the splitter to the sample string and returns a list of chunks.

> 🔎 Added context: `CharacterTextSplitter` measures length in characters unless it is given a token-counting function. The article calls the output "100 tokens," but the chunks it shows are about 100 characters, so read the units as characters.

### Resulting Chunks
The article reports two chunks of 100 units each:

- Chunk 1: "Solar panels, or photovoltaic cells, convert sunlight into electricity. This process happens at the a"
- Chunk 2: "ns at the atomic level. Some materials exhibit a property known as the photoelectric effect. This ca"

The word "atomic" is sliced across the boundary. The method is the fastest of the five, but it can create semantic noise because the LLM receives partial words, which can degrade the generated response.

> ❌ **Anti-pattern — cutting by length alone:** splitting at a character count with no regard for words or sentences. **Why people do it:** it is the quickest thing to build, and it works on a prototype. **Consequence:** partial words and broken sentences reach the LLM, which degrades answer quality. **Fix:** use a recursive or structure-aware splitter for real content, and keep fixed-size windows for cases where speed is the priority.

## 2. Recursive Character Chunking
Recursive character splitting uses a hierarchy of separators to find a natural break point, rather than cutting at a hard character limit. The method first tries the most significant separator, then moves down the list until the chunk size is met. The article's diagram (Figure C) shows this hierarchy, from newlines and periods down to commas and spaces.

### Separator hierarchy
Each separator is a candidate break point, tried in order of how large a boundary it marks. A larger boundary, like a paragraph break, keeps more related text together than a space does. The method drops to a smaller separator only when the larger one leaves a chunk that is too big.

### Pros
Keeps related ideas together better than fixed-size splitting.

### Best For
- Maintaining the integrity of paragraphs and sentences.
- Articles and blogs.

### Practical Implementation
The article uses the `RecursiveCharacterTextSplitter` class from the same library:

```py
from langchain_text_splitters import RecursiveCharacterTextSplitter

recursive_splitter = RecursiveCharacterTextSplitter(
    chunk_size=100,
    chunk_overlap=20,
    separators=["\n\n", "\n", " ", ""]
)
recursive_chunks = recursive_splitter.split_text(text)
```

**What this does:**
- Imports the recursive splitter, which tries each separator in turn before falling back to the next.
- `chunk_size=100` and `chunk_overlap=20` set the same size and overlap as the fixed-size example, so the comparison is fair.
- `separators=["\n\n", "\n", " ", ""]` lists double newlines, single newlines, spaces, and finally any character, from most to least preferred.
- The empty string at the end is the last-resort fallback that cuts at any character.
- `split_text(text)` returns chunks that prefer paragraph and word boundaries.

> 🔎 Added context: the prose says the method splits on periods first, but the code's separator list has no period entry. The code is the reliable guide to what the splitter does. Also, the article says each chunk contains "exactly 100 tokens," but the chunks it shows are shorter. A size cap does not pad chunks, so a chunk is only as long as the text allows before the next separator.

### Resulting Chunks
- Chunk 1: 'Solar panels, or photovoltaic cells, convert sunlight into electricity.'
- Chunk 2: 'This process happens at the atomic level. Some materials exhibit a property'

Compared with fixed-size splitting, the recursive splitter stops at the period that ends the first sentence, so the grammar stays intact. The article says this makes the output more readable for the LLM.

## 3. Document-Specific Chunking
Document-specific chunking respects the native format of the file, such as Markdown, HTML, LaTeX or code. The splitter uses the document's own structure as its boundaries:

- **Markdown:** splits by headers (`#`, `##`, `###`).
- **HTML:** splits by HTML tags, commas or dots.
- **Code:** splits by function or class definitions.

### Structure-aware boundaries
The point is that the author already marked the logical sections. Using those markers keeps a section's content together and stops unrelated sections from merging.

### Pros
Preserves the logical hierarchy of the document.

### Best For
- Highly structured technical documentation, such as PDF reports and manuals.
- Codebases.

### Practical Implementation
The article uses the `MarkdownHeaderTextSplitter` class to define header levels and split the document:

```py
from langchain_text_splitter import MarkdownHeaderTextSplitter

markdown_document = """
# Solar Energy Guide
## The Physics
Solar panels, or photovoltaic cells, convert sunlight into electricity.
This process happens at the atomic level.
Some materials exhibit the photoelectric effect.
## The Hardware
Beyond the cells, an inverter is required to convert DC to AC.
Large-scale solar farms also require battery storage systems.
### Maintenance
Regular cleaning of panels ensures maximum photon absorption.
"""
# define the headers
headers_to_split_on = [
    ("#", "Header 1"),
    ("##", "Header 2"),
    ("###", "Header 3"),
]
# initialize the splitter
markdown_splitter = MarkdownHeaderTextSplitter(headers_to_split_on=headers_to_split_on)
# split
md_header_splits = markdown_splitter.split_text(markdown_document)
```

**What this does:**
- Defines a small Markdown document with a title, two level-2 sections and one level-3 subsection.
- `headers_to_split_on` maps each Markdown marker to a metadata key, so each chunk records which headers it sits under.
- The three tuples cover header levels 1 to 3.
- `MarkdownHeaderTextSplitter(...)` builds the splitter from that mapping.
- `split_text(markdown_document)` returns one chunk per section, with the header path stored as metadata.

> 🔎 Added context: the import line uses the singular package name `langchain_text_splitter`, while the earlier examples use `langchain_text_splitters`. As written, the import is likely to fail. Check the package name before running this snippet.

### Resulting Chunks
Chunk 1:
- Content: Solar panels, or photovoltaic cells, convert sunlight into electricity. This pro…
- Metadata: `{'Header 1': 'Solar Energy Guide', 'Header 2': 'The Physics'}`

Chunk 2:
- Content: Beyond the cells, an inverter is required to convert DC to AC. Large-scale solar…
- Metadata: `{'Header 1': 'Solar Energy Guide', 'Header 2': 'The Hardware'}`

Chunk 3:
- Content: Regular cleaning of panels ensures maximum photon absorption….
- Metadata: `{'Header 1': 'Solar Energy Guide', 'Header 2': 'The Hardware', 'Header 3': 'Maintenance'}`

The method uses the author's intent, so "The Physics" and "The Hardware" never merge into the same chunk. The article says this matters most for technical manuals.

## 4. Semantic Chunking
Semantic chunking measures how the meaning changes from one sentence to the next. It computes the cosine similarity between the embeddings of consecutive sentences. When the similarity falls below a threshold, the method assumes a new topic has started and begins a new chunk.

### Cosine similarity between consecutive sentences
Cosine similarity compares the direction of two vectors. Scores near 1 mean the sentences point the same way in embedding space, so they are about similar things. Lower scores mean the meaning has moved.

> 🔎 Added context: the article does not define cosine similarity or the threshold. The definition here is standard background. The threshold value is never stated in the article.

### Pros
High retrieval accuracy because chunks represent complete ideas.

### Best For
- Academic papers.
- Narrative-heavy documents.
- Long-form essays, where paragraphs do not line up with topic shifts.

### Practical Implementation
The article first embeds sentences with the `SentenceTransformer` class from the `sentence_transformers` library, then compares consecutive embeddings:

```py
from sklearn.metrics.pairwise import cosine_similarity
from sentence_transformers import SentenceTransformer

# split the text into sentences
sentences = split_into_sentences(text)

# create vector embeddings
model = SentenceTransformer(MODEL)
embeddings = model.encode(sentences)

# compute cosine similarity to split into chunks
chunks = []
current_chunk = [sentences[0]]
for i in range(1, len(sentences)):
    prev_emb = embeddings[i - 1].reshape(1, -1)
    curr_emb = embeddings[i].reshape(1, -1)
    similarity = cosine_similarity(prev_emb, curr_emb)[0][0]
    print(f"Similarity between sentence {i-1} and {i}: {similarity:.3f}")

    # if similarity drops → meaning shift → split
    if similarity < similarity_threshold:
        chunks.append(" ".join(current_chunk))
        current_chunk = [sentences[i]]
    else:
        current_chunk.append(sentences[i])

# add last chunk
if current_chunk:
    chunks.append(" ".join(current_chunk))
return chunks
```

**What this does:**
- Imports `cosine_similarity` from scikit-learn and `SentenceTransformer` from the sentence-transformers library.
- `split_into_sentences(text)` produces the sentence list the comparison runs over.
- `model.encode(sentences)` returns one embedding vector per sentence in a single batch.
- Each pair of neighboring embeddings is reshaped to a 2D row, which `cosine_similarity` requires, and the score is printed so the threshold can be tuned.
- A score below `similarity_threshold` closes the current chunk and starts a new one. Otherwise the sentence joins the current chunk. The final chunk is appended after the loop.

> 🔎 Added context: the snippet uses `split_into_sentences`, `MODEL` and `similarity_threshold` without defining them, and the `return` sits outside any function. Treat it as a sketch. The threshold has to be chosen and validated against your own data.

### Resulting Chunks
The article says the system notices a shift in meaning between "release electrons" and "Beyond the cells." The cosine similarity scores reported are:

- Chunk 1: Solar panels, or photovoltaic cells, convert sunlight into electricity. (0.196)
- Chunk 2: This process happens at the atomic level. (0.313)
- Chunk 3: Some materials exhibit a property known as the photoelectric effect. (0.546)
- Chunk 4: This causes them to absorb photons and release electrons. (0.266)
- Chunk 5: Beyond the cells, an inverter is required to convert DC to AC. (0.336)
- Chunk 6: Large-scale solar farms also require battery storage systems to manage peak load during non-sunny hours.

The article's conclusion is that semantic chunking recognizes the move from physics to engineering and forces a split.

> 🔎 Added context: the threshold is not stated. The output has one sentence per chunk, and the highest score listed (0.546) still produced a split. That implies a threshold above 0.546, which would split every pair. The example therefore does not show sentences grouping together. It shows only the scores, so the threshold needs to be set and tested before this output means anything.

## 5. Parent–Child (Hierarchical) Chunking
Parent-child chunking stores the same data at two sizes: a small child chunk for search and a larger parent chunk for context. The system searches the small, specific child chunks first. When one matches, it retrieves the larger parent around it and gives that to the LLM. The article's example uses about 100 tokens for the child and about 1000 tokens for the parent.

### Search small, return large
Precise matching needs small units, and good answers need surrounding context. Parent-child retrieval separates the two. The index holds the children, and the response holds the parent.

### Pros
Avoids the lost-in-the-middle problem by giving the LLM plenty of background information.

> 🔎 Added context: "lost in the middle" describes models using information less reliably when it sits in the middle of a long context. The article names the problem but does not explain it further.

### Best For
- Enterprise-grade RAG systems.
- Balancing high-precision search with comprehensive context.

### Practical Implementation
The article creates one parent chunk holding the whole sample text, then creates child chunks that each hold one sentence and an embedding:

```py
import re, uuid
from sentence_transformers import SentenceTransformer

# create parent chunk
parent_chunk = {
    "id": str(uuid.uuid4()),
    "text": text.strip() # the entire sample text
}
# load embedding model
model = SentenceTransformer("all-MiniLM-L6-v2")
def create_child_chunks(parent_chunk):
    sentences = split_into_sentences(parent_chunk["text"])

    children = []
    for sentence in sentences:
        child = {
            "id": str(uuid.uuid4()),
            "parent_id": parent_chunk["id"],
            "text": sentence,
            "embedding": model.encode(sentence)
        }
        children.append(child)
    return children
child_chunks = create_child_chunks(parent_chunk)
```

**What this does:**
- Creates the parent record with a UUID and the full sample text, stripped of outer whitespace.
- Loads `all-MiniLM-L6-v2`, a small sentence-embedding model from sentence-transformers.
- `create_child_chunks` splits the parent into sentences, and each sentence becomes a child with its own UUID.
- Each child stores `parent_id`, which is the link the retriever follows from a matched child back to its parent.
- Each child stores its embedding, so the search index is built over the children.

> 🔎 Added context: the code makes one child per sentence, but the article's text describes children of about 100 tokens and parents of about 1000 tokens. The code also imports `re` without using it, and `split_into_sentences` is not defined. Treat the code as a pattern, and size the children and parents to your own data.

### Results
Parent context returned to the LLM:

> Solar panels, or photovoltaic cells, convert sunlight into electricity.
> This process happens at the atomic level.
> Some materials exhibit a property known as the photoelectric effect.
> This causes them to absorb photons and release electrons.
> Beyond the cells, an inverter is required to convert DC to AC.
> Large-scale solar farms also require battery storage systems to manage peak load during non-sunny hours.

Matched child sentence:

> Large-scale solar farms also require battery storage systems to manage peak load during non-sunny hours.

The retriever matches the query against the child embeddings, then returns the parent chunk instead of just the matched sentence. The LLM can then read the full paragraph and see how solar panels, inverters and battery storage relate. In the article's words, this explains the "why" of the infrastructure, not just the "what" of the batteries.

> ❌ **Anti-pattern — returning only the matched fragment:** sending the small matched chunk straight to the LLM. **Why people do it:** it is the simplest pipeline, and the match looks like the answer. **Consequence:** the model gets a sentence without its surrounding reasoning, so it can explain what a component is but not why it is there. **Fix:** store children for search and return their parents for context.

## Wrapping Up
The article's conclusion is that chunking is not just slicing data. With a proper strategy, it becomes semantic optimization. Good grouping means a user's question returns a coherent piece of information, not a fragment that leaves the AI guessing.

### How to Choose Optimal Strategy
The article offers four points for choosing a strategy.

#### 1. Identify Patterns & Logical Structures
Look for repetition, sequence, or inherent connections in the data. In technical content, that means headers, Markdown tags or paragraph breaks, so a chunk does not cut through a vital sentence or thought. The key question: does the data have a predictable layout or syntax that carries meaning? If so, use **document-specific (structure-aware) chunking**, with parsers that respect the native format (Markdown, HTML or LaTeX) and split at logical boundaries rather than character counts.

#### 2. Maintain Semantic Context (The Mnemonic for AI)
Humans use mnemonics to link ideas, and AI systems use overlapping chunks for the same purpose. Including a small part of the previous chunk at the start of the next one builds a narrative bridge, so the model does not lose the broader context. The key question: if I read this chunk in isolation, would I still understand what the sentence is about? If not, use **fixed-size chunking with a sliding window**, with an overlap of 10 to 20 percent so the end of one chunk and the start of the next share enough connecting text.

#### 3. Prioritize Categorical Grouping
Organize information by category or hierarchy, the way a grocery list is grouped by produce or dairy. The key question: how granular is the information users need, specific facts or broad overviews? The strategy is **recursive character splitting**: start with a large separator, such as a double newline, and move to smaller ones (space, then character) until the target size is reached. This keeps neighboring ideas in the same chunk.

#### 4. Optimize for Retrieval & Relevance
Test the chunk size against real-world queries, because chunks that are too small lack context and chunks that are too large add noise that can confuse the LLM. The key question: am I retrieving irrelevant text that wastes the context window, or missing the answer entirely? If so, run experiments. The article's benchmark runs chunking at different sizes (256, 512 and 1024 tokens) and scores each with Hit Rate or MRR (Mean Reciprocal Rank), so you can see which size consistently gives the most accurate answers for the task.

## 🧠 Mental models
### Chunk as an answer unit
**The model:** a good chunk is a self-contained answer to some possible question, not an arbitrary slice of bytes.
**How to apply it:** run the article's isolation test on sample chunks. If a chunk makes no sense when read alone, the boundary is wrong.

### Search small, return big
**The model:** retrieval precision and answer context need different chunk sizes, so one index should store both.
**How to apply it:** index small child chunks for matching, keep a parent_id on each, and return the parent to the LLM.

### Overlap as a narrative bridge
**The model:** overlap carries connective text across a boundary, so a split sentence or idea is still understandable from either side.
**How to apply it:** start with a 10 to 20 percent overlap between neighboring chunks and adjust it against retrieval results.

### Benchmark, don't guess
**The model:** the right chunk size is an empirical result for your data and queries, not a constant.
**How to apply it:** test sizes such as 256, 512 and 1024 tokens against real queries, and compare Hit Rate and MRR for each.

## 🔑 Key takeaways
- The article's central claim is that chunk boundaries determine what retrieval can return, which makes chunking the main lever on RAG output quality.
- Fixed-size splitting is the fastest option, but it can cut words mid-token and add semantic noise to the LLM's input.
- Recursive splitting tries paragraph, line, then space boundaries before falling back to characters, so sentences stay intact where possible.
- Structure-aware splitting uses Markdown headers so that distinct sections, such as "The Physics" and "The Hardware," never merge into one chunk.
- Semantic chunking starts a new chunk when the cosine similarity between consecutive sentence embeddings drops below a threshold that must be tuned.
- Parent-child retrieval searches small child chunks and returns their parent, so the LLM gets the "why" as well as the "what."
- Choose chunk size by benchmarking candidates such as 256, 512 and 1024 tokens against real queries, scored with Hit Rate or MRR.

## 🏛️ For Kenji
This is your interpretation, not the author's. Chunking sets retrieval granularity, token cost and answer context at the same time, so it belongs in the architecture review, not in a preprocessing script. Parent-child retrieval is the pattern most relevant to enterprise RAG, because it separates the index unit from the context unit, which matters for governed, citable answers. The question it raises for your work: which metric, Hit Rate or MRR, would catch a retrieval regression after your documents or embedding model change?

## 📚 Reading plan
### From the article
The article cites no links.

### To go further
- LangChain text splitters documentation (the `CharacterTextSplitter`, `RecursiveCharacterTextSplitter` and `MarkdownHeaderTextSplitter` classes) — the reference for the splitter behavior and parameters used in the examples.
- sentence-transformers documentation (`SentenceTransformer`, `all-MiniLM-L6-v2`) — the embedding model and encoding API behind the semantic and parent-child examples.
- Hit Rate and Mean Reciprocal Rank as retrieval evaluation metrics — the basis for the chunk-size benchmark the article recommends.
- "Lost in the middle" research on how language models use long contexts — the background for the parent-child motivation.
