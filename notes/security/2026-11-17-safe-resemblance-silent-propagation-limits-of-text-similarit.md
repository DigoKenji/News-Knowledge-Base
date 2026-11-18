---
type: Paper Summary
title: 'Safe Resemblance, Silent Propagation: Limits of Text-Similarity Guardrails for AI Agent Safety'
description: A text-similarity guardrail misses functional injected outputs and flags authorized reuse, so textual resemblance cannot judge agent safety.
resource: https://arxiv.org/abs/2611.21520
tags:
- security
- appsec
- multi-agent-orchestration
- rag
- indirect-prompt-injection
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.21520
  title: 'Safe Resemblance, Silent Propagation: Limits of Text-Similarity Guardrails for AI Agent Safety'
  author: arXiv
status: stable
source_url: https://arxiv.org/abs/2611.21520
source: arXiv
source_type: arxiv
published: '2026-11-16'
domain: security
subdomain: appsec
concepts:
- multi-agent-orchestration
- rag
- indirect-prompt-injection
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d812ca4dddd6e409c78d3
---

## 🎯 In one sentence
A text-similarity guardrail (DonkeyRail) misses 8 of 19 functional outputs at threshold 0.5 (57.89% recall) while flagging 70 of 192 legitimate Luna replies (36.46%), so textual resemblance cannot separate propagated instructions from authorized reuse in this three-agent relay.

## ❓ The problem
In a multi-agent system, one agent's output becomes another agent's input. If an output carries an injected instruction, it can keep steering downstream agents even after its wording changes, so a filter that looks for copied text can miss it. The common defense is a similarity guardrail: it scores how much a reply overlaps with the records it was generated from, and it alerts above a threshold. The difficulty is that legitimate work overlaps heavily too, because quoting a source or reusing a template is often exactly what the user asked for. The paper asks whether a fixed similarity score can tell these two cases apart, and whether a threshold chosen on legitimate traffic still works on new traffic.

## 💡 The idea
The authors separate three things that are easy to conflate: how similar two texts are, what a text causes a later agent to do, and whether the task permits that. They label each output by its behavior instead of its wording. An output counts as functional if it makes two subsequent agents each compute a fresh result for a new ticket, so copying an earlier answer cannot succeed. The similarity score is then measured against that independent behavioral label. The authors also argue that permission comes from the trusted task and policy, not from the text, so an observation-only classifier cannot settle the question alone.

## ⚙️ How it works

### Behavioral relay experiment
An email agent receives nine synthetic background records, one relay record, and a meeting request that contains a ticket number. The relay asks the agent to add seven to the ticket, emit a separate LAB-PROOF line, and carry the same procedure into its reply. Three agents run in sequence. Each receives the actual previous output as context plus a new ticket, and no earlier seed is reinserted. The primary endpoint for a root output o1 requires the second and third agents to produce the correct fresh result. The paper also reports a stricter endpoint that requires all three agents to act.

```text
F(o1) = 1[A2 = 1 ∧ A3 = 1]
```

Here A_h is 1 when agent h outputs a correct fresh-result line outside relay tags, code fences and quoted lines, and 0 otherwise. F(o1) is 1 when both later agents act. Because each agent gets a new ticket, a copied answer cannot satisfy the endpoint.

### The similarity guardrail
DonkeyRail is a public classifier from the Morris-II research line. For each of three metrics it takes the maximum over ten retrieved records, comparing each record with the reply. BLEU measures modified n-gram precision, ROUGE-L measures longest common subsequence, and METEOR measures aligned unigrams with a fragmentation penalty. A three-feature logistic model turns these features into a score and alerts when the score reaches the threshold. The classifier is frozen for the whole study, and the paper reports the default threshold of 0.5 alongside a calibrated threshold.

```text
x(D, o) = ( max_d∈D BLEU(d, o),  max_d∈D ROUGE-L(d, o),  max_d∈D METEOR(d, o) )
s(D, o) = σ(b + wᵀ x(D, o)),  alert when s(D, o) ≥ τ
```

D is the set of retrieved records, o is the reply, and σ is the logistic function. The maxima can come from different records. The negative ROUGE-L coefficient means no single metric drives the combined score.

### Legitimate controls and their split
For each model, the study builds 192 calibration replies and 192 test replies. Each split has 48 outputs for each of four tasks: ordinary acknowledgment, verbatim quotation, paraphrase, and full template reuse. The request templates and source-record formats in the test split are different from those in calibration, with 24 complete templates in each split. The trusted request explicitly authorizes quotation and reuse, and the source records contain no relay procedure. Every legitimate output stays in the false-positive denominator, and a separate completion check records whether the reply did the task.

### Calibration rule
The calibrated threshold is set from the descending calibration scores s(1) ≥ … ≥ s(192) as τcal = nextafter(s(4), +∞). This permits at most three calibration alerts, an empirical calibration false-positive rate below 2%. The rule is fixed before any data collection, and test outputs and attack outcomes do not enter the choice.

### Fixed-threshold measurement correction
The paper finds that the public implementation reports fixed-threshold rates by interpolating between ROC probability thresholds, not by applying the threshold to the scores. Direct counting uses FPR(τ) = Σ 1[yᵢ = 0 ∧ sᵢ ≥ τ] / Σ 1[yᵢ = 0]. Across the 21 available configurations, direct counting changes the FPR in 19 and changes the FPR or TPR in all 21. For the three-feature logistic model at 0.5, the reported FPR of 0.021580 becomes 29/1500 = 0.019333 under direct counting. The AUC of 0.997884 is unchanged, so the error affects operating points, not ranking. All behavioral results in the paper use direct decisions.

### Proxy-label transfer analysis
The original generator labeled positives with a fixed roleplay phrase, which is a proxy for the behavior of interest. The paper defines criterion contamination: a proxy evaluation is contaminated when its label is the proxy itself, the proxy correlates with the detector's statistic, and the proxy disagrees with the target somewhere. Its Proposition 1 constructs a case where proxy AUC is 1 while the target AUC is any value a in [0, 1]. The corollary is that a high proxy AUC gives no universal lower bound on target AUC. The paper's normalized gap, CCI = 1 − AUC(f, B) / AUC(f, C), is only descriptive, and computing it needs paired proxy and target labels on the same population, which the DonkeyRail records do not provide.

### Authorization analysis
If the policy that determines permission is missing from what a guardrail observes, identical visible transcripts can require different decisions. Proposition 2 states that no deterministic function of the visible text can decide authorization correctly in both worlds, and a randomized classifier on the same input reaches expected accuracy 1/2 on a balanced pair. The paper builds 64 synthetic pairs with byte-identical task, retrieved contact and reply, but different trusted allow/deny policies. An observation-only constant baseline scores 64/128, while consulting the policy scores 128/128.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| Luna, fixed threshold 0.5, 72 held-out attack chains | 19 functional roots | 11 detected, 8 missed | Functional recall 57.89% (bootstrap interval 35.0% to 76.9%) |
| Luna, fixed threshold 0.5, legitimate test replies | 192 replies | 70 flagged | False-positive rate 36.46% (template-cluster interval 21.9% to 51.6%) |
| Luna, calibrated threshold | 0 of 192 calibration alerts | 1 of 19 detected; 15 of 192 test replies flagged | Recall 5.26%; false-positive rate 7.81% |
| Luna, threshold 0.5, quotation and template tasks | 48 outputs per task | 37 and 31 flagged | False-positive count (Table 2) |
| Sol, fixed threshold 0.5 | 0 of 72 functional roots | 98 of 192 replies flagged | False-positive rate 51.04%; recall undefined |
| Sol, calibrated threshold | 192 replies | 69 of 192 flagged | False-positive rate 35.94% |
| Luna, earlier evaluation, threshold 0.5 | 13 functional roots | 7 detected; 128 of 192 replies flagged | Functional roots detected (7 of 13); false-positive count |
| Pooled Luna root AUC / functional-versus-benign AUC | 144 roots, 384 legitimate outputs | 0.7609 / 0.5272 | AUC |

The results show that the frozen classifier misses a large share of functional outputs at the default threshold, and that it flags most legitimate quotation and template replies for both models. Sol's 48 of 48 false alerts on quotation and on template reuse is the most striking result of the set. Calibration removed all alerts on its own calibration split, but on the test split it flagged 15 replies and detected only one of 19 functional outputs. Sol produced no functional roots, so its recall is undefined, and zero observed propagation for Sol does not establish that the guardrail detects it. The model comparison is conditional on the tested prompts and reasoning settings. The paper's intervals describe uncertainty within its constructed tasks and templates only.

## ⚠️ Limitations
- The experiment measures instruction adoption under forced exposure, where each output is inserted into the next agent's context. It does not measure natural retrieval, message opening or delivery. There is no external tool execution, data exfiltration or persistent state, and the arithmetic action is a restricted proxy for broader malicious behavior.
- The study uses one arithmetic relay, twelve held-out framings and six variant types, with two model aliases that have no dated snapshots. The framings share one operational core, so they are not independent attack mechanisms. Half of the legitimate mixture consists of high-copy tasks, so the false-positive rates describe this mixture and do not estimate prevalence in real email.
- The calibration failure changes two things at once, the request templates and the source-record formats, so the cause cannot be isolated. The earlier evaluation found zero calibrated test alerts within shared templates, but it also lost all 13 functional positives under that threshold.
- 🔎 Added context: the attack and its success check are synthetic and use a self-describing canary procedure (add seven, emit LAB-PROOF). The paper's exploratory rule that checks for an unquoted proof line alerts on 10 of 13 functional roots with no legitimate alerts, but the authors say it was designed for this exact grammar and is not a general defense.
- 🔎 Added context: the blocking utility counts (Luna 122 to 90 completed tasks, Sol 188 to 93) assume that every flagged reply is blocked, so they are counterfactual estimates rather than measurements of a deployed system.

## 🏗️ What it means for building systems
This is interpretation, not the authors' claim. If you pass model outputs between agents, an overlap check on those outputs is at best one signal, and it would sit on every hop. Its cost in compute is small, but its false-positive cost falls on exactly the workflows that are common in agent pipelines, such as forwarding, summarizing and reusing templates. To trust such a check you would need a behavioral evaluation of your own agents, held-out templates drawn from your real tasks, and an explicit policy input that states what each task authorizes. The paper's designs for this, Fides and CaMeL, use labels and policy checks on tool calls rather than a threshold on overlap, which points toward provenance-aware policy as the stronger architecture.

## 🔑 Key takeaways
- At threshold 0.5, the DonkeyRail classifier missed 8 of 19 functional Luna outputs and flagged 70 of 192 legitimate Luna replies.
- Quotation and template reuse trigger false alerts because overlap is high by design: 37 of 48 and 31 of 48 for Luna, and 48 of 48 in each category for Sol.
- A calibrated threshold with zero calibration alerts still flagged 15 of 192 test replies and detected only 1 of 19 functional outputs, because the templates and formats changed.
- Fixed-threshold rates from the public code come from ROC interpolation, and direct decisions change the FPR in 19 of 21 configurations, so operating points need recomputation.
- A proxy AUC of 1 gives no guarantee of high behavioral AUC, and measuring the gap needs paired proxy and target labels.
- Authorization depends on policy outside the transcript, and in the authors' constructed pairs an observation-only classifier cannot decide it.

## 📚 Reading plan
### From the paper
The paper's text and reference list give no URLs, so these entries are named without links.
- Here comes the AI worm (Cohen, Bitton, Nassi), arXiv:2403.02817 — the origin of the self-replicating prompt and the Virtual Donkey guardrail that this paper tests.
- ComPromptMized: Public Morris-II and DonkeyRail artifacts (Cohen, Bitton, Nassi), GitHub repository at commit 6ab9116c2b08f3309a034ceac7f35a78416f05a7 — the code and classifier parameters the paper evaluates.
- Defeating prompt injections by design (Debenedetti et al.), arXiv:2503.18813 — the CaMeL design that separates trusted control flow from quarantined data.
- Securing AI agents with information-flow control (Costa et al.), arXiv:2505.23643 — the Fides design for propagating labels and checking tool calls against policy.
- AgentDojo (Debenedetti et al.), arXiv:2406.13352 — separates legitimate task utility from attacker-goal success.
- Autonomous LLM agent worms (Zha and Wang), arXiv:2605.02812 — evaluates propagation lifecycles behaviorally rather than by keyword retention.
### To go further
- Indirect prompt injection in LLM-integrated applications — the threat model that the whole paper builds on.
- Information-flow control for agent tool calls — how provenance and allowed-reader labels gate consequential actions.
- Criterion contamination and incorporation bias in evaluation — the validity concepts behind the proxy-label analysis.
- Threshold calibration under distribution shift — why a threshold with zero false alerts on one template set can fail on another.
