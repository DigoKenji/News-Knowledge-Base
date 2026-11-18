---
type: Article Summary
title: 'Presentation: Scaling LLM-Based Ranking Systems for Latency-Critical Search & Recommendation Workloads'
description: LLM ranking at LinkedIn scales only when model size, context length and inference efficiency are optimized together.
resource: https://www.infoq.com/presentations/scale-llm-ranking-systems-linkedin/
tags:
- ai-ml
- llm-engineering
- kv-cache
- inference-tradeoffs
- llm-as-a-judge
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://www.infoq.com/presentations/scale-llm-ranking-systems-linkedin/
  title: 'Presentation: Scaling LLM-Based Ranking Systems for Latency-Critical Search & Recommendation Workloads'
  author: InfoQ
status: stable
source_url: https://www.infoq.com/presentations/scale-llm-ranking-systems-linkedin/
source: InfoQ
source_type: blog
published: '2026-11-17'
domain: ai-ml
subdomain: llm-engineering
concepts:
- kv-cache
- inference-tradeoffs
- llm-as-a-judge
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d81809261e766c96542bf
---

## 🎯 Core thesis
An LLM used as a ranker only fits a latency-critical, billion-member system if the model, the context and the inference path are shrunk and tuned together, and the smallest representation that still meets the quality bar is usually the right one.

## 🧭 Why the author wrote it
LinkedIn search is moving from keyword and filter entry to natural-language intent, and an LLM can read that intent far better than older rankers. Running an LLM at that scale, with about 1.3 billion members and strict P99 latency targets, breaks the assumptions of chatbot-style serving, so the talk reports what LinkedIn changed at each layer and which problems appeared only in production. If the thesis holds, the cost and speed of LLM ranking are decided by how much text each candidate contributes and how efficiently the serving stack runs, not by model capability alone.

## LinkedIn Search
The speaker, Sundara Raman Ramachandran, is a tech lead at LinkedIn focused on LLM inference for ranking and recommendation, and an open-source contributor to SGLang, where he worked on the scoring API used for LLM ranking. He states that LinkedIn has about 1.3 billion members in about 200 countries and runs very high query volumes for ranking and recommendations. Search is evolving from specific keywords and filters toward natural-language intent, for example "find me design roles in the entertainment industry" for jobs, or "who in my network can help me file a patent for my business" for people. That raw text is hard to parse with classic methods, which is where LLMs help, but it also makes an already hard ranking problem harder.

## Architecture
The end-to-end pipeline has three stages: query understanding, retrieval and ranking. The talk focuses on ranking, but the first two stages define what the ranker receives, so the speaker covers them briefly.

### Query understanding
This layer turns ambiguous raw text into a structured, normalized query. The speaker walks through a member who is a software engineer in the Bay Area with 5 years of experience, searching "find software engineer jobs near my location". The query planner identifies the search criteria (here job title and location) and chooses how to execute them. The query tagger enriches the query with data from the member profile, such as job title and location, and with any filters the member already uses. The query rewriter resolves self-referencing text: "near my location" is replaced with the member's actual location, Bay Area. The packaged, normalized query then goes to retrieval.

### Retrieval
Retrieval must find the top-K semantically relevant jobs out of millions of postings without scoring each one. The query output is converted to an embedding, and a KNN (k-nearest-neighbour) vector search runs against an index built offline from embeddings of all jobs. This stage is tuned for recall rather than precision: passing a few extra candidates downstream is acceptable, while discarding a highly relevant job is not.

### Ranking
The ranker decides the display order of the retrieved candidates, and LinkedIn uses an LLM for it. The prompt gives the model the search query, the member profile description and one job, and asks whether the job matches the query, answering yes or no. The final score comes from the probability the model assigns to the "yes" token relative to "no". This is computed for every candidate, and business logic then decides which result is shown first. The speaker says the production prompt is essentially that one yes/no question.

## Part Zero - LLM Ranking is a Different Problem
Generative LLM work has a prefill phase, in which the prompt is processed, followed by a decode loop, in which tokens are generated one at a time through sampling and next-token prediction. Its latency grows with the number of output tokens. Ranking differs in three ways:

- Ranking 50 items means building 50 prompts, one per item, and running them as a single batched prefill.
- All 50 prompts share a long common prefix, made of the query, the system prompt and other context.
- There is no meaningful decode phase. The system needs only the final yes/no token probabilities, not generated text.

The constraints are strict. The speaker calls 10 seconds before showing results unacceptable, the P99 target is under 500 milliseconds later in the talk, and each request must score hundreds of candidates at high concurrency. The optimization approach is organized around three levers: the model, the context and the inference. The speaker's rule of thumb is that tokens are the currency, so the model should be as small as possible, the context as short as possible, and inference as efficient as possible.

### Prefill-only scoring
Because ranking needs one large prefill and a few output probabilities, most of the generation machinery (decode loop, sampling over the full vocabulary) is wasted work. The rest of the talk builds on this observation, and the scoring API described in the inference section exists to remove that waste.

## Part One - Model Optimizations
The goal is a model as small as possible, while keeping quality.

### Multi-teacher distillation
The team started with a teacher model of about 7 billion parameters. It understands context, product policies and query-item relevance well, but it was never meant for production. Its role is to supply high-quality training signals. Its knowledge was distilled into two smaller teacher models, each optimized for a different metric: relevance (how well the result answers the query) and engagement (how likely the user is to click or read). A small language model, called the SLM ranker, was then trained to learn the objectives of both teachers. The speaker says some quality loss is expected and acceptable for the business use case. The model went from the 7-billion-parameter teacher to a 0.6-billion-parameter model.

### Structured pruning
Pruning removes parts of the model, such as layers, MLP layers or attention heads, and then checks whether the quality loss is acceptable. Pruning is not free, so the pruned model must be re-distilled, retrained and fine-tuned to recover quality. The team took the 0.6 billion model, removed some layers, and reached about 400 million parameters, which they judged small enough to serve in production.

## Part Two - Context Optimizations
A job description can be around 2,000 tokens. A request that ranks 100 jobs therefore carries about 100 times 2,000, or roughly 200,000 tokens, in a single pass. This is manageable with thousands of members but not with 1.3 billion, so the team reduced the context.

### Summarized item text
An offline LLM summarizer compresses each job description from about 2,000 tokens to about 150 tokens. This is a 16x reduction in item prompt length, with less than 2% quality loss. All summarization runs offline. The online path only fetches the stored summary from a database and passes it to the ranker. The speaker says the team took prompt engineering seriously, because prompt length is the currency.

### Single-embedding item representation
The team went further and asked whether an item could be represented by one embedding instead of 150 tokens. An LLM takes the full description and compresses it into a single dense vector, so for compute purposes the item costs one token instead of 2,000. This is also computed offline, and the online path only fetches the stored result.

### Throughput versus quality
The speaker plots ranking quality against throughput in items per second per GPU:

- Full job description (baseline): best ranking quality, about 290 items per second.
- Summarized text: about 2,200 items per second, close to 8x to 9x, with less than 2% quality loss.
- Single embedding: about 22,000 items per second, roughly 10x the summarized throughput, and slightly better quality than the summarized text.

🔎 Added context: by the figures as stated, 2,200 divided by 290 is about 7.6x, a little under the 8x to 9x the speaker quotes. The 10x figure for the embedding step is consistent with 22,000 divided by 2,200.

The key takeaway is that the best representation is not the longest one but the smallest one that meets the use case's quality needs. The speaker recommends making model and context tight before any hardware estimate or capacity planning.

## Part Three - Inference Optimizations
Open-source engines such as SGLang, vLLM and TensorRT-LLM already deliver strong throughput for most workloads. The team did not assume that held for its own workload and found bottlenecks in CPU work, inter-process messaging, runtime behavior and the language itself.

### Batch tokenization
Each request has about 50 prompts. The tokenizer ran over them one at a time in a Python for loop on the CPU, which stalled the GPU while the CPU worked serially. The fix was the batch API of the Hugging Face fast tokenizer, which accepts a batch of prompts and parallelizes the work across cores. The team then moved batching to the server side, so that clients which cannot batch do not each have to repeat the client-side logic. Server-side batching groups requests and runs one batch tokenization.

> ❌ **Anti-pattern — per-item tokenization loop:** tokenizing a batch of prompts one by one on the CPU. **Why people do it:** it is the straightforward way to write the loop. **Consequence:** the CPU is serial and the GPU waits for it. **Fix:** use the fast tokenizer's batch API, or batch on the server so each client does not need to.

### Keep batch integrity across processes
The tokenizer runs in one process and the scheduler, which prepares work and dispatches it to the GPU, runs in another. Logs showed the 50-prompt batch being split into smaller batches. Profiling traced this to inter-process communication: the prompts were sent one per ZMQ message. 🔎 Added context: ZMQ (ZeroMQ) is a messaging library commonly used for passing messages between processes. The fix was to send the whole batch in a single message, so the batch reaches the scheduler intact.

> ❌ **Anti-pattern — one message per item across a process boundary:** sending each prompt in its own message. **Why people do it:** it is the default when items are handled individually. **Consequence:** the batch is split, and the GPU receives smaller batches than the request needs. **Fix:** send the batch in one message and keep batch integrity through every process hop.

The speaker's general point is that Python systems built on multiprocessing must pick their inter-process protocol deliberately.

### Scoring API and asynchronous copies
Text generation copies the full vocabulary of probabilities from GPU to CPU and then samples. Ranking needs only the yes/no logits. The team introduced a dedicated scoring API in the open-source engine that skips that work and returns the logits at the end.

After this, production GPU utilization was still poor. PyTorch profiling showed idle gaps between GPU kernel launches. Each yes/no probability was copied from GPU to CPU one request and one prompt at a time, in a synchronous, blocking call, so the scheduler could not launch the next kernel until the copy finished. The fix had two parts: an asynchronous, non-blocking copy, and one batched copy instead of one per item. This let the CPU work for the previous request overlap with GPU work for the next. The speaker warns that a simple `.tolist()` call (he says "dot to list") made the copy blocking.

> ❌ **Anti-pattern — blocking per-item device-to-host copy:** copying each item's result from GPU to CPU synchronously. **Why people do it:** a plain tensor-to-list conversion looks harmless. **Consequence:** the scheduler stalls and GPU utilization drops, with idle gaps between kernel launches. **Fix:** use one batched, non-blocking copy so CPU and GPU work overlap.

### Prefix sharing with an item-aware attention mask
Ranking repeats the query across every item, so the query work is redone for each prompt. The team concatenated all items into one prompt: the query first, then item 1, a delimiter, item 2, and so on. This scores n items in a single pass and computes the query's key-value entries only once. The challenge is that item 2 must not be influenced by item 1, or the scores are wrong. The team therefore added a custom item-aware attention mask inside the attention kernel (the speaker says "flash" in the kernel, presumably a FlashAttention-style kernel), so that each item sees the query and itself but not the other items.

🔎 Added context: the key-value (KV) cache holds the attention keys and values of tokens already processed, so that they do not have to be recomputed; this is why sharing the query's KV entries saves work.

### Python garbage collector spikes
Once the model was fast, production showed P99 latency spikes of about 300 milliseconds every few seconds, against a target under 500 milliseconds. The team ruled out the usual suspects, such as a surge in QPS or one long prompt. The cause was the Python garbage collector. The model server holds many long-lived Python objects created at model load, and the collector kept rescanning all of them to find what to free.

The fix was to start the server, run warm-up requests, and then call `gc.freeze`. This tells the collector to exclude all objects that existed up to that point. Only temporary objects created on the request path are scanned after that, and P99 became stable.

> ❌ **Anti-pattern — letting the collector rescan long-lived objects:** leaving objects created at model load in the collector's scan set. **Why people do it:** it is the default behavior and the cost is invisible until latency is measured at the tail. **Consequence:** periodic spikes of about 300 milliseconds in P99. **Fix:** warm up the server, then call `gc.freeze` before serving traffic.

### The Global Interpreter Lock and process-level parallelism
The Global Interpreter Lock (GIL) lets only one thread execute Python bytecode at a time. The speaker contrasts this with Java, C++ and other languages whose threads can run in true parallel. LinkedIn's model server places the SGLang engine behind a gRPC servicer process. The servicer deserializes the protobuf request, handles observability (logging and metrics), and dispatches the work to an SGLang worker process. At high QPS the single servicer became the bottleneck, so the team ran several servicer processes, each on its own CPU core. The SGLang worker then became the bottleneck, and the fix was again multiple processes. Each worker holds its own model copy and CUDA stream, and the processes time-slice the GPU. This worked because the model was small enough that several copies fit in GPU memory. The speaker's advice is that on Python production systems the GIL may be the single fix that most improves throughput.

## Part Four - Learnings at LinkedIn Scale
### Benchmark what you serve
The team optimized the model, context and inference, ran an offline benchmark, computed how many GPUs were needed, and got leadership's approval. In production, dark traffic delivered far less than the offline numbers. The cause was the benchmark itself: the synthetic load generator sent requests at evenly spaced intervals, while real traffic arrives in bursts and lulls, which deepens queues. The closest substitute for production behavior is to record and replay real traffic. The second-best option is to simulate arrivals with a Poisson process instead of uniform spacing.

🔎 Added context: a Poisson arrival process models requests that arrive independently at a fixed average rate, so random clumps and gaps occur naturally.

> ❌ **Anti-pattern — evenly spaced synthetic load:** driving a benchmark with uniformly spaced requests. **Why people do it:** it is simple to generate and gives a clean average rate. **Consequence:** the measured throughput overstates what production delivers under bursty arrivals. **Fix:** replay recorded production traffic, or use Poisson-distributed arrivals.

### Offline must match online
Prompt engineers, model engineers and the serving team each work with their own code, and the production system uses an open-source engine or an in-house model server with its own defaults. Small defaults, such as whether a tokenization config adds beginning- or end-of-sequence tokens, can change the entire prompt. The speaker requires absolute parity between offline and online, with disparities treated as a gate that blocks any launch.

### No single fix
Throughput has to be squeezed from every place at once: a smaller model, shorter context and more efficient inference. Work on only one of them does not get the system where it needs to be.

### Bring the actors together
Inference engineers, model engineers, prompt engineers and product managers, who define product policy, should meet regularly, for example weekly, in one recurring forum. The speaker says many issues are caught there before production. His conclusion is that the first optimization is removing communication barriers between these roles.

## Questions and Answers
**Single-token embedding.** The speaker uses the LLM as a cross-encoder: the full text goes through one forward pass, and the embedding is taken from the last token, which is expected to carry the context of the whole text. The other option he mentions is averaging the token embeddings. Last-token pooling worked best for the team. Inside an LLM each token is already an embedding that feeds the matrix multiplications, so compressing 2,000 tokens into one item embedding is consistent with how the model works internally.

**Structured pruning.** The method was trial and error in a research setting. The team took the 0.6 billion student ranker, masked certain layers in SFT-style experiments, starting with MLP layers just before the last layer, and measured quality. Quality was recovered through post-training or further training, with reinforcement-learning-based fine-tuning as an additional option.

**Product quality.** The team optimizes first for relevance (the exact person searched for should rank first) and engagement (clicks and reads for jobs). Throughput and cost are addressed only after product gains are confirmed. The earlier ranker was a small MLP that computed something like a cosine similarity. It was faster but gave worse answers. The speaker reports a 5% to 10% increase in the relevance metrics.

🔎 Added context: the transcript says "auction metrics", which is most likely a transcription error for AUC (area under the ROC curve). The speaker's number is about the relevance metrics either way.

**Security and privacy.** Job postings are public and contain no personal data, so they need no masking. For people search, the team masks any information that is not already public on the member's profile, for example a date of birth, before the text reaches the model, so only publicly available information is an input.

**Serving architecture.** People search and job search rolled out first to premium users, and the speaker expects gradual rollout to all users by the end of that year. The model server is one Python application built around a Python gRPC server that embeds SGLang. It is not a set of separate services. This server is the ranker block in the architecture: it takes the query and the candidate list and returns a score for each candidate.

## 🧠 Mental models
### Token is the currency
**The model:** every token in the model or the prompt costs compute and latency, so the design problem is to shrink the model, the context and the inference path together.
**How to apply it:** before planning hardware, ask what each candidate contributes in tokens, and whether that can be computed offline and stored.

### Ranking is prefill-only scoring
**The model:** a ranking request is a batch of prompts that need one prefill and a few output probabilities, with no decode loop.
**How to apply it:** when a generation-oriented serving stack is reused for scoring, remove the generation steps the workload does not need, such as full-vocabulary copies and sampling.

### Smallest representation that clears the bar
**The model:** the best representation is the smallest one that meets the quality the use case requires, not the most complete one.
**How to apply it:** test full text, summaries and single embeddings against the same quality measure, and plot quality against throughput before choosing.

### Benchmark with production-shaped traffic
**The model:** a benchmark is only as good as its arrival pattern. Evenly spaced load hides the bursts that create queues.
**How to apply it:** replay recorded traffic first, and use a Poisson arrival process if that is not possible.

### Parity gate
**The model:** the offline pipeline and the online serving system must produce identical prompts, including tokenizer defaults.
**How to apply it:** make offline-online parity a launch requirement, and test it with the production tokenizer and serving defaults.

## 🔑 Key takeaways
- Ranking with an LLM is prefill-only scoring: 50 candidates become 50 prompts in one batch, and the output is a yes/no probability, so generation-oriented serving assumptions do not hold.
- Summarizing each job from about 2,000 tokens to about 150 tokens cost under 2% quality and raised throughput from about 290 to about 2,200 items per second per GPU.
- Replacing the item text with one embedding raised throughput to about 22,000 items per second per GPU, with slightly better quality than the summaries.
- Python serving stacks hit bottlenecks in per-item tokenization, split batches across processes, blocking GPU-to-CPU copies, garbage collection and the GIL, and each has a specific fix.
- P99 spikes of about 300 milliseconds from garbage collection, against a P99 target under 500 milliseconds, were removed by warming up the server and then calling `gc.freeze`.
- Offline benchmarks with evenly spaced synthetic traffic overstated capacity, so the team recommends replaying production traffic or using Poisson arrivals.
- Offline and online configurations must match exactly, including tokenizer defaults, and any mismatch should block launch.

## 🏛️ For Kenji
Where the request path includes a model, the cost is roughly input size times request volume, so how context is prepared (summaries and embeddings computed offline) is an architecture decision as well as an ML one. The benchmark and parity lessons apply to any latency-critical service, not only to LLM serving. Interpretation, not the author's: a governance or risk review that runs an LLM over many records could face the same trade-off. The question it raises is which quality measure you would accept before replacing full text with a summary or embedding, and who signs off on that decision.

## 📚 Reading plan
### From the article
- [presentations with transcripts](https://www.infoq.com/transcripts/presentations/) — the InfoQ index linked from the transcript, for other talks in the same format.

### To go further
- LinkedIn engineering blog posts and papers on LLM-based ranking — the speaker says the results are described there; titles were not given in the talk.
- The SGLang scoring API and its open-source code — the serving mechanism behind the scoring path described here.
- Python's `gc.freeze` and garbage-collection behavior — the fix for the tail-latency spikes.
- Poisson arrival processes for load testing — the alternative to evenly spaced synthetic traffic.
- Multi-teacher knowledge distillation and structured pruning — the two methods used to shrink the model.
