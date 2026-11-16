---
type: Handoff
url: https://lnkd.in/p/diiMkdvS
source_type: linkedin
origin: forwarded
---

Google's AX is like Kubernetes but for AI agents. It lets you deploy your AI agents in seconds, and then scale them as needed.

It's an open-source orchestrator to run agent workloads at scale, potentially billions of tasks across a cluster.

Agents aren't just stateless services or batch jobs. They accumulate state, execute code, interact with tools, and can keep running in loops that consume resources.

To serve them, AX introduces three core primitives:

- Tasks: Run agent workloads inside isolated sandboxes with resource limits.

- Workspaces: Preconfigure Git repositories, MCP servers, and skill packages so agents have their environments ready.

- Models: Configure the LLMs used by the platform, with credentials managed through Kubernetes secrets.

It also handles the agent lifecycle. You can watch a task's status, connect to its sandbox to inspect what it's doing, or suspend it and resume from its saved state later.

The developer experience is deliberately familiar if you've used Kubernetes. Define your task and workspace in YAML, then use commands like:
- ax apply
- ax watch
- ax ssh
to manage the workload.

As teams move towards fleets of coding agents and autonomous workers, sandboxing, environment management, lifecycle control, and resource governance become increasingly important.

♻️ Share it with anyone building AI agents at scale :)

I share tutorials on how to build + improve AI apps and agents, on my newsletter AI Agent Engineering: https://lnkd.in/gaJTcZBR

#AI #LLMs #AIAgents
