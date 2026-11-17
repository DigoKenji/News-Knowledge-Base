---
type: Article Summary
title: Build a real-time next-best-offer recommender using Databricks Feature Views
description: Feature Views let one feature definition serve both point-in-time training and low-latency online ranking, avoiding train/serve skew.
resource: https://www.databricks.com/blog/build-real-time-next-best-offer-recommender-using-databricks-feature-views
tags:
- data
- data-science
- feature-store
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://www.databricks.com/blog/build-real-time-next-best-offer-recommender-using-databricks-feature-views
  title: Build a real-time next-best-offer recommender using Databricks Feature Views
  author: Tian Tan
status: stable
source_url: https://www.databricks.com/blog/build-real-time-next-best-offer-recommender-using-databricks-feature-views
source: Databricks
source_type: blog
published: '2026-11-16'
domain: data
subdomain: data-science
concepts:
- feature-store
- inference-tradeoffs
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d8138b6bde7d04a65dc14
---

# Build a real-time next-best-offer recommender using Databricks Feature Views

**Source:** https://www.databricks.com/blog/build-real-time-next-best-offer-recommender-using-databricks-feature-views
**Author:** Tian Tan
**Date:** 2026-11-16
**Concepts:** [Feature store](/concepts/feature-store.md) · [Inference throughput/latency/cost](/concepts/inference-tradeoffs.md)

---

## 🎯 Core thesis
Defining each feature once as a Feature View lets the same logic serve both point-in-time-correct training and low-latency online serving, which removes the train/serve skew that comes from maintaining separate batch and streaming pipelines.

## 🧭 Why the author wrote it
A real-time recommender needs features computed the same way at training and serving time, and retrieved in milliseconds at inference. The article uses a working next-best-offer (NBO) recommender for retail banking to argue that Feature Views make this consistency possible without duplicate pipelines. If the thesis holds, the features a model trains on are the same definitions it uses in production.

## Opening: capturing customer intent in the session
> 🔎 Added context: The article's opening has no heading, so this title is a label I have added. Its content is kept in full below.

When a retail banking customer checks their accounts, explores credit cards or opens a loan calculator, each interaction signals their current intent. A relevant offer has to reach them while they are still in the session. Raw clickstream data must be aggregated into time-windowed features for two purposes: training on historical values as they existed at a given point in time, and serving current values with millisecond latency. The article says that building these pipelines separately creates duplicate feature definitions that can drift over time and degrade model accuracy.

The article then introduces an open-source Solution Accelerator that uses Feature Views to build a real-time NBO recommender for financial services. The accelerator has two parts: a recommender that uses batch features, and an extension that adds streaming, in-session intent. Both are built end-to-end on Databricks, with runnable notebooks that can be cloned and deployed.

### Session intent
Session intent is what a customer wants right now, inferred from what they do in the current visit, such as checking accounts, browsing credit cards or using a loan calculator. The article's point is that this signal is useful only while the customer is still in the session, so it has to reach the model with low latency. That is the reason the accelerator adds a streaming path on top of the batch recommender.

### Time-windowed features
Time-windowed features summarize events over a bounded interval, such as a 30-day average balance or a count over the last 10 minutes. The article's approach uses these features in both training and serving, so the windowing logic has to be the same on both sides.

## Consistent, real-time features for training and serving
Real-time recommendations need both consistency and speed. Features must be computed the same way during training and serving to avoid train/serve skew, which can degrade model accuracy. At inference time, those features also have to be retrieved and scored in milliseconds. Traditionally, meeting both requirements meant a batch pipeline for training features plus a hand-written streaming job for online serving, which the article says creates duplicate logic that can drift over time.

> ❌ **Anti-pattern — Separate training and serving feature logic:** A batch pipeline builds the training features while a hand-written streaming job builds the online features. **Why people do it:** The article says that meeting the consistency and speed requirements traditionally meant maintaining separate systems, so each requirement got its own implementation. **Consequence:** The duplicate logic drifts over time, which the article says can degrade model accuracy. **Fix:** Declare the feature logic once as a Feature View and let Databricks reuse it for training and serving.

[Feature Views](https://www.databricks.com/blog/introducing-feature-views) remove the need for separate feature definitions. You declare the logic once, and Databricks reuses it across training and serving:
- Batch features are materialized to offline Delta tables for point-in-time-correct training and to an online store for low-latency serving.
- Streaming features are continuously aggregated into the online store, while historical values are computed point-in-time from source events for training. Unlike batch features, they are not materialized to an offline store.

### Feature Views
A Feature View is a declared feature definition over either a batch table or a streaming source. The same declaration serves both paths: training reads historical values from the offline Delta tables or from source events, and serving reads current values from the online store. Because the logic is declared once, there is no second implementation to keep in sync. The article presents this as the central design choice of the accelerator.

### Point-in-time training with create_training_set
At training time, the `create_training_set` API retrieves feature values as they existed at each labeled event, which prevents future data leakage.
> 🔎 Added context: Future data leakage means a training example is built with information that would not have been available when its label occurred. The model then learns from signal it will not have at prediction time.

### Online lookup by customer_id
At inference time, the Model Serving endpoint fetches batch features from the online store using `customer_id`. The article notes that both workflows use the same feature definitions, which avoids inconsistencies between separately implemented offline and online transformations.

## Real-time next-best-offer recommendations
The accelerator implements a real-time NBO recommender for retail banking. As a customer browses accounts, explores credit cards or uses a loan calculator, the system ranks a catalog of offers, including credit card upgrades, savings products and personal loans, and recommends the most relevant one during the session. The recommender combines stable customer context, such as balances, spending, tenure and risk band, with recent clickstream activity that captures in-session intent. A single ranking model uses both types of data, and Feature Views keep the definitions consistent across offline training and online serving.

### Stable context and in-session intent
The ranker sees two kinds of input at once. Stable customer context such as balances, spending, tenure and risk band describes who the customer is. Recent clickstream activity describes what they are doing right now. The article uses a single model over both rather than separate models for each, so the feature layer has to serve the two freshness levels from one place.
> 🔎 Added context: The article does not state how often the stable features refresh. The contrast drawn here is between slowly changing context and minutes-old session activity.

## How we built it
The accelerator has three steps, delivered as runnable notebooks across two parts that share a catalog, schema, online store and feature definitions.

### Step 1: Define batch and streaming features
Feature Views let you declare features over batch tables and live Kafka streams using the same approach. The article's feature table:

| Feature | Declared as | What it gives the model |
|---|---|---|
| 30-day average balance | SlidingWindow | Rolling behavior |
| 90-day spend | TumblingWindow | Fixed-period behavior |
| Loyalty tier, risk band, income, tenure | ColumnSelection | Stable customer context |
| Category views in the last 10 minutes | RollingWindow on a stream | Freshest in-session intent |
| 30-day category interest | SawtoothWindow | Long window, fresh leading edge |
| Spend-to-income ratio | CustomUDF | Derived, computed per request |

For streaming features, you point the declaration at a Kafka topic instead of a table. Feature Views then continuously aggregate events into the online store, without a separate hand-written streaming job or duplicate feature logic. The article says SawtoothWindow keeps longer streaming windows practical: a 30-day category count stays fresh as new events arrive, without recomputing the entire history for every event.

#### Window types (SlidingWindow, TumblingWindow, RollingWindow, SawtoothWindow)
The article uses these names without defining them, so the following is added context.
> 🔎 Added context: A sliding window covers a trailing period that moves with the current time, a tumbling window covers fixed non-overlapping periods, and a rolling window covers the most recent interval on a live stream. A sawtooth window, as the article uses it, keeps a long window whose leading edge stays fresh as new events arrive.

#### ColumnSelection and CustomUDF
ColumnSelection takes stable columns directly from a table, which the article uses for loyalty tier, risk band, income and tenure. CustomUDF runs a user-defined function, which the article uses for the spend-to-income ratio, computed per request rather than stored.

### Step 2: Train with point-in-time-correct features
Using the [Feature Engineering SDK](https://docs.databricks.com/aws/en/machine-learning/feature-store/python-api), a point-in-time join reconstructs each feature's value as it existed at the time of the historical label, which prevents future data leakage. The resulting training set feeds a LightGBM ranker registered in Unity Catalog. Because training and serving use the same feature declarations, features cannot diverge through separately implemented offline and online transformations.
> 🔎 Added context: A point-in-time join matches each labeled event only with feature values that were already available at that event's timestamp.

### Step 3: Serve recommendations in real time
The ranker runs on a route-optimized Model Serving endpoint. Each request contains `{customer_id, candidate offers, session context}`. The endpoint retrieves customer features from the online store by key and scores all 40 offers directly, without a separate candidate-narrowing step. A second, route-optimized Feature Serving endpoint exposes the feature values, so the application can show why an offer was selected without querying the online store directly.

## Benchmarking latency, feature freshness and throughput
Two measurements matter most for a real-time recommender: how quickly a click becomes a model-visible feature, and whether training and serving produce identical feature values. The article benchmarked the accelerator end-to-end in its smallest configuration, a small serving endpoint and a single-node online store, without tuning. Streaming events became model-visible in about a tenth of a second, and training and serving produced identical values.

Figure 1 in the article shows three latencies: request latency for the two serving paths and feature freshness, from event ingestion to online availability. Each pair of bars shows p50 and p95, and a dashed line marks the 50 ms budget that both serving paths meet. The figure itself is not in the extracted text, so only the stated values are used here. A complete ranking request, including online feature retrieval and inference, fits within the 50 ms budget on the smallest endpoint size. Adding the live streaming feature increases latency by only 3-4 ms.

### Throughput and capacity planning
The ranking endpoint sustained about 56 requests per second (RPS). The Feature Serving endpoint sustained about 390 RPS before p95 latency exceeded 50 ms. The article reports that measured throughput tracked concurrency divided by service time to within 15%, and gives this sizing formula:

> required concurrency = 4 × ceil(target RPS ÷ measured RPS at your SLO)

Applied to the article's numbers, supporting 150 ranking requests per second within a 50 ms p95 service level objective (SLO) requires concurrency 12.
> 🔎 Added context: 150 ÷ 56 is about 2.68, which rounds up to 3, and 4 × 3 = 12.

### How we measured
The benchmarks used the accelerator's notebooks with warm endpoints, an in-region client and light load. Request keys were spread across 2,000 customers with a Zipfian distribution to increase cache misses. Under those conditions, ranking latency measured 33 ms at p50 and 48 ms at p95. Feature freshness was measured per event, from the Kafka write to the value becoming readable in the online store. For consistency, offline materialized values were compared with the online store for every customer using null-safe equality.

The article lists two limitations. First, ranking endpoints scale to zero, so the first request after an idle period can incur a cold start of tens of seconds. Second, the reported latency combines feature retrieval and inference, so the 27 ms measurement the article cites is not directly comparable with benchmarks that report feature-read latency alone.
> 🔎 Added context: The article's summary and FAQ quote 34 ms p95 for ranking, while this section reports 48 ms p95 for the same endpoint. The text does not reconcile the two figures, so both are reported here as written.

> ❌ **Anti-pattern — Comparing unlike latencies:** Setting end-to-end ranking latency beside feature-read-only latency from another benchmark. **Why people do it:** The article does not say; the figures are easy to put side by side in a table. **Consequence:** The comparison overstates or understates the gap, because one number includes model inference and the other does not. **Fix:** Compare like with like, and state which components each measurement includes.

## The business case: Faster iteration, measurable results
The article is explicit that the accelerator demonstrates a working real-time recommender, not a proven revenue uplift. Because it uses synthetic data, the authors do not claim conversion improvements. Instead, it gives teams a starting point for measuring business impact. Its three claimed benefits are:
- Less pipeline maintenance: a single feature layer replaces three hand-maintained pipelines. Batch, streaming and serving share declarative definitions, with governance and lineage tracking in Unity Catalog to support auditing in regulated financial services.
- Faster implementation: clone the repository, deploy the bundle and run. Part 1 provides a governed ranker with online feature lookups, while Part 2 adds live in-session intent without a separate feature stack.
- A baseline for A/B testing: compare the streaming, intent-aware ranker against a batch-only model, using incremental accepted offers per eligible session and downstream contribution margin, with complaint and opt-out rates as guardrails.

The article also says that before production, teams must implement eligibility, consent, suitability and suppression controls. Ranking offers is a modeling step, not a final offer decision, and the accelerator demonstrates the feature engineering and serving infrastructure, not these business rules.

> ❌ **Anti-pattern — Treating the ranker as the offer decision:** Showing the top-ranked offer to the customer directly, once the model has scored it. **Why people do it:** The article does not say; a high score can look like a final answer. **Consequence:** The offer can bypass eligibility, consent, suitability and suppression rules that the article says must exist before production. **Fix:** Run ranked offers through those business-rule controls before any offer is shown.

## Get started with the Solution Accelerator
The accelerator lets you start with batch features, then add real-time streaming without rebuilding your feature infrastructure.
- [Part 1 — Feature Views fundamentals](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/notebooks/part1_feature_views): clone the repository, configure a Databricks CLI profile, deploy the bundle and run Part 1 to build a governed ranker with online feature lookups.
- [Part 2 — Real-time streaming](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/notebooks/part2_realtime_two_stage): configure a Unity Catalog Kafka connection and Amazon MSK reachability, then run Part 2. It reuses Part 1's catalog, features, online store and ranker.
- [Demo and monitoring](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/apps/recommender-ui): launch the recommender Databricks app and open the AI/BI latency dashboard.

The accelerator is deployed with [Declarative Automation Bundles (DABs)](https://docs.databricks.com/aws/en/dev-tools/bundles/). The extracted text refers to a deployment command but does not include it. The article notes that deployment order matters, because the app depends on the Part 2 serving endpoints, which the notebooks create at runtime. Everything runs on serverless compute governed by Unity Catalog.

> 📋 **Checklist — Deployment order**
> - [ ] Deploy the jobs.
> - [ ] Run Part 1 and Part 2.
> - [ ] Deploy the app.

> 📋 **Checklist — Part 2 prerequisites**
> - [ ] Configure a Unity Catalog Kafka connection.
> - [ ] Confirm Amazon MSK reachability.

## Learn more about Feature Views
The article says Feature Views let you define features once for point-in-time-correct training and low-latency online serving across AWS, Azure and GCP. Its further reading is the accelerator repository, the Databricks Feature Store documentation, and the documentation for querying route-optimized Model Serving endpoints.

## FAQs
**What are Feature Views?** They let you define feature transformations once and reuse them across model training and online serving. They support batch and streaming features, with point-in-time-correct historical values for training and low-latency access to current values for inference.

**How do Feature Views prevent train/serve skew?** They use shared feature definitions across training and serving, which avoids inconsistencies caused by maintaining separate transformation logic. The `create_training_set` API also uses point-in-time joins to reconstruct feature values as they existed at each historical label, which prevents future data leakage.

**Can Feature Views support real-time streaming features?** Yes. They can continuously aggregate events from Kafka into an online store without a separate hand-written streaming job. The accelerator uses RollingWindow for recent session activity and SawtoothWindow for longer historical windows.

**What performance does the next-best-offer accelerator achieve?** In the benchmark configuration, the ranking endpoint achieved 34 ms p95 latency, including online feature retrieval and inference. It sustained about 56 requests per second, while the Feature Serving endpoint sustained about 390 requests per second before exceeding the 50 ms p95 latency budget.

**What do I need to run the accelerator?** A Databricks workspace, a configured Databricks CLI profile and the accelerator's GitHub repository. Part 1 deploys the batch-feature recommender using Declarative Automation Bundles. Part 2 additionally requires a Unity Catalog Kafka connection and Amazon MSK connectivity to incorporate live session activity.

## 🏛️ For Kenji
> This is your interpretation, not the author's.

Any architecture where batch training and online inference share features faces the same drift risk the article describes, so this pattern applies to feature reviews and model-serving design. The question it raises: which of our current features have separate training and serving implementations, and how would we detect when they diverge?

## 🧠 Mental models
### Declare once, serve everywhere
**The model:** A feature is one declaration, and the offline (training) and online (serving) paths are two read patterns over that declaration, not two implementations.
**How to apply it:** When reviewing an ML system, look for any feature computed in two places, and treat that duplication as a drift risk to remove.

### Concurrency sizes the endpoint
**The model:** The article's rule of thumb is that required concurrency equals 4 × ceil(target RPS ÷ measured RPS at your SLO), because measured throughput tracked concurrency divided by service time to within 15%.
**How to apply it:** Benchmark one endpoint size under your SLO, then apply the formula to find the concurrency for your target load.

### Measure freshness separately from request latency
**The model:** Request latency and feature freshness are different measurements. One is what the user waits for, and the other is how long a click takes to become visible to the model.
**How to apply it:** Report both, and keep each one labeled with the components it includes.

## 🔑 Key takeaways
- One feature declaration serves both point-in-time training and online serving, so the features a model trains on are the features it sees in production.
- Streaming features are aggregated from a Kafka topic into the online store without a hand-written streaming job, according to the article.
- SawtoothWindow keeps a 30-day category count fresh as events arrive, without recomputing the whole history for each event.
- The ranker scored all 40 offers directly on each request, with no candidate-narrowing step. Live streaming added only 3-4 ms, and the complete request fit the 50 ms budget on the smallest endpoint.
- Capacity planning uses concurrency = 4 × ceil(target RPS ÷ measured RPS at SLO). In the article's tests, 150 RPS at a 50 ms p95 SLO needs concurrency 12.
- Streaming events became model-visible in about 0.1 s, a figure the article measures separately from request latency.
- Ranking offers is a modeling step. Eligibility, consent, suitability and suppression controls must be built before production.

## 📚 Reading plan
### From the article
- [Solution Accelerator on GitHub](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator) — the full repository and notebooks, the starting point for everything in the article.
- [Part 1 — Feature Views fundamentals](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/notebooks/part1_feature_views) — the batch-feature ranker and its feature declarations.
- [Part 2 — Real-time streaming](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/notebooks/part2_realtime_two_stage) — the streaming extension and its Kafka prerequisites.
- [Demo and monitoring](https://github.com/databricks-industry-solutions/feature-views-nbo-accelerator/tree/main/apps/recommender-ui) — the recommender app and the latency dashboard.
- [Introducing Feature Views](https://www.databricks.com/blog/introducing-feature-views) — the background on the feature definitions the article builds on.
- [Feature Views](https://docs.databricks.com/aws/en/machine-learning/feature-store/feature-views) — the reference for the window and column declarations in Step 1.
- [Feature Engineering SDK](https://docs.databricks.com/aws/en/machine-learning/feature-store/python-api) — the API behind the point-in-time training set.
- [Declarative Automation Bundles (DABs)](https://docs.databricks.com/aws/en/dev-tools/bundles/) — the deployment method the accelerator uses.
- [Query route-optimized Model Serving endpoints](https://docs.databricks.com/aws/en/machine-learning/model-serving/query-route-optimization) — the endpoint type behind the ranking and feature serving.

### To go further
- Point-in-time joins for training data — how historical labels are matched to feature values without leakage.
- Train/serve skew — the failure mode this design is meant to remove.
- Little's law — the concurrency relationship behind the article's sizing formula (added by me; the article does not name it).
- Scale-to-zero and cold starts in model serving — relevant to the tens-of-seconds first-request delay the article reports.
- Consent, suitability and suppression controls for financial offers — the business rules the article says must be added before production.
