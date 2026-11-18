---
type: Article Summary
title: External sandboxes for coding agents
description: Keeping coding agents outside a sandbox while their commands run inside one enforces limits no model judgment can bypass.
resource: https://simonwillison.net/2026/Nov/17/external-sandboxes-for-coding-agents/
tags:
- security
- appsec
- threat-modeling
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://simonwillison.net/2026/Nov/17/external-sandboxes-for-coding-agents/
  title: External sandboxes for coding agents
  author: Simon Willison
status: stable
source_url: https://simonwillison.net/2026/Nov/17/external-sandboxes-for-coding-agents/
source: Simon Willison
source_type: blog
published: '2026-11-17'
domain: security
subdomain: appsec
concepts:
- threat-modeling
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d8126954ddc27d6eb42ef
---

## 🎯 Core thesis
Coding agents can be made much safer by keeping the agent outside a sandbox and sending only the commands it runs into a separate sandbox that enforces file, network and credential limits, because that boundary does not depend on any model's judgment.

## 🧭 Why the author wrote it
Simon Willison's link post introduces work he did with Prime Radiant on running coding agents safely, especially smaller local models that are more prone to mistakes and to prompt injection. If the thesis is right, you can let an agent run commands freely inside a boundary you control, instead of approving every action by hand or trusting full access and hoping for the best.

## Link blog introduction (Simon Willison)
Simon Willison's post is a short link-blog entry dated 17 November 2026. He says he has been exploring ways to sandbox code that agents execute, with a focus on making it safer to try out local models without worrying that a mistake or a prompt injection attack could make them take damaging actions on a machine. He worked on the project with Prime Radiant, the AI research lab behind Superpowers and other projects.

The entry summarises the external sandbox pattern: the code the agent executes runs inside a dedicated sandbox while the agent itself lives outside that sandbox. To demonstrate it, the team released three sandbox server implementations:

- [external-sandbox-v86](https://github.com/prime-radiant-inc/external-sandbox-v86) runs a Node.js server that uses v86 to run Alpine Linux inside WebAssembly.
- [external-sandbox-container](https://github.com/prime-radiant-inc/external-sandbox-container) is a Python web server that runs the sandbox in Alpine Linux inside an Apple Containers instance.
- [external-sandbox-quicksand](https://github.com/prime-radiant-inc/external-sandbox-quicksand) is another Python web server, using Microsoft's Quicksand sandboxing library, which bundles QEMU and Alpine Linux in a Python package.

All three offer the same localhost API, so you can configure Pi, Claude Code or Codex to send API calls to whichever sandbox you run. The sandboxes also include an optional HTTP/HTTPS allow-list proxy, and a credential replacement mechanism that makes authenticated API calls without sharing a secret with the agent. A web UI lets you change the proxy configuration while the sandbox is running. Simon closes by saying he remains fascinated by the challenge of safely running untrusted code, and that this pattern is a neat way to experiment with different approaches. The entry ends with a list of recent blog articles, which is not expanded here.

## Try it out with two commands
The full article opens with a quick demonstration that needs two commands and Node.js 22.19 or later. You change to your project directory and start a sandbox with npx. This downloads Alpine Linux and runs it inside v86, an x86 emulator compiled to WebAssembly and hosted in Node.js:

```bash
npx --yes external-sandbox-v86 \
  --preserve-paths --mount-rw "project=$PWD"
```

**What this does:**
- `npx --yes` runs the published `external-sandbox-v86` package without a confirmation prompt.
- `--mount-rw "project=$PWD"` mounts the current project directory read-write inside the sandbox, under the name `project`.
- `--preserve-paths` keeps the original host path valid inside the sandbox, so commands that mention host paths still work.

In a second terminal in the same directory you start a Pi coding agent. If you have not used Pi before, you run `/login` inside the agent to grant it access to a model:

```bash
npx --yes @earendil-works/pi-coding-agent \
  -e npm:external-sandbox-pi --no-builtin-tools \
  --sandbox-url http://127.0.0.1:8686
```

**What this does:**
- `-e npm:external-sandbox-pi` loads the extension that routes Pi's tools to the sandbox.
- `--no-builtin-tools` disables Pi's own tools, so nothing runs on your computer even if the extension fails to load.
- `--sandbox-url` points Pi at the v86 server on port 8686.

Running `!uname -a` inside Pi should show that commands run in Alpine Linux. The article's example output begins `Linux (none) 6.12.114-0-virt`, which is the kernel of the Alpine guest rather than your Mac. Claude Code and Codex also work, with more setup, described below.

## What this gets you
The article lists five properties of the setup:

- **Your agent stays on your Mac.** Pi, Claude Code and Codex all run on your computer as usual. Only the commands they execute go to the sandbox.
- **Your sandbox only sees the folders you mount.** You choose which folders are available to those commands, and whether each one is read-only or read-write.
- **No network access unless you allow it.** By default the sandbox cannot reach the outside world. Adding the `--proxy` option provides access through an HTTP/HTTPS proxy, limited to the hosts you allow.
- **API keys the agent cannot see.** The proxy performs credential replacement: requests to specific hosts have API keys added, so the secrets stay outside the sandbox and invisible to the calling agent.
- **A web UI for changing the rules.** It offers an in-browser terminal, tools for changing the proxy configuration, and a log of requests made over the network.
- **Any agent works with any sandbox.** The three sandboxes implement the same protocol, so you can mix and match them with your preferred agent.

## As a diagram
The article includes a diagram of the architecture at this point. The diagram is an image and is not present in the text extraction, so its content is not reproduced here. The surrounding prose describes the same split: the agent on the host, and a sandbox that executes its commands.

## Why use external sandboxes?
The article's motivation starts from the observation that coding agents can get things wrong, and that anything they can read or edit may end up damaged or compromised when they do. In an ideal world, agents would run in environments where any damage they can cause is strictly limited. In practice many people give agents full access to both their computers and the wider world, and hope for the best.

One common fix is to approve every action the agent wants to take by hand. The author calls this a terrible way to work, because it is frustratingly slow and leads to decision fatigue, where people approve everything without understanding the question.

> ❌ **Anti-pattern — Manual approval of every action:** Requiring a human to confirm each command or file edit. **Why people do it:** It feels like the only safe option when the agent cannot be trusted. **Consequence:** Slow work and decision fatigue, so reviewers stop reading and approve everything. **Fix:** Constrain what the agent can touch at the boundary, so routine actions need no review.

The external sandbox pattern lets coding agents execute code and edit files inside a sandbox that gives the user control over the files, environment variables and network connections the agent can use. The agent keeps the flexibility to run commands and write and execute code.

The article then compares the approaches of the two big vendors. Claude Code's auto mode and Codex's auto-review both use additional AI checks to decide when it is likely safe to continue, and the author says these appear to work well. The author's point is that those checks are features of particular products, and they still rely on a model judging each action correctly. A sandbox is a protection that does not depend on any model's judgment. This matters most when you explore alternative models, including cheaper models or models that run locally, because these lighter models are more prone to mistakes and are easier victims of deliberate prompt injection.

🔎 Added context: Prompt injection is an attack where text in content the agent reads, such as a web page or file, tries to make the agent follow instructions the user never gave. A model that misreads such text can then run harmful commands.

The team's goals are robust, trustworthy and easy-to-use ways to have coding agents work in a sandbox. Commands should be able to read and write only the files the user chooses, and make only the network requests the user allows.

Two ways of putting an agent in a sandbox were considered. The first runs the agent inside the sandbox, which is the approach of systems such as Fly's Sprites. The second runs the agent outside the sandbox and has it send commands to a sandbox that exists separately. The team chose the second. It avoids odd behaviour in which the agent modifies itself or its direct environment, and it keeps the sandbox boundary well defined and easier to audit. The name "external sandboxes" reflects that the sandbox is external to the agent, not to the computer, since both can run on the same machine.

## The three prototypes
The three implementations are separate but compatible projects, each using a different strategy for external sandboxing:

- **external-sandbox-v86** is a Node.js process that uses v86 in WebAssembly to run Alpine Linux. Its npm package bundles a complete Linux distribution that runs inside the sandbox.
- **external-sandbox-container** is a Python process that wraps Apple Containers running Linux. It can launch a container, run commands in it through the protocol, and terminate the container at shutdown.
- **external-sandbox-quicksand** is a Python process that runs a full QEMU virtual machine through Microsoft's Quicksand. QEMU and the Alpine image arrive as Python wheels, so there is nothing else to install.

The three systems offer identical localhost web APIs, but each agent connects to them differently:

- Claude Code sends only its Bash tool commands to the sandbox. Its native Read and Edit tools still run on the host, outside the sandbox.
- Codex sends its process and filesystem operations, including its native apply_patch.
- Pi sends its bash, read, write, edit, grep, find and ls tools. grep and find need ripgrep inside the guest, which all three default images include.

The team also produced a specification and a conformance testing tool for the API, so that future compatible sandbox servers can be built on other technologies.

## Using it with other sandboxes and agents
The v86 Alpine Node.js server runs on port 8686 by default. The Python Apple Containers server uses port 8687, and the Python Quicksand server uses port 8688, so all three can run side by side. The v86 server is published to npm and runs with npx. The two Python servers are published to PyPI as external-sandbox-container and external-sandbox-quicksand, and run with uvx.

Start the v86 server like this:

```bash
# v86 Alpine Node.js (Node.js 22+)
npx --yes external-sandbox-v86 \
  --preserve-paths --mount-rw "project=$PWD"
```

**What this does:**
- The comment records the Node.js 22 or later requirement.
- The command is the same as the quick demonstration, and it serves the default port 8686.
- The project directory is mounted read-write, and `--preserve-paths` keeps the host path valid.

For Apple Containers, the article gives this:

```bash
# Python Apple Containers (Apple silicon Mac with Apple's container CLI)
container system start
uvx external-sandbox-container \
  --preserve-paths --mount-rw "project=$PWD"
```

**What this does:**
- `container system start` starts Apple's container runtime, which the server needs.
- `uvx` runs the published Python package without installing it permanently.
- The Apple Containers server requires an Apple silicon Mac with Apple's container CLI.

For Quicksand, the article gives this. The first run takes longer while it prepares and caches the Alpine image:

```bash
# Python Quicksand (Apple silicon Mac)
uvx external-sandbox-quicksand \
  --preserve-paths --mount-rw "project=$PWD"
```

**What this does:**
- `uvx` fetches and runs the Quicksand server, which bundles QEMU and Alpine as Python wheels.
- The mount and `--preserve-paths` flags behave as they do for the other servers.
- The first run is slower because the Alpine image is prepared and cached.

Each server mounts the current directory read-write at `/mnt/project` and, thanks to `--preserve-paths`, at its original path too. Add `--mount-ro name=/some/path` for read-only folders. Opening the server's port in a browser gives you the terminal and network rules UI.

## Claude Code
Claude Code's read and edit tools do not run through the sandbox. The mount rules limit what its Bash commands can touch, but Claude can still read and edit files on your Mac directly. This is a limit of the setup that the article states plainly.

To run Claude Code against a sandbox, get the shared shell prefix script from the external-sandbox repository:

```bash
git clone https://github.com/prime-radiant-inc/external-sandbox.git
PREFIX_DIR="$(cd external-sandbox/claude-code && pwd)"
```

**What this does:**
- The first line clones the repository that holds the protocol, the conformance tool and the Claude Code prefix script.
- The second line stores the absolute path of the `claude-code` folder in `PREFIX_DIR`, so later commands do not depend on the working directory.

Then run Claude Code from the project directory the server was started in. The example uses v86 on port 8686:

```bash
mkdir -p .external-sandbox-claude-tmp
CLAUDE_CODE_SHELL=/bin/bash \
CLAUDE_CODE_TMPDIR="$PWD/.external-sandbox-claude-tmp" \
CLAUDE_CODE_SHELL_PREFIX="$PREFIX_DIR/claude-shell-prefix.sh" \
EXTERNAL_SANDBOX_API_URL=http://127.0.0.1:8686 \
claude
```

**What this does:**
- `CLAUDE_CODE_SHELL=/bin/bash` selects the shell Claude Code uses for its Bash tool.
- `CLAUDE_CODE_TMPDIR` points Claude Code's temporary directory at a folder inside the writable mount, because Claude Code uses it for its own bookkeeping.
- `CLAUDE_CODE_SHELL_PREFIX` makes every Bash command pass through the prefix script, which forwards it to the sandbox.
- `EXTERNAL_SANDBOX_API_URL` tells the prefix script which sandbox to contact. Use port 8687 for Apple Containers or 8688 for Quicksand.

The `.external-sandbox-claude-tmp` directory must be inside the writable mount. Otherwise Claude Code cannot do its bookkeeping.

## Codex
To run Codex against a sandbox, create a dedicated Codex home with an `environments.toml` file that points at the sandbox. The example uses v86 on port 8686:

```bash
mkdir -p ~/.codex-external-sandbox
cat > ~/.codex-external-sandbox/environments.toml <<'EOF'
default = "external-sandbox"
include_local = false
[[environments]]
id = "external-sandbox"
url = "ws://127.0.0.1:8686/codex"
connect_timeout_sec = 5
initialize_timeout_sec = 10
EOF
```

**What this does:**
- `default = "external-sandbox"` makes the sandbox the environment Codex uses by default.
- `include_local = false` stops Codex from also offering its own local environment.
- The `[[environments]]` entry gives the sandbox an id and a WebSocket URL on the server's `/codex` endpoint.
- The two timeouts bound how long Codex waits to connect (5 seconds) and to initialise (10 seconds).

Log in once with that home, then launch Codex with it:

```bash
CODEX_HOME=~/.codex-external-sandbox codex login
CODEX_HOME=~/.codex-external-sandbox codex --sandbox danger-full-access \
  -c features.shell_snapshot=false \
  -c features.unified_exec_tty=false \
  --cd "$PWD"
```

**What this does:**
- `CODEX_HOME` keeps this configuration separate from any normal Codex setup.
- `--sandbox danger-full-access` tells Codex not to add its own sandbox inside the external one.
- The two `-c` flags turn off two Codex features, the shell snapshot and the TTY-based unified exec, so Codex works with the external sandbox protocol.
- `--cd "$PWD"` starts Codex in the project directory.

The article reports that its automated tests pass against Codex CLI 0.164.0, using `codex exec` and a simulated model. Codex's exec-server protocol is experimental, so other versions may need checking.

## Pi
To run Pi against a sandbox, load the external-sandbox-pi extension with Pi's built-in tools disabled. Run this from the project directory the server was started in. The example uses Quicksand on port 8688:

```bash
pi -e npm:external-sandbox-pi --no-builtin-tools \
  --sandbox-url http://127.0.0.1:8688
```

**What this does:**
- `-e npm:external-sandbox-pi` downloads the extension for this run only.
- `--no-builtin-tools` means nothing runs on your computer if the extension fails to load.
- `--sandbox-url` points Pi at the Quicksand server on port 8688.

The extension needs Pi 1.5 or later. Pi's status bar shows `Sandbox: ...` once it has connected.

## Controlling network access
By default, none of the three implementations has external network access. This protects against bugs or prompt injection attacks that could leak private data to an external source. The article refers to the lethal trifecta, the combination of risks described in Simon Willison's earlier post on the subject.

🔎 Added context: The lethal trifecta, as Simon Willison has described it, is an agent that can access private data, is exposed to untrusted content, and can communicate externally. The sandbox removes the last of these three by default.

Each sandbox can be configured to allow HTTP and HTTPS access through a proxy. The proxy controls which hosts can be reached and which HTTP verbs can be used with them. This allows read-only or full access to specific hosts. When the proxy is enabled, the `HTTP_PROXY` and `HTTPS_PROXY` variables are set inside the sandbox, so clients such as wget know how to reach the network through it. The sandbox also makes the proxy's CA certificate available, pointing variables such as `SSL_CERT_FILE`, `REQUESTS_CA_BUNDLE` and `NODE_EXTRA_CA_CERTS` at it. Clients can then reach secure sites while the proxy inspects and filters their traffic. Code that ignores these settings and tries to connect directly is still blocked.

The `--proxy-config` option points at a file that lists the hosts and methods enabled for the session. Here is an example configuration file:

```json
{
  "allow": [
    {
      "origin": "https://pypi.org",
      "methods": ["GET", "HEAD"]
    },
    {
      "origin": "https://files.pythonhosted.org",
      "methods": ["GET"]
    },
    {
      "origin": "https://api.github.com",
      "methods": ["GET"],
      "credential": {
        "header": "authorization",
        "placeholder": "Bearer placeholder-github-token",
        "secretEnv": "GITHUB_TOKEN",
        "prefix": "Bearer "
      }
    }
  ],
  "guestEnv": {
    "GITHUB_TOKEN": "placeholder-github-token"
  }
}
```

**What this does:**
- Each `allow` entry names an `origin` and the HTTP `methods` permitted for it. PyPI and the Python file host allow only `GET` (and `HEAD` for PyPI), which is read-only access.
- The `api.github.com` entry is the only one with a `credential` block, which enables credential replacement for that host.
- `secretEnv` names the variable on the host that holds the real secret, here `GITHUB_TOKEN`. `prefix` and `placeholder` define the header value the sandbox sends.
- The `guestEnv` block gives code inside the sandbox a `GITHUB_TOKEN` variable that contains only the placeholder.

## Credential replacement for HTTP APIs
Credential replacement lets code in the sandbox make authenticated calls to web APIs without the key being exposed to the agent or made available inside the sandbox.

The example above configures this for the GitHub API. Requests go through the proxy to `https://api.github.com/` with an authorization header of `Bearer placeholder-github-token`. The proxy replaces that header with the value of the `GITHUB_TOKEN` environment variable, which exists in the environment of the sandbox server on the host. The `guestEnv` block gives code inside the sandbox a `GITHUB_TOKEN` variable that holds only the placeholder. Agents can therefore call authenticated APIs without the API key being visible to the agent.

The article adds a warning. An allowed host could still echo a credential back in its response. So use credential rules only with services you trust, and keep their methods and paths narrow.

To run the sandbox with the GitHub token available to the proxy:

```bash
GITHUB_TOKEN="$(gh auth token)" npx --yes external-sandbox-v86 \
  --proxy-config proxy.json \
  --preserve-paths --mount-rw "project=$PWD"
```

**What this does:**
- `GITHUB_TOKEN="$(gh auth token)"` sets the real token on the host only, taken from the GitHub CLI's login.
- `--proxy-config proxy.json` loads the allow-list and credential rules shown above.
- The token never enters the sandbox. Only the placeholder does.

## Monitoring the network and editing the policy
The article shows a session in the sandbox web UI where different requests are blocked or allowed according to the example network configuration. The `fetch.py` script in that session makes one request and prints the result.

The `/proxy` page of the web UI lets you edit the network policy while the session is running. It also shows a log of recent requests that the proxy allowed or denied. The `/proxy` page exists only if the server was started with either `--proxy` or an initial configuration given with `--proxy-config proxy.json`.

The full policy format is shared by all three sandboxes and is described in the proxy section of the protocol specification. The v86 proxy guide has more examples. The article also includes screenshots of the web UI, which are not in the text extraction.

## How each agent connects to a sandbox
Different coding agents offer different ways to make every executed command run in a custom environment. The article covers three mechanisms.

### CLAUDE_CODE_SHELL_PREFIX
Claude Code's mechanism is an environment variable. When `CLAUDE_CODE_SHELL_PREFIX` is set, Claude's Bash tool runs every command with that value as a prefix. The effect looks like this:

```bash
claude-shell-prefix.sh 'uname -m; pwd; printf "hello from Linux\n"'
```

**What this does:**
- The prefix script receives the full command as a single shell string, here `uname -m; pwd; printf "hello from Linux\n"`.
- Quoting the whole command as one argument keeps semicolons and environment variables intact for the sandbox to run.
- The script can then send that string to the sandbox API instead of running it on the host.
- The article notes that this works provided the execution environment can handle full shell strings, including environment variables and semicolons.

### Codex exec-server
Codex has an `exec-server` mechanism, a small JSON-RPC server for spawning and controlling subprocesses. OpenAI provides its own implementation as part of Codex. Because Codex is open source, the details can be extracted and a separate implementation built. The team did this, so Codex can be pointed at a server that implements the JSON-RPC spec and control another environment. The `environments.toml` file shown in the Codex section does the pointing, and its `url` is the sandbox's `/codex` WebSocket endpoint.

### Pi extensions
The Pi coding agent lets extensions replace its default tools with custom versions. external-sandbox-pi replaces the bash, read, write, edit, grep, find and ls tools with versions that act against the configured sandbox. It also points Pi's `!command` and `!!command` shortcuts at the sandbox. `!command` runs a command and adds its output to the conversation, while `!!command` runs it without sending the output to the model.

Running Pi with `-e npm:external-sandbox-pi --no-builtin-tools` ensures that Pi has no tools that run on your own computer, even if the extension fails to load. If the sandbox cannot be reached, the tools return an error rather than falling back to the defaults.

To confirm that commands go to the sandbox, run this inside Pi:

```text
!uname -a
```

**What this does:**
- `!uname -a` runs through the sandbox because the extension rewires the `!` shortcut.
- You should see `Linux` rather than `Darwin`. The rest of the output depends on which sandbox you run.

To make Pi always use the sandbox, install the extension and set an environment variable:

```bash
pi install npm:external-sandbox-pi
export EXTERNAL_SANDBOX_API_URL=http://127.0.0.1:8686
```

**What this does:**
- `pi install` adds the extension permanently, unlike the per-run `-e` form.
- `EXTERNAL_SANDBOX_API_URL` tells the extension which sandbox to use. Put the export line in your shell profile to keep it across terminal sessions.

To remove the extension:

```bash
pi remove npm:external-sandbox-pi
```

**What this does:**
- `pi remove` uninstalls the extension that `pi install` added.

## Let us know what you think
The article says these sandboxes are still experimental prototypes. They demonstrate the external sandbox pattern effectively, but they have not been audited to the level needed before trusting them with sensitive workloads.

Two limitations are called out:

> ⚠️ **Read-write mounts can still cause damage.** A read-write mount lets the agent damage anything in that folder. Keep the folder under version control, and use read-only mounts where you can.

> ⚠️ **Platform-specific dependencies.** The sandboxes run Linux, so dependencies built for macOS, such as a `node_modules` or `.venv` folder in your project, may need reinstalling inside the sandbox.

The author's closing point is that three sandbox implementations working with three client agents show the protocol can mix and match models, agents and sandboxing platforms. To build a sandbox on another technology, start with the specification and the conformance tool. Feedback and reports of design flaws go to issues on the external-sandbox repository.

## 🧠 Mental models
### External sandbox pattern
**The model:** The agent runs on the host and makes its decisions there, while every command it executes runs in a separate sandbox that the agent cannot modify. The boundary sits between the agent and the commands, not around the whole computer.
**How to apply it:** When you give an agent new capabilities, ask which side of the boundary the new capability lives on, and whether the agent could change the sandbox from inside.

### A boundary that does not depend on judgment
**The model:** Safety from a model-based check, such as an auto mode or an AI review, depends on the model judging each action correctly. A sandbox limits what can happen whatever the model decides.
**How to apply it:** Use model-based checks as a convenience layer, and rely on enforced limits for anything that must not happen, especially when running weaker or local models.

### Least privilege by mount and host
**The model:** Give the sandbox only the folders it needs, read-only where possible, and only the hosts and HTTP methods it needs. Credentials stay on the host and are inserted by the proxy.
**How to apply it:** Start with no network and a read-only mount, then add one host or folder at a time when a task proves it needs it.

## 🔑 Key takeaways
- A sandbox protects regardless of the model's judgment, which the article contrasts with the AI checks in Claude Code's auto mode and Codex's auto-review.
- Keeping the agent outside the sandbox avoids the agent modifying itself or its own environment, and keeps the boundary easier to audit.
- By default the sandboxes have no external network access, and a proxy opens only the hosts and HTTP methods you configure.
- Credential replacement lets an agent make authenticated API calls while the real key stays on the host, invisible to the agent.
- Claude Code's Read and Edit tools run on the host, so only its Bash commands are sandboxed. The article calls this out as a limit.
- The same localhost protocol works across three sandboxes (v86 on port 8686, Apple Containers on 8687, Quicksand on 8688) and three agents.

## 🏛️ For Kenji
This is a practical way to run agents against local or cheaper models without giving them your full machine. It applies directly to how you would govern agent tooling in a team: decide which files, hosts and credentials each agent may use, and enforce that at a boundary rather than through per-action approvals. The question it raises for your architecture work is which of your own agent workflows still rely on a model's judgment for safety, and which could move that responsibility into an enforced boundary. (This is your interpretation, not the author's.)

## 📚 Reading plan
### From the article
- [external-sandbox-v86](https://github.com/prime-radiant-inc/external-sandbox-v86) — the Node.js and WebAssembly sandbox, the simplest one to start with.
- [external-sandbox-container](https://github.com/prime-radiant-inc/external-sandbox-container) — the Apple Containers variant, for Apple silicon Macs.
- [external-sandbox-quicksand](https://github.com/prime-radiant-inc/external-sandbox-quicksand) — the QEMU-based variant that ships as a Python package.
- [specification](https://github.com/prime-radiant-inc/external-sandbox/blob/main/PROTOCOL.md?ref=blog.primeradiant.com) — the protocol all three sandboxes implement, including the proxy policy format.
- [conformance tool](https://github.com/prime-radiant-inc/external-sandbox?ref=blog.primeradiant.com) — the testing tool to check a new sandbox server against the protocol.
- [v86 proxy guide](https://github.com/prime-radiant-inc/external-sandbox-v86/blob/main/docs/proxy.md?ref=blog.primeradiant.com) — more examples of proxy configuration.
- [the lethal trifecta](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/?ref=blog.primeradiant.com) — the risk framing behind the default of no network access.
- [auto mode](https://code.claude.com/docs/en/auto-mode-config?ref=blog.primeradiant.com) — Claude Code's AI-check approach, which the article contrasts with sandboxing.
- [auto-review](https://alignment.openai.com/auto-review/?ref=blog.primeradiant.com) — Codex's equivalent AI-check approach.
### To go further
- Fly's Sprites — a hosted approach that runs the agent inside the sandbox, the alternative the team considered and rejected.
- The external sandbox protocol specification's proxy section — read it alongside the v86 proxy guide if you plan to write a policy for a real project.
