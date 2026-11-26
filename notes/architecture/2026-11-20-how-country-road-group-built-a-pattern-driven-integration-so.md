---
type: Article Summary
title: How Country Road Group built a pattern-driven integration solution on AWS
description: Replacing BizTalk-era middleware with cloud-native services that implement integration patterns directly cut
  one retailer's integration cost 80 percent.
resource: https://aws.amazon.com/blogs/architecture/how-country-road-group-built-a-pattern-driven-integration-solution-on-aws/
tags:
- architecture
- software-architecture
- idempotency
- saga-pattern
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/how-country-road-group-built-a-pattern-driven-integration-solution-on-aws/
  title: How Country Road Group built a pattern-driven integration solution on AWS
  author: AWS Architecture Blog
status: stable
source_url: https://aws.amazon.com/blogs/architecture/how-country-road-group-built-a-pattern-driven-integration-solution-on-aws/
source: AWS Architecture
source_type: blog
published: '2026-11-19'
domain: architecture
subdomain: software-architecture
concepts:
- idempotency
- saga-pattern
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81ceb69de35a42f4d863
---

## 🎯 Core thesis
Legacy integration middleware can be replaced by cloud-native services that implement the same integration patterns directly, provisioned declaratively from code, with no central middleware runtime between the systems being connected.

## 🧭 Why the author wrote it
The post starts from a common problem: organizations pay large sums for middleware licences, hosting and maintenance, and the skills to run it are disappearing from the market. It shows how one retailer, Country Road Group, moved off BizTalk with a pattern-driven platform on AWS. If the thesis holds, middleware cost and talent risk fall away, because the interface patterns themselves never changed and cloud services can provide them directly.

## Introduction
The introduction frames the problem. Enterprise integration products such as BizTalk, MuleSoft, TIBCO and Boomi solved the right problem when they appeared, but they tightly coupled applications to the middleware through proprietary adapters, orchestrations and message formats. The author says many organizations now pay hundreds of thousands per year in licensing, hosting and maintenance for middleware that delivers no new capability.

### Enterprise integration middleware
Middleware here means a central integration product that every connected system must go through. Its adapters, orchestrations and message formats are specific to that product, so applications depend on it and cannot easily leave. The coupling becomes the main source of cost and risk over time.

### Interface patterns
The author names four interface patterns that have not changed: publish-subscribe, content-based routing, request-reply and scatter-gather. A system that sends events to many subscribers uses publish-subscribe, while one that picks a destination from message content uses content-based routing. Cloud-native services now implement these patterns directly, so no middleware runtime is needed in between.

### Country Road Group context
CRG is an Australian fashion retailer with five brands (Country Road, Mimco, Witchery, Politix and Trenery) and more than 600 stores. Its 21 integration interfaces ran on Microsoft BizTalk Server hosted on VMware and connected point-of-sale systems, Oracle RMS databases and ecommerce platforms. Three pressures drove the change: BizTalk was approaching end-of-support, VMware costs were rising after the Broadcom acquisition, and the team could no longer hire BizTalk developers.

### Declarative, pattern-led approach
Instead of writing orchestration code for each interface, CRG's teams declare which pattern an interface uses, and pre-built Terraform modules provision the infrastructure behind it. The post also covers the four processing lanes that replaced BizTalk and the custom Error Management System that handles failures across them. The approach does not depend on any one infrastructure-as-code tool.

> 🔎 Added context: Terraform is an infrastructure-as-code tool that describes cloud resources in configuration files and creates them through a plan-and-apply cycle. Its state file records which real resources the configuration manages.

### Starter kit
The authors release an open-source starter kit, the Enterprise Integration Patterns AWS Cloud Development Kit (AWS CDK) Library, with 29 constructs. Teams using Terraform can use it as a reference implementation. The kit also includes an agent skill that helps AI coding assistants such as Kiro and Claude Code select, configure and compose the right pattern.

## Why CRG built SIP
CRG's integration estate suffered from tool sprawl and end-of-life middleware (BizTalk 2013), which led to production instability and operational overhead. The Strategic Integration Platform (SIP) was built to fix this. The section names four value drivers.

### Legacy debt
The goal is to decommission BizTalk 2013 and its unsupported VMware and Broadcom infrastructure. Legacy debt here means software that is out of support and tied to hardware or licensing the company no longer wants to pay for.

### Tool sprawl
SIP consolidates BizTalk, Workato, Apollo and Kafka into one integration solution with one engineering skillset. Fewer tools means fewer runtimes to patch, fewer vendor contracts and fewer kinds of expertise to hire for.

### Low velocity
The platform adopts API-first and continuous integration and continuous delivery (CI/CD) practices so that new partner integrations ship faster. Velocity is treated as an architecture concern, not only a delivery-process concern.

### Security and resiliency risk
The section sets two rules. Authentication and authorization are standardized across every interface. Dead-letter queues (DLQs) are treated as data stores, because a failed message holds a full copy of a customer payload. DLQs therefore need the same encryption, retention and access controls as the primary channel to stay within data residency and retention requirements.

> ❌ **Anti-pattern — disposable dead-letter queues:** Treating a DLQ as a temporary holding area with looser controls than the main channel. **Why people do it:** failed messages look like transient debris, and the queue is rarely reviewed. **Consequence:** a failure path leaks customer data outside the controls that protect the primary path. **Fix:** give DLQs the same encryption, retention and access rules as the primary channel, as the article prescribes.

## SIP architecture overview
The SIP follows a "Compose on Cloud-Native" strategy. It combines AWS serverless compute (Spring Boot on AWS Lambda, AWS Step Functions and Amazon EventBridge) with Apache Camel on Amazon Elastic Container Service (Amazon ECS) with AWS Fargate for long-running and stateful flows. The architecture has three layers, shown in Figure 1, and traffic flows through a four-lane, pattern-led service bus shown in Figure 2.

### Compose on Cloud-Native
The strategy chooses the runtime by workload. Serverless compute handles most event processing, and a container runtime with Apache Camel handles flows that run longer or keep state. Each lane routes to the right runtime based on payload characteristics and processing duration.

### Pattern layer
Each of the 21 interfaces is expressed as a composition of standard integration patterns such as publish-subscribe and content-based routing. This layer holds the business-specific logic: Lambda handlers that validate, transform and route payloads, orchestrated by Step Functions for multi-step workflows and triggered by Amazon EventBridge rules or Automic. The patterns act as a shared vocabulary for the engineering team. Deploying an interface starts by selecting a declarative pattern or Terraform code, not by writing infrastructure from scratch.

### AWS service layer
The solution provisions its integration surface from AWS managed services. Amazon API Gateway is the entry point, with AWS WAF shielding and rate limiting in front. AWS Lambda handles stateless event processing, and AWS Step Functions orchestrates multi-step workflows with standardized retry and catch logic. Amazon EventBridge is the canonical event bus, and Amazon Simple Queue Service (Amazon SQS) and Amazon Simple Notification Service (Amazon SNS) provide asynchronous buffering and fan-out. Amazon Simple Storage Service (Amazon S3) persists messages, with S3 Intelligent-Tiering and archival to S3 Glacier storage classes. AWS Transfer Family provides managed SFTP and FTP endpoints. Amazon ECS with AWS Fargate runs Apache Camel routes, and Amazon DynamoDB holds metadata and the error index. Terraform is the authoritative provisioning engine for every AWS resource.

### Source and target connectivity layer
On the source side, SIP connects to on-premises systems, Oracle Cloud Infrastructure (OCI), legacy core applications (Oracle RMS, Manhattan WMS and Magenta Retail), cloud and digital platforms (Salesforce Service Cloud and eStar Online) and B2B partner portals. On the target side, flows deliver to Oracle databases, eStar (ecommerce), file systems and partner endpoints.

### Pull-based consumption
SIP pulls data and sources do not push it. SIP retrieves data from source systems on its own schedule instead of accepting unbounded pushes, so the consumer controls the rate. Where pull does not fit, as with SaaS webhooks that can only push, SIP accepts the push into a queue and processes it at its own pace, so the same principle holds.

> 🔎 Added context: A webhook is an HTTP callback that a SaaS product sends when an event occurs. Because the sender decides when to send, a queue is needed to absorb bursts.

### Figures 1 and 2
Figure 1 shows the key building blocks in three layers. Figure 2 shows the multi-lane service bus with four processing paths (batch, API, JDBC and event), transformation routing and error management. The extracted text contains only the figure captions, so the diagrams themselves are not described further here.

## SIP routes traffic through four pattern-led lanes
Each of the four lanes handles one kind of traffic, and each routes to the runtime that fits its payload and duration.

### Lane 1: MFT/Batch
This lane handles file-based ingestion. Automic or Amazon EventBridge triggers AWS Transfer Family to receive SFTP traffic, archive the files to S3 and hand off to a parser Lambda for validation and transformation.

### Lane 2: API/Real-Time
This lane processes synchronous REST requests through Amazon API Gateway into a Step Functions orchestrator. A transformation engine routes each request to Lambda for jobs under five minutes, or to Apache Camel on ECS Fargate for long-running and stateful flows.

### Lane 3: JDBC/Data Aggregator
This lane handles direct database connectivity for bulk extractions. It routes through Lambda and Amazon AppFlow for lightweight aggregation, or through ECS Fargate for heavier workloads.

### Lane 4: Event/Message
This lane manages asynchronous flows. External events enter through API Gateway and internal events route through Amazon EventBridge, with Amazon SNS for fan-out broadcasting and Amazon SQS for buffered consumption.

### Error Management System (EMS)
Across all four lanes, EMS provides one error-handling layer. Failed transactions route to a dead-letter queue in Amazon SQS. EMS stores the error metadata in DynamoDB and the payload in S3, and every failure carries a correlation ID and a standard JSON error contract. EMS is built on Spring Boot on Lambda, with a `Next.js` front end on AWS Amplify.

L1 and L2 operators use the EMS dashboard to inspect, modify and resubmit failed transactions idempotently, without developer involvement. New interfaces join EMS automatically through a unique interface ID. The author says this moved CRG from developer-led (L4) recovery to operator-led (L1) first-line resolution.

### Idempotent resubmission
Idempotent resubmission means that replaying the same failed message produces the same result as sending it once, so an operator can retry safely. The article relies on this property to let operators fix and resend failures without changing data twice.

> 🔎 Added context: Idempotency is a property of an operation. Repeating it has the same effect as running it once. Many message systems deliver at least once, so consumers must tolerate duplicates.

## Key design decisions
The section lists four design choices.

### Lambda-first, ECS as fallback
Lambda handles short-duration event processing. Apache Camel on ECS Fargate handles long-running, stateful and large-file flows. The trade-off is that teams run two runtimes and must know which flows belong on each.

### Reproducible infrastructure
Every AWS resource is defined in Terraform, so any environment can be rebuilt exactly from code. This fixes configuration drift, a bad change or an accidentally deleted resource, and the same templates can stand up a new environment or Region. Terraform state sits in a versioned, access-controlled S3 bucket. Infrastructure as code rebuilds the platform but not its data, so data is protected separately. DynamoDB point-in-time recovery restores tables, S3 versioning restores objects, and DLQs hold failed messages for redrive. Lambda, SQS, DynamoDB and Step Functions run across multiple Availability Zones by default, so a zone outage needs no redeploy.

### Amazon EventBridge as canonical event bus
All inter-pattern communication flows through EventBridge. The bus provides content-based routing, event replay and native integration with over 200 AWS services. Putting every pattern on one bus centralizes routing, but it also makes the bus a shared dependency for the whole platform.

### Five-layer security
The five layers are:
- AWS Identity and Access Management (IAM) least-privilege role-based access control (RBAC).
- AWS WAF with virtual private cloud (VPC) isolation.
- Data classification from Public through Restricted.
- TLS 1.2 or later in transit, with AWS Key Management Service (AWS KMS) encryption at rest.
- Structured audit logging with automated personally identifiable information (PII) redaction.

## Open source: Enterprise Integration Patterns CDK Library
The open-source AWS CDK construct library codifies 29 canonical integration patterns as reusable L3 constructs. Each construct provisions messaging infrastructure, IAM permissions, dead-letter queues and retry logic from a single declaration. Amazon CloudWatch alarms are deliberately left to the consumer because thresholds and notification targets are environment-specific. The library is written in TypeScript and AWS CDK. Teams that use Terraform or another infrastructure-as-code tool can read the patterns and translate them to their own provisioning framework.

## Prerequisites
You need an AWS account with the AWS CDK bootstrapped in your target Region, Node.js 20 or later, and the AWS CDK CLI v2. Familiarity with TypeScript helps, but the examples are short. Path 2 also requires Kiro or Claude Code.

## Getting started
There are two ways to go from a requirement to a deployed pattern. Path 1 is hands-on: you write the construct declaration and run the CDK commands yourself. Path 2 is agent-assisted: the agents in the repository select and configure the construct, and you run the final deploy. Both produce the same result, a small `app.ts` that instantiates one or more of the 29 constructs, so the security posture is identical either way. The author suggests Path 1 if you already know the pattern you need, and Path 2 if you are mapping a legacy platform's vocabulary. Both paths begin with the same setup:

```bash
git clone https://github.com/aws-samples/sample-enterprise-integration-patterns-cdk.git
cd sample-enterprise-integration-patterns-cdk
npm ci
npm run build
npm test
```

The repository is a construct library, not a deployable application. `app.ts` ships as an empty stub, so `cdk synth` on a fresh clone produces no stacks by design.

### Path 1: Declare a pattern yourself
Open `app.ts` and declare the construct you need. The README's Implementation Guide has worked examples for request-reply, publish-subscribe, scatter-gather and saga. Then synthesize and deploy:

```bash
npx cdk synth
npx cdk deploy
```

**What this does:**
- `npx cdk synth` builds the CloudFormation templates from the `app.ts` declaration without creating anything in AWS.
- `npx cdk deploy` creates the stack in the AWS account and Region bootstrapped earlier.
- Running `npx cdk deploy` is a separate step from synthesis, so deployment is a deliberate action.
- The stub `app.ts` produces no stacks until a construct is declared, which is why a fresh clone synthesizes nothing.

### Path 2: Let the agents do the selection and configuration
Open the repository in Kiro or Claude Code and select an agent from the chat input. Kiro, the agentic coding IDE and CLI from AWS, uses spec-driven development to turn natural-language requirements into pattern compositions. Describe the integration requirement in your own terms. The agent selects the construct, writes the declaration into `app.ts` with correct properties, and validates it with `npm run build`, `npm test` and `cdk synth`. Deployment stays with you: the shipped `eip-architect` agent is denied `cdk deploy` and `cdk destroy` by design. When the agent reports the declaration ready, you run the same two commands as Path 1.

The repository ships two agent skills and two custom agents:
- `integration-advisor` runs discovery. It asks questions, maps each existing interface to a construct and writes an assessment file, without touching code.
- `eip-architect` takes that assessment, writes the CDK declarations with the right configuration for each pattern, and verifies them.

Use them in order, or go straight to `eip-architect` if you already know what you are building. Figure 3 shows the two agents running in the Kiro CLI.

## Clean up
Remove anything you deployed with `npx cdk destroy`. The constructs default to `RemovalPolicy.DESTROY`, so queues, tables and buckets created by a pattern are removed with the stack.

> 🔎 Added context: A removal policy tells CloudFormation what to do with a resource when its stack is deleted. Destroy is convenient for a demo, but for production data it means deleting the stack can delete the data too.

## Results
Annual integration service costs fell from about $300,000 to $60,000, an 80 percent saving. Four middleware tools (BizTalk, Workato, Apollo and Kafka) were consolidated into one. The main goal, full decommissioning of Microsoft BizTalk Server and its VMware and Broadcom infrastructure, has been achieved. CRG now deploys new interfaces through standard patterns in hours rather than weeks, using cookie-cutter Terraform templates and automated CI/CD pipelines.

The Error Management System moved failure resolution from engineering to L1 operators, which changes the support cost model. Distributed telemetry across Amazon CloudWatch, AWS X-Ray and Dynatrace traces each transaction end to end with correlation IDs through every integration hop. CRG migrated interfaces in waves grouped by business function, and service-group rollback lets any failed wave revert on its own. Because the pattern layer is decoupled from the service layer, new AWS services can be added as steps in an existing pattern composition without re-architecting the platform.

### Wave migration with service-group rollback
Interfaces move in waves grouped by business function, not all at once. Each wave can roll back independently if it fails, so one failed group does not force the whole migration to revert.

## Conclusion
For organizations retiring legacy middleware or designing integrations from scratch, the goal is a decoupled architecture in which systems exchange data through well-understood patterns, not through a central runtime that they all depend on. The author calls CRG's approach a replicable playbook. Middleware carries compounding costs in licensing, hosting, scarce talent and slow delivery, and a pattern-driven migration onto cloud-native services addresses all four at once. The author states that the integration patterns are the same whether a team runs BizTalk, MuleSoft, TIBCO or Boomi, and that only the execution layer changes.

## About the authors
The byline names Aman Sharma, Andrew Hooker, Tim O'Sullivan and Vijay Krishnaswamy. The author bios were not present in the extracted text.

## 🔑 Key takeaways
- Declaring an interface's integration pattern and provisioning it from Terraform modules replaced per-interface orchestration code at CRG.
- Four pattern-led lanes (batch/MFT, API, JDBC, event) each route to Lambda or to Apache Camel on ECS Fargate by payload and duration.
- One Error Management System with dead-letter queues, correlation IDs and idempotent resubmission moved first-line recovery to L1 operators.
- Consolidating BizTalk, Workato, Apollo and Kafka into one platform cut annual integration service costs from about $300,000 to $60,000.
- Dead-letter queues hold full customer payloads, so they need the same encryption, retention and access controls as the primary channel.
- Pulling from sources, not accepting unbounded pushes, lets the consumer control the rate; push-only sources go through a queue.
- Standard patterns are the stable part of integration; cloud services change only the execution layer.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Application (pattern layer, four lanes, EMS, interface composition); Technology (AWS services, Terraform, multi-AZ resilience, CI/CD); Data (payload retention, DLQ handling, S3 versioning, DynamoDB point-in-time recovery, data residency). Business appears only as motivation (cost, talent, velocity). |
| Scope | Segment or capability: one integration platform serving CRG's 21 interfaces across its retail, ecommerce and partner systems. It is not an enterprise-wide architecture. |
| Stakeholders and concerns | Integration architects and platform leads (cost and tool sprawl); engineering managers (delivery speed and skills); operators (first-line error resolution); security and compliance teams (encryption, PII redaction, data residency); finance (VMware and licensing cost). |
| Frameworks and notations | None named for TOGAF, ArchiMate, BPMN, BIZBOK or C4. Enterprise Integration Patterns is named as the pattern catalogue behind the CDK library. Terraform, AWS CDK, Kiro and Claude Code are named as tools. |

### Decisions and trade-offs
- **Replace BizTalk with a pattern-led cloud-native platform:** The article does not list the alternatives it weighed. It cites end-of-support, VMware and Broadcom cost increases, the hiring gap and tool sprawl as reasons to change. The cost is that CRG now owns the platform and its patterns, which need their own engineering skills.
- **Lambda-first, ECS Fargate as fallback:** Lambda is used for short event processing and Apache Camel on ECS for long-running or stateful flows. The cost is two runtimes that teams must choose between.
- **Pull over push:** SIP pulls from sources and queues push-only sources. The cost is extra queueing and polling logic, in exchange for consumer-controlled rate.
- **Terraform as the provisioning authority:** Every AWS resource is defined in code, so environments can be rebuilt. The cost is that state must be secured and kept consistent, and data must be protected separately from the infrastructure.
- **EventBridge as the single inter-pattern bus:** It gives content-based routing, replay and wide AWS integration. The cost is a shared dependency for all patterns.
- **Operator-led error handling through EMS:** Operators resubmit failed transactions without developers. The cost is building and running the dashboard and the error contract.
- **Open-source CDK library with CloudWatch alarms left to consumers:** Teams get reusable constructs, but thresholds and notifications stay environment-specific and must be set by each team. The default removal policy also deletes resources with the stack.
- **Agent-assisted selection with human deployment:** The agents write and validate CDK code, but `eip-architect` is denied `cdk deploy` and `cdk destroy`. The cost is that a person still runs the final deploy.
- **Wave migration with service-group rollback:** Interfaces move in business-function groups, each able to roll back alone. The cost is running old and new paths side by side during the migration.

### Foundations it rests on
- [Application Architecture](/foundations/application-architecture.md) — The SIP is an application architecture: a structure of capabilities (lanes, patterns, EMS) that interact to move data between systems.
- [Technology Architecture](/foundations/technology-architecture.md) — The AWS services, Terraform modules and CI/CD pipelines form the technology services and components the platform depends on.
- [Architecture domain](/foundations/architecture-domain.md) — The article covers Application and Technology most directly, and Data through payload retention and recovery, which matches the TOGAF split of domains.

## 🏛️ For Kenji
This is a useful case for an architect deciding whether to keep, replace or re-platform a middleware estate. It separates the stable interface patterns from the product that implements them, which is a good frame for an architecture-governance decision. For an MBA lens, the case is a clear cost and risk story: licence and hosting costs, talent scarcity, and migration risk managed in waves. Question it raises: how would you measure the operational cost of owning a Terraform-and-CDK pattern library over the years, compared with licence fees? This is your interpretation, not the author's.

## 📚 Reading plan
### From the article
- [GitHub repository: sample-enterprise-integration-patterns-cdk](https://github.com/aws-samples/sample-enterprise-integration-patterns-cdk/tree/main) — The source of the 29 constructs, the setup steps and the agent skills.
- [Implementation guide](https://github.com/aws-samples/sample-enterprise-integration-patterns-cdk/blob/main/docs/implementation-guide.md) — Worked examples for request-reply, publish-subscribe, scatter-gather and saga.
- [bin/app.ts](https://github.com/aws-samples/sample-enterprise-integration-patterns-cdk/blob/main/bin/app.ts) — The entry file where you declare constructs.
- [Kiro](https://kiro.dev/) — The agentic IDE and CLI used for agent-assisted pattern selection in Path 2.
- [Claude Code](https://claude.com/product/claude-code) — The other supported coding assistant for Path 2.

### To go further
- Enterprise Integration Patterns (the catalogue of messaging patterns the library implements) — the original reference for publish-subscribe, content-based routing, request-reply, scatter-gather and saga.
- Terraform state management and remote backends — how to secure and share the state file that the article keeps in a versioned S3 bucket.
- Dead-letter queue design and redrive — how to keep failed messages safe and recover them without duplicating side effects.
- Idempotent consumer design — the property that lets operators resubmit failed transactions safely.
