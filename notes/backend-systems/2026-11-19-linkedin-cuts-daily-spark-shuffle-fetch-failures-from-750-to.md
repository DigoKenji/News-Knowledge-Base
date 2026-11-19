---
type: Article Summary
title: LinkedIn Cuts Daily Spark Shuffle Fetch Failures from 750 to 20 with Apache Celeborn
description: Moving shuffle storage to dedicated Celeborn workers cut LinkedIn's daily Spark shuffle fetch failures from 750 to 20.
resource: https://www.infoq.com/news/2026/11/linkedin-celeborn-remote-shuffle
tags:
- backend-systems
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/linkedin-celeborn-remote-shuffle
  title: LinkedIn Cuts Daily Spark Shuffle Fetch Failures from 750 to 20 with Apache Celeborn
  author: InfoQ
status: stable
source_url: https://www.infoq.com/news/2026/11/linkedin-celeborn-remote-shuffle
source: InfoQ
source_type: blog
published: '2026-11-18'
domain: backend-systems
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d81539f9cee297624f5f3
---

## 🎯 Core thesis
Moving shuffle storage off the Spark compute cluster and onto a dedicated Apache Celeborn service cut LinkedIn's daily shuffle fetch failures from about 750 to 20, while lowering compute cost and improving runtimes compared with its earlier Magnet implementation.

## 🧭 Why the author wrote it
LinkedIn runs more than 300,000 Spark jobs a day and is moving large backfills toward a unified Flink architecture, which adds shuffle load. The article argues that shuffle storage should scale independently of compute, and that doing so fixes the reliability, cost and runtime problems that came with the older design.

## Scale and the shuffle problem
LinkedIn's offline data platform runs more than 300,000 Spark jobs daily across thousands of production nodes, consuming about 200 petabyte-hours of compute. As the company moves toward a unified Flink architecture for real-time and historical processing, large-scale backfills add more shuffle workloads. Shuffle moves intermediate data between processing stages and can stress networks, disks and application runtimes.

### Shuffle
Shuffle is the step where data produced by one stage of a distributed job is redistributed so the next stage can read the part it needs. In Spark, the producing tasks write their output and the consuming tasks fetch it over the network, so the data passes through storage on every run. The article's concern is that this intermediate data is large, short-lived, and sits on the same machines that run the computation.
> 🔎 Added context: a shuffle is the step where every map task's output is redistributed so that each reducer can read the partition it needs.

## Magnet: the previous push-based shuffle
LinkedIn had earlier improved shuffle reads with Magnet, its push-based shuffle implementation, which it contributed to Apache Spark 3.2. Magnet merges shuffle blocks by reducer partition into larger files, which reduces random disk reads. The limit was location: shuffle storage stayed inside the compute cluster, so the External Shuffle Services competed with compute workloads for resources.

### Magnet (push-based shuffle)
Push-based shuffle changes the read pattern rather than the location of the data. Instead of many small blocks that each reducer must find and read, the blocks are combined by reducer partition into larger files. Fewer and larger reads mean less random disk I/O, which is the gain Magnet delivered. The article shows that this gain alone did not solve the resource contention that came from running shuffle inside the compute cluster.

> ❌ **Anti-pattern — shuffle storage inside the compute cluster:** External Shuffle Services run on the same nodes as compute and share their CPU, memory, disk I/O and network. **Why people do it:** the article does not say; it is the setup Magnet ran on. **Consequence:** shuffle load and compute load contend for the same resources, and shuffle data is tied to executor lifecycles. **Fix:** move shuffle storage to dedicated workers that scale on their own, as Celeborn does.

## Apache Celeborn: shuffle storage separated from compute
With Apache Celeborn, map tasks push their intermediate data to dedicated workers. The workers aggregate and store each partition independently of the compute executors, and reducers fetch the data directly from those workers. Celeborn runs as a separate shuffle service with its own workers and a replicated control plane, unlike Magnet, which relied on merger services inside the Spark cluster. The article says this separation reduces resource contention between compute and shuffle.

### Apache Celeborn
Celeborn is an open-source remote shuffle service for big-data engines, and the article uses it with Spark and Flink. Its main idea is that shuffle data is held by a service, not by the executor that produced it. Storage can therefore grow with the volume of shuffle data, and an executor can finish or fail without losing the data other stages still need. This matters most at LinkedIn's scale, where shuffle volume and compute demand grow at different rates.

### Push to dedicated workers
In this design the push step moves data away from the executor as soon as it is produced. The workers do the merging by partition, so the reducers see a cleaner read path. Because storage is separate, the workers can be sized, upgraded and replaced without touching the compute fleet. The article credits this separation with reducing contention, not with a single named mechanism.

The article quotes Aravind Patnam, a staff software engineer at LinkedIn:
> "Operating it reliably across thousands of nodes and hundreds of thousands of daily applications required substantial engineering beyond the upstream project."

The article also shows a figure captioned "Celeborn architecture at LinkedIn", which is not reproduced in the extracted text.

## Reliable control plane
LinkedIn's deployment uses a five-node Raft-based control plane spread across availability zones. Apache Ratis provides metadata replication, leader election and failover. LinkedIn added network-aware replication, and it integrated Celeborn with its host health platform so that workers near maintenance are deprioritized and degraded workers are excluded. The article credits these changes with a 20-fold reduction in task failures during infrastructure disruptions compared with Magnet.

### Raft-based control plane
The control plane holds the metadata that tells reducers where their data is, so losing it would stall jobs. A Raft-based design keeps that metadata replicated across several nodes and elects one leader to coordinate changes. The five nodes here are spread across availability zones, so a single zone failure does not remove the quorum. Apache Ratis is the library that supplies the replication, election and failover behavior.
> 🔎 Added context: Raft is a consensus algorithm that keeps a replicated log across several nodes and elects one leader to order writes. Apache Ratis is an open-source Java implementation of Raft.

### Network-aware replication and host health
Network-aware replication places copies of shuffle data with the network topology in mind. Integration with the host health platform lets Celeborn avoid workers that are about to go down for maintenance or that are degraded. The article describes this as a way to protect jobs during infrastructure disruptions, which is where the 20-fold reduction in task failures comes from.

## Data plane optimizations
LinkedIn then tuned the data plane. It moved shuffle storage to NVMe SSDs, which cut overall shuffle duration by 40%. It switched from LZ4 to Zstandard compression, which reduced the required shuffle worker fleet by about 35%. Workers got 100 Gbps network interfaces, and JVM and data-plane changes made slot selection six times faster. Across the fleet, shuffle cost per task fell by nearly 90%, and the share of executor runtime spent waiting on shuffle fell by about 10-15% from its peak.

### NVMe SSDs
NVMe SSDs have much lower latency and higher throughput than traditional SATA SSDs, which suits the many small random writes and reads that shuffle creates. Moving shuffle storage onto them shortened the time shuffle takes by 40% in this deployment. The article does not give the baseline hardware, so the gain applies only to LinkedIn's setup.

### Zstandard instead of LZ4
Compression trades CPU for bytes moved and stored. Zstandard typically compresses more than LZ4 at a higher CPU cost, so the choice only pays off when storage and network are the bottleneck. Here the switch shrank the worker fleet by about 35%, which the article presents as the saving. The article does not report the CPU cost of the change.

### Worker sizing and slot selection
Workers were provisioned with 100 Gbps network interfaces, so network capacity is not the limit for fetching shuffle data. Slot selection, the step that assigns work to workers, became six times faster through JVM and data-plane optimizations. These changes sit alongside the hardware changes and together reduced per-task shuffle cost by nearly 90%.

## Converging on a unified Flink platform
The converged Flink platform processes hundreds of millions of events per hour in historical backfills, completing multi-day workloads in single-digit hours. Thousands of pipelines now use unified Flink computations for nearline processing and historical corrections, replacing parallel Spark implementations. LinkedIn is exploring caching and faster upstream upgrades, and it continues to contribute to Celeborn.

### Unified Flink computations
A unified computation uses one code path for both nearline (recent) and historical data, so the team no longer maintains a Spark version and a Flink version of the same logic. The article's evidence is the scale: thousands of pipelines moved over, and multi-day backfills now finish in single-digit hours. The article credits the shuffle work with making this migration possible, but it does not measure that dependency directly.

## 🧠 Mental models
### Shuffle storage that outlives its compute
**The model:** Intermediate shuffle data should be a service of its own, not a by-product that lives and dies with the executor that wrote it. The article's argument is that this lets storage scale independently of compute and reduces dependence on executor lifecycles.
**How to apply it:** Before placing a stateful intermediate store on compute nodes, ask whether its I/O, failures and scaling need to follow the compute or can be separated.

## 🔑 Key takeaways
- Celeborn cut LinkedIn's daily shuffle fetch failures from about 750 to 20, compared with the Magnet design.
- Compute cost fell 10% and application runtimes improved 14%, both measured against Magnet.
- Dedicated shuffle workers, separate from executors, reduce resource contention between compute and shuffle.
- A five-node Raft control plane across availability zones, with Apache Ratis, cut task failures during infrastructure disruptions 20-fold versus Magnet.
- NVMe SSDs cut shuffle duration 40%, and Zstandard in place of LZ4 cut the worker fleet by about 35%.
- Shuffle cost per task fell nearly 90%, and executor time waiting on shuffle fell by about 10-15% from its peak.
- Unified Flink backfills finish multi-day workloads in single-digit hours and have replaced thousands of parallel Spark pipelines.

## 🏛️ For Kenji
*This is your interpretation, not the author's.* For an architect of data platforms, the article is a worked case of treating shuffle as a separately owned service with its own failure domain and scaling curve, which is a data architecture decision as much as an engineering one. The question it raises is what operating cost, in control-plane ownership and upgrades, a team takes on when it moves a stateful layer out of the compute cluster, and whether the failure reduction would justify it on a smaller platform.

## 📚 Reading plan
### From the article
- [InfoQ article](https://www.infoq.com/news/2026/11/linkedin-celeborn-remote-shuffle) — the source article, for its wording and figure captions.
- LinkedIn Blog Post (cited twice in the article; its URL is not in the extracted text, so it is not linked here) — the primary source for the deployment details.

### To go further
- Apache Celeborn project documentation — how the workers, control plane and push-based shuffle are configured.
- Apache Spark push-based shuffle (Magnet, Spark 3.2) — the design that Celeborn replaces at LinkedIn.
- Raft consensus and Apache Ratis — the replication and leader-election model behind the control plane.
- Apache Flink unified batch and streaming — the processing model behind LinkedIn's converged backfill platform.
