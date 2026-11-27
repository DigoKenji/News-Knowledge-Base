---
type: Article Summary
title: 'EP234: Docker vs Kubernetes'
description: Docker packages applications and Kubernetes orchestrates them across machines; the article also explains Redis
  internals, model tradeoffs, watermarking and nine distributed patterns.
resource: https://blog.bytebytego.com/p/ep234-docker-vs-kubernetes
tags:
- cloud-infra
- virtualization-containers
- container-orchestration
- consistency-models
- replication-partitioning
- consensus
- message-queue
- saga-pattern
- distributed-system
- fine-tuning
- inference-tradeoffs
- large-language-model
- circuit-breaker
- text-watermarking
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-27T06:47:00-03:00'
sources:
- resource: https://blog.bytebytego.com/p/ep234-docker-vs-kubernetes
  title: 'EP234: Docker vs Kubernetes'
  author: ByteByteGo
status: stable
source_url: https://blog.bytebytego.com/p/ep234-docker-vs-kubernetes
source: ByteByteGo
source_type: blog
published: '2026-11-21'
domain: cloud-infra
concepts:
- virtualization-containers
- container-orchestration
- consistency-models
- replication-partitioning
- consensus
- message-queue
- saga-pattern
- distributed-system
- fine-tuning
- inference-tradeoffs
- large-language-model
- circuit-breaker
- text-watermarking
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/425baf74172d81fb9796e6d110bf3f08
---

## 🎯 Core thesis
Docker and Kubernetes work at different layers: Docker packages an application into a portable container and runs it, usually on one machine, while Kubernetes manages many containers across many machines.

## 🧭 Why the author wrote it
The article is a short newsletter edition that explains how several common infrastructure and AI systems work, one brief section at a time. The title frames the Docker versus Kubernetes comparison, which is often mentioned in the same breath, so the first question is where one tool stops and the other starts. If the thesis is right, choosing between them becomes a question of scope: one host calls for Docker and Compose, while production needs such as scaling and self-healing across machines call for Kubernetes.

## Docker vs Kubernetes

### Container image and container (Docker)
Docker is a tool for packaging an application, including its code, libraries, runtimes and configuration, into a portable unit called a container image. A container is started from that image and can run on a laptop, a server or in the cloud. The image is the packaged artifact and the container is the running instance started from it. The point of packaging everything together is portability: the same unit can be deployed in each of those places.

> 🔎 Added context: A container shares the host operating system's kernel instead of booting its own full OS, which is the main difference from a virtual machine. This is general background, not a claim from the article.

### Docker Compose
Docker usually runs on a single machine. The article names Docker Compose as the way to manage several containers on that one host. This is the boundary of Docker's scope: Compose coordinates containers on one host, not a fleet of machines.

### Kubernetes
Kubernetes is an orchestration platform that manages the deployment and maintenance of containerized applications, particularly across multiple machines. The article lists what it does: it starts and stops containers, creates more containers or reduces their number as needed, distributes network traffic and rolls out application updates. The article says Kubernetes is useful when a production application needs automatic scaling, self-healing, high availability and deployment across multiple machines. A small application on a single machine may not need those features.

## How Redis Works Under the Hood
The article's starting point is that Redis is fast because it keeps everything in RAM and uses efficient in-memory data structures. It then walks through what happens to one command.

### Data held in RAM
Redis keeps its data in memory, and each later step operates on that in-memory state. Speed comes from avoiding disk on the normal read and write path. The article gives this as the reason for Redis's speed and does not quantify it.

### Sending the command and decoding it (RESP)
The application sends a Redis command to the server. If replication is configured, commands that update data are normally sent to the primary server. Redis decodes the request, which is usually encoded in the Redis Serialization Protocol (RESP), and separates it into the command name, the key or keys, values and other arguments.

### Parsing, validation and authorization
Redis parses and validates the request. It checks that the command exists and that its arguments are correctly formatted, and it checks that the client is authenticated and authorized. If the request is invalid, an error is returned and nothing runs.

### Command table lookup and execution
Redis looks the command up in its internal command table, which points to the code that executes it. Commands are executed sequentially, one after another.

### Updating keys in memory and expiration
Redis locates the relevant keys in its in-memory dictionaries and performs the requested operation. If a key has a time-to-live, Redis stores its expiration timestamp separately, and expired keys are removed by lazy or active expiration.

> 🔎 Added context: Lazy expiration checks a key when it is accessed, while active expiration removes expired keys in the background. Both are standard Redis behaviour, not claims the article makes in detail.

### Persistence and replication
Commands that change data in RAM may need to propagate those writes to supporting systems. The article describes AOF as a persistent mechanism based on logging writes, and RDB as a persistent mechanism based on snapshots. It also mentions replication, which copies writes to other Redis servers.

> 🔎 Added context: In Redis, RDB is a snapshot file format, and replication is a separate mechanism that streams writes to replica servers. The article's sentence combines the two loosely.

### Eviction policy
When Redis reaches its configured memory limit, it applies its eviction policy. The article does not name the policies.

> 🔎 Added context: An eviction policy decides which keys to remove to free memory when the limit is hit, for example least recently used keys.

### Response
Finally, Redis builds the response in RESP format and returns it to the client.

## Open Weights vs Closed Models: 7 Tradeoffs in 1 Diagram
For every AI project there are two options: download the weights of an open model and run it yourself, or call a vendor's API. The article's diagram is not in the extracted text, so the seven tradeoffs below come from its written descriptions.

### Fine-tuning
Open weights allow fine-tuning on your own GPUs and data. Closed models do not let users access their weights, so the user cannot fine-tune the model's weights directly.

> 🔎 Added context: Fine-tuning means further training a pre-trained model on a smaller, task-specific dataset.

### Versioning
With open weights, you keep the artifact. You can pin any version and run it for as long as you want. Closed models serve pinned, dated snapshots, but the vendor retires them on its own schedule. The practical difference is who controls how long a given version stays available.

### Transparency
Open weights are inspectable. A closed model is a black box with an API.

### Deployment
Open weights can be deployed on various hardware. With closed models, every device connects inward to one endpoint.

### Licensing
Open-weight licenses are custom, and their terms vary by model. Closed models run under a commercial terms of service that the vendor sets.

### Self-hosting
Open weights are self-hosted by default, so the stack is yours. Closed models are API-first, though some vendors sell on-premises or private VPC deployments.

### Cost
Self-hosting wins only at high volume, once you add ops overhead. APIs are cheaper at low volume and have zero upfront cost. The article gives no break-even figure.

## How does Claude's text watermark work?
The author says Anthropic recently shared its intent to watermark text so that AI-generated text can be identified. The author states that the post is based on their own understanding of how it works, so this section is not an official description.

### Word-by-word generation
LLMs produce text word by word. At each step they generate probabilities for the next likely word. Normally, a random number generator picks one of the good candidates.

> 🔎 Added context: In practice models work on tokens, which are subword pieces, so the article's word is used loosely. The sampling step works the same way on tokens.

### Watermarking by constraining the choice
Instead of sampling freely from the candidates, a keyed function takes a secret key plus the previous few words and decides which candidates are valid to pick from. This repeats for the whole response. Places where several plausible choices exist are where the watermark signal is carried.

### Detection by match rate
To detect a watermark, the checker takes each candidate word in the text and asks whether it is a valid choice given the secret key and the few words before it. A valid word counts as a match. Across the whole text, watermarked text matches far more often, so the overall match rate can serve as an AI-generated score. Detection therefore depends on the same secret key.

The author adds a personal view: they are frustrated by false negatives from AI text detection techniques, especially for technical writing. They ask whether detection is useful or will create more problems.

## 9 Distributed Systems Patterns You Should Know
The article gives one short definition for each of nine patterns. Its list is complete: nine items, one per pattern.

### Replication
Replication makes exact copies of your data and stores them on different servers. The article gives only this definition.

> 🔎 Added context: Replicas are usually kept for availability and for scaling reads. The quorum pattern below depends on having several replicas.

### Sharding
Sharding breaks a large database horizontally, so different rows live on separate machines. Unlike replication, which copies the same data, sharding splits the data across machines.

### Consistent hashing
Consistent hashing distributes data across machines or servers using a virtual circular ring.

> 🔎 Added context: Each key is hashed to a point on the ring and assigned to a server by position on the ring. This way adding or removing a server moves only part of the keys.

### PubSub
PubSub is an asynchronous messaging pattern that decouples service creators from service consumers. The creator publishes and does not need to know who consumes.

### Circuit breaker
A circuit breaker stops an application from repeatedly executing an operation that is likely to fail.

> ❌ **Anti-pattern — repeated calls to a failing operation (implied by the article):** the application keeps executing an operation that is likely to fail. **Why people do it:** the article does not say. **Consequence:** the article implies the application keeps hitting a failing operation. **Fix:** stop calling it while failures persist, which is what the circuit breaker does.

### Retry with backoff
Retry with backoff handles temporary network glitches or brief service timeouts. It is a resiliency pattern.

> 🔎 Added context: Backoff means waiting progressively longer between attempts, so retries do not pile onto a struggling service all at once.

### Leader election
Leader election designates a single master node to manage actions and maintain cluster state. The article says this is what stops two nodes from thinking they are in charge.

> 🔎 Added context: When two nodes both believe they are the leader, the cluster is said to have split-brain.

### Quorum read/write
Quorum read/write ensures enough replicas agree on each read and each write so that the sets overlap. The article says it is used in distributed databases to guarantee up-to-date information.

> 🔎 Added context: If the number of replicas in a write and the number in a read together exceed the total replica count, every read set shares at least one replica with every write set. The actual guarantee depends on how a given database configures these numbers.

### Saga
A saga manages a distributed transaction through a sequence of local transactions across multiple microservices. If one step fails, each earlier step is undone by its own compensating action.

## 🧠 Mental models
### Image versus container
**The model:** The image is the packaged, portable artifact. A container is a running instance started from it.
**How to apply it:** Treat images as what you version and ship, and containers as what you start, stop and replace.

### Quorum overlap
**The model:** Reads and writes each touch enough replicas that their sets must overlap.
**How to apply it:** When choosing replica counts, check that the read and write sizes guarantee overlap, not just that they look large.

### Pinned artifact versus vendor snapshot
**The model:** With open weights you hold the artifact and decide its lifetime. With closed models you depend on the vendor's retirement schedule.
**How to apply it:** When choosing a model, ask who controls how long this exact version stays available.

## 🔑 Key takeaways
- Docker packages an application and its dependencies into a container image that runs on a laptop, a server or in the cloud.
- Docker usually runs on one machine, and Docker Compose manages several containers on that host.
- Kubernetes is aimed at production needs across machines: automatic scaling, self-healing, high availability and rolling updates.
- Redis is fast because it keeps data in RAM, and each command is validated, looked up in a command table and executed sequentially.
- Self-hosting open weights wins only at high volume, once ops overhead is counted. APIs are cheaper at low volume, with zero upfront cost.
- The article describes watermarking as a secret-keyed choice of valid words, detected by a match rate across the text.
- The nine patterns solve different problems: replication and sharding place data, circuit breaker and retry handle failure, leader election and quorum coordinate agreement, and saga undoes partial transactions.

## 🏛️ For Kenji
This is your interpretation, not the author's. The Docker versus Kubernetes line works as a decision boundary: once a service needs self-healing and multi-machine deployment, the orchestration layer's cost is easier to justify. The open-versus-closed comparison applies to AI workloads in your architecture, where the question is at what volume self-hosting's ops overhead stops being worth it. A second question is who would own the model version if a vendor retired it.

## 📚 Reading plan
### From the article
- None. The extracted article text contains no links.

### To go further
- Redis persistence options (RDB snapshots and AOF logs): to see the trade-offs behind the persistence step.
- Kubernetes self-healing and scaling: to see how the orchestration features the article lists are implemented.
- Consistent hashing on a ring: to understand how keys move when servers are added or removed.
- Quorum reads and writes in replicated databases: to check the overlap condition against a specific database's settings.

## Concepts
- [virtualization-containers](/concepts/cloud-infra/virtualization-containers.md)
- [container-orchestration](/concepts/cloud-infra/container-orchestration.md)
- [consistency-models](/concepts/backend-systems/consistency-models.md)
- [replication-partitioning](/concepts/backend-systems/replication-partitioning.md)
- [consensus](/concepts/backend-systems/consensus.md)
- [message-queue](/concepts/backend-systems/message-queue.md)
- [saga-pattern](/concepts/backend-systems/saga-pattern.md)
- [distributed-system](/concepts/backend-systems/distributed-system.md)
- [fine-tuning](/concepts/ai-ml/fine-tuning.md)
- [inference-tradeoffs](/concepts/ai-ml/inference-tradeoffs.md)
- [large-language-model](/concepts/ai-ml/large-language-model.md)
- [circuit-breaker](/concepts/backend-systems/circuit-breaker.md)
- [text-watermarking](/concepts/security/text-watermarking.md)
