---
type: Article Summary
title: How we raised Databricks cluster rate limits by 7.5×
description: Fixed request-rate limits cannot protect long-running work, so control how many operations run as health changes.
resource: https://www.databricks.com/blog/how-we-raised-databricks-cluster-rate-limits-75
tags:
- backend-systems
- system-design
- retry-storm
- adaptive-concurrency-limit
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
sources:
- resource: https://www.databricks.com/blog/how-we-raised-databricks-cluster-rate-limits-75
  title: How we raised Databricks cluster rate limits by 7.5×
  author: Databricks
status: stable
source_url: https://www.databricks.com/blog/how-we-raised-databricks-cluster-rate-limits-75
source: Databricks
source_type: blog
published: '2026-11-18'
domain: backend-systems
subdomain: system-design
concepts:
- retry-storm
- adaptive-concurrency-limit
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d8196bb0dc80812916152
---

## 🎯 Core thesis
For long-running work, a fixed request-rate limit cannot match capacity that changes over time, so the service should decide how much work to start from the number of active operations and the health of its dependencies, keeping a fixed rate only as an outer guardrail.

## 🧭 Why the author wrote it
Databricks protected cluster creation with fixed request-rate limits that were hard to tune: they turned customers away during healthy periods and kept accepting work during slowdowns, so engineers adjusted them by hand. The author argues that counting active work and adapting to health lets the service safely raise its defaults, and the post reports the rollout results.

## Introduction: a kitchen with a fixed rule
The article opens with a summary. Adaptive concurrency limits track long-running cluster-creation workflows and adjust how much work runs as system health changes. Bounded queues and fair scheduling absorb bursts and share capacity, and a fixed API request-rate ceiling stays as a customer-facing guardrail. In the reported scope, Databricks raised default cluster-creation request-rate limits by 7.5× and removed over a hundred customer-specific overrides.

The authors work on the Compute Platform team, which turns cloud infrastructure into clusters ready for customer workloads. That means provisioning virtual machines and coordinating cluster creation across AWS, Azure and Google Cloud. Their goal is to keep these systems available while letting customers use as much capacity as they can safely provide.

> 🔎 Added context: the opening section has no heading in the source text, so it is titled here by its content.

### Fixed request-rate limit
Historically, the platform was protected by fixed request-rate limits, tested against different load patterns and failure scenarios. Scaling meant adding resources and adjusting per-customer limits. The article says conservative limits turned away requests despite spare capacity, yet kept accepting requests when services or dependencies such as cloud providers slowed down.

> ❌ **Anti-pattern — Manual limit rewriting during incidents:** Lowering the limits by hand whenever load must be shed, then raising them again after recovery. **Why people do it:** Lowering a limit is a direct way to shed load during an incident. **Consequence:** Every incident needs a human to set the number twice, and the number is always stale for the next load pattern. **Fix:** Let the service adjust admission from active work and health signals, so no one has to rewrite the limit.

### Kitchen analogy: concurrency as meals in progress
The article compares the compute platform to a kitchen that aims to serve as many orders as its cooks and equipment can handle. A manager posts a fixed rule: accept at most ten new orders per minute, whatever is happening at the stove. Even with spare cooking capacity, the rule turns orders away. When an oven breaks, the kitchen keeps accepting orders faster than the remaining cooks and equipment can finish them, and the manager has to keep rewriting the sign.

A better kitchen keeps a short queue and limits how many meals are being prepared at once. If an oven breaks, fewer meals start, and as capacity returns, more can begin. Orders can wait briefly, but only while there is still time to serve them. The conclusion is that for long-running operations a fixed request rate is a poor model of capacity. The goal is to let customers run at full throttle when capacity allows, then ease off enough to keep useful work moving during degradation.

### Short queue as a buffer
In the analogy, a short queue holds orders that cannot start yet. The queue is useful only while each order can still be served in time. The later sections make this precise with bounds on queue size and waiting time.

## Background
An API call can launch much more work than the handler performs itself. For cluster creation, the handler validates the request, records a work item and returns. A background workflow then coordinates services, updates state and provisions resources such as cloud virtual machines. These operations complete asynchronously, and their duration depends on the health and capacity of their dependencies. The API call may take milliseconds, while the workflow itself may take minutes.

A request-rate limiter at the API entry point caps requests per time window. The article notes that this is useful for enforcing quotas, preventing abuse and protecting the resources used to handle API calls.

### Asynchronous workflow
The handler and the workflow are separate. The handler finishes quickly by recording a work item, and the real work continues in the background for minutes. A limit placed only at the handler therefore sees the cheap part of the operation and not the expensive part.

### Request-rate limiter
A request-rate limiter caps how many requests are accepted in a time window. It is a good tool for quotas and abuse protection, but the article argues it is a poor model of capacity for work that outlives the request.

## The problem
A fixed request-rate limit is easier to tune when available capacity and the work behind each request stay reasonably predictable. Cluster creation makes both difficult. One request may provision a few VMs and another many more, and cloud-provider health changes how long provisioning takes. During a shared slowdown, many workflows take longer at the same time, so the same incoming rate leaves much more work in flight while the system's ability to finish it falls.

The article's figure makes this concrete. On the left, arrivals stay fixed at one operation per time unit. Increasing duration from two to six time units triples the work in flight. On the right, processors become slow or unavailable, which reduces the capacity available to handle that work.

When a dependency degrades, more calls fail or time out. Long-running workflows retry those calls to recover from temporary faults. Across many active workflows, those retries add pressure to the struggling dependency, which causes more failures and yet more retries.

These internal retries come from work that has already been accepted, so the API request-rate limiter never sees them. They can sustain overload even after incoming traffic returns to normal. Limiting active workflows helps contain that pressure and gives unfinished work a chance to drain.

Neither direction of tuning solves the mismatch. A lower rate buys headroom by rejecting more work during healthy periods, and a large enough slowdown can still exhaust that headroom. A higher rate makes better use of healthy capacity but accepts more work during degradation. As the article puts it, one number has to cover both days.

> ❌ **Anti-pattern — One fixed rate for healthy and degraded periods:** Choosing a single requests-per-window number and treating it as the capacity of the service. **Why people do it:** The article notes that a fixed rate is easier to tune when capacity and per-request work stay predictable. **Consequence:** A low number rejects work during healthy periods, and a high number accepts too much during a slowdown. **Fix:** Limit the number of active workflows, which can follow capacity as it changes, instead of tuning one number for both.

### Work in flight
Work in flight is the amount of accepted work that has not finished. It grows with arrival rate multiplied by how long each operation takes, so a slowdown can multiply it even when arrivals are steady. The figure's tripling from two to six time units shows the effect. Work in flight is what a concurrency limit controls, which is why it is a better signal than arrivals for long-running work.

### Retry storm
A retry storm is a cycle in which a degraded dependency causes failed calls, the failed calls trigger retries across many active workflows, and those retries add load that causes more failures. The retries come from already accepted work, so an admission limit at the API does not see them. The article's remedy is to limit active workflows so that unfinished work can drain.

## Terminology
The article says "rate limiting" is sometimes used broadly to include limits on concurrent work. It distinguishes the two mechanisms by what each counts.

### Operation
An operation is the full workflow, including background work that continues after the API returns.

### Request-rate limit
A request-rate limit caps requests accepted per unit time, such as 10 requests per second, with a possible burst allowance.

### Concurrency
Concurrency is how many operations are active at once, including those waiting on dependencies.

### Concurrency limit
A concurrency limit caps active operations, such as 100 at once. Each operation holds a slot until it finishes.

### Backlog
A backlog is accepted work waiting to start. It has not acquired an active slot yet.

The article notes that either kind of limit can be fixed or adaptive. Its comparison is between fixed request-rate limits and adaptive concurrency limits for protecting long-running work.

## Solution
The kitchen needs to track meals still being prepared and the capacity available to finish them. In Databricks classic compute, the equivalent is the set of cluster-creation workflows still provisioning infrastructure. The approach is to give each active workflow a slot and to adjust the number of slots as conditions change.

### Slot
A slot is the unit of admission for one active provisioning workflow. Consider a CreateCluster request for a cluster with a driver and several worker VMs. Once the request is accepted, its provisioning workflow waits for a slot before it starts. The platform tracks whether that work is queued, actively processing or complete.

If all slots are occupied, the request stays queued. An active workflow keeps its slot while it provisions VMs and waits on dependencies. When the workflow finishes, its slot becomes available for another cluster launch. A successfully launched cluster can keep running customer workloads for hours without occupying a provisioning slot.

The slot must cover the whole provisioning operation, including background work after the API returns. Tracking a slot does not require holding a thread. The controller counts active provisioning workflows. Running clusters, allocated VMs and vCPUs are separate measures. One workflow can provision many VMs, so one slot does not imply a fixed resource cost.

### Adaptive concurrency limit
An adaptive concurrency limit is a cap on active operations that changes with conditions. Useful signals for cluster creation include how long provisioning takes and whether critical thread pools are saturated. When these signals indicate congestion, the controller reduces the number of concurrent provisioning workflows. As conditions recover, it cautiously allows more.

> 🔎 Added context: RPC (remote procedure call) is a request from one internal service to another, so "RPC rate-limit rejections" means requests rejected at the service-to-service boundary.

### Adapt to available capacity
Now suppose a cloud provider takes longer to provision VMs. Active workflows hold their slots for longer, so new cluster launches start less often even when the slot limit stays fixed, and customers see longer launch waits as accepted requests queue. Adaptive control also changes the slot limit itself.

The article makes a clear separation: capacity pressure changes when accepted work starts, while RPC rate-limit rejections are reserved for exceptionally high incoming request rates. Reducing the limit prevents new operations from starting until active work falls below it, but existing operations still need time to finish. That response time matters when workflows last minutes.

Retries also need backoff and a limit on attempts, so active workflows do not keep overwhelming a recovering dependency. The article's conclusion is that concurrency limiting cannot repair an unavailable dependency, but it can limit the extra pressure while that dependency recovers.

### Retry backoff with an attempt limit
Backoff spaces out retries, and an attempt limit caps how many times a workflow retries. Together they stop active workflows from adding load to a dependency that is trying to recover. Retries are useful for temporary faults, which is why the article keeps them and bounds them rather than removing them.

> ❌ **Anti-pattern — Retrying without backoff or an attempt limit:** Long-running workflows retry failed dependency calls immediately and without limit. **Why people do it:** Retries are how workflows recover from temporary faults, as the article notes. **Consequence:** Across many active workflows, retries keep a recovering dependency under pressure, which sustains the overload. **Fix:** Add backoff and a limit on attempts, and pair them with a concurrency limit.

## Devil's in the details
The control loop answers how much work to start. Building a useful service also means deciding who waits, how long they wait and what the customer is told.

### Explain why a request is being throttled
"Too many requests" can hide two very different situations. In the first, a customer sends its usual traffic while a shared dependency slows down. Capacity has fallen, and asking the customer to fix its traffic pattern misattributes a service problem. In the second, a customer submits an unusually large burst while the service remains healthy. Other customers compete for the same resources, and throttling that burst protects their fair share.

To tell these cases apart, the platform tracks system health, each tenant's incoming demand and the scheduling decision separately. A large number of in-flight operations alone does not prove that a customer sent too much, because those operations may be stuck behind a slow dependency. A burst need not be throttled for fairness if spare capacity is available and policy permits it.

The distinction belongs in metrics and in customer-facing explanations. It tells operators whether to investigate lost capacity or an unusual workload, and it tells customers whether changing their behavior will help. Failures caused by service degradation should be counted in service reliability metrics, even when they surface as throttling. For accepted requests that wait in the queue, the impact is measured on cluster launch latency.

> ❌ **Anti-pattern — Blaming the customer for degradation-driven throttling:** Returning a "too many requests" response to a customer whose usual traffic is being held back by a slow shared dependency. **Why people do it:** A large number of in-flight operations looks like a customer sending too much. **Consequence:** The customer changes behavior that will not help, and the real service problem stays hidden. **Fix:** Track system health, per-tenant demand and the scheduling decision separately, and explain the throttle by its cause.

### Buffer while the request still has time to succeed
For an operation that takes minutes, a short wait is often preferable to an immediate error followed by a retry. A queue can absorb a burst and let the system catch up. That only works if the wait still fits the request's deadline.

The article's guidance is to bound the queue's size and each request's waiting time, to make progress visible, and to reject or expire work when it can no longer finish usefully. A queue cannot turn sustained overload into capacity. An unbounded queue can turn a brief disruption into hours of delay.

> ❌ **Anti-pattern — Unbounded queue:** Accepting work into a queue with no limit on its size or its waiting time. **Why people do it:** Waiting avoids the immediate error that would otherwise trigger a retry. **Consequence:** A brief disruption becomes hours of delay, because the backlog never expires. **Fix:** Bound the queue's size and each request's waiting time, make progress visible and expire work that can no longer finish in time.

### Keep queueing and cleanup inexpensive
A queued request should be a small record, not a running worker holding scarce resources. A larger backlog should not make every active operation more expensive through repeated scans or through competition for threads and connections.

Cleanup needs the same attention. If a large backlog expires together, the service must discard it within a predictable time. The article's advice is to plan for the worst backlog, keep expiration and rejection cheap, and avoid expensive setup for work whose deadline has already passed. Otherwise the queue can keep the service busy long after the original congestion has ended.

### Queued request record
A queued request is a small record that waits for a slot. Keeping it small means the backlog costs little to hold, so a large backlog does not slow the active operations. This is the cheap-queue property the article asks for.

### Decide what "fair" means for the product
Suppose a service must delay a hundred requests. Is it better for a hundred customers to each see one slow request, or for one customer to see a hundred slow requests? The aggregate metric may look the same, but the customer experience will not.

A scheduler makes this choice whether or not it is specified. The article's advice is to define which work should go first, what share each tenant should receive under contention and how lower-priority work avoids waiting forever. Tenants may use otherwise idle capacity where the policy allows it, while fair access is preserved when other tenants need their share.

### Scheduling policy and fair share
Scheduling policy decides the order in which queued work starts. Fair share sets each tenant's portion of capacity under contention. Both must be defined explicitly, or the scheduler applies an implicit policy that nobody chose.

## Current state and future work
The ideas were applied to an integrated load management system for cluster creation on Databricks classic compute. A short API request starts a much longer workflow that coordinates several dependencies, including public clouds, to provision compute resources. The system combines queued requests, adaptive concurrency limits and fair scheduling. It uses the work already in flight and the system's health to decide when queued operations can start.

The team kept the API request-rate limit as a final line of defense against exceptionally high incoming traffic. It provides a request-rate boundary that can be communicated to customers. Adaptive concurrency limits stay inside the service and govern when accepted work starts. The article is explicit that a successful API response does not guarantee an immediate cluster launch or completion within a fixed time.

### Integrated load management
The integrated system is the combination the article describes: queue, adaptive limit and fair scheduler, working from in-flight work and health. The API boundary sits outside this system and does not control it.

### Results from the rollout
The initial rollout analysis reported three outcomes. Default cluster-creation request-rate limits were 7.5× higher. There were more than 99.99% fewer cluster-creation rate-limit errors in the reported scope. Over a hundred customer-specific rate-limit overrides in scope were removed.

The article credits adaptive concurrency control with making the 7.5× increase possible, because it protects the long-running workflows as conditions change. Since the increase, few customers have hit the higher cluster-creation request-rate limits, and no new manual overrides have been added for those limits. Customers have more room to scale, with less need for individually tuned exceptions.

### Open design questions
The article raises two directions for future work. When operations have very different resource costs, counting each as one slot may be too coarse, and weighted slots or separate limits are possible extensions. For workflows with several bottlenecks, the question is where to apply control so that a slow dependency does not hold up unrelated work.

During a cloud-provider slowdown, workflows already provisioning VMs can keep retrying even after new launches slow down. The article's recommendation is to pair concurrency limits with retry backoff and a limit on attempts to reduce that pressure during recovery.

### Weighted slots
Weighted slots would give expensive operations more than one slot, so that a single large cluster request counts for its real cost. The article presents this as a possible extension, not as something the rollout did.

## FAQs
### What is the difference between request-rate and concurrency limits?
A request-rate limit caps how many requests are accepted per unit time, while a concurrency limit caps how many operations are active at once. Each active operation holds a slot until it finishes, including time spent waiting on dependencies. The two limits protect different things, so the article treats them as separate mechanisms.

### Why must a concurrency slot cover the whole cluster-creation workflow?
A CreateCluster workflow holds its slot while it provisions VMs, including time spent waiting on dependencies after the API returns. When provisioning finishes, the slot is released even though the cluster may keep running customer workloads. The controller counts active provisioning workflows, not all running clusters or allocated VMs and vCPUs, and tracking a slot does not require holding a thread.

### How do adaptive concurrency limits respond to system degradation?
When a cloud provider slows down, provisioning workflows hold their slots longer and accepted requests wait longer to launch. The controller also reduces the slot limit when congestion signals, such as provisioning latency or thread-pool saturation, indicate pressure. As conditions recover, it cautiously allows more concurrent work.

### Why does Databricks still keep a fixed API request-rate limit?
The fixed API request-rate limit protects against exceptionally high incoming traffic and provides a boundary that can be communicated to customers. Adaptive concurrency limits work inside the service, so capacity pressure queues accepted requests and delays provisioning. A successful API response still does not guarantee an immediate cluster launch or completion within a fixed time.

### What results did Databricks report after adopting adaptive concurrency limits?
The initial rollout analysis reported 7.5× higher default cluster-creation request-rate limits, more than 99.99% fewer cluster-creation rate-limit errors in the reported scope, and removal of over a hundred customer-specific rate-limit overrides in scope. Since the increase, few customers have hit the higher limits, and no new manual overrides have been added for those limits. The article says the FAQ's companion topic is enforcing API quotas at scale.

## 🧠 Mental models
### Kitchen with a short queue
**The model:** Capacity is the cooks and equipment, so the kitchen admits orders only as fast as they can be finished and queues the rest briefly. **How to apply it:** When a dependency slows down, admit fewer new items instead of rewriting a fixed rule.

### Work in flight, not arrival rate
**The model:** At a fixed arrival rate, the work in the system grows with how long each operation takes; tripling the duration from two to six time units triples the work in flight. **How to apply it:** Measure active operations and their duration alongside the request rate, not the request rate alone.

### A slot covers the whole operation
**The model:** A slot is held through the end of background provisioning, not only for the handler's few milliseconds. **How to apply it:** Define a slot's lifetime to match the work it protects, and do not count the cheap front end as the whole operation.

## 🔑 Key takeaways
- A fixed request-rate limit cannot match capacity for long-running work, because one number has to cover both healthy and degraded periods.
- Counting active operations with slots controls how much work starts, and each slot is held through background provisioning.
- Retries from accepted work bypass the API limiter, so limiting active workflows is what contains a retry storm.
- Adaptive limits shrink on congestion signals such as provisioning latency and thread-pool saturation, and grow back cautiously as conditions recover.
- Bounded queues with per-request deadlines and cheap expiration absorb a brief disruption without creating hours of delay.
- Throttling should be explained by its cause: degradation-driven throttling is a service problem, not customer misbehavior.
- The fixed API limit stays as a final guardrail, and the reported rollout showed 7.5× higher defaults with more than 99.99% fewer rate-limit errors in scope.

## 🏛️ For Kenji
This is your interpretation, not the author's. For a Staff Architect, the transferable idea is the split between admission control at the boundary you promise customers and internal control over how much accepted work runs at once. The question it raises: which of your services has a fixed limit that was tuned on healthy days, and what does that limit do during a dependency brownout?

## 📚 Reading plan
### From the article
- High Performance Ratelimiting at Databricks (no URL in the extracted text) — the companion piece the article names for enforcing API quotas at scale, the complementary problem to this one.

### To go further
- Retry with exponential backoff and jitter — how to space retries so that they do not synchronize into a retry storm.
- Adaptive concurrency limiting with AIMD-style controllers — the family of feedback rules that grow a limit slowly and cut it on congestion.
- Little's law — the relation between arrival rate, time in the system and work in flight, which is the arithmetic behind the article's figure.
- Load shedding with bounded queues — how to reject or expire work before it wastes scarce capacity.

## Concepts
[Retry storm](/concepts/retry-storm.md) · [Adaptive concurrency limit](/concepts/adaptive-concurrency-limit.md)
