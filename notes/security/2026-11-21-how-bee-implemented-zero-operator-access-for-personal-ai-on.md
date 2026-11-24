---
type: Article Summary
title: How Bee implemented Zero Operator Access for personal AI on AWS
description: Bee proves its servers run approved, publicly logged software before releasing user keys, so operators cannot
  read sensitive data.
resource: https://aws.amazon.com/blogs/architecture/how-bee-implemented-zero-operator-access-for-personal-ai-on-aws
tags:
- security
- zero-trust-architecture
- remote-attestation
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/how-bee-implemented-zero-operator-access-for-personal-ai-on-aws
  title: How Bee implemented Zero Operator Access for personal AI on AWS
  author: AWS Architecture
status: stable
source_url: https://aws.amazon.com/blogs/architecture/how-bee-implemented-zero-operator-access-for-personal-ai-on-aws
source: AWS Architecture
source_type: blog
published: '2026-11-20'
domain: security
concepts:
- zero-trust-architecture
- remote-attestation
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d810c89bbfb452ee48a48
---

## 🎯 Core thesis
Bee can credibly promise that its own operators cannot read users' sensitive AI data, because each user's device releases keys only to servers whose boot and workload measurements match approved, publicly logged software, and the network and organizational controls close the paths attestation cannot measure.

## 🧭 Why the author wrote it
Bee is a wearable personal AI that listens to a user's day, remembers commitments and ideas, and handles conversations about health, finances and family. The author argues this data must stay protected from Bee's own engineers, other Amazon employees and AWS infrastructure operators, which the article calls Zero Operator Access (ZOA). Bee's own controls cover the first two groups; protection from AWS infrastructure operators is inherited from the AWS Nitro System. If the thesis holds, a user does not have to trust a policy statement, because the user's device can verify the software before it shares any key.

## Solution overview
The article frames security with the AWS shared responsibility model: security of the cloud belongs to AWS, and security in the cloud belongs to the customer. AWS operator access controls are designed so that AWS personnel have no technical pathway to customer data. Bee applies the same principle to its own operators, which means hardening every layer of the stack.

### EC2 instance attestation
This is Bee's core control. A user's device releases the keys needed to decrypt their data only to instances running software it has verified. Attestation proves what runs inside an instance, but it cannot measure what surrounds the instance, such as which networks the instance can reach or who can change account permissions. Bee therefore pairs attestation-gated key release with network, storage and organizational controls that close those gaps. The article's Figure 1 shows how the controls fit together; only its caption is in the extracted text, so the diagram is not reproduced here.

### AWS shared responsibility model
The shared responsibility model divides security into two domains with two owners. AWS owns security of the cloud, and the customer owns security in the cloud. The article uses it as the baseline: AWS's operator controls protect the infrastructure, and Bee extends the same no-access idea to its own people. The model is the reason the article separates what Bee can control from what it inherits from AWS.

### AWS operator access controls
These are the AWS controls that make sure AWS personnel have no technical route to customer data. Bee relies on them for the infrastructure layer and does not claim to implement them itself. Bee's own work begins above that layer, with its own operators and its own software.

### Build time
A transparency service, run by a separate Amazon team that Bee does not control, keeps a copy of every approved Amazon Machine Image (AMI), container and model for later audits. It publishes the expected measurements for each approved deployment to Sigstore Rekor, a public transparency log. The attestable AMI is shared to the Bee account, and the transparency proofs are stored in Amazon S3. At boot, each instance records what it is running in NitroTPM, then gets a TLS certificate from AWS Private Certificate Authority (AWS Private CA) with its attestation and transparency proofs embedded. The Bee app checks this certificate before it shares any keys. If the software has changed, the app refuses to connect.

### Runtime
The Bee mobile app connects only to an attested instance, over TLS. The instance presents its certificate, and the app verifies the attestation against the AWS Nitro root and the transparency log. Only if both match does the app release a short-lived key. That key exists only in memory, in the app servers and Redis key stores. Inference servers never receive keys. Data stored in Amazon RDS, Amazon ElastiCache and S3 is encrypted before it leaves the boundary. The overview adds that AWS Lambda integrations receive plaintext only with the user's consent. The article states this differently in its networking section, where Lambda functions never see plaintext. The two statements read as different cases, but the article does not reconcile them, so this note does not either.

## Establish a verifiable compute foundation
The first problem Bee had to solve was trust. A user's device must verify that Bee's servers booted only with approved software, and that verification needs evidence rooted in a mechanism outside the server application itself. Bee uses EC2 instance attestation to obtain signed measurements of the instance's boot software.

### Attestable AMIs
Bee builds its base images with an internal, hardened pipeline that produces attestable AMIs for Amazon EC2. During the build, the pipeline computes reference measurements offline from the Unified Kernel Image (UKI) and the Secure Boot configuration. These are the expected values for platform configuration registers (PCRs), which record cryptographic measurements during boot. Bee uses PCR4, PCR7 and PCR12 as its verification targets. Because the values are computed without booting the image, Bee knows the expected measurements before any instance launches.

### Hardened image properties
Following AWS guidance on isolating data from your own operators, every Bee attestable AMI has these properties:
- No interactive access: the image includes no SSH, SSM or shell, so operators have no path into a running instance.
- Only trusted software: the image contains only the components the workload needs, and every component is measured.
- Read-only file system and ephemeral local storage: the root file system is read-only, local instance storage is encrypted with a key held only in memory, and instances are replaced rather than rebooted, so no data outlives the instance.

### NitroTPM attestation document
At runtime, the bootloader and operating system measure the boot components and extend those measurements into NitroTPM PCRs. NitroTPM provides an attestation document with the resulting values, signed by the AWS Nitro System through the AWS Nitro Attestation PKI. Any verifier can check those values against the approved reference measurements. With the same measured components and configuration, the selected PCR values stay consistent, which makes them suitable for remote verification.

## Extend the chain of trust to your containers and data
A trusted OS is necessary but not sufficient. Bee's application containers and model weights can change independently of the AMI. If an unauthorized user could swap a container image or a model file without detection, the attestation document would still show clean boot measurements even though the workload had been altered.

> ❌ **Anti-pattern — attesting only the OS:** treating a clean boot measurement as proof that the whole running system is approved. **Why people do it:** the article does not say. **Consequence:** a container image or model file can be swapped while the attestation document still shows clean boot measurements. **Fix:** measure the workload as well and fold it into the same signed attestation, as Bee does with PCR14.

### Workload measurement (PCR14)
Bee solves this with a custom measured boot tool that runs before any application container starts. The tool resolves each Docker image in the manifest to its immutable Amazon Elastic Container Registry (Amazon ECR) digest, and retrieves each Amazon S3 artifact's SHA-256 checksum. It computes a deterministic hash across the manifest, all container digests and all artifact checksums, then extends NitroTPM PCR14 with that value. Any change to any container image, model weight or the manifest itself produces a different PCR14 and a different attestation document.

### Measurement ordering
PCR14 is extended before the NitroTPM attestation document is obtained. The attestation document that Bee's application servers present to clients therefore reflects both the OS state (PCR4, PCR7, PCR12) and the workload state (PCR14) as a single signed hardware measurement.

### Transparency log (Sigstore Rekor)
Before deployment, the transparency service runs an independent signing process. It resolves the same ECR digests and S3 checksums, recomputes the expected PCR14, and commits the full PCR map to Sigstore/Rekor, an independent, publicly auditable transparency log. The resulting proof bundle is stored in regionally replicated S3 buckets, so verification at boot time is fully offline, with no runtime dependency on external infrastructure.

## Validate attestation at every connection
At boot, the instance first confirms that its own measurements match the approved entry in the transparency log. If they do not match, it stops there. If they match, it requests a TLS certificate from AWS Private CA, with its attestation document and transparency proof embedded in the certificate. The attestation is tied to that certificate's key, so another instance cannot reuse it.

### Client-side verification
The user's device decides whether to trust the instance. When it connects, the device checks the embedded evidence offline against roots built into the app, including the AWS Nitro root. If the measurements do not match an approved configuration, the client rejects the connection and shares no keys.

### Mutual TLS between instances
The same certificates protect traffic between instances over mutual TLS. Only roles that need to start connections, such as application servers, receive client certificates. Inference instances do not, so they cannot reach key stores or other services.

## Limit what the network can reach
All attested EC2 instances run in isolated subnets of an Amazon Virtual Private Cloud (Amazon VPC) with no internet egress. The only outbound paths are VPC endpoints to Amazon RDS and Amazon ElastiCache, and tightly scoped AWS Lambda functions. The Lambda functions handle external integrations such as email or push notifications.

### Tightly scoped Lambda integrations
In the networking design, Lambda functions receive only data already encrypted with user keys. They never see plaintext. This is the stricter of the two statements about Lambda in the article (see Runtime above), and the article does not explain the difference between them.

## Keep encryption keys out of persistent storage
A user's encryption keys originate on their devices. After verifying an instance's attestation, the device releases a short-lived, scoped key to that instance. The key exists only in memory, in the application instance and in Redis key stores running inside attested instances with persistence disabled. It expires after 7 days without user activity. Because AWS never holds this key, no storage layer, backup, IAM policy or KMS grant can provide access to it.

### Ciphertext at rest
Sensitive data is encrypted inside the attested instance before it reaches Amazon RDS, Amazon ElastiCache or Amazon S3, so those services store only ciphertext. Plaintext stays inside the attested boundary. The article refers readers to the Bee Private Compute whitepaper for the full key hierarchy and data flow; that whitepaper is not expanded here.

## Use AWS organizational controls to eliminate unilateral access
Strong technical controls also need strong permission boundaries. If one person could change those controls, they could be bypassed. Bee is designed so that no single team can do this. The Bee team manages the private CA and the deployment pipeline but does not control the transparency service. The transparency team can commit entries to the Sigstore log but has no access to Bee's AWS accounts. Deploying code that does not appear in the public transparency log would require coordination across independent teams.

> ❌ **Anti-pattern — one team controls both the controls and their record:** a single team that can change the attestation pipeline and also the approved-software record. **Why people do it:** the article does not say. **Consequence:** that team can change the controls and bypass them. **Fix:** split the duties across independent teams, as Bee does, and enforce the split in the cloud accounts.

### Service control policies (SCPs)
AWS Service Control Policies (SCPs) back this separation at the infrastructure level. They apply across all Bee accounts, limit which actions are possible in each account, and need multiple levels of approval to change. This makes the separation of duties enforceable in AWS itself, not only through process. SCPs add defense in depth. The primary safeguard is still attestation: any change to the software changes its measurements, and the Bee app refuses to connect to software it does not recognize.

## Conclusion
The article's summary is that users hold their own keys and so decide who can read their data. Before the Bee app shares a key, it checks that the server runs approved, publicly logged software, and no operator or policy can override that check. EC2 instance attestation makes the check verifiable. Network isolation, ephemeral storage and organizational controls close the paths attestation cannot measure.

Bee is moving its inference to Amazon Bedrock Mantle, which the article describes as providing zero operator access as a managed capability. User-held keys and attested application servers stay part of the Bee architecture, so data stays protected from Bee operators. The article presents the same pattern as reusable for any workload that handles sensitive data.

> 📋 **Checklist — Getting started**
> - [ ] Read the EC2 instance attestation documentation.
> - [ ] Follow the guide to isolating data from your own operators.
> - [ ] Explore the full architecture in the Bee Private Compute whitepaper.
> - [ ] If you have a similar use case or want help building it, contact your AWS account team or reach out to AWS.

## Further reading
The article says Trail of Bits independently assessed the Bee Private Compute whitepaper, which covers the complete architecture, including client-side cryptography. It also points to AWS Nitro Sandboxes, which give each Amazon EKS pod, Amazon ECS task or Amazon EC2 workload its own hardware-isolated VM, with attestation and zero operator access built in. These are the article's own claims and were not checked here.

## About the authors
The extracted text contains only this heading and no author text, so nothing is expanded here.

## 🧠 Mental models
### Attestation proves what runs, not what surrounds it
**The model:** A signed measurement covers the software inside a machine, not the networks, permissions or accounts around it. **How to apply it:** For each trust decision, list what the measurement cannot see, and give each of those items its own control.

### Separation of duties across independent teams
**The model:** A control is only as strong as the team that can change it. If one team can change both the control and its record, the control can be bypassed. **How to apply it:** Give the private CA, the deployment pipeline and the approved-software log to different owners, as Bee does, and enforce that split with cloud policy.

### Replace instead of repair
**The model:** Instances are replaced rather than rebooted, so no data outlives an instance. **How to apply it:** Keep state that must survive outside the instance, in the article's sense of encrypted data that leaves the boundary only as ciphertext, and treat the instance itself as disposable.

## 🔑 Key takeaways
- Bee's core control is attestation-gated key release: the user's device releases a key only after verifying the instance's signed boot and workload measurements.
- Expected PCR values are computed offline from the image before any instance launches, so the verifier knows the approved values in advance.
- A change to any container image, model weight or manifest changes PCR14 and the attestation document, which closes the gap that a clean OS measurement leaves open.
- Sigstore Rekor proofs make the approved-software record publicly auditable, and boot-time verification runs fully offline.
- Keys exist only in memory and expire after 7 days without user activity. AWS never holds them, so no storage layer, backup or IAM grant can expose them.
- Separation of duties is enforced with SCPs and multi-level approval, with attestation as the primary safeguard.

## 🏛️ For Kenji
This is your interpretation, not the author's. For an architect working on AI governance, the pattern turns "we do not look at customer data" from a policy promise into a control that users can check, and the PCR14 idea applies to model weights in any serving stack. Which of your current trust decisions rely on a policy or process that a measured or enforced control could replace, and who in your organization would own the transparency record?

## 📚 Reading plan
### From the article
The extracted article text includes link text only, not URLs, so each item below is named by its link text.
- Bee Private Compute whitepaper — the full architecture, key hierarchy and client-side cryptography, independently assessed by Trail of Bits.
- EC2 instance attestation documentation — the mechanism behind the core control.
- Isolating data from your own operators (AWS guidance) — the basis for the hardened image properties.
- AWS shared responsibility model — the baseline the article extends.
- AWS Nitro System — the source of protection from AWS infrastructure operators.
- Sigstore Rekor — the public transparency log used for approved measurements.
### To go further
- Zero-trust architecture — the principle of checking every connection regardless of network location.
- Remote attestation and measured boot with TPM PCRs — the general technique under the core control.
- Transparency logs for software artifacts — how a public log makes approved builds auditable.
- AWS Nitro Sandboxes — the article's pointer for per-workload hardware isolation in containers.
- AWS Service Control Policies — how separation of duties is enforced across accounts.

## Concepts
- [zero-trust-architecture](/concepts/security/zero-trust-architecture.md)
- [remote-attestation](/concepts/security/remote-attestation.md)
