---
type: Article Summary
title: 'DoorDash Dispatch: Building a Lightning-Fast Delivery System'
description: DoorDash rebuilt dispatch by splitting coordination from heavy computation and decomposing optimization, then
  rolled out safely via shadow testing and switchbacks.
resource: https://blog.bytebytego.com/p/doordash-dispatch-building-a-lightning
tags:
- backend-systems
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-25T06:47:00-03:00'
sources:
- resource: https://blog.bytebytego.com/p/doordash-dispatch-building-a-lightning
  title: 'DoorDash Dispatch: Building a Lightning-Fast Delivery System'
  author: ByteByteGo
status: stable
source_url: https://blog.bytebytego.com/p/doordash-dispatch-building-a-lightning
source: ByteByteGo
source_type: blog
published: '2026-11-23'
domain: backend-systems
concepts:
- architectural-style
- quality-attributes
- scalability
- distributed-system
- grpc
- control-plane
- latency-throughput
- availability-reliability
- fault-tolerance
- canary-deploy
- training-vs-inference
- reinforcement-learning
- shadow-testing
selected_by: theme
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/423baf74172d8135be36cd9c83fd46e5
---

## 🎯 Core thesis
DoorDash could only make dispatch decisions both good and fast by separating coordination from heavy computation and decomposing the optimization problem into partitions, and it replaced the old engine safely by validating the new one first in shadow mode and then in switchback experiments on production traffic.

## 🧭 Why the author wrote it
The article starts from a monolithic dispatch engine whose computational work grew faster than delivery volume, so peak demand forced it to cut corners and made its decisions arrive too late to be useful. It explains the redesign that split the work into tiers and smaller pieces, and the staged testing that let DoorDash swap such a critical system without breaking live deliveries. If the thesis is right, latency-sensitive decision systems need an architectural change, because adding hardware alone does not fix coupled optimization problems.

## Introduction
> 🔎 Added context: The article opens without a heading. The text below is its opening section, followed by the roadmap the author gives.

The article begins by saying that DoorDash's dispatch platform has to make many good delivery-related decisions while the surrounding situation keeps changing. To keep up with the need for innovation, the DoorDash engineering team had to redesign the dispatch system. The redesign addressed two main problems. The first was how to divide the calculations into smaller pieces. The second was how to run those pieces across many machines without losing the information needed to make good assignment decisions.

The author's roadmap covers seven points:

- The role of a dispatch system
- Why the old dispatch system at DoorDash struggled
- Why DoorDash could not just add more servers
- Separating coordination from heavy computation
- Breaking the optimization problem into smaller pieces
- How DoorDash replaced such a critical system safely
- The improvements after the new system

The article includes a disclaimer that it is based on publicly shared details from various sources, and it invites readers to point out inaccuracies. Treat the numbers below as the author's summary of public information, not as DoorDash internal data.

## What Does a Dispatch System Actually Do
When someone orders food, several things must happen. The restaurant cooks the food, a delivery driver travels to the restaurant to collect it, and then the driver brings it to the customer. DoorDash calls its drivers Dashers. The dispatch system coordinates all of this by deciding which Dasher should handle which order.

Making that decision requires more than knowing which Dasher is closest to the restaurant. The article gives an example: a Dasher two minutes away may have to wait for the restaurant, which still needs 15 minutes to prepare the food. Sending that Dasher immediately creates a long wait. A Dasher who is much farther away may arrive close to the moment the food is ready, which is the better choice. Two orders heading in a similar direction might also be combined into one delivery trip.

### Dispatch must reason about space and time together
Space covers the locations of Dashers, restaurants and customers. Time covers travel durations, traffic, food preparation and delivery expectations. The author's point is that a dispatch decision has to account for both at once, since a close Dasher can still be the wrong choice if the timing is off.

### The vehicle routing problem as an analogy
The author compares dispatch to the vehicle routing problem, which asks how vehicles should handle a collection of destinations while working within constraints. DoorDash's version is harder because its inputs keep changing. New orders arrive constantly, Dashers keep moving, and food preparation estimates shift while the system is still calculating.

> 🔎 Added context: The vehicle routing problem is a well-known optimization problem about planning routes for a fleet of vehicles that serve a set of destinations under constraints. The article uses it as an analogy and does not describe the exact variant DoorDash solves.

### Decision quality and decision speed
The author argues that a dispatch decision must be good enough and also fast enough to be useful. A perfectly calculated assignment is worthless if the calculation takes too long. The article presents this trade-off between decision quality and decision speed as the starting point for the whole redesign.

> ❌ **Anti-pattern — Dispatching to the nearest Dasher:** Choosing the closest available Dasher without considering when the food will be ready. **Why people do it:** Distance is the simplest signal to compute and to explain. **Consequence:** The Dasher waits at the restaurant, which creates long wait times and wastes the driver's time. **Fix:** Evaluate space and time together, including preparation time and the chance to combine orders. *(The article presents this as a flawed approach through its example, not as a named anti-pattern.)*

## Why Did the Old System Struggle as DoorDash Grew?
DoorDash's previous dispatch engine was a monolith, meaning several major responsibilities lived inside one tightly connected system. It maintained the state of the delivery network, applied business rules and performed expensive routing calculations all in the same place.

### Monolith and the state of the network
The author defines state as information about the current situation, such as available Dashers and orders waiting for assignment. In a monolith, this state, the business logic and the heavy calculations are tangled together, so a change in one area touches all the others. The article presents this coupling as the root of the struggle.

### Candidate generation and optimization
The old engine ran its work in stages. It generated possible assignments, checked proposed routes and then optimized the choices. The author defines candidate generation as producing the possibilities the system can evaluate. Optimization means selecting a good enough combination of those possibilities according to the system's goals and constraints.

### Quadratic growth in computation
The difficulty was that computational work grew much faster than delivery volume. The important algorithms in the original engine had roughly quadratic complexity, so doubling the size of the input could produce approximately four times the computational work. The pressure came from potential Dasher-to-order pairings: if both the number of Dashers and the number of orders grow, the number of possible pairings grows quickly. Evaluating routes and batches added further work on top of that.

### Peak demand and degraded decisions
During peak periods, demand could reach nearly four times the system's dispatch processing capacity. The engine then had to limit what it considered or fall back to simpler decision rules. Delays also reduced the usefulness of its calculations: a Dasher might leave a promising pickup, or ready food might sit waiting while the engine was still deciding. The author's conclusion is that the problem went beyond slow software, because slow computation directly affected the quality of real deliveries and the user experience.

## Why Couldn't DoorDash Just Add More Servers?
The article describes adding servers as horizontal scaling. It works well when work can be split into independent requests, such as many requests for unrelated product pages handled by different servers, where no server needs to coordinate its answer with the others.

Dispatch assignments have more dependencies. Consider two nearby restaurant orders that could share a delivery trip. If they are sent randomly to separate workers, each worker may find a reasonable individual assignment, but neither gets to consider both orders together. Two workers could also independently pick the same Dasher as their best choice.

There is also a systems problem. Even if the calculations are distributed, some component must prepare the work, send the data, gather the results and combine them. If that component becomes overloaded, the bottleneck has only moved somewhere else.

DoorDash therefore had to answer two questions at the same time:

- How can the application distribute computation across more machines?
- How can the optimization problem be divided while keeping useful relationships between orders, routes and Dashers?

The author notes that the first question needed an architectural change and the second needed a change in how the work was organized.

> ❌ **Anti-pattern — Scaling out a coupled optimizer by adding identical servers:** Running more copies of the whole engine and splitting incoming requests across them. **Why people do it:** Horizontal scaling is the default answer that works for stateless web requests. **Consequence:** Workers make conflicting or duplicate decisions, and the component that splits and merges the work becomes the new bottleneck. **Fix:** Separate the coordinating responsibilities from the heavy computation, and decompose the problem so each worker has a meaningful, bounded piece. *(The article presents this as the reason horizontal scaling alone was not enough; the label is an interpretation of its argument.)*

## Separating Coordination from Heavy Computation
DoorDash rebuilt the platform around two independently scalable tiers. A tier is a group of components with a particular responsibility.

- The **coordinator tier** runs the dispatch process. It maintains the broader logistics state, applies business rules and organizes the work that needs to happen.
- The **compute tier** consists of workers that run expensive calculations. These workers generate and evaluate routing possibilities, run optimization routines and execute ML models.

Separating the tiers lets compute capacity grow without every worker taking on all the responsibilities of the original engine. The execution flow works like this: the workers operate in parallel, and a reconciliation step combines their results into coordinated dispatch decisions.

Distributing computation introduces a communication cost. If every worker received a complete copy of the entire logistics state on every run, transmitting and processing that data could become very costly.

### Localized payload sharding
DoorDash addresses this with localized payload sharding. A payload is the data sent with a job, and sharding divides that data into smaller portions. Each worker receives only the local context needed for its assigned calculation. The coordinator sends these payloads over gRPC on each dispatch tick, which the article defines as one round of dispatch processing.

> ❌ **Anti-pattern — Sending the full logistics state to every worker:** Shipping the entire state to each compute node on every run. **Why people do it:** It is the simplest way to guarantee every worker has what it needs. **Consequence:** Transmission and processing costs grow with the whole system, eroding the gains from parallelism. **Fix:** Send each worker a self-contained, localized payload. *(The article describes this cost as the reason for sharding; the label is an interpretation.)*

### gRPC as the transport
> 🔎 Added context: gRPC is a framework for communication between services. The article uses it as the channel between coordinator and workers and does not discuss its configuration.

### Stateless workers
The payloads are self-contained, which lets the workers stay stateless. A worker may still hold a trained model or temporary calculation data. The key separation is that responsibility for managing the changing logistics state stays with the coordinator. This division means workers can be added or replaced without carrying the authoritative state of the network.

## Breaking the Optimization Problem Into Smaller Pieces
Separating the tiers allows parallel execution, but it does not automatically reduce the size of the optimization problem. If every worker still had to evaluate a huge, interconnected set of possibilities, the original computational pressure would remain. DoorDash therefore introduced hierarchical decomposition, which means breaking a large problem into smaller problems through multiple stages.

The coordinator groups the broader dispatch problem into localized geographic clusters and delivery trajectories. The goal is to create manageable pieces that keep the local relationships important to routing.

### Pre-clustering and centroid refinement
The first stage is rapid pre-clustering, which creates initial groups. The second stage is centroid-based refinement, which adjusts those groups using their centers. The article describes a centroid as a representative center of a group of points.

Partitioning therefore happens through an initial grouping followed by refinement. Each partition becomes a unit of work for a compute node, which generates and checks possibilities within its local context while other nodes work on other partitions.

This brings two benefits. Smaller problems reduce the number of combinations each worker must consider, and parallel execution lets several problems be processed at the same time.

### Geographic boundaries are organizational, not real
The author stresses that the geographic boundary is a device for organizing computation. It does not correspond to a real separation between delivery opportunities. Orders and routes near the boundary may still be related, so local solutions can conflict with each other.

### Global reconciliation and scoring
To handle this, DoorDash uses a global reconciliation and scoring layer that combines results across partitions before final assignments are selected. The author says this preserves much of the optimization quality of the original problem while letting expensive candidate generation run in parallel. Workers produce useful local options, and reconciliation considers how those options can be combined into the broader dispatch decisions.

## How Did DoorDash Replace Such a Critical System Safely
The author notes that dispatch directly affects active deliveries. A defective setup could send Dashers to poor assignments, increase waiting time or disrupt fulfillment, so the replacement had to be validated carefully.

### Shadow testing
The team started with shadow testing. In shadow mode, the incoming production work is duplicated: one copy goes to the existing engine and another to the new platform. The existing engine keeps controlling actual deliveries. The new engine also calculates what it would have done, but its output is only recorded for analysis.

This let developers check the new platform under real production conditions, including peak traffic. They could compare proposed matches, delivery-time-related behavior and resilience against the existing system.

### Behavioral regression analysis
The article calls this approach behavioral regression analysis. A regression here means a change that makes previously acceptable behavior worse. The team had to show that the new architecture preserved dispatch quality while also improving performance.

> ❌ **Anti-pattern — Treating shadow testing as the final proof:** Relying on shadow results alone to decide that a new decision engine is safe. **Why people do it:** Shadow mode is risk-free for deliveries and shows realistic behavior on live inputs. **Consequence:** Its decisions are never executed, so their real-world effect on deliveries stays unmeasured. **Fix:** Follow shadow testing with controlled production experiments. *(The article states this limit directly; the label is an interpretation.)*

### Switchback experiments
Shadow testing has a practical limit: because the new system's decisions are never executed, it cannot show the real-world impact of those decisions. DoorDash therefore followed shadow testing with production switchback experiments. In a switchback experiment, the two systems take turns controlling real production activity under controlled conditions, which allows their outcomes to be compared.

The team used limited exposure and fallback mechanisms while checking system reliability and fulfillment results. They expanded the rollout to a wider user base only after they were satisfied.

The article summarizes the progression: shadow testing gave strong confidence in the new engine's behavior on live inputs, while controlled production experiments tested what happened when decisions actually affected deliveries.

## The Improvements
The author reports substantial gains in the platform's capacity and responsiveness after the new dispatch system was implemented. The detailed results are:

- **Ten times the dispatch processing scale**, enabled by distributing computation.
- **More than 99.96% of runs completed in under 20 seconds**, compared with runs stretching to minutes under heavy load in the old system.
- **More than 99.99% availability during Super Bowl traffic spikes**, despite a 10% week-over-week increase in peak volume.

The three results measure different properties. Processing scale is how much work the system can handle. Execution latency is how long a run takes. Availability is whether the system remains operational. The author argues that all three matter for dispatch, because a platform that can eventually process large volume can still cause problems if individual decisions are too slow.

Extra capacity also lets the system evaluate a larger set of possibilities instead of narrowing its search during busy periods. The author connects this broader search to improved Dasher acceptance rates, fulfillment efficiency and Dasher earnings, and reports millions of dollars in annualized fulfillment savings. The article does not give figures for the acceptance-rate or earnings gains. The practical benefit, in the author's framing, is that decisions arrived faster and the platform had enough computational room to consider better assignments.

## Conclusion
The author says the redesign also made machine learning practical inside dispatch. The new platform made it feasible to run more computationally demanding ML models in the dispatch process.

### Inference inside dispatch
A trained model performs inference when it processes an input and produces an output, such as a score for a proposed route. If dispatch needs enormous numbers of these evaluations, even a small cost per evaluation becomes significant. This is why the placement of models matters.

### Two model-serving tiers
DoorDash introduced two tiers to match different needs.

- **L1** serves models locally inside compute workers. Each worker contains ONNX Runtime, software that executes trained ML models. Because evaluations happen locally, they avoid a network round trip to a separate model server.
- **L2** serves models through separate remote infrastructure. It handles more complex policy evaluations that happen less often. These heavier models run in isolated ML serving clusters, which keeps their computational demands separate from the coordinator's state-management work.

The principle is that frequent evaluations need a very short execution path, while heavier but less frequent evaluations can use dedicated remote capacity.

> 🔎 Added context: ONNX Runtime is an engine for running models saved in the ONNX interchange format. The article names it but does not describe which models or formats DoorDash uses.

### Multi-agent reinforcement learning for objective weights
The platform also enabled a multi-agent reinforcement learning system that adjusts the optimizer's priorities. Reinforcement learning is an approach in which a system learns which decisions to make by receiving feedback on results. In DoorDash's application, the agents adapt objective weights, which determine how strongly the optimizer favors different goals, such as delivery speed and batching efficiency.

The author's final point is that the biggest contribution of the new platform was not one isolated performance gain. The platform changed the long-term trajectory of DoorDash's dispatch system.

## References
The article has a References heading, but it lists no citations. Its two closing paragraphs make commentary points rather than reporting results.

- **Switchback carry-over:** In dispatch, a Dasher assigned near the end of a new-engine window is still on that trip when the old engine takes over. Each window therefore inherits the previous window's assignments. The commentary asks how long the windows were and whether DoorDash dropped the edge minutes from the comparison.
- **Shadow-then-switchback sequence:** For a system where a slow decision is also a worse decision, the commentary argues that proving the new engine on live traffic before it affects any delivery is the only sensible way to replace it.

> 🔎 Added context: The article does not attribute these two closing paragraphs to DoorDash, so read them as the commentary of the article's writer rather than as DoorDash's reported findings.

## 🧠 Mental models
### Space and time together
**The model:** A dispatch decision is only good if it accounts for where resources are and when they will be ready. A close resource can still be the wrong choice when its timing is off.
**How to apply it:** Before optimizing a placement decision, list the time dimensions, such as preparation, travel and expectations, alongside the spatial ones.

### Good enough and fast enough
**The model:** A decision must be good enough in quality and fast enough to be useful. Perfect but late output has no value in a live system.
**How to apply it:** Set a latency budget for each decision first, then choose the best algorithm that fits within it.

### Partition, solve locally, reconcile globally
**The model:** Splitting a large optimization into local pieces lets them run in parallel, but a global layer must combine the results so the pieces fit together.
**How to apply it:** Define partition boundaries for computation, then design a reconciliation step that scores the combined options, not just the local ones.

### Shadow first, then switchback
**The model:** Shadow testing proves behavior on live inputs without affecting outcomes. Switchback experiments prove the effect on outcomes. Each covers a gap the other leaves.
**How to apply it:** Run the new system in shadow mode to compare decisions, then move to limited, reversible production experiments with fallback before a wider rollout.

### Frequent evaluations stay local
**The model:** Cheap, frequent model calls belong on the same machine as the work that makes them. Heavy, rare calls can go to dedicated remote capacity.
**How to apply it:** Measure how often each model is called and its cost per call, then place it in local or remote tiers accordingly.

## 🔑 Key takeaways
- Dispatch decisions must weigh space and time together, since a nearby driver can still be the wrong choice when the food is not ready.
- Quadratic growth in computation means doubling the input can roughly quadruple the work, which is why the old monolithic engine could not keep up at peak demand.
- Adding servers helps only when work is independent. Dispatch requires coordination, so DoorDash separated the coordinator tier from the compute tier.
- Localized payload sharding and stateless workers keep the cost of distributed computation under control, while the coordinator owns the changing logistics state.
- Hierarchical decomposition with pre-clustering and centroid refinement makes the problem manageable, and a global reconciliation layer keeps the partitions consistent.
- Shadow testing verifies behavior on live inputs, and switchback experiments measure real-world impact before a full rollout.
- The author reports ten times the processing scale, more than 99.96% of runs under 20 seconds, and more than 99.99% availability during Super Bowl spikes, all based on publicly shared details.

## 🏛️ For Kenji
> This is your interpretation, not the author's.

The same pattern applies to any latency-sensitive decision service an architect might own: a coupled optimizer that must answer in seconds, a central coordinator that must not become the bottleneck, and a core engine that must be replaced without an outage. The sequence of shadow testing, then limited switchback rollout with fallback, is a reusable migration plan. Question it raises: in a decision service in your own portfolio, which dependencies between partitions would force a global reconciliation layer, and how would you measure the quality you lose by partitioning?

## 📚 Reading plan
### From the article
- [DoorDash Dispatch: Building a Lightning-Fast Delivery System](https://blog.bytebytego.com/p/doordash-dispatch-building-a-lightning) — the full article, to check the numbers and the sequence of the redesign against this note.
### To go further
- Vehicle routing problem and its heuristics — to understand the optimization family that dispatch resembles.
- Switchback experiments and carry-over effects in marketplace experimentation — to judge whether a rollout comparison is valid.
- Shadow traffic and dark launching — to design a risk-free comparison of a new engine against production.
- Centroid-based clustering, such as k-means — to see how pre-clustering and centroid refinement work in practice.

## Concepts
- [architectural-style](/concepts/architecture/architectural-style.md)
- [quality-attributes](/concepts/architecture/quality-attributes.md)
- [scalability](/concepts/backend-systems/scalability.md)
- [distributed-system](/concepts/backend-systems/distributed-system.md)
- [grpc](/concepts/backend-systems/grpc.md)
- [control-plane](/concepts/backend-systems/control-plane.md)
- [latency-throughput](/concepts/backend-systems/latency-throughput.md)
- [availability-reliability](/concepts/backend-systems/availability-reliability.md)
- [fault-tolerance](/concepts/backend-systems/fault-tolerance.md)
- [canary-deploy](/concepts/cloud-infra/canary-deploy.md)
- [training-vs-inference](/concepts/ai-ml/training-vs-inference.md)
- [reinforcement-learning](/concepts/ai-ml/reinforcement-learning.md)
- [shadow-testing](/concepts/backend-systems/shadow-testing.md)
