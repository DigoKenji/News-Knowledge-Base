---
type: Article Summary
title: Running coding agents without handing them my credentials
description: Keep credentials and the commands that use them on your laptop, and let remote agents request policy-approved actions locally.
resource: https://blog.antonio.cx/running-agents-with-lend
tags:
- security
- appsec
- model-context-protocol
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://blog.antonio.cx/running-agents-with-lend
  title: Running coding agents without handing them my credentials
  author: Antonio Mika
status: stable
source_url: https://blog.antonio.cx/running-agents-with-lend
source: Hacker News (100+ pts)
source_type: blog
published: '2026-11-13'
domain: security
subdomain: appsec
concepts:
- model-context-protocol
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81c8ba91c6ed87dc70f6
---

# Running coding agents without handing them my credentials

**Source:** https://blog.antonio.cx/running-agents-with-lend
**Author:** Antonio Mika
**Date:** 2026-11-13
**Concepts:** [Model Context Protocol (MCP)](/concepts/model-context-protocol.md)

---

## 🎯 Core thesis
Keep your credentials, and the commands that use them, on your laptop, and let remote agents ask for specific actions that you approve locally instead of giving them the secrets.

## 🧭 Why the author wrote it
The author runs coding agents on disposable remote VMs, but those agents often need to act as him: reading CI through `gh`, calling APIs with 1Password tokens, and SSHing into other hosts. The two usual fixes, copying tokens onto the VM or forwarding the SSH agent with `ssh -A`, both leak more authority than the task needs. If the thesis holds, agents get working access without ever possessing the secret, and each sensitive use is approved and recorded on the machine the secret lives on.

## Background
The author uses agents heavily as a toolchain: Claude Code in one repository, Codex reviewing a branch in another, and a long-running job in the background, often reviewing each other's work. None of them run on the laptop. They run on VMs that can be discarded, while the laptop holds the things that matter: SSH keys, a 1Password vault, and the `gh` and cloud CLIs logged in as him.

### Remote VMs on picd
The agents run on picd, the hosting service at pico.sh. It provides a personal VM with a Docker engine and a shell over SSH. The author runs herdr and the agents there, and the home directory persists across restarts, so dotfiles and tools stay in place.

The setup works well until an agent needs something that exists only on the laptop. The rest of the article explains how the author runs agents in parallel with herdr and Ovis, and how the lend tool handles credentials.

## herdr
herdr is a terminal multiplexer built for coding agents. It offers the usual workspaces, tabs and panes, and it also knows which agent runs in each pane and what that agent is doing. The sidebar shows whether each agent is working, waiting for the user, or finished, which makes it easy to see which agent needs attention.

### Agent control through the herdr CLI
The CLI talks to the running session, so agents can drive other agents. An agent can start Codex in a second pane, send it a prompt, wait until it finishes, and then read what it said. The three commands below show that flow.

```bash
herdr agent start reviewer --kind codex --pane "$PANE"
herdr agent prompt reviewer "Review the diff on am/fix-retries" --wait --until done
herdr agent read reviewer --source recent
```

**What this does:**
- `agent start` creates a named agent of a given kind (here `codex`) in the pane identified by `$PANE`.
- `agent prompt ... --wait --until done` sends a task and blocks until the reviewer reports it is done, so the caller does not poll.
- `agent read ... --source recent` pulls back the reviewer's recent output so the calling agent can act on the review.
- The author uses this for agent-on-agent review, which is why the reviewer is a separate pane rather than a subprocess.

### Remote machines
Remote hosts work the same way. `herdr machine add agent-vm` sets up herdr on the VM and adds that VM's sessions to the local client. Those sessions keep running after the local client disconnects.

## Ovis
Ovis is a browser and phone client for herdr. It shows each agent's transcript as a chat, with the live terminal one tap away. When an agent asks for approval or has a question, Ovis turns that into a card the user can answer, and it sends a push notification. It reads the machines already saved in herdr, so the VMs appear without extra setup. The author reaches it over Tailscale.

> 🔎 Added context: Tailscale is a private network service that connects a user's devices to each other, so the phone can reach the VM without a public address.

## The credential problem
Agents on a VM still need to act as the user. There are two usual ways to arrange that, and the author dislikes both.

### Copying credentials onto the VM
The first way is to copy the credentials onto the VM. A token in `~/.config/gh/hosts.yml` can be read by every process on that machine, including the agent and everything it runs, for as long as the token stays valid.

> ❌ **Anti-pattern — copying credentials to the remote host:** Placing a long-lived token file on the VM so tools can use it directly. **Why people do it:** It is the simplest way to make `gh`, cloud CLIs and API scripts work on a remote machine. **Consequence:** Every process on the VM, including any agent and anything it runs, can read the token, and the token keeps working until it is revoked or expires. **Fix:** Keep the token on the machine where it was issued and route the call there, as lend does.

### Forwarding the SSH agent with ssh -A
The second way is to forward the SSH agent with `ssh -A`. The private keys stay on the laptop, but the VM can use every key in the agent without asking and without any record of what it signed. This also covers only SSH keys, so it does not help with `gh` or API tokens.

> ❌ **Anti-pattern — blanket agent forwarding:** Enabling `ssh -A` so the remote host can use every loaded key. **Why people do it:** It is the standard way to let a jump host or VM use SSH keys without copying them. **Consequence:** The remote side can sign anything at any time, and nothing records which signatures were made. **Fix:** Expose only the keys you need, require approval per request, and log what was signed.

The goal is for credentials to stay on the laptop and for the commands that need them to run there too. The agent asks to run something, the author approves it with Touch ID, and the agent receives the output.

## lend
lend is the tool the author wrote for this. It is a proxy that sits in front of the existing SSH agent, and the author forwards its socket in place of the real one. SSH and Git keep working normally, and lend adds a way to run commands through an SSH agent extension.

### lend as a proxy in front of the SSH agent
When an agent on the VM asks to run `gh`, the request crosses the forwarded socket and `gh` runs on the laptop under the author's login. The token never leaves the laptop, so the agent never has it. Every request, whether it is a signature or a command, is checked against a policy on the laptop and needs the author's approval before anything happens.

```text
Remote: lend exec -- command args
                  |
          forwarded SSH agent socket
                  |
Laptop: lend agent -> policy + local approval -> local command
                                           -> original SSH agent (signing)
```

**What this does:**
- The first line shows the remote entry point: `lend exec` takes a command and its arguments.
- On the laptop, `lend agent` applies the policy and local approval first, then runs the approved command locally.
- The signing path goes to the original SSH agent, so normal SSH and Git signing still work through the same proxy.

### Policy: default deny on the laptop
The policy lives on the laptop and lists what the VM may request. Everything else is denied. The author's policy is short:

```json
{
  "default": "deny",
  "commands": [
    {"executable": "/opt/homebrew/bin/gh", "approval": "biometric:inline"}
  ],
  "ssh": {
    "default": "deny",
    "keys": [
      {"fingerprint": "SHA256:...", "approval": "biometric:inline"}
    ]
  }
}
```

**What this does:**
- `"default": "deny"` at the top means any command not listed is refused without a prompt.
- The `commands` entry allows exactly one executable, the Homebrew path to `gh`, and attaches an approval mode to it.
- `"approval": "biometric:inline"` places Touch ID in the request dialog itself, so the author reads the request and touches once.
- The `ssh` block is also default deny, and only the listed fingerprint is offered to the VM.

A rule can also take an `args_prefix` such as `["pr", "view"]`, which restricts the rule to some subcommands. Plain `biometric` is the other approval mode. It asks for Touch ID in a second prompt after the author clicks Allow, while `biometric:inline` combines the two into one step.

### Connecting with lend ssh
To connect, the author runs `lend ssh` from the laptop. It starts a proxy for that one connection and stops it when SSH exits, so the proxy's lifetime matches the session.

```bash
lend ssh --config ./policy.json -- agent-vm
```

**What this does:**
- `lend ssh` wraps the SSH connection, so the forwarded socket exists only while the session is open.
- `--config ./policy.json` names the laptop policy that governs every request during this session.
- `-- agent-vm` is the host to connect to, passed after the separator so it is not read as a lend flag.

### Running commands through MCP
The author runs herdr on the VM, and Claude Code talks to lend through `lend mcp`. That is an MCP server with one tool, `run_local_command`. The tool takes the command to run and a reason, and the reason is required.

```json
{
  "mcpServers": {
    "laptop": {"command": "lend", "args": ["mcp"]}
  }
}
```

**What this does:**
- The `laptop` key is only a server name; the agent sees the tool as coming from that server.
- `"command": "lend"` with `"args": ["mcp"]` starts the lend MCP server locally on the VM.
- The required reason is shown to the approver on the laptop, which makes each request explain itself.

In one example, the author asked Claude Code on the VM for the lend repository's description. The agent called the tool with `gh repo view`, the Mac showed the request, and one touch approved it. The dialog shows the full path to the executable, every argument, the directory the command will run in on the laptop, the reason, and what the remote process said about itself. After approval, stdout, stderr and the exit code stream back to the agent. The reason and caller details come from the VM, so lend marks them as unverified. They help the approver decide but cannot change what the policy allows.

### Using lend exec without MCP
The same request works as a plain CLI call from the VM, without MCP:

```bash
lend exec --reason "Read the repo description" -- gh repo view antoniomika/lend
```

**What this does:**
- `lend exec` sends the command to the laptop the same way the MCP tool does.
- `--reason` is required, so the approver always sees why the command is being run.
- Everything after `--` is the command and its arguments, which the policy checks before anything runs.
- The command here is `gh repo view antoniomika/lend`, a read-only call that fits the author's `args_prefix` style of narrow rules.

### Keeping the socket stable across reconnects
herdr panes survive an SSH disconnect, but the forwarded socket belongs to one SSH connection. After a reconnect, SSH gives a new socket path, and agents still running in the panes keep the old `SSH_AUTH_SOCK`. The author fixes this with a symlink at a fixed path that gets repointed on each connection.

#### The ~/.ssh/rc hook
The author updates the symlink from `~/.ssh/rc` on the VM. SSH runs that file before it starts the shell, so the script sees the socket forwarded by the new connection.

```sh
#!/bin/sh

if [ -n "${SSH_AUTH_SOCK:-}" ] &&
    [ "$SSH_AUTH_SOCK" != "$HOME/.ssh/lend/agent.sock" ] &&
    "$HOME/.local/bin/lend" doctor --remote --socket "$SSH_AUTH_SOCK" --timeout 1s >/dev/null 2>&1; then
  install -d -m 700 "$HOME/.ssh/lend" &&
    ln -sfn -- "$SSH_AUTH_SOCK" "$HOME/.ssh/lend/agent.sock"
fi

if [ -n "${DISPLAY:-}" ] && read -r _ssh_proto _ssh_cookie; then
  case $DISPLAY in
    localhost:*) _ssh_display="unix:${DISPLAY#localhost:}" ;;
    *) _ssh_display=$DISPLAY ;;
  esac
  printf 'add %s %s %s\n' "$_ssh_display" "$_ssh_proto" "$_ssh_cookie" | /usr/bin/xauth -q -
fi
```

**What this does:**
- The first condition checks that a socket is present, that it is not already the stable path, and that `lend doctor --remote` confirms it speaks lend within one second.
- Only after that check does `install -d -m 700` create the private directory and `ln -sfn` repoint the stable link.
- The script calls lend by its absolute path because it runs before the shell adds `~/.local/bin` to `PATH`.
- The second block handles X11: when `DISPLAY` is set, it passes the cookie to `xauth`, since SSH delegates that setup to this file when it exists.

#### The ~/.bashrc check
Shells in herdr panes then read the stable path, but only when it works.

```bash
if [ -S "$HOME/.ssh/lend/agent.sock" ] &&
    "$HOME/.local/bin/lend" doctor --remote --socket "$HOME/.ssh/lend/agent.sock" --timeout 1s >/dev/null 2>&1; then
  export SSH_AUTH_SOCK="$HOME/.ssh/lend/agent.sock"
fi
```

**What this does:**
- The `-S` test confirms the stable path is a socket before the shell uses it.
- The same `lend doctor --remote` check runs again with a one-second timeout, so a dead or non-lend socket is never exported.
- `export SSH_AUTH_SOCK` changes the environment only when both checks pass.
- If either check fails, the variable is left alone, which avoids pointing agents at a dead socket.

New Bash shells in herdr panes pick up the stable path when it works. MCP, approval hooks and Git inherit the path when they start. After a reconnect, the SSH hook updates the symlink, and those processes use the new socket for later requests. Agents already running with an old path need one restart with the stable path set.

The SSH connection from the laptop has to stay open. Requests fail while it is disconnected, and a reconnect starts a new lend session with fresh approvals and leases. The link points at one connection at a time, so the most recent update wins.

### Secrets the agent never sees
`gh` works because it is already logged in on the laptop. Some tools need a token passed in, and for those a command rule can read the secret with `op read` and pass it to the approved command as environment variables.

```json
{
  "secret_reader": "/opt/homebrew/bin/op",
  "secret_reader_args": ["read", "--no-newline"],
  "commands": [
    {
      "executable": "/opt/homebrew/bin/gh",
      "args_prefix": ["pr", "view"],
      "approval": "biometric",
      "secrets": {"GH_TOKEN": "op://Development/github/token"}
    }
  ]
}
```

**What this does:**
- `secret_reader` and `secret_reader_args` tell lend to fetch secrets with `op read --no-newline` on the laptop.
- `args_prefix` limits this rule to `gh pr view`, so the injected token is used only for that subcommand.
- `"approval": "biometric"` asks for Touch ID in a second prompt after Allow, as opposed to the inline mode used earlier.
- `"secrets": {"GH_TOKEN": "op://Development/github/token"}` maps the environment variable name to a 1Password reference, so the VM supplies only the reference and never the value.

When the agent asks to run `gh pr view` and the author approves, lend reads the token from 1Password, runs `gh` with `GH_TOKEN` set, and returns the output. The token stays on the laptop, the VM cannot choose which variables are set or which references are read, and the prompt shows only variable names and references, never the values. The author also ran `gh api user` on the VM with this rule, and 1Password asked for its own Touch ID before `op read` returned the token. The VM got the login back, and its own environment had no `GH_TOKEN`.

There is one catch. This only holds if the approved command does not print its environment. Approving `env` or `sh` hands the values back to the agent.

> ❌ **Anti-pattern — approving env or sh:** Allowing a rule that runs `env`, `printenv` or a shell that can echo variables while a secret is injected. **Why people do it:** A shell or `env` seems like a convenient general-purpose command to approve. **Consequence:** Injected secrets appear in the command's output, which streams straight back to the agent. **Fix:** Approve only specific tools that do not print their environment, and never a general shell for a rule that injects secrets.

> ❌ **Anti-pattern — allowing op as a command:** Putting the 1Password CLI itself on the allow list. **Why people do it:** It seems natural to allow the tool that holds the secrets. **Consequence:** `op read` prints the secret, and that output goes straight to the agent. **Fix:** Keep `op` as the `secret_reader` only, and never as an approved command.

### SSH keys
Keys have their own policy. Keys that are not listed are hidden from the VM. A listed key can be limited by what it may sign, and `max_signatures` caps how many signatures the key can make in one session. The article does not give a value for that cap.

### Leases
A rule can offer a lease when the author approves, such as five minutes or three uses. A lease exists only inside a `lend ssh` session and matches only the exact command and arguments that were approved. It ends when the session ends or the screen locks, so a lease cannot outlive the connection that created it.

### Seeing what happened
`lend status --all --watch` shows what is connected, what is waiting, what is running, and any active leases. `lend pause --all` cancels everything and refuses new requests until the author resumes. `lend history` keeps a record of what ran and how it was approved. It leaves out arguments, output and secrets, and the author can opt in to logging arguments.

### Answering agent prompts from the laptop
Agents also ask permission for things unrelated to credentials, such as running a shell command on the VM, editing a file or fetching a URL. `lend approve` sends those questions to the same dialog on the Mac. It runs nothing on the laptop. It asks the author, and the agent decides what to do with the answer.

#### The PermissionRequest hook
For Claude Code, the hook is a `PermissionRequest` hook on the VM:

```json
{
  "hooks": {
    "PermissionRequest": [
      {"matcher": "*", "hooks": [{"type": "command", "command": "lend approve --hook claude-code"}]}
    ]
  }
}
```

**What this does:**
- `"PermissionRequest"` is the event Claude Code raises when it would otherwise stop and ask in the terminal.
- `"matcher": "*"` sends every permission request through the hook.
- The hook runs `lend approve --hook claude-code`, which forwards the request to the laptop dialog instead of the terminal.
- The approval itself happens on the laptop, so the VM's terminal is no longer the place the decision is made.

#### Approval topics on the laptop
The laptop policy lists which topics may ask, and each topic has its own approval mode.

```json
"approvals": {
  "topics": [{"name": "claude-code", "approval": "biometric:inline"}]
}
```

**What this does:**
- The `topics` array names the `claude-code` topic, which the hook above uses.
- `"approval": "biometric:inline"` applies the inline Touch ID dialog to every Claude Code permission prompt.

There are adapters for Codex, Antigravity and pi, plus a plain JSON format for other tools. The same mechanism works in scripts. For example, `lend approve --topic deploy --title "terraform apply" && terraform apply` runs the apply only after the author approves on the laptop. If the laptop cannot be reached, the hook steps aside and the agent asks in its own UI, as it normally would.

## What lend doesn't protect against
An approved command runs as the author's user and can touch his files. Approval decides what runs, but lend does not sandbox the command. If the author approves a shell, an interpreter, or anything that reads instructions from stdin, he has effectively approved running anything on the laptop, because stdin streams after approval and is not shown in the prompt.

> ❌ **Anti-pattern — approving a shell, interpreter or stdin reader:** Allowing `sh`, `python`, or any command that reads its program from standard input. **Why people do it:** A general-purpose tool saves writing narrow rules for each task. **Consequence:** After one approval, the input stream can carry arbitrary code that the prompt never displayed. **Fix:** Allow narrow subcommands, such as `gh pr view` with `args_prefix`, and never a general interpreter.

The caller details in the prompt come from the VM and can be faked. Anyone who can reach the forwarded socket on the VM, root included, can make requests while the session is open. The policy still limits what they can ask for, and every request still needs approval.

> ❌ **Anti-pattern — trusting caller-supplied context:** Reading the reason or caller details in the dialog as verified facts. **Why people do it:** The dialog looks authoritative, and the reason is written in plain language. **Consequence:** A compromised or confused agent can describe a harmless command while asking for a harmful one. **Fix:** Judge the actual executable, the full arguments and the working directory, which lend shows, and treat the reason as unverified.

The article also states its limits plainly. lend is early and runs on macOS and Linux. It does not allocate a PTY, so interactive programs do not work through it. On Linux, approval is a Zenity dialog or a terminal prompt, and biometric approval is macOS only.

> 🔎 Added context: A PTY (pseudo-terminal) is what lets a program behave as if it has an interactive terminal, so without one, programs that expect keyboard input or full-screen terminal control will not run correctly.

## Prior art
[ssh-rev-exec](https://github.com/KOBA789/ssh-rev-exec) sends local execution requests through an SSH agent proxy. [op-forward](https://github.com/ekovshilovsky/op-forward) forwards 1Password CLI commands to a machine that has desktop authentication. lend is a separate implementation of the same agent-extension technique, and it adds per-request policy, local approval and key filtering.

## Trying it
Install lend on both the laptop and the remote host. The article gives Homebrew and a release-script option.

```bash
brew install antoniomika/tap/lend
# or
curl -fsSL https://github.com/antoniomika/lend/releases/latest/download/install.sh | sh
```

**What this does:**
- `brew install antoniomika/tap/lend` installs from the author's Homebrew tap.
- The `curl ... | sh` line runs the release installer from the latest GitHub release, an option that does not need Homebrew.
- The article says to install lend on both the laptop and the remote host, so both sides have the same tool available.
- Installing comes first, before the policy, the doctor checks or a session.

Run `lend doctor` on the laptop to check the policy, the executables, the upstream agent and the approval setup. Run `lend doctor --remote` on the VM to confirm the forwarded socket is actually lend. The [getting started guide](https://lend.antonio.cx/getting-started.html) walks through a first session, and the [docs](https://lend.antonio.cx) cover policies, approval and MCP. The author asks for feedback on [GitHub](https://github.com/antoniomika/lend).

## 🧠 Mental models
### The laptop is the trust boundary
**The model:** The VM can ask for things, but only the laptop decides what happens and holds the secrets. Remote agents are clients of a local policy.
**How to apply it:** When designing agent access, ask where the secret physically lives and whether the remote side only ever requests named actions.

### Approval decides what runs, not what it does
**The model:** Approving a command is a decision about which program starts, not a guarantee about its behavior. A narrow program with narrow arguments keeps that decision meaningful.
**How to apply it:** Approve specific subcommands with `args_prefix`, never a shell, interpreter or stdin reader, and check whether the program prints its environment before injecting secrets.

### Stable names over changing handles
**The model:** Connection-scoped resources, such as an SSH socket path, change on every reconnect. A fixed symlink lets long-running processes keep one name while the target changes underneath.
**How to apply it:** Point long-lived processes at a stable path, repoint it on each connection, and verify the target before updating the link.

## 🔑 Key takeaways
- Copying a token to a VM exposes it to every process there for as long as it is valid, so the article moves the credential to the laptop instead.
- `ssh -A` lets the remote side use every loaded key without asking or recording, so lend exposes only the keys listed in the policy.
- A policy that is default deny, with narrow executables and `args_prefix` rules, lets only the commands the author listed run at all.
- Secrets can be injected into one approved command as environment variables from a `secret_reader`, and the VM never chooses the values or the references.
- Approval covers the exact executable, arguments and working directory shown in the dialog, while the reason and caller details remain unverified.
- Approving a shell, an interpreter, `env`, or the 1Password CLI itself defeats the design, because each one can return secrets or run arbitrary code.

## 🏛️ For Kenji
This pattern fits any architecture where agents run in less trusted environments but need access to credentials or privileged actions, such as CI monitoring, cloud CLIs and internal APIs. The design question it raises is which secret-bearing commands should be reachable through a narrow, approved interface instead of being given to the agent. *This is your interpretation, not the author's.*

## 📚 Reading plan
### From the article
- [lend on GitHub](https://github.com/antoniomika/lend) — the source, releases and issue tracker for the tool the article describes.
- [lend getting started guide](https://lend.antonio.cx/getting-started.html) — a first session with lend, step by step.
- [lend docs](https://lend.antonio.cx) — policies, approval modes and MCP, which the article refers to without detail.
- [herdr on GitHub](https://github.com/herdrdev/herdr) — the terminal multiplexer for coding agents that the author runs on the VM.
- [Ovis](https://ovis.sh) — the browser and phone client for herdr, useful for seeing how approval cards reach a phone.
- [picd](https://cd.pico.sh) — the VM hosting service the author uses for remote agents.
- [sshd man page, SSHRC section](https://man.openbsd.org/sshd.8#SSHRC) — how `~/.ssh/rc` runs before the shell, which the stable-socket fix depends on.
- [ssh-rev-exec](https://github.com/KOBA789/ssh-rev-exec) — a prior implementation of remote execution through an SSH agent proxy.
- [op-forward](https://github.com/ekovshilovsky/op-forward) — forwarding 1Password CLI commands to a machine with desktop authentication.
