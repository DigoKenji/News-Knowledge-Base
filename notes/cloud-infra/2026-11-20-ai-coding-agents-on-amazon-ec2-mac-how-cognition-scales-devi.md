---
type: Article Summary
title: 'AI coding agents on Amazon EC2 Mac: How Cognition scales Devin for macOS'
description: Cognition runs a fresh macOS VM per Devin session on EC2 Mac bare-metal hosts, letting AI agents build and test
  Apple apps.
resource: https://aws.amazon.com/blogs/architecture/ai-coding-agents-on-amazon-ec2-mac-how-cognition-scales-devin-for-macos/
tags:
- cloud-infra
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/ai-coding-agents-on-amazon-ec2-mac-how-cognition-scales-devin-for-macos/
  title: 'AI coding agents on Amazon EC2 Mac: How Cognition scales Devin for macOS'
  author: Adam Hesch et al. (AWS Architecture Blog)
status: stable
source_url: https://aws.amazon.com/blogs/architecture/ai-coding-agents-on-amazon-ec2-mac-how-cognition-scales-devin-for-macos/
source: AWS Architecture
source_type: blog
published: '2026-11-19'
domain: cloud-infra
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81c2aff8d9e451289421
---

## 🎯 Core thesis
An AI coding agent can only build, run and verify Apple apps if it gets a real macOS environment, and Cognition shows one way to provide that at scale: a fresh macOS virtual machine per session, running on Amazon EC2 Mac bare-metal hosts, with the same disk, network, setup and live-view properties that its Linux and Windows workspaces already had.

## 🧭 Why the author wrote it
Before this launch, Devin ran only on Linux and Windows VMs. It could write Swift, but without macOS and Xcode it could not build or test the app, so customers had to rely on their own CI or Mac fleets. The article explains how Cognition built Devin's macOS workspaces on EC2 Mac and offers guidance for planning a macOS fleet for agents or automated testing. If the thesis holds, an agent platform needs macOS as a first-class cloud workspace, not as an add-on to someone else's CI.

## Why an AI coding agent needs its own Mac
Cognition's goal was to let Devin build Apple apps. Building, testing and signing apps for iPhone, iPad and Mac requires Apple hardware and macOS, whoever or whatever writes the code. Cognition now runs macOS environments on Amazon EC2 Mac instances, and Devin can build an iOS app, run it in the iOS Simulator and check its behavior before handing back a merge-ready change.

### Build success is not behavioral proof
A successful build shows only that the code compiles. The article's example is an iPhone game designed to resume where the player paused it, even after the app closes. To verify that, an agent has to play the game, pause it, quit, relaunch and confirm the saved state. Only a running app on a running Mac can show that.

### Workspace properties Cognition required
Cognition needed each macOS workspace to match its other cloud workspaces:
- A disk that keeps the session's work, so an idle session does not keep a VM running.
- Network access that follows the session's access rules.
- A development environment that is ready before Devin starts.
- A way for both the user and Devin to see and operate the running app.

🔎 Added context: A virtual machine (VM) is a software-emulated computer that runs its own operating system on shared physical hardware.

## Why Cognition chose AWS
Cognition needed macOS machines to scale Devin beyond Linux and Windows, but did not want to buy, rack and maintain physical Mac computers. The article lists the EC2 Mac properties that shaped the design.

### Apple silicon hardware in the cloud
Each EC2 Mac instance is a physical Mac mini or Mac Studio. Because it is attached to the AWS Nitro System, Cognition can run Apple's Virtualization framework on EC2 Mac the same way it would on an on-premises Mac.

### Capacity through an API
Cognition allocates Dedicated Hosts and launches Mac instances through the Amazon EC2 API, instead of ordering and shipping machines. Capacity becomes a provisioning call rather than a hardware purchase.

### Regions around the world
EC2 Mac is available in several AWS Regions. Cognition runs in six of them.

### Standard AWS networking and storage
Mac instances run in an Amazon Virtual Private Cloud (VPC) and boot from Amazon Elastic Block Store (EBS) volumes, so Cognition manages them like any other EC2 instance.

## Solution overview
Cognition builds Devin's macOS workspaces on EC2 Mac instances. These run as bare-metal instances on Amazon EC2 Dedicated Hosts, built on Apple Mac computers and the AWS Nitro System.

### Dedicated Hosts and the 24-hour minimum
Each EC2 Mac instance runs on its own Dedicated Host, and a host has a minimum allocation period of 24 hours. Cognition therefore separates the life of a Devin session from the life of a host. A session can come and go while the host remains allocated, and the host is the unit that costs money. Where Cognition has no preallocated capacity, it uses an Amazon EC2 Auto Scaling group to allocate and release hosts.

### The VM manager and Apple's Virtualization framework
On each host, a Cognition VM manager uses the Apple Virtualization framework to run macOS guest VMs. The guest images come from images Cognition prepares and manages.

🔎 Added context: Apple's Virtualization framework is the macOS API for creating and running guest operating systems on a Mac.

### Two workspaces per host, by license
Each host runs two isolated Devin workspaces at a time. The macOS Software License Agreement (SLA) allows up to two additional copies of macOS in virtual environments on each Mac, and macOS enforces that limit. Idle sessions release their VMs for other sessions, so one host can serve several Devin sessions a day.

### Figure 1: one host, many sessions
The host runs a bare-metal Mac instance in one of the six Regions, with one guest VM per active session. Each guest runs Xcode tools and the iOS Simulator with the app under test. It also runs a live view and an accessibility tree, provided by accessibility-cli. The user's browser exchanges video and input with the live view, and the Devin agent queries and acts on the accessibility tree.

On the host, the VM manager runs the guests. A snapshot process with a Network Block Device (NBD) frontend handles disk I/O for each guest and exchanges snapshots with snapshot storage. The Devin agent and snapshot storage run as Devin Cloud services in the same Region, with sessions on Amazon EC2 and snapshots in Amazon Simple Storage Service (Amazon S3). A user-space Ethernet gateway handles networking, described below.

> 📋 **Figure 1 reference**: the diagram is titled "Devin macOS sessions on an Amazon EC2 Mac Dedicated Host" in the source article.

### Keep session state with disk snapshots
Much of the work with Devin Cloud is asynchronous. An idle session does not need a running VM, so Cognition uses the same snapshot system for macOS that it uses for its Linux VMs. The snapshot system runs as a separate process on the host and handles the VM's disk reads and writes. The guest sees only an ordinary virtual disk, so it does not need to know about snapshots.

#### vhost-user and why it did not transfer
On Linux, Cognition connects VMs to the storage process with `vhost-user`. The Virtualization framework does not support `vhost-user`, but it does support NBD attachments. Cognition added an NBD frontend to its snapshot system and kept the same storage design behind it, so the storage layer did not have to change.

#### Shutdown persistence
When a guest shuts down, the storage process keeps running to persist pending writes and upload the updated snapshot. The VM manager waits for the upload to finish before cleanup completes, so stopping a session does not interrupt the work that saves it.

#### Idle VM release and resume
Cognition reports that about 90% of sessions give up their Mac VM when idle and resume from a snapshot in seconds. On average, a session frees and reclaims its VM several times, so Mac capacity goes to active work and not to idle sessions.

> ❌ **Anti-pattern — keeping the VM for an idle session:** leaving a running macOS VM attached to a session that is waiting on the user. **Why people do it:** it is the simplest model, since the VM never has to be rebuilt. **Consequence:** the scarce Mac capacity sits idle, and the cost of a host keeps running. **Fix:** persist disk state as snapshots and release the VM when the session goes idle, as Cognition does.

### Apply network access rules with a user-space Ethernet gateway
Devin Cloud applies each session's access rules to outbound traffic and blocks access to protected host infrastructure. The Virtualization framework offers three ways to connect a guest to the network: bridged networking, network address translation (NAT), and a file-handle attachment that exchanges raw Ethernet frames over a local Unix datagram socket.

#### NAT and the shared packet filter
NAT needs the least setup, but Apple's implementation also manages rules in `pf`, the macOS packet filter. Cognition also uses `pf` for its own network controls, so the two systems raced to configure the same filter.

> ❌ **Anti-pattern — two controllers for one packet filter:** letting the hypervisor and your own policy engine both write rules into the same `pf` configuration. **Why people do it:** the built-in NAT option is the quickest path to a working guest network. **Consequence:** the two systems overwrite or conflict with each other's rules, so policy enforcement becomes unpredictable. **Fix:** pick one owner of the filter. Cognition chose a different attachment type so that it could own the policy path.

#### The user-space Ethernet gateway
Cognition chose the file-handle attachment and built its own Ethernet gateway that runs in user space on the host. Each Mac VM gets a separate gateway and its own session policy enforcement. The gateway answers Address Resolution Protocol (ARP) requests from the guest and validates Ethernet addressing. It also checks that the source IP address of each packet matches the address assigned to the guest. Accepted IP packets go into a tunnel interface on the host, where `pf` rules and proxies route the traffic and apply the session's access policy.

🔎 Added context: ARP is the protocol that maps an IP address to a link-layer (Ethernet) address on a local network.

### Prepare guest images before the first boot
A Devin workspace needs more than a fresh macOS install. It needs a user account, a usable desktop, Xcode tools, and the permissions Devin needs to observe and control apps. A fresh install asks for permissions such as accessibility and screen recording in dialogs, and each dialog would block Devin until a person approved it.

#### Preparing state on the host
To avoid the dialogs, Cognition mounts the guest image disk on the host and prepares its state directly. It creates the user and updates the macOS internal configuration databases so the permissions are in place before the guest boots.

#### Booting the prepared image
Cognition then boots the prepared image to install the remaining developer tools, verify Xcode, warm up the iOS Simulator, and let Spotlight indexing and other background setup finish. It also turns off background services that development sessions do not need. With the preparation done ahead of time, a new session can start building and testing as soon as its VM starts.

> ❌ **Anti-pattern — doing setup at session start:** letting each new VM install tools, approve permissions and index files while the agent waits. **Why people do it:** it avoids maintaining a golden image. **Consequence:** every session pays the full setup cost, and dialogs can stall the agent indefinitely. **Fix:** prepare the image once, outside the session path, and boot from it.

### Give users a responsive live view of the app
Users can watch the macOS desktop in a Virtual Network Computing (VNC) view, and their app in a new iOS Simulator pane in the Devin session. From either view, users can try the app themselves and take control when they want.

🔎 Added context: VNC is a remote-desktop protocol that sends screen images to a viewer and sends keyboard and mouse input back.

Figure 2 shows a recorded Devin session in which Devin builds a 3D iPhone game and tests it in the iOS Simulator, while the session's Computer pane shows the macOS desktop.

Two steps decide whether the live view responds fast enough for users to operate the app: capturing the screen and delivering the video.

#### ScreenCaptureKit and the buffer pool
For the macOS desktop, ScreenCaptureKit, Apple's desktop capture API, supplies screen images from a limited pool of reusable buffers. When Cognition needs to keep the last captured image, it keeps its own copy instead of holding a pooled buffer. Holding a buffer while the screen is idle can delay the next screen change.

> ❌ **Anti-pattern — holding a pooled capture buffer:** keeping a buffer from the capture pool to use as the "last frame" while the screen is idle. **Why people do it:** it seems cheaper than copying the image. **Consequence:** the capture pool runs short, and the next screen change waits. **Fix:** copy the image you need to keep.

#### Frame pipelines and stale backlogs
After capture, the desktop and Simulator video is encoded, sent to the user's browser and displayed. When one stage cannot keep up, frames wait between stages. The stream can look smooth while it shows an old state of the app. In the iPhone game example, a user might press Pause and still see the game move, because the viewer is still showing frames from before the press. To keep the view current, Cognition limits how many frames can wait between stages and does not replay a stale backlog.

> ❌ **Anti-pattern — replaying a stale backlog:** queuing every frame between stages and playing them all back in order. **Why people do it:** it looks complete and loses nothing. **Consequence:** the user sees an old state, and input appears to do nothing. **Fix:** cap the queue between stages and drop frames that are already out of date.

#### Keyframes and recovery
Compressed video frames often depend on earlier frames, so dropping one can leave the frames after it impossible to decode. A keyframe does not depend on earlier frames, so the decoder can start fresh from it. When the Simulator stream falls behind, it restarts from a recent keyframe or requests a new one, and skips the missed frames. Keyframes belong to the compressed video stream, so they are separate from the screen images Cognition copies at capture.

### Let Devin operate native apps through the accessibility tree
Like users, Devin needs to see and operate the app. It also needs an efficient way to check the result of each action. Devin does this through computer use. In its simplest form, computer use takes a screenshot, has a model find a control in the image, clicks it, and takes another screenshot. That loop is slow and costly across the many pause, quit and relaunch paths Devin tests. Web apps are cheaper to automate, because the browser exposes the page structure, so Devin can find a named button or read a field's value without working from pixels.

#### The accessibility tree
Native macOS apps and apps in the iOS Simulator run outside the browser, but they expose a structure of their own: the accessibility tree. Built for assistive technologies, the tree describes each control by its role, name, value, state and relationships to other controls. Cognition exposes this tree to Devin's computer-use tool, so Devin can confirm that a control is a Resume button instead of guessing from pixels.

#### The observe, reference and act loop
Devin queries for a control, acts on the reference the query returns, and then reads the tree again to report what changed. The article's example, adapted from Cognition's earlier post, shows a query for a Resume button and the press that follows.

The query:

```json
{ "action": "query", "target": "ios", "role": "button", "name": "Resume" }
```

**What this does:**
- `action: "query"` asks for controls, not a screenshot. The agent reads structure.
- `target: "ios"` selects the Simulator app's tree rather than the macOS desktop.
- `role` and `name` narrow the result to a button with the accessible name "Resume". The agent names what it wants, not where the pixels are.

The shortened result lists a reference, the control's role, its accessible name and the actions it supports:

```text
@i17 button "Resume" {press}
```

**What this does:**
- `@i17` is a reference that the next action can use, so the agent does not need to re-identify the control.
- `button "Resume"` confirms the role and accessible name in one line.
- `{press}` lists the supported action, so the agent knows what it can do with this control.

Using that reference, Devin presses the control:

```json
{ "action": "act", "target": "ios", "reference": "@i17", "operation": "press" }
```

**What this does:**
- `action: "act"` performs an operation on an existing reference, rather than locating the control again.
- `reference: "@i17"` ties the press to the exact control from the query.
- `operation: "press"` is the same action the result advertised, so the agent does not guess at available operations.

Cognition built these capabilities on the open source accessibility-cli project from Dioxus, which is now part of Cognition. The project provides accessibility, input and screen-capture building blocks, and Devin turns them into the observe, reference and act loop.

#### Pixels still matter
Even with the accessibility tree, Devin relies on screenshots to check how the app looks. In the game example, a closed pause menu does not prove that the game restored the player's position, because the app might draw gameplay directly instead of describing it in the tree.

> ❌ **Anti-pattern — trusting the tree alone:** treating a control's state in the accessibility tree as proof of the whole app state. **Why people do it:** the tree is structured and cheap to read. **Consequence:** the agent reports success for a state the app never restored, such as a game drawn directly to the screen. **Fix:** confirm important state with a visual check as well as the tree.

## Results
Cognition launched macOS support for Devin on September 15, 2026. Within six weeks, nearly 1,000 organizations were running Devin on macOS. Devin no longer needs a customer's CI system or Mac computers to build and test Apple apps, because it builds, runs and verifies iPhone, iPad and Mac apps in its own cloud workspace. From that workspace, Devin also uploads builds to TestFlight, Apple's beta testing service.

### Startup time
Cognition starts a fresh macOS VM for each Devin session in under a minute, using a fleet of EC2 Mac hosts across six AWS Regions. By Cognition's estimate, that startup time puts customers into a working macOS environment 15 to 20 times faster than setting up a Mac host themselves.

### Customer results
Cognition's Evan Almloff, a Member of Technical Staff, says that EC2 Mac gave Devin macOS environments in the cloud and unlocked iOS and macOS development. The Cognition fleet uses mac-m4pro.metal and mac-m4.metal instances across six Regions, and it can mint fresh macOS VMs for Devin in under a minute.

Wispr, which makes the Wispr Flow voice dictation app, uses Devin for macOS to test its iOS app end to end. Its Lead iOS Engineer, Sumit Oberoi, says the recording and transcription tests were the biggest blocker, and that Devin's macOS support cut code review time by 70% and improved velocity by nearly 2x.

## Plan your own macOS fleet on EC2 Mac
The article lists these points from the Amazon EC2 User Guide as factors in designing a macOS fleet.

> 📋 **Checklist — macOS fleet design points**
> - [ ] Each Dedicated Host runs one Mac instance, and you can release a host only after its 24-hour minimum allocation period.
> - [ ] Share Dedicated Hosts with other AWS accounts, or within an organization in AWS Organizations, using cross-account Dedicated Host sharing.
> - [ ] Mac instances are available as On-Demand Instances, and a Savings Plan can lower their cost.
> - [ ] Mac instance types are available in specific Regions, so check instance types by Region before you plan capacity.

🔎 Added context: The "each host runs one Mac instance" rule is why Cognition's two-workspace-per-host design depends on running guest VMs on the host, not on the host running a Mac instance per session.

## 🧠 Mental models
### Separate the session's lifetime from the host's lifetime
**The model:** The unit you rent (a host with a 24-hour minimum) and the unit that does work (an agent session) run on different clocks. Cognition keeps hosts allocated and moves sessions on and off them.
**How to apply it:** When your billing or allocation unit is bigger than your work unit, schedule work into the unit you already pay for, and release the unit only when its minimum has passed.

### Persist state as snapshots, not as running machines
**The model:** A VM is disposable. Its disk is what matters, and the disk lives in snapshots that can be restored in seconds.
**How to apply it:** Release idle compute as soon as its state is saved. Make resuming a restore, not a rebuild.

### Structure over pixels
**The model:** When the platform exposes structure (the accessibility tree), an agent should query and act on named references, not on screenshots.
**How to apply it:** Use screenshots for verification, not for locating controls. Keep a visual check for state the structure cannot show.

### Drop stale frames and resync at keyframes
**The model:** A live view is only useful when it is current. A slow stage should lose old frames, not delay new ones, and the decoder recovers from a keyframe.
**How to apply it:** Cap the queue between stages, never replay a backlog, and make the stream restartable from a recent keyframe.

## 🔑 Key takeaways
- Cognition runs each Devin macOS session in a fresh VM started in under a minute on EC2 Mac hosts across six AWS Regions.
- Each EC2 Mac host runs two isolated Devin workspaces at a time, the limit set by the macOS SLA, and idle sessions release their VMs.
- About 90% of sessions give up their Mac VM when idle and resume from a snapshot in seconds.
- Cognition chose a file-handle network attachment and its own user-space Ethernet gateway, so one system owns the packet filter policy.
- Guest images are prepared on the host before first boot, so permission dialogs never block the agent.
- Devin operates native apps through the accessibility tree and uses screenshots only to verify appearance.

## 🏛️ For Kenji
*This is your interpretation, not the author's.* For an architect, the useful part is the capacity model: a 24-hour minimum on the bare-metal unit forces you to pack short-lived agent sessions onto long-lived hosts, and the snapshot layer is what makes that packing possible. The question it raises is how your own platform's cost model would change if the minimum billing unit were much longer than the average session.

## 📚 Reading plan
### From the article
- [Amazon EC2 Mac instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-mac-instances.html) — the official user guide for planning and running EC2 Mac.
- [Bringing macOS to Devin](https://devin.ai/blog/devin-gets-a-mac) — Cognition's own, more detailed account of how it built Devin for macOS.
- [accessibility-cli](https://github.com/DioxusLabs/accessibility-cli) — the open source project behind the observe, reference and act loop.
- [Amazon EC2 Dedicated Hosts sharing](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dh-sharing.html) — how to share hosts across accounts and within an organization.
- [Instance types by Region](https://docs.aws.amazon.com/ec2/latest/instancetypes/ec2-instance-regions.html) — check Mac instance availability before you plan capacity.
- [Apple Virtualization framework](https://developer.apple.com/documentation/virtualization) — the API that runs the macOS guests.
### To go further
- ScreenCaptureKit (Apple) — the capture API behind the buffer-pool trade-off in the live view.
- Network Block Device (NBD) protocol — the disk attachment that let Cognition keep its existing storage design.
- Virtual Network Computing (VNC) — the remote desktop protocol behind the desktop view.
- Video keyframes and decoder recovery — why a dropped frame can make later frames undecodable.
