---
type: Article Summary
title: How Rakbank built centralized observability at scale with Amazon OpenSearch Service
description: Rakbank tiers append-only logs across hot, UltraWarm and S3 cold storage to hold seven-year retention at about 55 percent less cost than all-hot.
resource: https://aws.amazon.com/blogs/architecture/how-rakbank-built-centralized-observability-at-scale-with-amazon-opensearch-service/
tags:
- cloud-infra
- observability
- model-context-protocol
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/how-rakbank-built-centralized-observability-at-scale-with-amazon-opensearch-service/
  title: How Rakbank built centralized observability at scale with Amazon OpenSearch Service
  author: AWS Architecture
status: stable
source_url: https://aws.amazon.com/blogs/architecture/how-rakbank-built-centralized-observability-at-scale-with-amazon-opensearch-service/
source: AWS Architecture
source_type: blog
published: '2026-11-17'
domain: cloud-infra
subdomain: observability
concepts:
- model-context-protocol
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d811386fef0fb4afbfe3f
---

## 🎯 Core thesis
Rakbank runs a cross-account, Multi-AZ Amazon OpenSearch Service logging platform that ingests about 150 GB of logs a day and keeps seven years of retention by tiering indices from hot EBS-backed nodes to UltraWarm to S3-backed cold storage, which the authors say cuts cost about 55 percent against an all-hot cluster.

## 🧭 Why the author wrote it
The platform's log volume grew from about 5 GB/day to about 150 GB/day with 25 dashboards and about 25 concurrent users, and compliance requires seven years of retention. The article argues that a single hot cluster would be expensive at that scale, while a hot/warm/cold design with an append-only data model gives predictable capacity-based cost, fast recent search and cheap long-term retention. If the thesis holds, observability cost stops growing linearly with how long you keep data.

## Context and workload profile
The workload runs as containerized microservices on Amazon Elastic Kubernetes Service (Amazon EKS), and the observability requirements come from a few hard numbers. Logs now average roughly 150 GB per day, up from about 5 GB/day before the migration. That produces about 9 TB retained in OpenSearch Service over a 60-day window (150 GB/day × 60 days), with the most recent 30 days directly searchable. Retention is seven years, aligned with Central Bank of the UAE (CBUAE) requirements and Rakbank's internal data-retention policy. At 150 GB per day the authors estimate about 54 TB of log data archived per year, or about 383 TB over seven years. On the consumption side, 25 dashboards serve about 25 concurrent users.

### Append-only logs
The article's key data property is that logs are append-only. Once a log document is written, it is never updated or deleted, so the platform has no update or delete-by-query churn. The article notes that where regulatory write-once-read-many (WORM) immutability is required, the S3 archive bucket can enforce it with Amazon S3 Object Lock.

> 🔎 Added context: WORM storage means data can be written once and read many times, but not modified or deleted for a set period. Amazon S3 Object Lock is the AWS feature that enforces this at the bucket or object level.

The authors treat append-only as architecturally significant for two reasons. First, indices stop receiving writes once they roll over, so they can be force-merged to a single segment per shard before moving to the warm tier, and that is safe. Second, there is no update overhead as indices move down the tiers.

## Telemetry pipeline: Fluent Bit
The team follows a principle the article summarizes as "instrument once, standardize everywhere." It uses one Fluent Bit standard across all banking domains (retail, business and wholesale), so every domain produces telemetry the same way.

### Fluent Bit as a DaemonSet
Fluent Bit runs as a DaemonSet on the EKS cluster. A DaemonSet places one pod on every node, so each node's collector tails container stdout and stderr. Before routing, it enriches each record with Kubernetes metadata: pod, namespace and labels. That enrichment is what lets users filter logs by service or team later.

> 🔎 Added context: Fluent Bit is a lightweight open-source log processor and forwarder. In Kubernetes, a DaemonSet is the standard way to run a per-node agent.

### Dual-write parallel archive
Fluent Bit does a dual write. One path ships records to the OpenSearch Service ingest endpoint, which is the hot tier. A parallel path lands raw log objects directly in Amazon S3 as a durable backup that does not depend on the search tier. If the search cluster fails or is rebuilt, the raw archive still exists.

## Cross-account topology and private connectivity
The design separates the workload from its observability plane across two AWS accounts.

- **Application account:** the Amazon EKS cluster running the app pods in the Application virtual private cloud (VPC), the Fluent Bit DaemonSet for log collection and routing, and a parallel archive path to Amazon S3 (the Log Archive and Backup bucket).
- **Observability account:** a dedicated OpenSearch Service centralized logging domain in the OpenSearch Service VPC, spanning three Availability Zones, plus OpenSearch Service Dashboards as the visualization layer, routed to the hot data nodes.

### Private transport over AWS Transit Gateway
Cross-account log flow goes through AWS Transit Gateway, so all communication stays private within AWS and no traffic leaves to the public internet. In practice, Transit Gateway route tables provide reachability between the application VPC and the OpenSearch Service VPC.

> 🔎 Added context: AWS Transit Gateway is a hub that connects many VPCs and accounts through one routing layer. Traffic passes over AWS's network instead of the public internet.

### Scoped network access
Access is limited at the VPC boundary by security groups and network ACLs. They permit only the required ingest traffic, HTTPS on port 443, to the domain. The article describes this as the network-layer control, which sits beneath the identity controls in the next section.

## Identity and cross-account authorization flow
The article describes access as governed by three layers of the OpenSearch Service security model: network isolation (VPC and private Transit Gateway connectivity), the domain's resource-based access policy, and fine-grained access control (FGAC) on the VPC domain.

### Single sign-on for people
End users reach OpenSearch Service Dashboards through SAML 2.0 single sign-on federated to Rakbank's corporate Active Directory. Access follows existing enterprise identities and group memberships, so people do not need separate OpenSearch Service credentials.

> 🔎 Added context: SAML 2.0 is an XML-based federation standard that lets an identity provider, here Active Directory, vouch for users to a service provider, here the OpenSearch domain, without sharing passwords.

### Separate roles for writing and reading
Log ingestion from the application account is authorized separately from user access. The write path and the read path use distinct, least-privilege roles. The article's summary is that all telemetry flows privately within AWS, while access stays centrally governed in Active Directory.

## Cluster topology (per-AZ node roles)
The domain is a 3-AZ deployment with dedicated cluster-manager nodes separated from data nodes.

| Role | Per-AZ instance | Count | Purpose |
|---|---|---|---|
| Cluster manager | m8g.large.search | 3 (1/AZ) | Cluster state, shard allocation, no data |
| Hot data node | m8g.2xlarge.search | 3 (1/AZ) | Active indexing + real-time query |
| UltraWarm node | ultrawarm1.medium.search | 3 (1/AZ) | Read-only, S3-backed warm segments |
| Cold tier | No nodes | — | Detached S3-backed storage, managed by OpenSearch Service |

### Dedicated cluster-manager nodes
Cluster-manager nodes handle cluster state and shard allocation but hold no data. The article's reason for separating them is that they isolate cluster-state management from indexing and query load. The authors say this matters once the hot tier is taking about 150 GB/day.

### Graviton instances
The hot data and cluster-manager nodes use Graviton (m8g) instances, which the article chooses for better price/performance.

> 🔎 Added context: Graviton is AWS's family of ARM-based processors. The "m8g" name indicates a general-purpose Graviton instance family.

## Storage tiering: right data · right store · right cost
The core mechanism is OpenSearch Service multi-tier storage. Indices age through tiers under Index State Management (ISM) policies keyed to index age.

| Tier | Backing | Retention | Access pattern | Instance/store |
|---|---|---|---|---|
| Hot | Amazon Elastic Block Store (Amazon EBS) (gp3) volumes on data nodes | 0–15 days | Real-time dashboards, active troubleshooting, alerting | m8g.2xlarge.search |
| Warm (UltraWarm) | Amazon S3 + local cache | 15–30 days | Read-only historical analysis. Cost-optimized, still fully searchable | ultrawarm1.medium.search |
| Cold | Amazon S3 (detached, no nodes) | 30–60 days | Older logs for on-demand investigation. Attach to UltraWarm when needed | S3-backed, 0 compute at rest |

### Hot tier
The hot tier runs on gp3 EBS volumes attached to data nodes. It serves real-time dashboards, active troubleshooting and alerting, so it is the only tier where the platform pays for full-speed local storage.

### Hot-to-UltraWarm transition
ISM moves indices to UltraWarm at about 15 days. UltraWarm keeps primary shard data in S3 and caches hot blocks locally. Queries still run, but with higher latency and at much lower cost per GB. Because rolled-over indices are no longer written to, they are force-merged before migration, which improves warm-tier read performance.

### UltraWarm-to-cold transition
At about 30 days, indices detach to the cold tier. Cold storage is pure S3 with no dedicated compute. Cold indices are not directly searchable. To query one, you attach it back to UltraWarm nodes on demand. The article notes that querying the raw S3 archive without reattaching is a separate path that uses Amazon Athena.

### Compliance archive with S3 and Athena
Everything also lands in a separate Amazon S3 bucket that holds the full seven-year window. It is queried with Amazon Athena at the lowest cost per GB. The article's point is that this decouples how long data must be kept from how much hot compute is paid for.

> 🔎 Added context: Amazon Athena is a serverless SQL query service that reads data directly from S3, so you pay per query scanned rather than for standing compute.

### Durability without replicas on warm and cold
The article says S3 durability lets the design run zero replicas on the warm and cold tiers and remain durable, because S3 handles redundancy. That cuts cost further. UltraWarm is read-only as a warm tier, and Graviton (m8g) powers the hot and cluster-manager nodes.

## Alerting
Hot-tier data drives OpenSearch Service alerting, with email and Microsoft Teams as destinations for real-time operational alarms. Alerting is limited to the hot tier, which matches the hot tier's role for real-time operations.

## Production sizing and cost model
The sizing target is 150 GB/day on average with 60-day retention, of which 30 days is directly searchable. The cluster is provisioned with headroom for a peak of about 200 GB/day, and the node sizing follows from that peak. Rakbank evaluated two topologies.

**Option 1: Hot + UltraWarm (recommended)**
- 3 × m8g.2xlarge hot data nodes.
- 3 × ultrawarm1.medium.search nodes plus the cold tier.
- 3 × m8g.large dedicated cluster-manager nodes.
- About 55 percent savings compared with all-hot.

**Option 2: All hot**
- 3 × r8g.4xlarge, about 8,320 GB each.
- 3 × m8g.large admin nodes, with no warm tier.
- Higher cost, with all data resident on hot nodes.

Pricing is On-Demand, and Reserved Instances reduce it further. The article's takeaway is that in a 9 TB, 60-day window, most data is rarely queried after the first couple of weeks. Moving older indices to S3-backed warm and cold tiers removes the need to keep the full working set on hot r8g nodes. The authors attribute the roughly 55 percent difference to that change.

## Scaling the observability architecture with OpenSearch Service
As the platform grew, the architecture had to meet three requirements: predictable cost at high ingestion volume, repeated dashboard queries without per-query cost coupling, and multi-tier retention from real-time access through seven-year archival. The article says OpenSearch Service meets these with an instance-based model and hot/warm/cold tiering. Ingestion and queries run on provisioned capacity, and older indices age into S3-backed tiers at low cost per GB. The result is a capacity-based cost model that scales predictably as data volume and dashboard usage grow.

## Resilience: Multi-AZ and the Standby trade-off
The article compares two deployment options.

| Deployment option | SLA | Failover | Nodes |
|---|---|---|---|
| Multi-AZ (no Standby) | 99.9% out-of-the-box | automatic | 3 AZ · 3 data nodes + 3 dedicated cluster-manager nodes |
| Multi-AZ with Standby | 99.99% | < 1 min automatic on AZ failure | 3 AZ · 2 data active + 1 standby |

### Multi-AZ without Standby
Rakbank runs its production domain without Standby. Data nodes are spread across all three Availability Zones, and replica shards provide redundancy. All data nodes keep serving search.

### Multi-AZ with Standby
With Standby enabled, only two of the three data nodes serve search requests, and the third is reserved for failover. The article describes this as a 33 percent reduction in active search capacity in exchange for the higher SLA. Standby also requires at least two replicas per index, which increases storage. It is supported only on specific instance families, so the article advises checking the current list before planning. If a 99.99 percent SLA is not a hard requirement, running without Standby keeps all data nodes serving search.

## AI-powered observability capabilities
The article says Rakbank is evaluating newer machine-learning and agent-driven features that can layer on top of the tiered store.

- **AI Assistant:** natural-language to query generation, for example "Show error spikes in the last 24h", backed by Amazon Bedrock.
- **MCP Server and Agent investigator:** DevOps agents autonomously run PPL queries for root-cause analysis. A Memory Store keeps context across agent conversations.
- **Direct Query:** queries Amazon S3 data without ingesting it into OpenSearch Service. It is an alternative to Amazon Athena for the S3 archive.
- **Trace Correlation:** available as trace collection is added to the pipeline. It unifies logs, traces and metrics in one view, with automatic trace-log correlation and service maps.
- **Query surface:** PPL, DSL, SQL and PromQL, with integration into Amazon Managed Service for Prometheus and Amazon OpenSearch Ingestion for high-performance ingest.

> 🔎 Added context: PPL (Piped Processing Language) is a pipe-style query language for OpenSearch, and DSL refers to OpenSearch's JSON query DSL. The article presents these as options, not as things the team has already adopted.

## Outcomes
Running on the Fluent Bit standard, the platform serves three audiences from one view. Support operations get faster triage and guided troubleshooting. Engineering teams use it for root-cause analysis and debugging across the request path. The business gets journey trends and actionable insights. The article's net results are lower mean time to resolution (MTTR), higher reliability, data-driven insights and better operational efficiency. Rakbank measured faster performance after upgrading across OpenSearch Service versions (1.3 to current), with single-digit-millisecond latencies for lexical and vector queries in its own environment.

## Engineering lessons learned
The article ends with four lessons.

- **Multi-tier from day one.** Design hot, warm and cold tiers up front. The article says retrofitting tiering onto a live 150 GB/day cluster is significantly harder than starting with ISM policies in place.
- **Size for peak, not today.** Ingestion grows as more microservices come online, so provision headroom in the hot tier and shard count.
- **Model the Standby trade-off explicitly.** The article's numbers: 99.99 percent costs 33 percent of active search capacity and requires two replicas per index. Decide per workload whether that SLA is worth it.
- **Exploit the append-only advantage.** Append-only logs are the recommended candidate for multi-tier storage, with no update overhead, a clean force-merge before warm migration and predictable rollover.

## 🧠 Mental models
### Right data, right store, right cost
**The model:** Each tier stores data at the cost and performance the data's access pattern justifies. Recent data sits on fast local storage, and old data sits on S3 with no compute.
**How to apply it:** For each data class, ask how long it is queried in real time, how long it must be kept, and who queries it. Map each answer to a tier, and separate retention from hot compute.

### Append-only data makes tiering cheap
**The model:** When data is never updated, rolled-over indices are immutable. That makes force-merge safe, avoids update overhead as data moves down tiers, and keeps rollover predictable.
**How to apply it:** Before choosing a tiering design, check whether the data is written once. If it is, plan force-merge and tier transitions into the lifecycle policy.

### Price the SLA in capacity
**The model:** A higher availability SLA can cost active capacity. Standby trades 33 percent of search capacity for a 99.99 percent SLA in the article's configuration.
**How to apply it:** Write down the SLA the workload needs and the capacity it gives up for it, then decide per workload instead of by default.

## 🔑 Key takeaways
- Tiering hot, UltraWarm and S3-backed cold storage by index age cut Rakbank's cost by about 55 percent against an all-hot cluster, in the authors' sizing model.
- Append-only logs make force-merge before warm migration safe and remove update overhead as indices move down the tiers.
- Cross-account telemetry travels privately over AWS Transit Gateway, with security groups and network ACLs allowing only HTTPS on port 443 to the domain.
- Access is governed separately from ingestion. Users sign in through SAML 2.0 federation to Active Directory, and the write and read paths use distinct least-privilege roles.
- Multi-AZ with Standby buys a 99.99 percent SLA at the cost of 33 percent of active search capacity and two replicas per index.
- Designing tiering up front is much cheaper than retrofitting it onto a live cluster.

## 🏛️ For Kenji
This is a useful reference for a multi-account, regulated-retention logging design on AWS, especially where the data is append-only and must be kept for years. For an architect, the reusable part is the decision structure: separate retention from hot compute, set the SLA trade-off explicitly, and keep the write and read paths on separate roles. Question it raises: which of your workloads have data that is never updated and could move to S3-backed tiers, and what is the access pattern that would justify keeping any of it hot? _This is your interpretation, not the author's._

## 📚 Reading plan
### From the article
The article text cites no links, so there is nothing to follow from the article itself.

### To go further
- Amazon OpenSearch Service Index State Management (ISM) documentation — to see how the tier-transition policies in this design are written.
- Amazon OpenSearch Service UltraWarm and cold storage documentation — to check the current instance list and the attach-on-demand behavior for cold indices.
- Amazon OpenSearch Service Multi-AZ with Standby documentation — to verify the supported instance families and replica requirements before planning a Standby deployment.
- Amazon S3 Object Lock documentation — to see how write-once-read-many retention is enforced for the compliance archive.
