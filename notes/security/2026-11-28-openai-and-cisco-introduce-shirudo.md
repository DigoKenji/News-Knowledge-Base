---
type: Article Summary
title: OpenAI and Cisco introduce Shirudo
description: OpenAI and Cisco's open-source Shirudo uses eBPF shields to block exploits in running Linux systems while patches
  are prepared.
resource: https://openai.com/index/introducing-shirudo
tags:
- security
- security-operations
- vulnerability-management
- least-privilege
- remote-attestation
- security-controls
- incident-response
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-28T06:47:00-03:00'
sources:
- resource: https://openai.com/index/introducing-shirudo
  title: OpenAI and Cisco introduce Shirudo
  author: OpenAI
status: stable
source_url: https://openai.com/index/introducing-shirudo
source: OpenAI News
source_type: blog
published: '2026-11-23'
domain: security
subdomain: security-operations
concepts:
- vulnerability-management
- least-privilege
- remote-attestation
- security-controls
- incident-response
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/426baf74172d810386bcd52eaae20e4a
---

## 🎯 Core thesis
Shirudo is an open-source runtime from OpenAI and Cisco that lets operators apply a tested, signed eBPF "shield" to a running Linux system, blocking an exploit's code path while the permanent patch is still being prepared.

## 🧭 Why the author wrote it
Testing and deploying a patch can take days or weeks, and the obvious stopgap of blocking an operation can take a service offline. The article argues that operators need a targeted, approved safeguard they can deploy into a running service. If that works, teams can close the exposure window without restarts or downtime.

## Introducing Shirudo
The article announces Shirudo as an open-source runtime that helps teams protect Linux systems while they roll out patches. In a controlled test, a Codex-generated shield blocked an exploit attempt against a running Elasticsearch service without a restart. OpenAI and Cisco invite developers, security researchers and infrastructure operators to help develop it, combining OpenAI's frontier AI, security and infrastructure research with Cisco's infrastructure and security expertise.

### Shirudo
Shirudo is the name of the runtime, and it is Japanese for "shield". Its purpose is to protect running Linux systems so that protection can be applied without waiting for a patch or a restart. The article frames it as a first step in public development, not a finished product, and it asks for contributions. The mechanism that does the protecting is the shield, which the following sections explain.

## The patch gap and the shield idea
Testing and deploying a patch can still take days or weeks. Protecting a service in that window is hard, because an exploit and a legitimate request may rely on the same Linux operation. Blocking that operation entirely could take the service offline. Shirudo helps security teams test whether a temporary safeguard, called a shield, blocks the behavior that exposes the vulnerability while letting legitimate requests through. Once an operator approves the shield, it can be deployed while the service keeps running.

### The patch gap
The patch gap is the period between learning that a vulnerability exists and having a tested fix running everywhere. The article puts testing and deployment alone at days or weeks, and this is the window Shirudo targets. A shield is an interim mitigation, not a replacement for the patch, and the article describes it as protection "while preparing permanent patches."
> 🔎 Added context: Vulnerability management is the ongoing process of finding, prioritising, mitigating and verifying weaknesses. An interim mitigation like a shield is one step in that process, and the article does not use the term.

### Shield
A shield is a temporary safeguard aimed at the specific behavior that exposes a vulnerability, not at the whole Linux operation. The test for a shield is whether it blocks the exploit while letting legitimate requests through. An operator approves a shield before it is deployed, and the shield can then be applied while the service continues running. The article describes shields as a way to evaluate and deploy temporary protections before permanent patches are ready.
> 🔎 Added context: A compensating control is a safeguard used when the primary fix is not yet in place, which describes a shield. The article does not use the term.

> ❌ **Anti-pattern — blanket blocking of the operation:** blocking the Linux operation that the exploit uses, for all callers. **Why people do it:** the article does not give motives; blocking everything is the simplest response when no patch exists. **Consequence:** legitimate requests fail, and the article warns the service could go offline. **Fix:** a shield that targets the vulnerable code path and lets legitimate requests through, tested before enforcement.
> 🔎 Added context: The "why people do it" line is my reading, not the article's.

## How shields work
Shirudo uses eBPF to observe suspicious activity and apply targeted protections to running systems. The article organises its design around four core capabilities, covered below.

### eBPF
eBPF is a Linux technology that allows small programs to run at specific points inside the operating system. Those programs can observe activity at those points and apply targeted rules there, which is what makes a shield possible. Because the programs act on a running system, a protection can be added without restarting the system. The article gives no implementation detail beyond this, so the mechanism is described at the level the author provides.

### Live protection
The article describes eBPF shields that block vulnerable code paths without restarting the system. Security teams can monitor how a proposed shield would affect applications before enforcing it. The order matters: observe first, then enforce. This ordering is what lets a shield be introduced into a live service without first breaking legitimate traffic.

### Defense at machine speed
OpenAI and Cisco say they are designing Shirudo to give agents programmable controls. Agents would investigate vulnerabilities, develop targeted shields and validate their effectiveness against exploits and real workloads. The article is explicit that security teams control what gets deployed. The wording "are designing" matters here: this is a design goal in the article, not a described finished capability.

### Trusted deployment
Shields are cryptographically signed and verified before they load. The article names two checks: Linux's eBPF verifier checks eBPF programs against its safety rules, and kernel security controls restrict unauthorised changes. A shield is therefore accepted because it passes these checks. The article presents this as part of the design as well.

### Hardware-backed verification
Shirudo uses a Trusted Platform Module (TPM) to support a verifiable record of protections being loaded or removed. Operators can use this evidence to check whether their systems meet their security requirements. The record is evidence an operator can inspect, not just a log the system writes about itself.
> 🔎 Added context: This is a form of remote attestation, where a machine proves with hardware-signed evidence what it is running. The article does not use the term.

## The Java4Shell example
The article gives one worked example. Using Codex, the team generated a shield that added an input check to Java's Base64 decoder and attached it directly to a running Elasticsearch process. When an exploit attempt arrived, the shield rejected the malicious input, so the attack was blocked while Elasticsearch kept running. The Shirudo technical walkthrough from the Linux Plumbers Conference shows how to apply a shield and verify it on the target host.

### Java4Shell shield
Java4Shell is the vulnerability used to illustrate how a targeted shield protects a running application while a permanent patch is prepared. The shield is narrow: an input check placed on the path that decodes Base64 data, so only the malicious input is rejected. Attaching it to a running process meant the application did not need to be rebuilt or restarted. The article reports the result from a controlled test and gives no further details of the exploit or the patch timeline.
> 🔎 Added context: The pattern here is generate a targeted check, attach it to the live process, then verify it against the exploit. The article gives no timing or success-rate figures.

## The long-term direction
The article calls Shirudo "a first step" toward infrastructure that can adapt to emerging threats while staying reliable. The wider goals are hardening the kernel and core services, limiting workload permissions, and making updates and recovery safer. The long-term aim is for defenders and AI agents to identify affected systems quickly, validate protections and deploy eBPF shields that block exploits without disrupting workloads.

### Least privilege for workloads
The article's goal is that applications run with only the permissions they need, governed by policies that are automatically generated and maintained. This is the least-privilege principle applied to running workloads: a compromised application can do less damage when its permissions are narrow. The article presents automatic policy generation as a goal, not a current feature.

### Containment and known-good restore
The article lists the aims of detecting and containing compromises, preventing attackers from moving across systems, and restoring services to a verified, known-good state. It adds that hardware-backed attestation and independent network controls give extra assurance even if individual systems are compromised.
> 🔎 Added context: These aims correspond to the containment and recovery phases of incident response. The article does not name that process.

## Open source and how to take part
OpenAI and Cisco are open-sourcing Shirudo so developers, security researchers and infrastructure operators can inspect the code, test protections and contribute. The article welcomes contributions to the runtime, to new shields, and to improvements in Linux and eBPF. It lists three ways to start: follow the VM quickstart to sign, load and verify a test protection; work through Isovalent's self-guided lab; or register for the December 15 webinar on the technical deep dive.

### Licensing
Shirudo is licensed under BSD 2-Clause, and its BPF programs are dual-licensed under BSD and GPL. The article states both licences but does not explain what the dual licence means for any particular use. A team adopting or adapting Shirudo should check the BPF program licence separately from the runtime licence.

### Tested shields with disclosures
The article's stated goal is to make tested shields available alongside vulnerability disclosures. This would give operators something to evaluate and deploy while permanent patches are being prepared, instead of waiting for the fix. The article does not describe how these shields will be published or independently verified.

## 🧠 Mental models
### Shield while you patch
**The model:** A shield is a temporary, targeted safeguard that covers a known weakness until the permanent fix is deployed.
**How to apply it:** Scope it to the exploitable behavior rather than the whole operation, and plan to remove it once the patch is in place.

### Observe before enforcing
**The model:** A protection should be watched against real workloads before it is allowed to block anything.
**How to apply it:** Run the proposed shield in monitoring mode against live traffic, check its effect on applications, and only then approve enforcement.

### Evidence over assertion
**The model:** The article treats a shield as trustworthy because it is signed, checked by the kernel and recorded by hardware, so an operator can verify it rather than take it on trust.
**How to apply it:** Require a signed shield and a verifiable record of what was loaded or removed before accepting a protection on a production host.

## 🔑 Key takeaways
- Patch testing and deployment can take days or weeks, and the article targets that window with temporary, targeted protections.
- A shield should block the exploitable behaviour while letting legitimate requests through; blocking the whole Linux operation can take the service offline.
- eBPF shields can be applied to running systems without a restart, and security teams can monitor a shield's effect before enforcing it.
- Shields are cryptographically signed and checked by the eBPF verifier and kernel controls before loading, and a TPM supports a verifiable record of loads and removals.
- In the Java4Shell test, a Codex-generated input check on Java's Base64 decoder rejected the malicious input while Elasticsearch kept running.
- The article describes "defense at machine speed", where agents build and validate shields, as a design goal rather than a finished capability.
- The stated aim is to publish tested shields alongside vulnerability disclosures so operators can protect services while patches are prepared.

## 🏛️ For Kenji
*Interpretation, not the author's:* For an architect, the useful pattern is a time-boxed, approved mitigation path between disclosure and patch, with evidence of what was loaded and removed. It applies to how you design vulnerability-management runbooks and to deciding when a running service can be protected without downtime. The question it raises: in your estate, should a shield be approved by the security team or by the service owner?

## 📚 Reading plan
### From the article
- The extracted text has no link targets, so the resources the article cites are listed below without URLs.

### To go further
- Shirudo source code on GitHub (cited by the article) — inspect the runtime and the BPF programs.
- Shirudo project documentation (cited by the article) — the reference for the project's contents and use.
- Shirudo VM quickstart (cited by the article) — sign, load and verify a test protection.
- Isovalent self-guided lab (cited by the article) — hands-on practice with Shirudo.
- Shirudo technical walkthrough, Linux Plumbers Conference (cited by the article) — applying and verifying a shield on a target host.
- Shirudo technical deep dive, December 15 webinar (cited by the article) — live follow-up on the design.
- eBPF verifier — the safety rules that gate every shield before it loads.
- Trusted Platform Module (TPM) — the hardware basis of the verifiable protection record.

## Concepts
- [Vulnerability management](/concepts/security/vulnerability-management.md)
- [Least privilege and defense in depth](/concepts/security/least-privilege.md)
- [Remote attestation](/concepts/security/remote-attestation.md)
- [Security controls](/concepts/security/security-controls.md)
- [Incident response](/concepts/security/incident-response.md)
