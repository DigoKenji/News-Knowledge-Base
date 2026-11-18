---
type: Article Summary
title: Yelp Migrates Cassandra to EKS by Moving EBS Volumes
description: Yelp moved 90 Cassandra clusters to EKS by reattaching existing EBS volumes to new pods, avoiding data copies
  and keeping rollback trivial.
resource: https://www.infoq.com/news/2026/11/kubernetes-cassandra-eks/
tags:
- cloud-infra
- cloud-platforms
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/kubernetes-cassandra-eks/
  title: Yelp Migrates Cassandra to EKS by Moving EBS Volumes
  author: InfoQ
status: stable
source_url: https://www.infoq.com/news/2026/11/kubernetes-cassandra-eks/
source: InfoQ
source_type: blog
published: '2026-11-18'
domain: cloud-infra
subdomain: cloud-platforms
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d8172897feaf63fd0dc32
---

## 🎯 Core thesis
Yelp moved its Cassandra fleet to EKS by re-pointing existing EBS volumes at new pods instead of copying data between nodes, which kept downtime and data loss at zero and made rollback the same operation run in reverse.

## 🧭 Why the author wrote it
Yelp had already moved its stateless workloads to EKS, but Cassandra stayed on legacy self-managed clusters because Kubernetes has no built-in way to migrate StatefulSets and their persistent volumes across clusters. If the thesis holds, a stateful fleet can change clusters without the data-transfer cost, capacity doubling or weeks-long streaming that the usual approaches require.

## Scale and result
The migration covered about 90 production clusters with zero downtime for customers and no data loss. The largest cluster, a 54-pod user-data store, took about 12 hours end to end.

## Why the usual approaches were rejected
The standard approach is to add nodes to the new cluster and shut down the old ones. This temporarily doubles compute capacity and generates data transfer costs. For the largest clusters, streaming alone could have taken weeks. Yelp also considered a regional failover with a rebuild, and a new Cassandra datacenter in EKS, but rated both as less transparent to customers.

### StatefulSet
A StatefulSet is the Kubernetes workload type for stateful software such as databases. Each pod keeps a stable identity and is usually paired with its own persistent volume. Kubernetes can manage that pairing inside one cluster, but the article notes it has no built-in way to carry StatefulSets and their volumes to another cluster. That gap is what forces a migration strategy to be designed by hand.

> 🔎 Added context: A PersistentVolume (PV) is the cluster's record of a storage volume, and a PersistentVolumeClaim (PVC) is a pod's request for one. Pre-creating a PV that points at an existing volume ID is how the article's technique binds an old volume to a new cluster.

## The Rolling PV Transplant mechanism
Yelp's "Rolling PV Transplant" depends on two properties. First, an EBS volume is bound to an Availability Zone, not to a Kubernetes cluster. Second, a PersistentVolume is effectively a pointer to a volume ID. The team pre-creates PVs and PVCs in EKS that reference the existing volume IDs, then creates an identical StatefulSet there, whose pods wait in ContainerCreating.

The automation then deletes one pod at a time on the legacy cluster, waits for the volume to detach, and lets the EKS pod claim it. The pod rejoins the ring with the same data and token range, and the process repeats for the next pod. Cassandra's replication tolerates the few minutes each node is offline.

### AZ-bound volumes
An EBS volume lives in one Availability Zone and can only attach to nodes in that zone. This is usually treated as a limitation, because it prevents a volume from following a workload across zones. Here it is what makes the technique safe: the volume's location does not depend on which cluster is running it, so the same volume can be detached from one cluster and attached to another in the same zone.

> 🔎 Added context: A Cassandra ring is the set of nodes that share the token space, and each node owns a token range. Keeping the same token range on the replacement pod is why the data stays consistent with the rest of the ring.

## The skipReconciliation flag
Yelp set a `skipReconciliation: true` flag on the custom resources in both clusters. Two operators were managing the same logical ring during the migration, and without the flag they could conflict over pod state. The flag was cleared on the EKS side at the end of the migration.

> ❌ **Anti-pattern — two uncoordinated controllers on one ring:** two operators both reconcile the same logical ring during a move. **Why people do it:** the article does not say why the old and new operators ran together; it is a consequence of moving the workload in place. **Consequence:** the operators can fight over pod state. **Fix:** the article's fix is to set `skipReconciliation: true` on the resources in both clusters for the duration of the migration, then clear it on the EKS side at the end.

## Rollback and rollout pace
Across roughly 1,000 pods, about 1,000 TiB moved by pointer rather than over the wire. No node was ever decommissioned from the ring, so rollback is the same operation in reverse. Yelp used rollback a handful of times, for a missing subnet definition and a certificate mismatch, and restored pods within minutes.

The rollout followed a week of manual dev runs and a week of automation. One engineer then ran the production rollout in about three weeks, migrating three to six clusters on a typical day.

## Claude-generated orchestration script
The team had Claude generate the orchestration script, roughly 800 lines of bash, from its validated manual runbook. The procedure itself was not designed by Claude. The script needed minor adjustments after two days of testing.

> 🔎 Added context: A runbook is a written, step-by-step procedure. Here the team first proved each step by hand, then asked the model to turn that proven procedure into code.

## Prior CSI work and reuse beyond Cassandra
The approach relies on a previous Container Storage Interface (CSI) migration by Yelp's Compute Infrastructure team. That work gave the Data Infrastructure team confidence in the volume detach and attach process. Yelp also used the technique for Kafka clusters and says it applies to any AZ-pinned stateful workload on Kubernetes. The legacy clusters are now retired.

## Failure modes and unrelated findings
The one failure specific to this technique was a stale VolumeAttachment object left behind by the AWS CSI controller after pod deletion. The EKS pod sat in Pending with no error. Deleting the object on the legacy cluster fixed it. Yelp advises building explicit timeouts for stalled detach and attach operations into any automation.

Two other findings were unrelated to the mechanism. First, backups failed with IAM AccessDenied errors for 48 hours, because a new EKS role was not yet being assumed by scheduled jobs, and the alert only filed a ticket instead of paging. Second, pausing repairs for the migration exposed one cluster whose repairs had been silently disabled for three months, leaving accumulated data inconsistencies.

> ❌ **Anti-pattern — alerts that file tickets instead of paging:** a backup failure was reported as a ticket. **Why people do it:** the article does not say; a ticket is the lower-effort default for non-urgent problems. **Consequence:** the backup failure ran for 48 hours before anyone acted. **Fix:** treat backup failures as paging-severity alerts, and check alert severity before a migration starts.

> ❌ **Anti-pattern — repairs disabled without anyone noticing:** one cluster's repairs had been off for three months. **Why people do it:** the article does not say how repairs were disabled. **Consequence:** data inconsistencies accumulated unseen. **Fix:** audit repair state before starting a migration, and treat it as part of the plan.

## 🧠 Mental models
### Pointer, not payload
**The model:** A PersistentVolume that references an existing volume ID is a pointer. Moving the pointer moves the workload's data without moving the bytes.
**How to apply it:** When a stateful system must change infrastructure, first ask whether its storage can be re-attached rather than copied, and whether the storage's placement constraints allow it.

### Rollback as the same operation in reverse
**The model:** If no node leaves the ring during the move, rolling back is the same pointer swap run backwards, not a separate recovery procedure.
**How to apply it:** Choose migration steps that keep the old environment valid until the end, so each step can be reversed with the same tooling.

## 🔑 Key takeaways
- Yelp moved about 90 Cassandra clusters to EKS with zero downtime and no data loss, by re-pointing existing EBS volumes at new pods.
- The technique works because an EBS volume is bound to an Availability Zone, not to a cluster, and a PersistentVolume is effectively a pointer to a volume ID.
- No node was decommissioned from the ring, so rollback was the same operation in reverse, used a handful of times to restore pods within minutes.
- Two uncoordinated operators on one ring required the skipReconciliation flag to prevent conflicts over pod state.
- A stale VolumeAttachment left by the AWS CSI controller was the one technique-specific failure, so automation needs explicit timeouts for stalled detach and attach.
- Monday.com reached the same goal with a streaming model that keeps the old environment intact until the end, at the cost of a longer migration and running both sides at once.

## 🏛️ For Kenji
This is your interpretation, not the author's. The article's most useful lesson for an architect is to audit the surrounding operational state before a move: repair schedules, backup IAM roles and alert severity caused the most damage here, not the volume mechanics. A question it raises for your own platform work is which of your stateful systems would need a provable rollback path, and whether the storage placement constraints that make a pointer swap safe are documented anywhere.

## 📚 Reading plan
### From the article
- No links to cite: the extracted article text contains no URLs for Yelp's blog post or for the Monday.com post. The full article is at the URL in the source metadata, which is the best place to find the original references.
### To go further
- Yelp's blog post by the site reliability team on the Cassandra move to EKS — read it for the runbook details and the exact automation steps, which the article summarizes.
- Monday.com's engineering post on zero-downtime Cassandra migration between EKS clusters — read it to compare the streaming model against the pointer-based approach.
- Kubernetes StatefulSets and PersistentVolumes documentation — read it to understand the objects the technique depends on, before attempting a similar move.
- Container Storage Interface (CSI) and VolumeAttachment objects — read them to debug stalled detach and attach operations like the one the article describes.
