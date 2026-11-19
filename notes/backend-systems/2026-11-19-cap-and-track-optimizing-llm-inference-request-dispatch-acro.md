---
type: Paper Summary
title: 'Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity'
description: Refusal-driven dispatch with per-replica caps keeps LLM fleets busy as capacity shifts, serving a classifier on 43% fewer H100s.
resource: https://arxiv.org/abs/2611.24891
tags:
- backend-systems
- adaptive-concurrency-limit
- inference-tradeoffs
- kv-cache
- prefill-decode-disaggregation
- retry-storm
- goodput
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.24891
  title: 'Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity'
  author: arXiv cs.AI/cs.LG/cs.CL (top 3 by relevance)
status: stable
source_url: https://arxiv.org/abs/2611.24891
source: arXiv cs.AI/cs.LG/cs.CL (top 3 by relevance)
source_type: arxiv
published: '2026-11-18'
domain: backend-systems
concepts:
- adaptive-concurrency-limit
- inference-tradeoffs
- kv-cache
- prefill-decode-disaggregation
- retry-storm
- goodput
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d817a8222d1d1816ddf60
---

## 🎯 In one sentence
Cap-and-Track keeps LLM inference fleets full as capacity shifts by refusing excess work at each saturated replica and letting a dispatcher follow those refusals, and in a same-traffic production A/B it served a classifier on 43% fewer H100s than its precursor.

## ❓ The problem
Two quantities move under an LLM serving system. Demand is the work callers send, which swings with daily cycles and bulk backfills, and each request's output length is unknown when it arrives, so its cost is unknown too. Supply is the serving capacity behind a backend: shared provider pools congest, replicas get preempted, and new capacity is slow, because a new replica takes a median of 136 s (classifier) to 159 s (zero-shot) just to load its model. The usual fix is a hand-set global dispatch rate per job type, which works only for the traffic mix it was tuned on. The hard part is that engines queue work by default, so once they are overloaded, requests wait past their timeouts, and the authors measured goodput (requests completed within their timeout, per second) falling to zero in replay.

## 💡 The idea
Make the constrained resource say "no" immediately instead of accepting work it cannot finish, and keep the excess in a durable queue outside execution. A refusal is then a cheap, honest signal of the current capacity ceiling, so the dispatcher can find that ceiling by probing upward and backing off on refusals alone, with no prediction of output length and no fleet-wide rate to retune. The authors' analogy: "C AP - AND -T RACK paces dispatch as congestion control paces a network sender. A C AP refuses work early where capacity saturates. T RACK probes upward and backs off on those refusals, as a sender does on loss."

## ⚙️ How it works
### Cap: a fail-fast refusal at the saturated resource
Each inference replica gets a per-replica cap. The classifier and zero-shot deployments set vLLM's max num seqs to 128 and the model server's request and processing caps to 140 per replica, together called "the cap". The cap is deliberately a little above the batch limit, so a few requests wait just behind a nearly full batch and a freed slot refills without a dispatcher round trip. A request above the cap gets an overload exception that the gateway maps to HTTP 429 and returns at once, using no reserved tokens. The cap needs no precise tuning: in replay, caps at two and four times the plateau concurrency kept goodput within 2.4% of plateau goodput.

### TRACK: a rate controller that follows refusals
Every 15 seconds a leader computes, for each queue, the refusal share across the whole fleet: the fraction of successes and 429s that are 429s. Timeouts are excluded, because a timeout can mean a network fault where backing off only wastes capacity. The rate is then updated as a multiplicative increase and decrease (MIMD) rule, shown below in simplified form.

```text
refusal_share = 429s / (successes + 429s)

if refusal_share <= 2% (the production target):
    if dispatch >= 75% of target rate:  rate = rate * 1.03
    else:                               hold (utilization gate)
else:
    rate = rate * 0.99   if refusal_share <= 50%
    rate = rate * 0.85   if 50% < refusal_share <= 65%
    rate = rate * 0.5    if refusal_share > 65%
```

Here `rate` is the queue's dispatch rate and `dispatch` is the rate actually achieved. The 3% step is small enough that the rate traces a sawtooth just above the ceiling, and the three cut tiers act harder only when more than half of attempts are refused. The utilization gate stops the rate from climbing during quiet hours, which in early versions let targets run up to their maximum and then overran the backend when work returned.

### Durable priority queues
Work waits in FOQS, Meta's durable priority queue, and is dispatched only at TRACK's rate. Each item carries an integer priority score; higher priority dequeues first, ties dequeue roughly FIFO, and items gain no priority with age, by product choice. A caller may give an item a time to live, checked before dispatch, which bounds its wait under strict priority. Refused work stays durable, so a backlog stays visible and recoverable instead of timing out inside an engine.

### Leader, placement and freezes
One leader per capacity domain, chosen as the worker with the lexicographically smallest hostname, pools every worker's counts, decides each queue's fleet-wide rate and pushes each worker its share. Each queue is placed on just enough workers for its rate, using a consistent hash ring, so a worker joining or leaving moves about 1/N of placements. Restarts and worker-count changes would otherwise cause needless cuts, so the leader freezes rates until 10 polls after the last worker-count change, and a freeze starts only when at least 10% of workers drop out. Retries stay local: a refused request returns to the head of its worker's ready queue with media already staged, and only lease expiry returns it to FOQS.

### Operating lessons
The authors' Section 7 lessons are concrete. Turn on the engine's refusal path, because the vLLM version they deployed limited running sequences but not its waiting queue, and the model server's request cap was off by default. Keep little work waiting at the engine, and set batch size, waiting limit and timeout together: raising the batch from 256 to 320 sequences added 10–15% throughput, but with up to 300 requests waiting behind it, too many timed out, and lowering the waiting limit from 300 to 100 cut timed-out attempts from 15.4% to 1.6%. Keep dispatch stateful, because following the ceiling needs recent outcomes and the current rate, which stateless jobs do not hold.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| Classifier, production A/B, same traffic | Precursor: 68 H100s (profiled), erroring at about two-thirds of its profiled per-card rate | Shaper: 39 H100s at equal coverage and scores (45 in production) | H100s; "43% fewer" as reported |
| Zero-shot, production A/B, about 600 MI300X each, 23 days | Precursor, same priority ordering | Shaper drained every shared backlog first, by a median 3 hours | Backlog drain time |
| Zero-shot, both paths at dispatch limits | Precursor: median 41 of 128 sequences | Shaper: median 124 of 128 sequences, 9% more model requests per GPU | Sequences per engine batch; requests per GPU |
| Replay, one A100, stable ceiling (Table C1, cap 40) | Fixed 120% of capacity: 109.5 req/s per host, 15.5% refused | TRACK: 108.5 req/s per host, 3.8% refused | Goodput per host; refusal share |
| Replay, one A100, classifier shape, about twice capacity | No cap: goodput 0.01 req/s (classifier), 0 (generative) | With cap: 8.64 req/s (classifier), 1.83 (generative) | Goodput per second |
| Reserved provider capacity, launch | Static system, prior 23 days: 88.2% utilization, 16.1% refused | Shaper, first 4 full days: 94.8% utilization | Share of reservation used |
| Reserved, steady state after provider update | No baseline after the update | 95.8% over 15 weeks; 99.1% in a backlogged week | Share of reservation used |
| Classifier backfill, last three days | Freshness goals per band (30 min to 24 h) | Top band met its goal for 99% of items; second band 89%; lowest two bands 44–55% | Share of items within goal |

The results show that TRACK, without any capacity setting, stays close to the best hand-tuned rate at a stable ceiling and follows the ceiling when it moves, including through the loss of most on-demand capacity. They do not show that it beats a perfectly tuned fixed rate: the replay fixed rate at 100% was slightly better on goodput (107.0 req/s, with 1.1% refused) in the stable case. The production comparisons are same-traffic A/Bs on one company's internal fleets, on two workloads, and the fleet-size figures are a lower bound. Lower-priority work waited longer at peaks on the smaller fleet; the lowest band waited a median 6.6 hours against the precursor's half hour.

## ⚠️ Limitations
- The production results outside the replay and the two A/Bs are observational, with no control for backlog.
- The real-engine testbed used the classifier request shape, one 8B model on one A100 80 GB, and one-token outputs; generative workloads were evaluated only in production.
- The authors have not applied CAP-AND-TRACK to interactive or agent-session traffic.
- Serve-on-arrival fleet sizes (25 H100s mean, 99 at the busiest hour) are lower bounds, and the "about 48 fewer GPUs" estimate and the 1.6 times drain-speed estimate are untested.
- Lower-priority work waits longer at backfill peaks; the authors accept this as the price of a smaller fleet.
- 🔎 Added context: every production and A/B number comes from one company's own fleets and its own precursor system. Nothing here is an independent replication, and the workloads are mostly prefill-heavy (about 17 input tokens per output token), which the authors expect to gain more on decode-heavy work.
- 🔎 Added context: the replay used a small model on a single GPU, so the goodput figures (about 108 req/s per host) describe the dispatch behavior, not what a larger model would sustain.

## 🏗️ What it means for building systems
This is interpretation, not the authors' claim. The pattern would plug in as a dispatcher in front of any capacity-limited backend that can return a cheap, labeled "no" at its admission point, such as a model server with a per-replica cap, a provider quota, or an internal router. The cost is more retries and more network load from refused attempts, plus a durable queue and a stateful leader that must survive restarts and freezes. Before you trust it, you would need a bound on the engine's own waiting queue, a timeout that is longer than the expected queue wait, and a freshness goal per priority band measured on your own traffic.

## 🔑 Key takeaways
- A fail-fast refusal at the saturated resource, not a longer queue, is what keeps goodput from collapsing: without the cap, goodput fell to 0.01 req/s for the classifier shape in replay.
- TRACK learns the ceiling from labeled 429s only, adjusting by +3% per healthy 15-second poll and cutting by 0.99, 0.85 or 0.5 depending on the refusal share, so it needs no output-length prediction.
- In the production A/B, the shaper served the classifier on 39 H100s against the precursor's 68 and matched coverage and scores.
- The lowest-priority bands absorb the cost of a smaller fleet: 44–55% of items met their goals in the backfill's last three days, so fleet size must be chosen for freshness, not just throughput.
- Stateful dispatch with a per-queue leader, consistent-hash placement and freezes was most of the engineering effort, and it is what makes the simple rule safe on a changing fleet.

## 📚 Reading plan
### From the paper
- [Kwon et al., PagedAttention and vLLM (SOSP 2023)](https://doi.org/10.1145/3600006.3613165) — the engine whose batching and KV cache the design depends on.
- [Netflix concurrency-limits](https://github.com/Netflix/concurrency-limits) — the closest adaptive limiter, which reads rising latency as overload instead of fail-fast refusals.
- [Envoy adaptive concurrency filter](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/adaptive_concurrency_filter.html) — a production concurrency limiter the authors contrast with TRACK.
- [CONCUR, arXiv:2601.22705](https://arxiv.org/abs/2601.22705) — congestion-based admission for agentic batch inference, driven by KV-cache signals.
- [DCTCP, data center TCP (SIGCOMM 2010)](https://doi.org/10.1145/1851182.1851192) — the congestion-control design the authors use as their analogy for marks and refusals.
### To go further
- Sarathi-Serve (OSDI 2024) — the throughput-latency tradeoff in LLM inference, the reason batches are kept full.
- Metastable failures in distributed systems (HotOS 2021) — why a retry-driven overload can keep itself going after the trigger is gone.
- Site Reliability Engineering, "Handling overload" chapter — the classic treatment of load shedding that the paper's related work builds on.
- Little's law — how request rate and wait time set the queue length that a timeout must cover.

## 🔗 Concepts
- [adaptive-concurrency-limit](/concepts/adaptive-concurrency-limit.md)
- [inference-tradeoffs](/concepts/inference-tradeoffs.md)
- [kv-cache](/concepts/kv-cache.md)
- [prefill-decode-disaggregation](/concepts/prefill-decode-disaggregation.md)
- [retry-storm](/concepts/retry-storm.md)
- [goodput](/concepts/goodput.md)
