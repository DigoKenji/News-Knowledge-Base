---
type: Article Summary
title: How Google Traced GKE API Timeouts to a Linux Kernel TCP Bug
description: Intermittent GKE API timeouts came from three interacting bugs in PSC, Envoy and the Linux kernel, fixed by a
  kernel patch.
resource: https://www.infoq.com/news/2026/11/gke-kernel-network-bug
tags:
- cloud-infra
- control-plane
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/gke-kernel-network-bug
  title: How Google Traced GKE API Timeouts to a Linux Kernel TCP Bug
  author: Renato Losio
status: stable
source_url: https://www.infoq.com/news/2026/11/gke-kernel-network-bug
source: InfoQ
source_type: blog
published: '2026-11-21'
domain: cloud-infra
concepts:
- control-plane
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d81ff8352c31d0f2df011
---

## 🎯 Core thesis
Intermittent GKE API timeouts came from three interacting bugs in Private Service Connect, Envoy and the Linux kernel, and only a kernel patch removed the root cause.

## 🧭 Why the author wrote it
In January, a customer's pods timed out intermittently when reaching the Kubernetes API server, blocking around 800 developers from deploying to production. Losio summarizes Google's account of how engineers traced it. If the thesis holds, a single symptom such as an i/o timeout can originate at any layer, so debugging has to follow protocol state across layers.

## The incident: stale ACKs on new connections
In January, a customer running several GKE clusters with Private Service Connect (PSC) for private control-plane access saw intermittent timeouts when pods tried to reach the Kubernetes API server. Packet captures showed the control plane answering new TCP connection requests with stale ACK packets that belonged to connections already closed. The article describes three separate bugs across different networking layers, which is why no single layer explained the failure.

### Private Service Connect
> 🔎 Added context: PSC gives a VPC private access to a managed service through an endpoint inside the customer's network, instead of over the public internet. Here it carried the path from the customer's pods to the GKE control plane.

The article names packet reordering in PSC as one of three causes. Because PSC sits between the pods and the API server, the symptom appears inside the cluster even though the fault is in the network path.

### Control plane
The control plane is the set of components, here the Kubernetes API server, that decides what work runs and in what state. The pods depend on it for API calls, so its timeouts surface as application failures. The article uses PSC as the private path to this endpoint.

### Stale ACK
An ACK is the acknowledgement a TCP peer sends to confirm data or connection state. A stale ACK refers to a connection that no longer exists, so it cannot complete a new handshake. The control plane sent these in reply to new connection requests, which the client experienced as a timeout on a connection that never completed.

## Three interacting causes
The investigation identified three causes that interacted: packet reordering in PSC, Envoy's half-close behavior that stopped TCP connections from being closed properly, and a Linux kernel bug that discarded certain reordered FIN packets. Each layer offered a plausible explanation, but each explanation was incomplete on its own. Together they produced one symptom: an i/o timeout on a connection that never completed.

### TCP half-close
> 🔎 Added context: A TCP connection closes when each side sends a FIN packet. A half-close means one side has finished sending while it can still receive. Envoy is the proxy in this path.

The article says Envoy's half-close behavior prevented TCP connections from being properly closed. A connection that never completes its close leaves state behind on at least one side.

### Reordered FIN packets and FIN_WAIT_2
A FIN packet signals that one side is done sending. The kernel bug discarded certain reordered FIN packets, so the closing handshake never completed, and sockets stayed stuck indefinitely in the FIN_WAIT_2 state.
> 🔎 Added context: FIN_WAIT_2 is the state a closing socket enters after its own FIN is acknowledged, while it waits for the peer's FIN.

## Why the investigation was hard
Thakur argues this case differed from most networking incidents. Most cases resolve at one layer, such as a misconfigured firewall rule or an exhausted connection pool. Here the same i/o timeout appeared at the PSC layer, in the kernel's TCP state machine and in Envoy's connection management, so every layer had a plausible explanation and every explanation was incomplete. Engineers from GKE, PSC, networking, hostnet, the kernel and the upstream Linux community worked the problem together. Packet captures at several network layers were essential, and correlating network symptoms with infrastructure changes linked the three bugs.

> ❌ **Anti-pattern — Stopping at the first plausible layer:** Accepting the first explanation that fits the symptom (a PSC change, a proxy problem or a kernel problem) and moving on. **Why people do it:** Most networking cases do resolve at one layer, so the habit is usually right. **Consequence:** Here each explanation was incomplete, so fixing one layer leaves the others active. **Fix:** Check each explanation against packet captures and protocol state at every layer before calling it done. The article implies this through Thakur's account rather than labeling it.

## Three coordinated fixes
Google fixed the problem in three steps over several weeks. First it rolled back a PSC change to restore connectivity. Then it corrected the proxy protocol handling and added regression tests. Finally, a Linux kernel patch authored by Eric Dumazet, formerly a software engineer at Google, fixed the underlying TCP issue. The final fix was a patch to the kernel itself, not a config change or a feature flag.

### PROXY protocol
> 🔎 Added context: The PROXY protocol is a small header a proxy places at the start of a TCP connection to carry the original client address. The article says Google corrected how this was handled but does not give the details of the error.

### Regression tests
> 🔎 Added context: A regression test reproduces a fixed bug so that it cannot return unnoticed. The article mentions these tests without listing what they cover.

## Reactions from the field
Rohan Karne, managing director at Google Cloud, calls this "the unglamorous reality of mission-critical platform engineering": ruling out the obvious suspects and digging into packet captures until the kernel root cause appears.

Guangba Yu, a postdoctoral researcher at the Chinese University of Hong Kong, names the transferable method for AIOps and chaos engineering: organize simultaneous path observations, protocol state and rollout events, and separate symptom recovery from state-machine correction. He also notes the case supplies no population failure rate and does not explain all API timeouts.

Justin Garrison, field CTO at Sidero, points out that GKE ran a kernel from three years earlier (6.1), where the bug was already fixed upstream, so Google had to backport the fix.

> ❌ **Anti-pattern — Running an old kernel without tracking upstream fixes:** The node kernel lags upstream by years, so bugs fixed upstream stay live until someone backports them. **Why people do it:** The article does not say; a managed platform may pin an older kernel for stability. **Consequence:** A bug already fixed upstream still reaches production, and the fix must be backported. **Fix:** Track upstream fixes against the kernel you run and treat backports as routine work. The article raises this through Garrison's comment rather than as its own warning.

## Kernel fix rollout
The kernel fix was merged into Linux 6.9.7 and backported to the 6.6 and 6.1 LTS branches. It reached GKE customers in version 1.33.5 and later.
> 🔎 Added context: An LTS (long-term support) branch receives selected fixes for years after its release. A backport copies a fix from a newer kernel onto an older branch.

## 🧠 Mental models
### Same symptom, many layers
**The model:** The same symptom, here an i/o timeout, can come from faults at several layers, and each layer can offer a plausible but incomplete explanation.
**How to apply it:** Compare one failing connection across layers, using packet captures and protocol state, before accepting any single explanation.

### Symptom recovery versus state-machine correction
**The model:** Restoring traffic is not the same as fixing the protocol state that caused the failure.
**How to apply it:** After a mitigation such as a rollback, check the underlying state machine before closing the incident.

## 🔑 Key takeaways
- Three interacting causes in PSC, Envoy and the Linux kernel produced one symptom: an i/o timeout on connections that never completed.
- Packet captures at several network layers were essential, and correlating them with infrastructure changes linked the three bugs.
- Google rolled back a PSC change first to restore connectivity, then fixed proxy protocol handling and added regression tests.
- The final fix was a patch to the Linux kernel, not a config change or feature flag.
- Certain reordered FIN packets were discarded by the kernel, leaving sockets stuck in FIN_WAIT_2 indefinitely.
- The kernel fix merged in Linux 6.9.7 reached GKE through backports to 6.6 and 6.1 LTS, shipped in version 1.33.5 and later.
- Per Yu, the case supplies no population failure rate and does not explain all API timeouts.

## 🏛️ For Kenji
This is your interpretation, not the author's. In a platform architecture, correctness depends on a chain of managed networking, proxies and operating-system kernels, each owned by a different team, and this incident crossed all of them. The question it raises: how would your platform's incident process hand a timeout between the managed-network team, the proxy team and the OS team, and how would you learn that a kernel fix had landed upstream?

## 📚 Reading plan
### From the article
- [InfoQ article](https://www.infoq.com/news/2026/11/gke-kernel-network-bug) — the original summary, with the full expert quotes.
### To go further
- Google Cloud blog post by Akash Thakur on the GKE investigation — the primary source this article summarizes, for the packet-level detail (the article gives no link).
- Linux TCP state machine and the FIN_WAIT_2 state — to read the kernel bug in its full context.
- PROXY protocol — the proxy handling that Google corrected in its second fix.

## Concepts
- [control-plane](/concepts/control-plane.md)

