---
type: Article Summary
title: Announcing updates to the AWS Well-Architected Framework
description: AWS reorganized its Well-Architected Framework into environment-wide Foundations and per-workload guidance so reviews can be scoped and automated.
resource: https://aws.amazon.com/blogs/architecture/announcing-updates-to-the-aws-well-architected-framework-3/
tags:
- cloud-infra
- cloud-platforms
- well-architected
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/announcing-updates-to-the-aws-well-architected-framework-3/
  title: Announcing updates to the AWS Well-Architected Framework
  author: AWS Architecture Blog
status: stable
source_url: https://aws.amazon.com/blogs/architecture/announcing-updates-to-the-aws-well-architected-framework-3/
source: AWS Architecture
source_type: blog
published: '2026-11-16'
domain: cloud-infra
subdomain: cloud-platforms
concepts:
- well-architected
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81c6b6b5ce6f714ab326
---

# Announcing updates to the AWS Well-Architected Framework

**Source:** https://aws.amazon.com/blogs/architecture/announcing-updates-to-the-aws-well-architected-framework-3/
**Author:** AWS Architecture Blog
**Date:** 2026-11-16
**Concepts:** [Well-Architected Framework](/concepts/well-architected.md)

---

## 🎯 Core thesis
AWS has split the Well-Architected Framework so that environment-wide decisions are assessed once in a new Cloud Foundations area, and each workload review covers only the technical best practices that apply to that workload.

## 🧭 Why the author wrote it
Well-Architected reviews are valuable but slow, because account- and organization-level practices were repeated for every workload and the Framework mixed technical guidance with business and people guidance. If the thesis is right, reviews become faster to run and easier to automate, and the Framework reflects how teams build and operate in the cloud today.

## A brief history
The AWS Well-Architected Framework is a collection of best practices that organizations of every size and industry use to design, build, and operate workloads that are secure, reliable, high-performing, cost-effective, sustainable, and straightforward to operate. AWS launched the first version in 2012. It later published the Framework whitepaper, added the Operational Excellence pillar, and released pillar-specific whitepapers and Lenses for specific technologies and industries. AWS then refreshed the Framework so that best practices became its core unit of guidance, and added Sustainability as the sixth pillar. As customers began building with generative and agentic AI, AWS introduced new lenses. The article includes Figure 1, a timeline of Well-Architected guidance from 2012 to 2026; the text describes it only through its caption.

### Well-Architected Framework
The Framework is the central body of architectural knowledge. Its six pillars are Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, and Sustainability. The article describes it as a body of guidance that grew from a single set of recommendations into a full collection over the years. The 2026 update is the latest step in that growth.

### Well-Architected Lenses
A Lens is a set of guidance that applies the Framework to a specific technology or industry. The article names Lenses for generative and agentic AI as an example of this approach.

🔎 Added context: A Lens extends the general Framework with more specific guidance, so a team building a particular kind of system can use the Framework's principles alongside that specialized advice.

## What’s new
Customers said they value Well-Architected reviews but want a more efficient way to run them. Account- and organization-level best practices apply across the whole environment, so customers want to assess them once and then focus each review on the workload. To support this, the release organizes the Framework into two areas: Well-Architected Cloud Foundations and Well-Architected Cloud Workloads. A workload review now covers only the practices that apply to that workload, which makes it faster to run and to automate.

### Two areas
Cloud Foundations covers environment-wide decisions such as multi-account structure, identity, and cost governance. It sets guardrails that every workload inherits, and those guardrails are refined as the environment matures. Cloud Workloads covers how each workload is designed, built, and operated, including updated architecture patterns focused on specific business outcomes. In both areas, best practices are grouped into capabilities, which are sets of related best practices with a common objective.

### Technical focus
The release concentrates on technical architecture because AI agents increasingly run reviews. Guidance is most useful to an agent when it can be checked against how the environment and workloads are built and operated. Practices centered on business strategy, people, and organizational processes depend on human judgment and context that configuration or telemetry does not capture. AWS retired those practices and continues to offer guidance on them through other resources. Each remaining best practice now addresses a technical decision that a person, or an agent acting for them, can review and act on.

> ❌ **Anti-pattern — mixing non-technical guidance into technical reviews:** Business, people, and process practices appear in the same list as architecture decisions. **Why people do it:** the article does not say. **Consequence:** the article states that this mix makes reviews more difficult to automate. **Fix:** keep the review to technical decisions and take non-technical guidance to other resources.

The release also updated service references throughout the Framework. It removed services that no longer accept new customers or have reached end of support, and added guidance for services launched since the last release.

## Well-Architected Cloud Foundations
Best practices for account- and organization-level decisions were previously spread across the Framework and reviewed for every workload. Cloud Foundations consolidates them into guidance you apply when you set up or mature a multi-account environment. Of its best practices, 33 come from moving or merging existing practices from the pillars. The article says the rest are newly authored foundational guidance but does not give that split directly; by subtraction, 12 of the 45 are the newly authored ones.

The relocated practices were consolidated and rewritten from an environment-wide perspective. New practices cover centralized event monitoring, an auditable cloud inventory record, and centralized DNS management. The result is 45 best practices across seven capabilities: Multi-account environment; Identity and access management; Central observability management; Resource and inventory management; Security and compliance; Cloud financial management; and Networking and connectivity.

> ❌ **Anti-pattern — reviewing environment-wide practices for every workload:** Account- and organization-level practices are re-assessed in each workload review. **Why people do it:** the article says these practices were previously spread across the Framework and reviewed per workload, so the habit followed from the old structure. **Consequence:** the article says customers want to assess them once, so repeating them slows each review. **Fix:** assess them once in Cloud Foundations when setting up or maturing the environment, and inherit them in workload reviews.

### 🔎 Added context on guardrails
A guardrail here means a standing environment-wide control, such as account structure or identity policy, that applies automatically to every workload below it.

## Well-Architected Cloud Workloads
Cloud Workloads now has 216 best practices, down from 307, with no loss of technical coverage. Practices that apply across the environment moved to Cloud Foundations, similar practices were combined, and practices outside the Framework's technical scope were retired. Each pillar's changes are below.

### Operational Excellence
This pillar introduces new capabilities and best practices that follow the operations lifecycle: observation, deriving insights, managing change flow, deployment, support, response, and evolution. It adds a best practice for managing rollouts of external dependencies. It expands guidance on using AI assistants and agents to instrument telemetry, draft dashboards, and investigate incidents, with validation against baselines and named human ownership at every step.

### Security
Account-wide concerns move to Cloud Foundations. These include root account protection, sign-in mechanisms, permission guardrails, emergency access, centralized logging, and forensic readiness, which go into the Identity and access management and Security and compliance capabilities. Within Cloud Workloads, the pillar adds best practices for centralized workload permissions management, detection metrics and dashboards, and secure coding guidelines.

### Reliability
Three networking practices (IP subnet allocation, hub-and-spoke topologies, and non-overlapping address ranges) move to the Networking and connectivity capability of Cloud Foundations, where they are consolidated into a single network strategy best practice. Within Cloud Workloads, the pillar adds a best practice on cyber resilience for backup data, which helps protect backups against ransomware and other cyber threats.

### Performance Efficiency
The policies and reference architectures practice moves to Cloud Foundations. Within Cloud Workloads, Architecture selection now focuses on validating workload requirements and tracking them with key performance indicators (KPIs). Related practices are combined: compute configuration folds into compute selection, and load testing folds into workload performance evaluation.

### Cost Optimization
Account-level concerns move to Cloud Foundations, including account structure, cost attribution, billing tools, and cost controls, where they form the core of the Cloud financial management capability. Within Cloud Workloads, overlapping practices are combined; decommissioning guidance merges into a single resource lifecycle management best practice. A new best practice covers using generative AI for operational efficiency.

### Sustainability
This pillar introduces capabilities and best practices that follow how teams reduce workload impact: measuring and monitoring, provisioning resources, improving workload efficiency, and optimizing data and processing. Earlier general guidance has been converted to best practices based on customer feedback. A new best practice asks teams to use the most resource-efficient computational approach, including a less resource-intensive method or a smaller machine learning model where one meets the requirement. The pillar also expands guidance on combining resource usage efficiency metrics with carbon emissions data from the AWS Sustainability Console and API and from AWS Data Exports. Baselines and targets make progress measurable over time.

## Call to action
The article presents the Framework as more than a set of best practices and tools for finding improvement opportunities. It describes it as a mechanism for strengthening governance and driving better outcomes across an organization. Evaluating architecture consistently against best practices builds a culture of continuous improvement and surfaces critical issues before they affect workloads. The updated Framework will be available in 11 languages: English, Spanish, French, German, Italian, Japanese, Korean, Indonesian, Brazilian Portuguese, Simplified Chinese, and Traditional Chinese.

## About the authors
The extracted article has headings for author sections but no author text beneath them.

## 🧠 Mental models
### Inherited guardrails
**The model:** Environment-wide controls are set once at the foundation and every workload inherits them, so each workload review only checks what is specific to that workload.
**How to apply it:** When you review a workload, first confirm the account, identity, and cost guardrails exist, then spend the review time on the workload's own architecture decisions.

### Checkable guidance
**The model:** A best practice is only useful to an agent or reviewer when it names a technical decision that can be verified against configuration or telemetry.
**How to apply it:** Rewrite any practice that depends on judgment about strategy or people as a separate, non-automated governance item, and keep the technical checklist testable.

### Scope the review to what applies
**The model:** A review should cover only the practices relevant to the system under review, which keeps it short enough to run regularly.
**How to apply it:** Before a review, filter the practice list by the workload's architecture and drop items outside its scope, rather than working through the full framework each time.

## 🔑 Key takeaways
- The Framework now has two areas, Cloud Foundations for environment-wide decisions and Cloud Workloads for per-workload decisions, so reviews can be scoped.
- Cloud Foundations has 45 best practices across seven capabilities, and 33 of them come from moving or merging existing practices.
- Cloud Workloads drops from 307 to 216 best practices with no loss of technical coverage, through moves, merges, and retirements.
- Business strategy, people, and organizational process practices were retired from the Framework, not moved, because they need human judgment that configuration or telemetry cannot capture.
- Each remaining practice addresses a technical decision that a person or an agent acting for them can review and act on.
- New practices cover secure coding, cyber resilience for backup data, generative AI for operational efficiency, and resource-efficient computation.

## 🏛️ For Kenji
Your architecture reviews probably mix environment-wide controls with workload-specific ones, and the article's split suggests setting the environment controls once and inheriting them. The article also suggests that agent-run reviews work best on technical, checkable criteria, which matters for AI governance. This is your interpretation, not the author's. Question: which of your team's current review criteria can an agent verify from configuration or telemetry, and which need human judgment that should stay outside the automated review?

## 📚 Reading plan
### From the article
- [Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/) — the entry point for the full body of guidance and its history.
- [AWS Well-Architected Framework whitepaper](https://docs.aws.amazon.com/wellarchitected/latest/framework/framework.html) — the updated Framework text, including the six pillars.
- [Cloud Foundations](https://docs.aws.amazon.com/wellarchitected/latest/framework/cloud-foundations.html) — the environment-wide guidance you set up once per environment.
- [Cloud Workloads](https://docs.aws.amazon.com/wellarchitected/latest/framework/workloads.html) — the per-workload guidance that reviews now focus on.
- [Operational Excellence](https://docs.aws.amazon.com/wellarchitected/latest/framework/operational-excellence.html) — the pillar with new AI-assisted operations guidance.
- [Security](https://docs.aws.amazon.com/wellarchitected/latest/framework/security.html) — the pillar with the new secure coding and permissions practices.
- [Reliability](https://docs.aws.amazon.com/wellarchitected/latest/framework/reliability.html) — the pillar with the new backup cyber resilience practice.
- [Performance Efficiency](https://docs.aws.amazon.com/wellarchitected/latest/framework/performance-efficiency.html) — the pillar with the KPI-based architecture selection change.
- [Cost Optimization](https://docs.aws.amazon.com/wellarchitected/latest/framework/cost-optimization.html) — the pillar whose account-level practices moved to Cloud Foundations.
- [Sustainability](https://docs.aws.amazon.com/wellarchitected/latest/framework/sustainability.html) — the pillar with the resource-efficiency and carbon-data guidance.
- [AWS Sustainability API reference](https://docs.aws.amazon.com/sustainability/latest/APIReference/Welcome.html) — for pulling carbon emissions data alongside resource usage metrics.
- [AWS Data Exports](https://docs.aws.amazon.com/cur/latest/userguide/what-is-data-exports.html) — the source the article names for resource usage data to combine with emissions data.
### To go further
- AWS Well-Architected Lenses — the technology- and industry-specific extensions the article mentions, including the lenses for generative and agentic AI.
- The Well-Architected review process — the practice the new two-area structure is designed to make faster and more automatable.
