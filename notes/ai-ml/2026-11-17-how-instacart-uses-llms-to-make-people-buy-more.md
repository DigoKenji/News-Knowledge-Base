---
type: Article Summary
title: How Instacart Uses LLMs to Make People Buy More
description: Combining historical data, LLM interpretation and guardrails, then distilling into a cached small model, makes shopping query understanding both accurate and fast.
resource: https://blog.bytebytego.com/p/how-instacart-uses-llms-to-make-people
tags:
- ai-ml
- llm-engineering
- rag
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://blog.bytebytego.com/p/how-instacart-uses-llms-to-make-people
  title: How Instacart Uses LLMs to Make People Buy More
  author: ByteByteGo
status: stable
source_url: https://blog.bytebytego.com/p/how-instacart-uses-llms-to-make-people
source: ByteByteGo
source_type: blog
published: '2026-11-16'
domain: ai-ml
subdomain: llm-engineering
concepts:
- rag
- inference-tradeoffs
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d810d88d9cad76f39ce7c
---

# How Instacart Uses LLMs to Make People Buy More

**Source:** https://blog.bytebytego.com/p/how-instacart-uses-llms-to-make-people
**Author:** ByteByteGo
**Date:** 2026-11-16
**Concepts:** [Retrieval-augmented generation](/concepts/ai-ml/rag.md) · [Inference throughput/latency/cost](/concepts/ai-ml/inference-tradeoffs.md)

---

## 🎯 Core thesis
Instacart improved product search by combining three steps, historical purchase evidence to propose candidates, LLM interpretation grounded in catalog context, and embedding-based guardrails to filter the result, and then distilled the slow large-model pipeline into a cached plus fine-tuned small model so that live search stays fast.

## 🧭 Why the author wrote it
Shoppers type loose phrases such as "bread no gluten" that rarely match catalog wording, and the search system has to turn them into categories, alternative queries and structured tags before retrieval and ranking can work. Instacart's older setup used several separate models built on noisy behaviour data, cost a lot to maintain and still interpreted the same query inconsistently. If the thesis holds, one LLM-based interpretation layer can serve all of these search tasks, as long as it can meet live latency limits.

## The Problem With Instacart's Initial Approach

The article splits product search into three activities: query understanding (QU), retrieval and ranking. QU reads what the user typed, retrieval finds a set of relevant products, and ranking decides the order in which those products are shown. The article's key point is that QU's output drives the other two stages, so a phrase like "bread no gluten" should be read as one shopping intent with a gluten-free attribute and not as three unrelated words.

### Query understanding (QU)
QU is the step that infers the shopping intent behind the words in a search box. At Instacart it can produce three kinds of information: product categories that show which parts of the catalog matter, alternative queries that might find suitable products, and structured tags for the product, brand or specific attributes. Retrieval uses these outputs to gather candidates and ranking uses them to order those candidates. The Instacart team already had machine learning systems for this, but the difficulty was making them behave consistently across the huge range of things people search for.

### Supervised labels from behaviour
A supervised model learns from examples paired with the correct answer, so classifying a query would require a label that says which category the query belongs to. Instacart can observe searches, clicks and purchases, but none of these directly shows whether the system understood the query correctly. A shopper who searches for "bread" and buys bananas does not make "bread" a fruit query, so the link between query and purchase is noisy. Human reviewers give cleaner labels, but they cost time and money.

> ❌ **Anti-pattern — Learning labels straight from behaviour:** Treating clicks and purchases as the correct answer for a query. **Why people do it:** It is the data that is already logged and free to use. **Consequence:** The model learns the noise in shopping behaviour, such as bread leading to bananas, as if it were intent. **Fix:** Use behaviour only as evidence for candidates, then have a separate interpretation and validation step, as the article does later.

### The long tail
Rare, very specific queries such as "2% reduced-fat ultra-pasteurized chocolate milk" have little or no purchase history, so a system that learns mainly from past clicks and purchases has too few examples to handle them. Each such query may account for very little traffic, but taken together the long tail may represent a large set of customer needs. The article returns to this group later when it measures the results.

### Ambiguous broad queries
A broad query such as "healthy food" can point to many catalog categories at once. The article's point is that the system should handle that ambiguity instead of forcing the query into one narrow interpretation. This is one reason a flat, single-label approach falls short.

> ❌ **Anti-pattern — Forcing a broad query into one interpretation:** Choosing one category for a query whose meaning is wide. **Why people do it:** A single label is simpler to train, evaluate and display. **Consequence:** Relevant products from the other interpretations never reach retrieval. **Fix:** Return several candidate categories and let later stages rank or filter them.

### Maintenance cost of separate models
Instacart ran different systems for different jobs. A category classifier used a FastText model, and the query rewrite system mined patterns from user sessions. Each system had its own data processing, training and deployment requirements, which increased maintenance work. Even then, the systems gave inconsistent interpretations of the same query.

> ❌ **Anti-pattern — One model per search task:** Building a separate model, data pipeline and deployment for each part of query understanding. **Why people do it:** Each task looks narrow, so a specialised model seems efficient. **Consequence:** Maintenance grows with every task, and the outputs can contradict each other for the same query. **Fix:** Move the tasks onto one shared foundation, which is the direction the Instacart team chose.

## How a General LLM Becomes Useful for Instacart

A large language model starts with broad knowledge of language and the general world. That is why it can make sense of queries it has never seen in the historical data. The article's example is that an LLM may already know that "Italian parsley" and "flat parsley" are the same thing and that "curly parsley" can substitute for them, without needing examples to learn this.

General knowledge is not the same as knowing a particular store's catalog and customers, so Instacart used three techniques to close that gap.

### Pretrained general knowledge
This is the starting point the article describes. The model's training gives it a general sense of how products relate to each other, which helps with unfamiliar phrasings. The article is clear that this knowledge does not tell the model anything about one retailer's inventory or shopper behaviour, so the company had to add business context on top.

### Context engineering and RAG
The first technique puts the relevant business information into the prompt before the model is asked for a prediction. The article calls this context engineering, and RAG (retrieval-augmented generation) is the main method used. The system pulls catalog records and historical patterns and passes them to the model together with the query. In the article's words, RAG here means giving the model all the useful business information before asking it to predict.

### Post-processing validation
The second technique checks the model's output. Instacart added guardrails that test how suitable an output is and how well it fits the product catalog. A model can produce a plausible answer that still does not match any real product, so validation is kept as a separate part of the system.

> ❌ **Anti-pattern — Trusting plausible output:** Accepting whatever category or tag the model produces because it sounds right. **Why people do it:** Language models are fluent, so their answers look reliable. **Consequence:** Outputs that do not match real catalog items reach retrieval or ranking. **Fix:** Add a validation layer that checks each output against the catalog and a relevance score, which the article describes.

### Fine-tuning smaller models
The third technique is fine-tuning, meaning extra training that changes the model's parameters. Instead of repeating all the domain guidance in the prompt on every call, the team teaches a model to perform a particular task from high-quality examples. The article presents this as the route to a model that is smaller and faster while still carrying Instacart's domain knowledge. The later sections describe how this was done.

> ❌ **Anti-pattern — Repeating all domain guidance in every prompt:** Putting every catalog rule and example into each request. **Why people do it:** It is the quickest way to add knowledge without any training. **Consequence:** Each request carries more context, which adds cost and latency to every call. **Fix:** Teach the domain behaviour to a smaller model with high-quality examples, which the article attributes to fine-tuning. *(The article does not state this as an anti-pattern; it is the implied contrast in its wording.)*

## How Instacart Knows What the Query Is About

Instacart's catalog is organised as a hierarchy of categories. The article's example path is Meat → Beef → Beef Ribs → Short Ribs. Each step in the path makes the category more specific, so if the search system knows the whole path, it can identify the right part of the catalog precisely.

### Hierarchical category paths
A category path encodes the relationship between a broad group and its narrower members, which a flat list of labels does not. The path is the unit the system uses to decide which part of the catalog a query refers to. In the new approach the path is treated as one prediction and is not split into separate labels.

> ❌ **Anti-pattern — Flat category labels:** Predicting each category as an independent label with its own confidence score. **Why people do it:** Independent multi-label classifiers are simple to build and evaluate. **Consequence:** The relationships between categories are lost. For "buttermilk", the earlier classifier might predict "Dairy" and "Milk" separately, which is useful but incomplete. **Fix:** Predict the full category path so the hierarchy is part of the answer.

### Candidate proposal and LLM re-ranking
The new approach works in two parts. First the system finds the leading categories that have historically followed the query in purchases. These become the candidate set, which links the model to Instacart's own data. Because purchase behaviour can be erratic, the article then asks the LLM to assess those candidates. The LLM re-ranks them using the query and the Instacart context it was given. For example, "buttermilk" might map to Dairy, then Milk, then Buttermilk, while "vegan roast" might map to Meat, then Meat Alternatives. This shows how language understanding helps when the query's wording does not match any category name.

### Relevance guardrail with embeddings
After the LLM makes its prediction, a guardrail compares the meaning of the original query with the predicted category path. Both are converted into embeddings, which are numerical representations of text, and the system measures their similarity. If the score falls below a set threshold, the pairing is discarded. The guardrail is what removes predictions that are fluent but unrelated to the query.

### Division of responsibilities
The article summarises the three stages as having different jobs. Historical data proposes candidates, the LLM interprets them, and the guardrail filters out irrelevant results at the end. Each part can then be judged on the job it does, which makes the system easier to reason about than one opaque model.

## How Instacart Makes The Queries Stronger

Even a query that is understood well can retrieve very few useful products if its original words are used alone. Query rewriting addresses this by generating alternative phrases that help the search find more candidates.

### Query rewriting
The older rewrite system used patterns from user sessions, but it covered only about 50% of search traffic. The team first tried asking an LLM for rewrites with one general prompt, and this did not work as expected. The reason, according to the article, is that a general instruction to rewrite does not say what kind of alternative would be useful. A rewrite of "1% milk" to "one percent milk" keeps the meaning but gives the shopper nothing new to find.

> ❌ **Anti-pattern — One generic rewrite prompt:** Asking the model to rewrite the query without saying what kind of rewrite is wanted. **Why people do it:** A single prompt is the simplest design. **Consequence:** The rewrites are often equivalent wording and add little retrieval value. **Fix:** Split the task by the kind of change wanted, with a dedicated prompt for each.

### Synonym, broader and substitute rewrites
Instacart separated rewriting into three tasks, and the article explains why the difference matters:
- **Synonym rewrites** express the same request with different wording. "One percent milk" is the example, and the meaning stays intact.
- **Broader rewrites** remove some specificity, so "milk" is a broader rewrite of "1% milk". This relaxes the constraints.
- **Substitute rewrites** suggest a different product that could work as an alternative. "2% milk" is the example, and this changes the product requested.

Each type gets its own prompt with specific instructions, examples and reasoning guidance. After generation, the team applies post-processing checks and semantic filters. The result was rewrite coverage above 95%, with precision above 90% across all three types.

> 🔎 Added context: Precision here means the share of generated rewrites that are judged correct. Coverage means the share of queries for which at least one useful rewrite is produced.

## Extracting Search Tags from the Query

The third task is semantic role labeling (SRL), which pulls structured concepts such as product, brand and other features out of a shopping query. For "2% chocolate milk", the product is milk, and the fat content and flavour are the other features. Once these concepts are explicit, the search process can use them more easily.

Instacart uses these tags for retrieval, ranking, advertising and filters. The main challenge is to get accurate tags without making every search slow and expensive.

### Head queries and caching
Search traffic is unevenly distributed. A small set of frequent queries, called head queries, accounts for a large share of traffic, while a much larger set of uncommon or previously unseen queries makes up the rest. For common queries, Instacart can generate tags in advance and store them in a cache, so later searches can use them immediately.

### Offline teacher pipeline
Precomputing is not enough, because the number of possible phrases is effectively unlimited and new combinations keep appearing. The article therefore uses a second component: an offline teacher pipeline and a smaller model that runs during live search. "Offline" means the work happens outside the live request the user is waiting for, so the pipeline can use a powerful model and richer context. For a common query, it finds relevant information in internal systems, supplies that to the LLM, generates tags and validates them against the catalog.

The article's example is "verdant machine". Without business context, an LLM might read this as machinery. Instacart's internal evidence links it to a smoothie brand called MuchPure and to smoothie and juice categories. The validated output serves two purposes: it fills the cache for common queries, and it becomes training data for the smaller model.

> ❌ **Anti-pattern — Precomputing tags for every possible phrase:** Trying to cache the output for all queries in advance. **Why people do it:** A complete cache would make live inference unnecessary. **Consequence:** The cache cannot keep up with new combinations of words, so uncached queries still need a fallback. **Fix:** Cache the head queries and keep a fast model for the rest. *(The article states the limitation; the "anti-pattern" label is this summary's framing.)*

### Online student model
When a live query arrives, the system checks the cache first and returns the stored tags if they exist. If they do not, the query goes to the smaller fine-tuned model. The article describes this model as a student that has learned from teacher-generated examples, so it has picked up how queries map to structured tags, including patterns from Instacart's domain.

## Teaching the Smaller Model

For its real-time SRL system, Instacart fine-tuned Llama-3 8B, a model with about 8 billion parameters. Parameters are the numerical values a model learns in training. Although 8 billion is large, the model is much smaller than the frontier model used by the teacher pipeline.

### LoRA (Low-Rank Adaptation)
Instacart trained the model with LoRA. In LoRA the original model weights stay fixed, and the model learns a comparatively small set of additional parameters. This reduces the training work needed to adapt the model to the task. The training examples all come from the offline pipeline.

### Knowledge distillation
The article describes this as a form of knowledge distillation: a smaller model learns from outputs produced by a more capable teacher model. The result was a student that produced fewer incorrect tags but missed slightly more correct ones. The article presents this as the trade-off of the approach, which the team accepted to get a model that could serve live traffic.

## How Instacart Made the Model Fast For Live Search

Live search has to respond quickly, because long delays are visible to shoppers on a high-traffic site. The team targeted model-serving latency in the low hundreds of milliseconds. The article's first measured setup took nearly 700 milliseconds on an A100 GPU. Later, the article states a 300-millisecond target, which the team reached.

> 🔎 Added context: The A100 and H100 are NVIDIA data-centre GPUs, and the H100 is the newer generation. The article does not give separate timings for each change, so only the combined result is reported here.

### Merging the LoRA adapter
After training, the LoRA adjustments can be combined into the base model's weights. This removes the need to keep the adapter as a separate computation during inference. According to Instacart's engineering team, this merge reduced latency by about 30%. Combined with an upgrade to H100 GPUs, it reached the 300-millisecond target. The total gain came from both the way the model was served and faster hardware.

> ❌ **Anti-pattern — Serving the adapter unmerged (implied):** Keeping the LoRA adapter as a separate step at inference time. **Why people do it:** It is the default output of LoRA training and is easy to keep swappable. **Consequence:** Each request does extra computation, which adds latency. **Fix:** Merge the adapter into the base weights before deployment. *(The article reports the merge's effect; the anti-pattern label is an inference from that result.)*

### FP8 quantization
Quantization uses lower-precision numbers for model computation. The team tested FP8 quantization, which cut latency by another 10% but slightly reduced recall. Instacart deployed the unquantized model because it prioritised higher-quality results. Although a faster model was available, the team chose quality, and the article presents this as a deliberate engineering trade-off.

### Selective real-time inference
The team also reduced how often live inference is needed. Caching had a large effect on overall cost, and according to the team's estimate only 2% of queries required real-time model inference. The smaller model therefore handled a limited share of traffic while cached tags covered the common cases. GPU autoscaling then ran fewer GPUs during quieter periods.

## Conclusion

Instacart evaluated the real-time SRL system with an A/B test. For the tail queries discussed earlier, it reported a 6% reduction in average scroll depth. The article reads this as users finding the right items more quickly.

The system was handling millions of cold-start queries each week. These are queries with no query-specific history and no precomputed answer. For tail queries, user complaints about poor search results fell by 50%, with only a marginal latency increase. The article presents these results as connecting the problems to the solutions: better classification, rewrites and tags help shoppers find relevant products more easily.

The article ends with the next goal. Instacart aims to understand a shopper's broader journey, and the article says the current design gives a strong foundation for that.

> 🔎 Added context: Scroll depth is how far down the results a shopper scrolls on average. The article takes a lower value as a sign that the right items appeared sooner.

## 🧠 Mental models
### Division of labour: history proposes, LLM interprets, guardrail filters
**The model:** Each stage in the query pipeline has one job. Historical data supplies candidates, the LLM ranks and explains them in context, and an embedding check removes irrelevant results.
**How to apply it:** When designing an LLM feature, decide which component proposes options, which one judges them, and which one enforces the hard constraint.

### Teacher offline, student online
**The model:** A large model with rich context produces high-quality outputs outside the live request. A small model trained on those outputs answers the live requests.
**How to apply it:** Put expensive reasoning in batch jobs whose output is validated and stored, and reserve the live path for a model that meets the latency budget.

### Cache the head, model the tail
**The model:** Frequent queries cover most traffic, so their results can be precomputed and cached. The rare long tail needs a model that works without any prior result.
**How to apply it:** Measure the traffic distribution first, then decide which part of the workload is worth caching and which needs live inference.

### Rewrite type should match the change you want
**The model:** A synonym keeps the intent, a broader rewrite relaxes the constraints, and a substitute changes the product. Each one affects retrieval differently.
**How to apply it:** Write a separate prompt and a separate check for each kind of change, instead of one general rewrite instruction.

### Trade quality for latency only on purpose
**The model:** A faster variant such as FP8 quantization can reduce latency but cost some recall, and the cost is only acceptable if it is a conscious decision.
**How to apply it:** Measure both latency and quality for each optimisation, and record which quality loss the team has accepted.

## 🔑 Key takeaways
- Behaviour logs such as clicks and purchases are noisy evidence, so Instacart uses them to propose candidate categories rather than as the final label.
- Predicting the full category path, not separate flat labels, keeps the relationships between categories, as in Dairy, then Milk, then Buttermilk.
- Splitting rewriting into synonym, broader and substitute types raised rewrite coverage above 95% with precision above 90% across all three.
- A teacher pipeline with catalog-validated outputs fills the cache and trains a fine-tuned Llama-3 8B student that handles the uncached queries.
- Merging the LoRA adapter cut latency by about 30%, and with H100 GPUs the system reached the 300-millisecond target.
- Only about 2% of queries needed real-time inference, so caching carried most of the cost savings.
- The A/B test on tail queries showed a 6% reduction in average scroll depth and a 50% reduction in complaints about poor results.

## 🏛️ For Kenji
This is your interpretation, not the author's. The architecture fits a governance-minded design: an LLM proposes and explains, deterministic validation and an embedding guardrail enforce constraints, and the online path is a smaller model distilled from validated outputs. For a Staff Architect, the useful question is what evidence you would need to show that the validation layer still matches the catalog after it changes, and how you would detect drift in the student model's behaviour against the teacher's outputs.

## 📚 Reading plan
### From the article
- [Building the Intent Engine](https://tech.instacart.com/building-the-intent-engine-how-instacart-is-revamping-query-understanding-with-llms-3ac8051ae7ac) — the original Instacart Tech Blog post, which the article cites as its source for the system details.
### To go further
- LoRA: Low-Rank Adaptation of Large Language Models — the method behind the fine-tuning and adapter merge described in the article.
- Distilling the Knowledge in a Neural Network — the origin of the teacher-student idea the article applies to tag generation.
- Retrieval-augmented generation — background on pulling business context into prompts, the article's primary method for domain grounding.
