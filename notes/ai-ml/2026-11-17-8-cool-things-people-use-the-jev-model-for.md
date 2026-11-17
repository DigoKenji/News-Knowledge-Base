---
type: Article Summary
title: 8 Cool Things People Use the Jev Model For
description: Eight public projects show Jev's fast, cheap structured decisions powering code review, computer use, classification, games and research.
resource: https://medium.com/generative-ai/8-cool-things-people-use-the-jev-model-for-dd0ed8e785d4
tags:
- ai-ml
- llm-engineering
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/generative-ai/8-cool-things-people-use-the-jev-model-for-dd0ed8e785d4
  title: 8 Cool Things People Use the Jev Model For
  author: Jim Clyde Monge
status: stable
source_url: https://medium.com/generative-ai/8-cool-things-people-use-the-jev-model-for-dd0ed8e785d4
source: Generative AI
source_type: medium
published: '2026-09-20'
domain: ai-ml
subdomain: llm-engineering
concepts:
- inference-tradeoffs
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81dea956e1a305308ad7
---

# 8 Cool Things People Use the Jev Model For

**Source:** https://medium.com/generative-ai/8-cool-things-people-use-the-jev-model-for-dd0ed8e785d4
**Author:** Jim Clyde Monge
**Date:** 2026-09-20
**Concepts:** [Inference throughput/latency/cost](/concepts/inference-tradeoffs.md)

---

## 🎯 Core thesis
Jev is a fast, inexpensive model that answers structured questions with decisions and probabilities that software can use directly, and eight real projects show it working in code review, computer use, classification, games, agent context management, trading and research.

## 🧭 Why the author wrote it
Jev was spreading quickly through social feeds, and the author wanted to look past the hype at what people are actually building. He collects eight examples with the costs and timings reported by their creators. If his reading is right, the useful question is not whether a fast cheap model is impressive but which everyday decisions in software can now afford a model call.

## Intro: What Jev is
The author opens by saying Jev is "incredibly cheap and ridiculously fast," and he explains it in one line: Jev takes information and answers structured questions about it, returning decisions and probabilities that software can use directly. The article attributes the speed to TypeSafe, which says the probabilities are produced in parallel. There is no paragraph of text to generate one token at a time.

### Structured decisions and probabilities
Instead of writing prose, the model returns a fixed answer to a fixed question, such as "is this a security issue" or "which of these categories fits," together with a probability for each answer. Software can branch on that output directly without parsing free text. This is the core of every example that follows.

> 🔎 Added context: A token is a small chunk of text, often a word or part of one, that a language model generates one step at a time. Parallel probability output avoids that step-by-step generation for the answer.

### Parallel probability output
The article's speed claim rests on TypeSafe's description of parallel output. Because the answer is a set of probabilities rather than a sentence, all of them can be produced at once. The author treats this as the reason for the sub-second timings reported in later examples, though he does not measure it himself.

## 1. Reviewing Pull Requests at Almost 0 Cost
Paolo Rosson, a user on X, built a code reviewer that sends a pull request's diff to Jev and receives 14 checks in one call. The checks cover exposed secrets, SQL injection, authentication changes, deleted tests, and whether the description matches the code. The reported cost is $0.00007 per review, with responses in about half a second. At that rate, 1,000 reviews cost seven cents, compared with roughly $14.50 for the creator's Opus 5 setup.

### Automated pull request review with per-check probabilities
Each check is a separate question with its own probability, not one overall verdict. Surrounding code turns those numbers into actions: block the change, request a security review, flag minor issues, or recommend merging. Splitting the review into many narrow checks makes each decision easier to audit than a single "is this code good" score.

### Confidence-band escalation
The author admires how the system handles uncertainty. When a critical check lands between 0.35 and 0.65, the system escalates it to a person or to a larger model, so the cheap model only decides the clear cases. The band is adjustable, so the reviewer can be made more cautious by widening it.

> 🔎 Added context: A pull request is a proposed set of code changes submitted for review before it is merged into the main codebase.

The author would use it as a first pass on his own website projects. A small styling change could get a lighter review, while anything touching authentication or the database would get more attention.

## 2. Jev is Really Good at Computer Use
Shiv shared a computer use tool built on Jev. It responds in under a second and costs almost nothing per interaction. In the demonstrated workflow it also decides what to type, so no separate language model is needed. The project is Third Hand on GitHub, and the article points to a second computer use example from Instant Rice Cook for comparison.

### Computer-use agent loop
A computer-use agent looks at the current screen, chooses an action such as opening a menu, selecting a field or typing text, performs it, and then checks the result before continuing. The article's point is that the model's choice is only one part of the loop. Software still has to identify the available controls, carry out the chosen action, and verify what happened afterward.

### Per-action latency compounds
The author notes that an agent moving through menus and several screens performs many actions in sequence. A short delay on each action adds up over the whole task, which is why the sub-second response matters more here than in a single one-off call.

> ❌ **Anti-pattern — Ignoring the verify step (implied):** Treating the model's chosen action as done without checking the screen afterward. **Why people do it:** The action itself looks like the whole task, and the check is extra plumbing. **Consequence:** A wrong or failed action goes unnoticed and the sequence continues from a bad state. **Fix:** Confirm each step against what should have changed on screen before moving on.

The author's first experiment would be a repetitive admin dashboard task on his tech blog, Zeniteq: open an article, choose a category, change a setting and return to the list. Because he already knows what each action should accomplish, mistakes would be easy to spot.

## 3. Finding Internal Links Across an Entire Website
Another X user used Jev in a workflow that read 586 website pages and rebuilt the internal link map in 45.1 seconds. The run placed 584 links at a cost of $0.21. It also declined to link 139 pages because it could not find a suitable match, and the author says this detail interested him more than the page count.

### Relevance scoring for internal links
The workflow gathers candidate pages, asks Jev to score how relevant each one is, and suggests links where readers would benefit from more context. The author wants the proposed anchor text and destination shown together, so he can judge whether the link reads naturally in the sentence. A good suggestion sends a reader from a guide to AI coding tools to a detailed explanation of MCP, not to an unrelated post that merely mentions AI.

> 🔎 Added context: An SEO audit is a review of a site's structure, content and links to find problems that affect how search engines find and rank its pages.

### What the run does and does not prove
The author stresses that the run shows internal linking work, which is only one part of an SEO audit, and that it does not establish any ranking improvement. The immediate benefit he sees is making useful articles easier to find while spending less time searching his own archive. He would start with a preview of suggested changes on Zeniteq, review a sample, and pay special attention to links added to older posts whose information may need updating.

## 4. Sorting Tax Documents at About $0.001 per Page
Another user built a tax document classifier around Jev and reported a cost of about $0.001 per page. The team describes it as 34 times cheaper and six times faster than their previous LLM pipeline, with every document in their corpus classified correctly. The classifier reads page text and asks Jev to identify the form from predefined form descriptions, returning a form category and a confidence value for the next stage of the software.

### Classification with confidence thresholds and strict accuracy
A classifier that returns a label plus a confidence value lets the system treat uncertain pages differently from confident ones. The open source repository reports 100% strict accuracy across 261 IRS forms at about $0.001 per page, and no errors across 314 filled form pages. A separate set of 753 blank form pages had no wrong labels, but 38 pages fell below the confidence threshold and were counted as strict errors. The author values that distinction: a page sent for another look is a different outcome from one confidently sent to the wrong place.

### Repetitive sorting as the target use case
The author says this is the least flashy example but one a team could use more every day than a game demo. Sorting thousands of pages is repetitive, and a classification mistake can misdirect the rest of a process. He would apply the same approach to invoices or receipts by defining categories carefully, inspecting uncertain results, and measuring how much manual sorting remains.

> ❌ **Anti-pattern — Forcing a label on every page (implied):** Requiring a category for every input, even when the model is unsure. **Why people do it:** A complete output feels cleaner and skips a review queue. **Consequence:** Low-confidence pages are misfiled with no signal to a reviewer. **Fix:** Set a confidence threshold and route pages below it to a person, counting them separately from confident errors, as the repository does.

## 5. Playing Subway Surfers Across 50 Games
Max Blade used Jev to play Subway Surfers at what he describes as superhuman speed, including a demonstration of 50 games running at once. He says the featured run cost less than a cent. The author finds this kind of experiment useful because games show quickly whether a sequence of decisions works: the character either avoids the obstacle or runs into it.

### Game agent and bounded action space
A runner game has few legal moves, such as changing lanes or jumping. A controller evaluates the situation, picks one action, and repeats as the game advances. The author cautions that the clip's performance depends on the whole setup, including the usable game information and the software that executes each move, so Jev could not simply watch any game and play it equally well.

### Driving demo and its limits
JP Schroeder described rebuilding Tesla Full Self Driving with Jev in under an hour. The author calls that an ambitious description for an experiment and says he would need considerably more evidence before comparing it with a system that drives on real roads. He would stick to games and simulated environments, where he would build a small browser game with characters that react differently to the same situation and watch how changing their decision criteria changes outcomes. A cautious character might avoid obstacles early while another takes riskier routes, and the small action space would make each bad move easy to explain.

## 6. Clearing Out an Agent's Context More Quickly
Tamara Tran used Jev for compaction, which reduces the conversation history an agent carries into its next turn. Her project scores each tool call and result, then removes or shortens material judged unnecessary. The retained content keeps its original wording. The author likes the prospect of preserving an exact error message or command while clearing out old output that has already done its job.

### Context compaction for agents
A long coding session accumulates material that is no longer needed: test output made irrelevant by a fix, or a file read superseded by several edits. Keeping it makes the conversation longer without helping the next decision. The hard part is predicting what will matter later. A minor detail from an early tool result can explain a failure much further into the task, so compaction has to be conservative about what it removes.

The code is fast-jev-compaction, a Claude Code plugin that replaces the compaction summary with Jev decisions. The article describes its documented behavior: recent messages are protected, tool calls stay paired with their results, and user and assistant text is left unchanged in the resulting conversation.

### Testing compaction and measuring the pause
The author would test compaction by returning to an earlier requirement after compaction and checking whether the agent still has what it needs. He notes that "instant" describes the ambition of the demo rather than a guarantee. The repository allows larger histories to require multiple requests, and repeatedly reopening files would eat into the time saved. He would measure the pause in his own sessions before judging how much faster it feels.

> ❌ **Anti-pattern — Judging by the demo's speed (implied):** Treating the headline speed as the experience in real use. **Why people do it:** Demos show the best case with small inputs. **Consequence:** Large histories that need multiple requests erase the expected time savings. **Fix:** Measure latency on representative sessions before adopting the tool.

## 7. Jev as a Trading Bot
Jarrod Watts built a bot that uses Jev to decide whether to buy or sell based on an asset pair's price feed. He says it executes real trades through Kuru's order book on Monad, aiming at the chain's 300ms block interval. The model supplies the decision, and the surrounding application places the order. The author finds it a good demonstration of how quickly model output can feed into other software.

### Decision loop and latency budget
The loop is: read the price feed, ask the model for a buy or sell, execute, and repeat each block. The 300ms block interval sets the time budget for the whole cycle, not just for the model call. The article's Jev Trader demo includes a dry run mode and a standing instruction to submit a bid or ask on the MON/USDC order book every block, with no option to abstain.

### Why a fast decision says little about profit
The author is more interested in the implementation than in making money. A fast buy or sell tells us little about whether it is profitable. Evaluation needs results over time, including trading costs and the outcomes when prices move against the bot. His own experiment would stay in simulation: log every decision, compare it with a simple fixed strategy, and look for where the model repeatedly makes poor choices. Even an unprofitable result could teach something about building a responsive decision loop, but he would want the performance chart next to the latency chart before getting excited.

> ❌ **Anti-pattern — Always-act decision loop (implied):** Requiring the model to pick a side on every tick. **Why people do it:** It keeps the demo active and the decision path simple. **Consequence:** Weak signals are traded anyway, so the bot cannot be compared with a version that waits. **Fix:** Allow an abstain outcome and compare the two behaviors in simulation.

## 8. Organizing More Than 1,000 AI Research Papers
Hassan used Jev to classify 1,018 AI research papers. He reports a total classification cost of $0.08 and a median end-to-end latency of 256ms per paper. The finished project lets readers explore the collection by topic. The workflow first summarizes each paper with DeepSeek V4 Flash, then sends each title and summary to Jev along with 24 possible topics.

### Summarize-then-classify pipeline
Splitting the job between two models makes each one's role easy to see. DeepSeek creates the summaries, and Jev assigns the topics using the titles and summaries. The reported costs are $3.99 for the summaries through Together AI and eight cents for the Jev classification calls through TypeSafe, for a reported inference total of $4.07.

### Topic taxonomy for browsing
The author calls this his favorite example because he would use something similar for article research. Browsing a topic can surface a paper he would not have thought to search for. For his own version he would start with a smaller collection about AI coding agents, adjust the categories while browsing, and check whether they help him find related work. He says he would happily spend a few dollars on a research tool built around his subjects.

## Final Thoughts
The author calls Jev the biggest AI breakthrough of 2026 so far, which is his opinion rather than a measured claim. He points to code reviews, computer control, document sorting and games as evidence that many everyday tasks could benefit from fast, inexpensive decisions. Vercel made Jev free until September 25, which he calls a strong reason to experiment. He closes by noting there is still much to learn about how reliably these projects work beyond their demos, and says the research paper example is the most promising because it combines Jev with another model.

## 🧠 Mental models
### Probabilities as routing signals
**The model:** A model that returns a label and a probability lets the software decide what happens next, rather than the model deciding everything.
**How to apply it:** Define what each probability range triggers, such as automatic action, human review or a larger model.

### Escalate the uncertain middle
**The model:** Decide the clear cases cheaply and send the ambiguous band to a more careful path.
**How to apply it:** Choose thresholds, such as the 0.35 to 0.65 band in the reviewer, and make them adjustable so the team can tighten them over time.

### Separate the decision from the action
**The model:** The model chooses; surrounding code executes and verifies.
**How to apply it:** For any agent or bot, write the executor and the check-after-action step as carefully as the prompt.

### Per-step latency compounds
**The model:** A small delay repeated across many sequential actions becomes the dominant cost of the whole task.
**How to apply it:** Time each step in a representative run and estimate the total before choosing a model for every step.

## 🔑 Key takeaways
- Jev returns structured decisions and probabilities, and the article attributes its speed to parallel probability output.
- The reviewer's 14 separate checks, each with a probability, let the surrounding code block, escalate, or approve changes.
- A critical check between 0.35 and 0.65 goes to a person or a larger model in the reviewer's design.
- Reported costs are very low, such as $0.00007 per PR review and about $0.001 per tax form page, but they are creators' own figures.
- Classification accuracy claims depend on how uncertain pages are counted: 38 low-confidence blank pages were scored as strict errors.
- Compaction that keeps tool calls paired with their results and leaves user and assistant text unchanged is a concrete design constraint for coding agents.
- Speed and cost in demos do not establish profitability, ranking gains, or production behavior, and the author says so for several examples.

## 🏛️ For Kenji
This is your interpretation, not the author's. The most transferable pattern is the reviewer's split between a cheap model that decides clear cases and an escalation band for uncertain ones, which maps onto governance controls for AI decisions. Where it would apply in an architect's work is any high-volume review or classification path where the cost of a wrong automatic decision is high. A question it raises: who sets and audits the escalation thresholds, and how would you show that a band of 0.35 to 0.65 holds up over time?

## 📚 Reading plan
### From the article
- [generativeai.pub](http://generativeai.pub) — the author's earlier article, "New Jev Model is Insane! 200x Faster & 400x Cheaper Than Frontier Models", which explains how Jev works.
- [github.com/shhivv/third-hand](http://github.com/shhivv/third-hand) — the Third Hand computer use project, to see how screen state becomes model choices.
- [github.com/kyotofin/tax-doc-classifier](http://github.com/kyotofin/tax-doc-classifier) — the tax form classifier and its published evaluation, including the 38 low-confidence blank pages.
- [github.com/tamaratran/fast-jev-compaction](http://github.com/tamaratran/fast-jev-compaction) — the Claude Code compaction plugin and its documented behavior.
### To go further
- Confidence thresholds and selective prediction — how to decide when a model's answer is good enough to act on.
- Context compaction for coding agents — what to keep and what to drop from long tool-using sessions.
- Summarize-then-classify pipelines — when splitting work between two models helps cost and clarity.
