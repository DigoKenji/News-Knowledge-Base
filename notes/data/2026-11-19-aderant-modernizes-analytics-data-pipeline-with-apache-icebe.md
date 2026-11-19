---
type: Article Summary
title: Aderant modernizes analytics data pipeline with Apache Iceberg and AWS Lambda
description: Splitting gold tables by query complexity lets an event-driven Iceberg pipeline deliver near real-time analytics at lower cost.
resource: https://aws.amazon.com/blogs/architecture/aderant-modernizes-analytics-data-pipeline-with-apache-iceberg-and-aws-lambda
tags:
- data
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/aderant-modernizes-analytics-data-pipeline-with-apache-iceberg-and-aws-lambda
  title: Aderant modernizes analytics data pipeline with Apache Iceberg and AWS Lambda
  author: AWS Architecture
status: stable
source_url: https://aws.amazon.com/blogs/architecture/aderant-modernizes-analytics-data-pipeline-with-apache-iceberg-and-aws-lambda
source: AWS Architecture
source_type: blog
published: '2026-11-19'
domain: data
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d810b8b6bc06a421b2fb8
---

## 🎯 Core thesis
Splitting gold tables by query complexity and running them on an event-driven, Iceberg-based medallion pipeline delivers near real-time analytics for a multi-tenant SaaS product, with faster queries and lower pipeline cost than the alternatives the author measured.

## 🧭 Why the author wrote it
Aderant's Stridyn analytics tool serves 700 client environments, and its batch pipeline took about 15 minutes per cycle, so clients saw operational changes late when making billing and matter decisions. The post, co-written with Aderant, describes the redesign and the benchmarks behind it. If the thesis holds, other analytics teams running multi-tenant SaaS can reach minutes-level freshness without rewriting complex SQL or adopting streaming as the default.

## The challenge: Batch processing limiting analytics freshness
Stridyn serves 700 client environments running Expert Sierra, Aderant's cloud-based legal practice management product. It reads from 266 source tables in Microsoft SQL Server databases and delivers financial reports, billing analytics and operational dashboards to law firms worldwide.

### Batch processing cycle
A full processing cycle took approximately 15 minutes, and batch job overhead added latency before transformation could begin. Freshness is capped by the cycle length: a change made just after a cycle starts waits for the whole cycle before it reaches analytics.

> ❌ **Anti-pattern — Fixed batch cycles for freshness-sensitive data:** Running the whole pipeline on a fixed schedule, with batch overhead before any transformation starts. **Why people do it:** Not stated in the article. **Consequence:** Clients experience delays between operational changes and analytics visibility. **Fix:** Event-driven ingestion, where each new file triggers processing of its own changes.

### Real-time availability for clients
Clients experienced delays between operational changes and analytics visibility, which affected time-sensitive billing and matter management decisions. The stated goal is data accessible within minutes from source to target, without changing the end-user experience.

### Slow complex queries on external tables
Reports using window functions, joins across multiple fact tables and common table expressions (CTEs) were slow when they queried external tables. These query shapes later drive the design of the 34 complex gold tables.

### Scale of CDC volume
The architecture struggled with custom data, schema changes and a growing volume of change data capture (CDC) events, which already reached 380 million per month across all tenants.

## The solution: Event-driven medallion architecture with Apache Iceberg
The new pipeline has three layers, bronze, silver and gold, all built on Apache Iceberg tables stored in Amazon S3. AWS Lambda handles event-driven CDC processing, AWS Glue handles transformations, and Amazon Redshift serves final analytics.

### Medallion architecture
A medallion architecture organizes data into bronze, silver and gold layers, and each layer is built from the one before it. Bronze receives CDC payloads, silver holds normalized current-state records, and gold holds aggregates shaped for reports.

> 🔎 Added context: The bronze, silver and gold names are conventional labels; the article describes what each layer holds rather than defining the term.

### Apache Iceberg tables on Amazon S3
> 🔎 Added context: Apache Iceberg is an open table format whose metadata records which data files make up each table version, which enables commits and snapshots over files in object storage.

The article uses Iceberg for all three layers, so one format governs ingestion, transformation and gold aggregates. Amazon S3 holds the data, and the AWS Glue Data Catalog registers table metadata across the layers.

### Lambda, Glue and Redshift roles
Lambda processes individual CDC events into bronze, Glue runs set-based incremental transformations between the layers, and Redshift serves dashboards. Small per-event writes go to functions, while transformations go to Spark jobs.

## How it works: The lifecycle of a data change
The article follows one change from source database to dashboard in five stages.

### CDC capture and delivery
> 🔎 Added context: Change data capture reads a database's change stream so that inserts, updates and deletes flow downstream as they happen.

AWS Database Migration Service (AWS DMS) captures changes from the Microsoft SQL Server databases and writes Parquet files to an S3 landing zone, organized by table and tenant. The organization by table and tenant is what later lets processing stay per-tenant without separate pipelines.

### Event-driven ingestion (raw to bronze)
An S3 event notification sends an Amazon Simple Queue Service (Amazon SQS) message when a file lands, and that message invokes a Lambda function. The function processes the CDC payload and writes it to bronze Iceberg tables in single-digit seconds per event. SQS buffers burst traffic and provides dead-letter queue capabilities for failed events.

### Bronze-to-silver transformation
AWS Glue jobs read only incremental data from bronze tables, using watermarks kept in Amazon DynamoDB. A watermark records how far processing has got, so the next run starts from that point. The jobs apply normalization and deduplication through Iceberg MERGE operations, producing current-state silver tables with ACID adherence.

> 🔎 Added context: A MERGE statement updates rows that match a condition and inserts the rest, in one operation.

### Silver-to-gold aggregation
The gold layer uses two approaches, chosen by table complexity. Incremental AWS Glue jobs handle 77 standard gold tables, and Iceberg materialized views with snapshot comparison handle 34 tables with complex query patterns that the author says cannot be easily incrementalized.

### Gold-to-Amazon Redshift delivery
Delta changesets from materialized view snapshots are written to native Amazon Redshift tables, where columnar storage, zone maps and sort keys deliver sub-100-millisecond query performance for end-user dashboards.

> 🔎 Added context: Zone maps store per-block minimum and maximum values so the engine can skip blocks, and sort keys order data on disk to make those skips effective.

## Why Apache Iceberg
Aderant chose Apache Iceberg as the table format across all layers for three reasons.

### ACID transactions for concurrent operations
With 700 tenants generating CDC events at the same time, Iceberg's snapshot isolation helps prevent conflicts when several Lambda functions write to the same tables.

> 🔎 Added context: Snapshot isolation means each reader sees one consistent table version, and each commit becomes visible all at once.

### Time travel and snapshot comparison
For the 34 complex gold tables, Iceberg's snapshot metadata identifies deltas without manual watermarking. The system compares consecutive snapshots to find exactly which rows changed, then writes only those deltas to Amazon Redshift. The heading names time travel, but the body's use is snapshot comparison.

### Schema evolution without pipeline disruption
As Aderant onboards new data sources and clients customize their schemas, Iceberg handles column additions and type changes without full table rebuilds. The article presents this as keeping the pipeline running through schema change, and gives no figures for it.

## Why AWS Lambda for CDC processing
After evaluating three architectural approaches, the team chose Lambda for cost, latency and operational simplicity. The article does not name the other two approaches.

### Pay-per-invocation serverless compute
The pay-per-invocation model fits CDC workloads, where event volume fluctuates through the day. Capacity scales to zero in quiet periods and handles 10x seasonal spikes without pre-provisioned capacity, so cost stays proportional to actual data volume rather than reserved infrastructure.

### Docker container image deployment
Lambda functions are deployed as Docker container images through Amazon Elastic Container Registry (Amazon ECR). The container model accommodates large library dependencies for Iceberg operations, including PyIceberg and pandas, which would be awkward to package otherwise.

## Architecture
The figure caption reads: "Figure 1: End-to-end data flow through the medallion architecture, from source databases to the Amazon Redshift serving layer."

> 🔎 Added context: The Figure 1 image is not in the extracted text, so the flow below is described from the surrounding paragraphs.

### Source layer and SQS buffer
AWS DMS replicates CDC events from Microsoft SQL Server into an S3 landing zone as Parquet files. When files arrive, S3 Event Notifications publish messages to an SQS queue, which buffers bursts and holds failed events in a dead-letter queue.

### Bronze ingestion with Lambda containers
Lambda functions, deployed as Docker containers from Amazon ECR, consume SQS messages and write bronze Iceberg tables in S3. They use the PyIceberg library for ACID-aligned writes. DynamoDB stores watermark state to track progress and help prevent duplicate writes.

### Scheduled orchestration with EventBridge
Amazon EventBridge triggers a decision Lambda on a 5-minute schedule, which checks whether bronze-to-silver Glue jobs should run based on data availability. AWS Glue 5.0 jobs (Apache Spark 3.5 on Python 3.11) then read bronze incrementally, apply MERGE operations and write silver. The article's flow is event-driven at ingestion but schedule-driven from bronze onward.

### Silver-to-gold paths
EventBridge detects completion of the bronze-to-silver jobs and triggers a second decision Lambda for the 77 standard gold tables. That Lambda launches incremental Glue jobs that write to Redshift native tables through the Amazon Redshift Data API. For the 34 complex tables, Iceberg materialized views run the transformation in SQL, and a separate Glue job compares snapshots to write delta changesets to Redshift.

### Redshift Serverless serving layer
Amazon Redshift Serverless holds native tables with columnar storage, zone maps and sort keys, giving sub-100-millisecond query latency. A GraphQL Data API layer sits between Redshift and the dashboards, serving 42 pre-built reports, including financial statements, billing analytics and firm overview dashboards.

### Observability, security and delivery
Amazon CloudWatch provides end-to-end observability, with custom metrics for processing latency, error rates and throughput per tenant. AWS Identity and Access Management (IAM) enforces least-privilege access, and S3 data is encrypted with server-side encryption through AWS Key Management Service (AWS KMS). The infrastructure is defined in Terraform and deployed through GitHub Actions continuous integration and continuous delivery (CI/CD) pipelines.

## Iceberg materialized views: Solving the complex query problem
The 34 complex gold tables involve LAG and window functions, joins across multiple fact tables, UNIONs with aggregations, and CTEs. Traditional incremental processing would require redesigning each query's logic, so the team uses Iceberg materialized views as an intermediate step instead.

> ❌ **Anti-pattern — Rewriting complex analytical SQL to make it incremental:** Redesigning each complex query's logic so that it can be processed incrementally. **Why people do it:** Not stated in the article; the pull is that incremental processing seems to require it. **Consequence:** Every complex query needs its own redesign, which the article says traditional incremental processing would require for these 34 tables. **Fix:** Materialize the query as an Iceberg materialized view and extract deltas from snapshots.

### Iceberg materialized views
> 🔎 Added context: A materialized view stores a query's result, and refreshing it updates that stored result.

The team creates materialized views with standard Spark SQL (CREATE MATERIALIZED VIEW AS SELECT ...) against silver Iceberg tables. The article describes this as a five-step process:

- Create the Iceberg materialized view against silver-layer tables.
- Iceberg tracks source changes automatically, with no manual watermarking.
- An AWS Glue job refreshes the view every 2–5 minutes, bypassing the 1-hour minimum for automatic refresh.
- Delta extraction compares Iceberg snapshots between refreshes.
- Only the changed rows are written to Amazon Redshift as native table updates.

### Snapshot comparison for deltas
The view handles the computational complexity, and snapshot comparison identifies what changed. Together they keep incremental processing without rewriting the analytical query, which is the article's central argument for this approach.

## Quantifiable results
The article reports gains across three dimensions: query performance, pipeline latency and infrastructure cost.

### Query performance
The team benchmarked 16 analytical queries, from simple aggregations to complex pivots over 9 million rows, comparing Redshift native tables with external tables queried through Redshift Spectrum. Native tables were 10–26x faster across all query types, consistently returning results in under 100 milliseconds regardless of dataset size. The largest gain was 25.8x on smaller, frequently accessed datasets, and complex pivot queries with window functions still gained 13.7x.

The benchmarks ran on Redshift Serverless at 16 RPU. The author expects provisioned clusters with dedicated compute and persistent caching to perform better, but this was not measured.

#### Redshift native tables
Native tables store data in columnar form with zone maps and sort keys, which is why they beat external tables in this benchmark. The article attributes the gain to these table features and to the serving layer's design.

#### Amazon S3 Express One Zone
The team also validated S3 Express One Zone for workloads that stay on S3-based Iceberg tables without Redshift. It reduced query latency by 31–78 percent compared with S3 Standard, with the greatest gains on complex queries involving multiple table scans.

### Pipeline latency
The largest operational improvement is end-to-end time, down from approximately 15 minutes per full processing cycle to single-digit seconds from source database to bronze Iceberg tables. Gold layer refreshes complete in under 5 minutes for initial loads and under 1 minute for incremental updates. The system sustains throughput of 186,000+ events per second across all 266 tables and 700 tenants.

### Infrastructure cost
Choosing Lambda-based event processing over streaming alternatives cut pipeline compute costs by 77 percent. The serverless, pay-per-invocation model scales to zero in quiet periods and handles 10x seasonal spikes without pre-provisioned capacity.

> ❌ **Anti-pattern — Streaming as the default for every tenant (implied by the comparison):** Sending every tenant's CDC events through a streaming pipeline. **Why people do it:** Not stated in the article. **Consequence:** The article's comparison implies higher pipeline compute cost, measured as the 77 percent difference. **Fix:** Keep standard tenants on Lambda and move only the highest-volume tenants to streaming, as the article plans.

## In production: Multi-tenant operations
The architecture runs in production today, processing data for 700 client environments across 266 source tables. Three aspects stand out.

### Tenant isolation with shared infrastructure
Each tenant's CDC events flow through the same Lambda functions and AWS Glue jobs. Iceberg's partition-by-tenant structure maintains data isolation, and configuration files drive per-tenant behavior without code changes.

### Graceful handling of complex reports
The Expert Bill Report involves multiple dimension tables and fact aggregations. It processes 900,000 records in approximately 5 minutes for initial load, and subsequent incremental runs handle only changed records in seconds.

### Self-healing watermark management
DynamoDB watermarks track the last processed timestamp per table per tenant. If a Lambda function fails, the watermark remains unchanged, and the next invocation reprocesses from the correct point. The author says this provides exactly-once semantics without complex distributed transaction logic.

> 🔎 Added context: Exactly-once semantics means that each change takes effect once even when processing is retried.

## Looking ahead
Aderant plans to keep expanding the tool. These are plans, not described as built features.

### Streaming analytics for high-volume tenants
The Lambda architecture supports a hybrid model. Tenants with the highest event volumes can migrate to Amazon Kinesis streaming for sub-second latency, while standard tenants stay on the cost-effective Lambda path.

### AI-powered analytics
The plan integrates Amazon Bedrock for natural language querying of the gold layer, so law firm users can ask questions about billing and matter data conversationally.

### Automated data quality
The validation framework will expand to continuously monitor data consistency between pipeline layers, with automated alerting when discrepancies are detected.

## Conclusion
The journey moved from batch-oriented processing to an event-driven, Iceberg-based pipeline. CDC events now move in single-digit seconds instead of 15-minute batch cycles, query performance is 10–26x faster through native Redshift tables, and infrastructure costs are 77 percent lower than streaming alternatives.

The key insight is that not every table needs the same processing strategy. Aderant uses incremental AWS Glue jobs for straightforward tables and Iceberg materialized views with snapshot comparison for complex patterns. This delivers near real-time freshness across all 111 gold tables (77 plus 34) without rewriting complex SQL logic.

The conclusion also presents Iceberg's ACID alignment, time-travel capabilities and schema evolution as a strong foundation for production pipelines at scale. The introduction promises a decision framework, but the body has no separate framework section; the closest equivalents are the two-path gold split and the three reasons for Iceberg.

The conclusion points readers to the Glue Iceberg documentation, the Lambda and Glue service pages and an AWS Samples reference architecture, all listed in the reading plan below.

## About the authors
The extracted text includes this heading with no body, so there is nothing to expand.

## 🧠 Mental models
### Split the pipeline by table complexity
**The model:** Not every table needs the same processing strategy. Classify each derived table by how hard its query is to make incremental, then pick the cheapest mechanism that fits.
**How to apply it:** Before building a refresh path, sort tables into standard and complex groups, and reserve materialized views with snapshot diffs for the complex group.

### The watermark as the only progress record
**The model:** A per-table, per-tenant timestamp is the single record of what has been processed. A failed run leaves it unchanged, so the next run resumes from the right point.
**How to apply it:** Advance the watermark only after the write succeeds, and have each job read only the data past it.

### Pay per event, not per reserved capacity
**The model:** For bursty CDC workloads, cost should track event volume and fall to near zero in quiet periods.
**How to apply it:** Use pay-per-invocation compute for standard tenants and reserve streaming capacity for the highest-volume ones, as the article plans.

## 🔑 Key takeaways
- Replacing the 15-minute batch cycle with CDC-driven, event-based ingestion brought changes into bronze Iceberg tables in single-digit seconds.
- The gold layer uses two strategies: incremental Glue jobs for 77 standard tables and Iceberg materialized views with snapshot comparison for 34 complex tables, covering all 111 without rewriting complex SQL.
- Iceberg snapshot comparison identifies changed rows without manual watermarking, so only the deltas are written to Redshift.
- In the author's benchmark of 16 queries on Redshift Serverless at 16 RPU, native tables were 10–26x faster than Spectrum external tables.
- Lambda pay-per-invocation processing was credited with a 77 percent reduction in pipeline compute cost against streaming alternatives; the article gives no absolute cost figures.
- Per-table, per-tenant DynamoDB watermarks let a failed Lambda run resume from the correct point without distributed transaction logic.
- Multi-tenant isolation comes from partitioning by tenant on shared Lambda and Glue code paths, with configuration files driving per-tenant behavior.

## 🏛️ For Kenji
This is your interpretation, not the author's: the two-path gold split is a reusable rule for architects designing data platforms for multi-tenant products, where each derived table is classified by query complexity before its refresh mechanism is chosen. The speed and cost figures (10–26x, 77 percent) are relative, with no absolute baselines, so any reuse would need your own measurements. The question it raises: how does the exactly-once claim hold when a write succeeds but the watermark update fails, and what would you test to confirm it?

## 📚 Reading plan
### From the article
- [Aderant modernizes analytics data pipeline with Apache Iceberg and AWS Lambda](https://aws.amazon.com/blogs/architecture/aderant-modernizes-analytics-data-pipeline-with-apache-iceberg-and-aws-lambda) — the original post, with the Figure 1 architecture diagram this note can only describe in text.

> 🔎 Added context: The article's cited links have no URLs in the extracted text, so they are listed below by title.

### To go further
- Using Apache Iceberg tables in AWS Glue — the author's recommended starting point for Iceberg on AWS.
- AWS Lambda and AWS Glue service documentation — the two services the architecture depends on most.
- AWS Samples repository on GitHub — the author's reference architecture with sample code.
