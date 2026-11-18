---
type: Article Summary
title: Inference engineering for software engineers
description: Moving to open models means choosing a model, provider and deployment, and graduating from multi-tenant APIs to dedicated GPUs.
resource: https://newsletter.pragmaticengineer.com/p/inference-engineering-for-software-engineers
tags:
- ai-ml
- llm-engineering
- inference-tradeoffs
- kv-cache
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://newsletter.pragmaticengineer.com/p/inference-engineering-for-software-engineers
  title: Inference engineering for software engineers
  author: Pragmatic Engineer
status: stable
source_url: https://newsletter.pragmaticengineer.com/p/inference-engineering-for-software-engineers
source: Pragmatic Engineer
source_type: blog
published: '2026-11-17'
domain: ai-ml
subdomain: llm-engineering
concepts:
- inference-tradeoffs
- kv-cache
selected_by: discovery
content: open
complete: 'no'
notion_url: https://app.notion.com/p/41cbaf74172d81578e4ec98970018783
paywalled: true
---

## 🎯 Core thesis
Moving from frontier-lab APIs to open-weight models served by inference providers is a practical engineering path, and it is usually taken in stages: first a drop-in swap to a multi-tenant API, then dedicated GPUs once cost, latency or control demands it.

## 🧭 Why the author wrote it
Companies with large LLM bills are moving to open models to cut AI spend. The article cites Uber cutting per-token costs by 50%, Pinterest by 90% and AT&T by 56%. The author argues that inference engineering is becoming a skill software engineers should understand, and maps the vendor options, the reasons to switch, and the operational problems that come with them. If the thesis holds, the choice is no longer "frontier API or nothing" but a graded decision about model, provider and deployment.

## Opening: two ways to move to open models
The article's introduction names two routes. The first is "bare metal": with time and money, you buy a few dozen NVIDIA GPUs and run them yourself. The article's example is Revolut, which runs 56 separate B200 units to serve its inference needs. Setup is a lot of work, but once it runs, operational cost is as low as it gets.

The second route is an inference provider. Most companies contract with a provider that operates large GPU fleets and makes it easy to onboard and start using open models quickly. The article then lists the challenges of this route: tail latencies, cache hit rates, rate limits and more. It frames these as the core of "inference engineering", a set of practices the author believes software engineers should learn.

The author spoke with three providers, Modal, Fireworks AI and Baseten. The article's roadmap covers: what inference providers are and how they differ from labs; when to switch; how to choose a model, provider and deployment; Stage 1 (multi-tenant API replacement); the signs you have outgrown it; Stage 2 (dedicated GPU nodes); and speculative decoding.

> 🔎 Added context: "Multi-tenant" means many customers share the same endpoint and hardware. "Dedicated" means capacity reserved for one customer.

### Bare metal
Owning or leasing the GPUs yourself. High fixed effort, lowest marginal cost per token once running.

### Inference engineering
The practices of running LLM workloads reliably and cheaply: latency, caching, capacity, rate limits and deployment choices. The article presents it as increasingly useful for software engineers, not only for ML specialists.

## 1. What are inference providers & how do they differ from AI labs?
### Inference provider
An inference provider sells access to LLMs that it runs on users' behalf. From the outside it looks much like OpenAI or Anthropic: an API endpoint, token streaming, and payment by token usage. The difference is the model behind the endpoint. Inference providers run open-weight models, while frontier labs train their own frontier models. Providers also run similar GPU infrastructure to frontier labs.

### Three vendor strategies
- **Modal** sees itself as a cloud infrastructure provider whose offering is programmable through its SDKs. It supports inference natively through those SDKs and leans on the open-source ecosystem for efficient inference. Performance tweaks are merged upstream. Richard Gong, Inference Lead at Modal, says: "when you run code on Modal, you should be able to see all the code that's running."
- **Fireworks** positions itself as a provider for specialized AI workloads. Most of its business is dedicated deployments, often including fine-tuning and latency guarantees. Fireworks also offers forward-deployed researchers to help post-train custom models. Its CTO, Dmytro Dzhulgakov, says: "our tagline is specialized intelligence, which means we will tune everything from model to infrastructure for your particular use case."
- **Baseten** describes itself as an all-in-one place to build agents. It starts with an infrastructure layer offering uptime guarantees, flexible commercial terms and no vendor lock-in. It then adds layers for agentic workloads on top of inference: sandboxes, governance services and self-serve post-training.

### Why providers are not interchangeable
To customers, providers offer LLM APIs that can act as drop-in replacements for frontier labs' APIs. The article lists four differences:
- **No pre-training.** A single pre-training run for a frontier-level LLM can cost millions of dollars, so only hyperscalers and specialized frontier labs do it. Providers post-train open-weight models instead, because that is significantly cheaper.
- **Competitive pricing.** The main business of providers is offering open-weight models at competitive rates.
- **Dedicated hardware.** A big part of the appeal is dedicated hardware, with the ability to work with the vendor at the infrastructure layer.
- **Efficiency and customization.** Providers offer similar commoditized open models and compete on efficiency and customization services.

As an example of efficiency, the article cites Fireworks' Ember-1 model, announced in September 2026. It is post-trained on the Kimi K3 foundation and delivers similar quality with 40% fewer reasoning tokens. That gives Fireworks a cost edge over vanilla Kimi K3.

### Frontier lab uptime
The article notes that frontier labs' API uptime falls short of cloud-provider standards. Self-declared figures cited are 99.5% for Anthropic, 99.97% for the OpenAI API and 99.2% for ChatGPT. The article contrasts these with the four nines (99.99%+) that cloud providers such as AWS are known to uphold. Competitors can win share on this gap.

The article also points out that the uptime figures do not count degraded performance as downtime. Because all these APIs are multi-tenant, a degradation hits some tenants without them being able to see it or act on it. For a 99.99% service, the author argues, you cannot rely on one frontier provider for the critical path.

### Reliability: gateways with fallback
Inference providers also go down, so many companies run an LLM gateway with fallback logic to another provider or to a frontier API. The article points to a deep dive on Ramp, whose LLM router project began with this exact problem.

## 2. When to switch from frontier labs?
The article says moving to an independent provider is usually driven by three factors: cost, control and the need for customization.

### Most common reason: cost
Many companies start looking at providers because of pressing cost concerns. A frontier model is often more capable than a given request needs, so a cheaper model can be just as good for simpler tasks, and few people would notice the difference. In such cases it is a "no-brainer" to route some requests to a cheaper model hosted by a provider.

**Smart routing** is the obvious way to cut costs. A "smart router" is an LLM router that is prompt-aware. It sends each request to the most cost-efficient provider that can solve it. The article says this approach fuelled Uber's roughly 50% cut in per-token costs.

Providers offer OpenAI- or Anthropic-compatible endpoints, so switching is trivial: point your app at the new endpoint, which is operated by a smart router.

The hard part is deciding which model to serve. Generic benchmarks are often insufficient because they rarely reflect a team's own traffic patterns. Most providers ask for prompt samples or eval sets to judge which model fits. Using a new endpoint feels like using a frontier lab's API, with per-token charges but no response-time SLA.

> ❌ **Anti-pattern — choosing a model from generic benchmarks:** picking a model because it scores well on public leaderboards. **Why people do it:** 🔎 public scores are the easiest comparison to get. **Consequence:** the scores do not represent your traffic, so a model that looks good on paper can underperform on your workload. **Fix:** build a private eval set from your own prompts and traces, or run an A/B test with real users.

### More control over latency
With a frontier lab's endpoint there is little control over response latency or model availability. A dedicated setup with a provider gives control over both:
- **Latency:** response times are more predictable, because the app has dedicated capacity that scales with its usage.
- **Model availability:** you run the model you choose on your own GPUs and keep it as long as you need. This avoids the risk of a lab changing models overnight.

### Fine-tuning and customization
Dedicated hardware with open-source models makes it possible to work with a provider to fine-tune quality, latency or throughput. Richard Gong says around 10% of Modal's customers go this far, which he describes as a sign that customization is for "power users". The article names two typical scenarios:
- **Tweaking a smaller model.** You reach similar quality with fewer parameters, using performance tweaks that off-the-shelf models do not offer. A post-trained small model can outperform the original in your specialized scenario.
- **Niche use cases.** You improve quality where general-purpose models are not good enough, often because the data is not publicly available. The Fireworks team says legal and medical projects tend to benefit.

## 3. Choosing a model, provider, and deployment strategy
Most providers offer the same models, so the differences lie in performance and quality. Quality can differ by default context length, since some providers cap it below what the model supports. Quantization (compressing weights to lower numerical precision) and default temperature (how random outputs are) also matter. The article's practical approach has three decisions, in order: the model, the provider (based on quality, performance and cost), and the deployment model (multi-tenant service or dedicated GPU nodes).

### Choosing a model
The most obvious approach is to run an eval set against the candidate models. Satya Nadella, Microsoft's CEO, is quoted in the article as arguing that private evals will become a major part of tech companies' intellectual property:

> "Public benchmarks are great, but your private evals will matter more: you need your own eval for the outcome that matters to you. Then you look at performance across all these models and continuously optimize. Our customers start with a frontier model, then say: 'I've got all those traces. This is a deterministic workflow, so I don't need the next big model drop. I want to optimize the cost.' They drop in [another] model, use the traces to fine-tune it – and there you have it."

The article uses Artificial Analysis as a public example. It maintains an eval set comparing the cost of generic tasks across models. For a team building on Anthropic's Sonnet 5.5, the average cost is $5.46 per task in that index. If Kimi K3 is sufficient, it is about 60% cheaper at $2.00 per task. The article stresses that this is cost per task, not per token. Models need different numbers of tokens for the same task, so per-token prices mislead.

The article adds that a public score is not enough to decide a model is good enough. Ideally you have an eval set to compare against. Without one, an A/B experiment works. For example, you could let some engineers use the new model in agentic coding workflows and collect their feedback.

### Choosing a provider
Providers tweak open models to gain performance without degrading quality too much, so outputs vary between them. The article recommends Artificial Analysis' endpoint accuracy index. It scores each provider's results against Artificial Analysis' own self-hosted version, where 100% means identical results. Provider scores range from 93% to 102%. Minor differences fall within the same level as the reference, which is why bars technically below 100% are shown green. Because LLMs are non-deterministic, the measurement has a 95% confidence interval, which adds another layer of complexity to evaluation.

### Deployment model: multi-tenant or dedicated?
All providers offer a multi-tenant API and dedicated GPUs. Most deployments follow a graduation path: they start as drop-in replacements for a frontier API and move to dedicated infrastructure as usage grows.

Switching to an open model via a provider lowers the per-token cost. You are still in a multi-tenant environment, though, with the same kind of performance bottlenecks as a frontier API. With a dedicated deployment you control performance, and you can fine-tune the model through techniques such as post-training.

### The typical graduation path
For a startup using LLMs, the article describes this path:
1. Use a frontier lab's API endpoints.
2. Move to an open model through a provider's API endpoint.
3. Rent dedicated GPUs from the provider.

Moving to a provider usually coincides with product-market fit. All three providers said that by the time a customer reaches Stage 1, the product has traction. Per-token costs matter only at non-trivial levels of user activity. Stage 2, dedicated GPUs, implies significant budget commitments, a more mature product and large-scale usage.

### GPU capacity and commitments
Getting inference capacity can be hard because of the global GPU shortage. The article links to an earlier piece on CPU shortages inside cloud providers, and says the GPU shortage is even more pressing. Even large providers struggle to get the GPUs they need. The providers interviewed run on several cloud providers, and some also manage their own bare metal, to get as much capacity as possible. Running across clouds adds operational complexity, but it is worth it for access to more GPUs. Quickly onboarding a new cloud with spare capacity is becoming a key differentiator.

Dedicated GPUs will probably be expensive. Scarce capacity usually means long-term budget commitments upfront: you pay more, and commit for longer.

## 4. Stage #1: API replacement, using a multi-tenant inference setup
### Replacing frontier APIs in coding tools
For teams with large LLM bills, the easiest cost cut is to replace frontier models in tools such as Claude Code, Codex and OpenCode with a frontier-level open model. The article points to OpenCode's data and the OpenRouter rankings. The popular choices it names are DeepSeek 4.1 Flash, GLM 5.3, and others including Kimi K3.1 Pro.

The easiest method is to point the application at the provider's API. In practice that means a multi-tenant API, where other customers hit the same endpoint, as with a frontier lab. At first it works well. Then you may run into time-to-first-token (TTFT) latency, caching costs and other latency issues. The article notes these are also problems with frontier-lab APIs, but with a provider you can solve most of them by moving to a dedicated deployment.

### Latency: TTFT and inference time
Time-to-first-token is how long it takes from the moment a request hits the LLM API until the client receives the first tokens. The article's point is that TTFT has a long tail. A long right tail means that a significant share of requests take double the median latency or more. So p95 and p99 latencies matter as much as p50.

TTFT can be measured in two places:
- **Inference time:** provider-side TTFT shows pure inference time. Some providers expose it on dashboards or in responses. Frontier labs typically do neither.
- **User-experienced latency:** client-side TTFT includes the full path, including network transport and the TLS handshake.

Server-side inference is only part of the time a user waits. To get fast p95 user latency, the article suggests optimizations on the consumer side, such as connection pooling.

### Why TTFT varies
Providers run many copies of the inference engine, and one engine can serve several requests at once. A new replica is created when concurrent requests exceed current capacity. Replica scaling is one reason inference-only TTFT varies so much. The article lists other causes:
- **Noisy neighbours:** your request is batched with a more expensive request, so it takes longer than it would with a similar one.
- **Cold start:** a request waits in a queue because no replica has capacity. A new replica is starting, which is not instant.
- **Cold cache:** a large request lands on a replica without the relevant KV cache loaded (a "cold" cache, the opposite of a "hot" one). Providers try to keep requests sticky to the same replica. That fails when your cache expired or the request landed elsewhere. The KV cache must then be copied from cold storage if available, or fully recomputed before the first token.

The article says prompt-cache utilization is observable and actionable. OpenAI's completions API returns cache usage metrics in its responses. Building apps that exploit the prompt cache is a key way to improve performance.

### Caching: cost and latency (partial)
The article's preview ends here. It starts to explain that most major LLM API endpoints are stateless REST APIs, modeled on OpenAI's chat completions API. Because most LLM use cases are stateful, this creates problems for caching. Dmytro Dzhulgakov, Fireworks' CTO, calls the decision to make the API stateless "the original sin". The client behaves as if it talks to a stateless API, but the provider must then ensure efficient caching in a distributed environment.

> 🔎 Added context: The available text stops partway through this section. The rest of the caching discussion, Stage 2 with dedicated GPUs, and speculative decoding are not covered in this note.

## 🧠 Mental models
### Stage 1 to Stage 2 graduation
**The model:** Adoption moves in steps. You start with a frontier API, move to a provider's multi-tenant endpoint, and only later rent dedicated GPUs once you have traction and budget.
**How to apply it:** Match your infrastructure commitment to your usage. Per-token savings matter first, and dedicated capacity comes once volume and product maturity justify the commitment.

### Smart routing
**The model:** Not every request needs the most capable model. A prompt-aware router sends each request to the cheapest provider that can handle it.
**How to apply it:** Measure which requests a cheaper model handles acceptably, then route only those to it, keeping a fallback path for the rest.

### Cost per task, not per token
**The model:** Models use different numbers of tokens to finish the same task, so per-token prices can mislead.
**How to apply it:** Compare candidate models on the cost of completing your real tasks, using your own eval set.

### Tail latency matters more than the median
**The model:** TTFT has a long right tail, so the slowest requests define how the app feels for some users.
**How to apply it:** Track p95 and p99, measure from the client, and reduce client-side overhead such as connection setup.

## 🔑 Key takeaways
- Open models served by inference providers can cut per-token costs sharply, with the article citing reductions of 50% (Uber), 90% (Pinterest) and 56% (AT&T).
- The three reasons to switch from frontier labs are cost, latency and availability control, and customization.
- Use your own eval set and traffic patterns to choose a model. Public benchmarks do not capture your workload.
- Provider endpoint accuracy varies from 93% to 102% of a reference, so compare providers on quality as well as price.
- Multi-tenant APIs bring TTFT tail latency, cold starts and cold caches. Dedicated GPUs fix most of these, but they need long-term commitments and scarce capacity.
- Frontier-lab uptime figures (99.5% to 99.97%) fall short of four nines, and degraded performance is not counted as downtime. Plan fallbacks for critical paths.

## 🏛️ For Kenji
Your interpretation, not the author's: the article's three-stage path maps well onto an architect's decision record. Model choice, provider choice and deployment model are separate decisions that should each have an owner and an evaluation criterion, and an LLM gateway with fallback is a natural place to put governance controls. A question it raises for your AI governance work: how do you evaluate and audit a model that a provider has quietly tuned, when the endpoint accuracy score is only a 95% confidence measure?

## 📚 Reading plan
### From the article
- [Modal](https://modal.com/) — the first provider profile, with its view of programmable infrastructure.
- [Fireworks AI](https://fireworks.ai/) — the specialized-workloads provider, useful for the fine-tuning discussion.
- [Baseten](https://www.baseten.co/) — the all-in-one agent platform, for the infrastructure-to-agents layering.
- [Revolut's talk at AI Engineer NYC](https://ai.engineer/nyc/2026/talks/revolut) — how a bank runs its own GPU fleet, the bare-metal option.
- [Fireworks' Ember-1 announcement](https://fireworks.ai/blog/ember-1) — the post-training example behind the 40% fewer reasoning tokens claim.
- [Artificial Analysis](https://artificialanalysis.ai/) — the per-task cost index and the endpoint accuracy index.
- [OpenRouter rankings](https://openrouter.ai/rankings#top-models) — which open models engineering teams actually use.
- [Ramp's LLM router deep dive](https://newsletter.pragmaticengineer.com/p/why-ramp-built-inspect) — a real gateway built around the fallback problem.
- [Model quantization concepts](https://developer.nvidia.com/blog/model-quantization-concepts-methods-and-why-it-matters/) — NVIDIA's primer on the precision trade-off.

### To go further
- Private evals as IP — the Satya Nadella interview discussed in the article, on why your own evals will matter more than public benchmarks.
- Speculative decoding — the article's final topic (not in the available text), where a cheap draft model proposes tokens for the main model to verify.
- Prompt caching and stateless LLM APIs — the caching discussion the article continues, worth reading alongside your own cache metrics.
