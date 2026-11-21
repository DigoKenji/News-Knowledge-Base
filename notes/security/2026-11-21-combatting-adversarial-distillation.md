---
type: Article Summary
title: Combatting adversarial distillation
description: Adversarial distillation can copy frontier capabilities without safeguards, so OpenAI now protects models' hidden
  reasoning across requests, APIs and campaigns.
resource: https://openai.com/index/combatting-adversarial-distillation
tags:
- security
- distillation
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://openai.com/index/combatting-adversarial-distillation
  title: Combatting adversarial distillation
  author: OpenAI News
status: stable
source_url: https://openai.com/index/combatting-adversarial-distillation
source: OpenAI News
source_type: blog
published: '2026-11-20'
domain: security
concepts:
- distillation
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d81dcaa04d2672affa431
---

## 🎯 Core thesis
Adversarial distillation can copy a frontier model's capabilities without its safeguards, so protecting the model's intermediate reasoning at the request, API and campaign levels is a central part of defending against it.

## 🧭 Why the author wrote it
The article starts from a specific risk: an actor can use a frontier model's outputs without authorization to train a similar model and deploy it without comparable protections, for example to carry out cyberattacks the original would have refused to help with. It explains how OpenAI detects and disrupts this, what it changed in GPT-6.2 Astra and GPT-6.2 Sol, and what other developers can take from it. If the thesis is right, safeguarding only the visible answer is not enough, because the reasoning behind that answer can be the most valuable training data for a copy.

## Introduction
### Adversarial distillation
The article opens by saying that as frontier AI models become more capable, safeguards matter more for using them responsibly. Adversarial distillation is the path around those safeguards: an actor takes a frontier model's outputs without authorization, uses them to train another model with similar capabilities, and deploys that model without comparable protections. The article's example is an adversary that distills a model's cyber capabilities and then uses the result to help with cyberattacks that the original model would have refused, or that its safeguards would have blocked.

The opening also previews three things: how OpenAI detects and disrupts adversarial distillation, how it has strengthened protections for its models' protected reasoning, and what it has learned that can help the wider industry strengthen its defenses.

### Protected reasoning
Protected reasoning is the chain-of-thought: the intermediate steps a model generates internally while working toward an answer. The article places particular emphasis on preventing attackers from extracting it, because these traces can contain information the final answer leaves out, which makes them especially valuable training data for another model. Most of the defenses described in the article are aimed at this one asset.

## Our approach to adversarial distillation
### Combined safeguards
OpenAI's approach combines adversarial testing, model training, classifiers and API-level protections with investigations into coordinated attempts to extract model capabilities. The article says these updated safeguards align more closely with the ones OpenAI uses for biology and cybersecurity.

### Updated safeguards in GPT-6.2 Astra and GPT-6.2 Sol
OpenAI says it strengthened its defenses against protected reasoning extraction in GPT-6.2 Astra and GPT-6.2 Sol. In an internal adaptive evaluation using strong automated red-teaming, testers checked whether attackers could recover protected reasoning the model had generated while answering an earlier prompt. The average attack success rate fell from 54.0% against GPT-6 Astra with its original safeguards to 0.2% against GPT-6.2 Astra with its updated safeguards.

> 🔎 Added context: Red-teaming means testing a system by actively trying to break it. Automated red-teaming uses AI systems to generate those attacks at scale.

## Why distillation is a safety issue
### Distillation
Distillation is a common machine-learning technique in which one model learns from data produced by another. A model can learn from another model's answers, its reasoning, or its judgments about which answers are better and why. The article says that beyond intellectual property concerns, unauthorized distillation poses serious safety risks, because a second model can learn advanced frontier capabilities without inheriting the safeguards that govern how the original model can use them.

### Dual-use domains
The concern is especially clear in dual-use domains such as cybersecurity and biology. A model that helps defenders find software vulnerabilities could also help an attacker exploit them, and a model that assists biological research could help someone develop a harmful pathogen. If such capabilities were transferred to a model with weaker safeguards, that model might provide help the original would have refused, or that the original safeguards would have blocked.

## The importance of protecting exact reasoning traces for cyber safety
### Teacher and student models
OpenAI's models help users with coding and defensive cybersecurity tasks. A second model trained on many of those benign outputs may learn skills that also apply to offensive cybersecurity tasks. The model that produces the training data is called the teacher, and the model that learns from it is called the student. The article's internal study asks how distillation, especially distillation that involves reasoning traces, may transfer risky cyber capabilities.

### Exact reasoning traces versus reasoning summaries
The study used the content normally visible to users (final responses, reasoning summaries and tool interactions) as its baseline for distillation. It then tested how much additional cyber capability transfers when the student has access to the teacher's exact reasoning traces. Each training run started from the same pretrained student model. The supervised fine-tuning dataset consisted primarily of trajectories from Astra-level teacher models working on coding and defensive cybersecurity tasks, and each trajectory included every step the model took, including tool interactions and the final response. Each student was evaluated after fine-tuning and again after a lightweight round of reinforcement learning in cybersecurity and coding environments.

> 🔎 Added context: Supervised fine-tuning trains a model further on example inputs and desired outputs. Reinforcement learning trains a model by rewarding outcomes it should produce, and "lightweight" here means a small round of it.

After supervised fine-tuning, the student trained on exact reasoning traces generally outperformed the student trained on reasoning summaries. After reinforcement learning, the exact-trace student still scored higher at the largest inference budget tested, on both benchmarks shown in the article, while the summary-trained student scored higher at lower budgets. The extracted text does not name the benchmarks or define the units of the inference budget.

The article concludes that protecting reasoning traces can limit cyber capability transfer, even when the underlying task is one allowed under its usage policies. Across many such interactions, exact reasoning traces can teach a student skills that generalize beyond the tasks the teacher originally performed. OpenAI says it will keep studying how much additional capability transfers across different student models and distillation training recipes.

Footnote 1 notes that Anthropic reported a similar advantage when training on full reasoning transcripts rather than summaries, on SWE-bench Verified, a software engineering benchmark. OpenAI says its result shows this advantage extends to frontier cybersecurity.

## Defenses that reinforce one another
### Request-level and campaign-level defense
The article describes two complementary levels in its safeguard stack. At the request level, model behavior, classifier checks and API controls limit reasoning trace extraction. At the campaign level, OpenAI looks for patterns in request-level and account-level signals that could reveal coordinated extraction attempts. Attacks uncovered in investigations then become examples for training and testing the safeguards, so the two levels feed each other. In short, the defenses combine safeguards for individual requests with campaign detection and enforcement, and findings from investigations and automated red-teaming strengthen both.

## Protecting individual requests and responses
### Defensive training for model behavior
Adversarial training teaches models to recognize reasoning extraction attempts and to refuse to disclose protected reasoning, while still answering legitimate requests for explanations and assistance. Its training examples come from successful attacks identified through internal automated red-teaming, investigations, and responsible disclosures of potential security weaknesses from outside researchers or industry partners.

### Classifier safeguards
OpenAI uses input classifiers to detect attempts to extract protected reasoning, and output classifiers to detect leaks of protected reasoning in assistant messages and tool-call arguments. These safeguards apply to both streaming and non-streaming responses. Tool-call coverage matters because a model can expose protected reasoning in a tool's arguments without including it in its final answer. Output classifiers can block suspected leaks before that content reaches the caller.

> ❌ **Anti-pattern — Checking only the final answer:** Screening just the assistant's final message for leaked reasoning and ignoring tool-call arguments. **Why people do it:** The article does not say; the final message is the visible output and the most obvious place to check. **Consequence:** Reasoning can leak through a tool's arguments and reach the caller unnoticed. **Fix:** Also check tool-call arguments, and block suspected leaks with output classifiers before the content reaches the caller.

### API-level protections
API-level protections restrict how protected reasoning can be reused across interactions and models. OpenAI's September disclosure described attacks that moved encrypted reasoning from one conversation into another and asked the model to reveal it in user-readable form. OpenAI strengthened protections across user, workspace, organization and model-family boundaries, including closing a pathway that could expose another user's encrypted reasoning.

With GPT-6.2 Astra and GPT-6.2 Sol, OpenAI ties a model's protected reasoning to the conversation history that produced it. As new messages are added, the model can build on its earlier reasoning. If an earlier message is rewritten, the API drops the reasoning that followed it. Older model families with weaker safeguards are also prevented from reading GPT-6.2 Astra's and Sol's protected reasoning.

> 🔎 Added context: Encrypted reasoning is reasoning stored in a form users cannot read directly. The article does not describe the encryption scheme.

## Investigating and disrupting coordinated campaigns
When OpenAI's systems detect unauthorized adversarial distillation campaigns, which often include coordinated attempts to extract protected reasoning, it investigates the accounts responsible. When it identifies violations, it enforces against the network of accounts, consistent with its policies and terms of service. Each campaign investigation produces insights that let OpenAI upgrade its defenses to detect similar activity.

### Sharing indicators of compromise
OpenAI shares indicators of compromise with other model providers so they can investigate related abuse on their own services. The article says this work extends beyond OpenAI's own platform.

> 🔎 Added context: An indicator of compromise is a traceable sign of suspected malicious activity that another party can search for. The article does not say what specific indicators OpenAI shares.

### Consistent-standards policy
OpenAI's consistent-standards policy establishes shared standards for safeguards and coordinated enforcement across providers that offer its models. OpenAI works closely with partners where appropriate to uphold these standards and strengthen protections across deployments.

## Measuring progress
### Resistance to reasoning extraction
To measure how well the request-level safeguards work together, OpenAI tests whether attackers can still extract protected reasoning in a setup that closely mirrors production deployments. In static evaluations, testers replay a fixed set of previously successful attacks to see which still expose protected reasoning. In adaptive evaluations, automated AI red-teamers adjust their approach based on how the system responds, which can uncover weaknesses that static tests miss.

Testers' visibility also varies. Black-box testing gives them the same view an external user would have, while white-box testing gives them extra information about the system to help build stronger attacks. In the adaptive black-box evaluation, testers tried to recover protected reasoning generated while answering either an earlier request or the current one. On GPT-6.2 Astra with its updated safeguards, the average attack success rate was below 1% for each attack type, substantially lower than on GPT-6 Astra with its original safeguards. OpenAI is now applying selected safeguard improvements to GPT-6 Astra as well.

| Attack type | GPT-6 Astra, original safeguards: adaptive extraction ASR (%) | GPT-6.2 Astra, updated safeguards: adaptive extraction ASR (%) |
|---|---|---|
| Extracting protected reasoning from an earlier request | 54.0 | 0.2 |
| Extracting protected reasoning from the current request | 22.9 | 0.9 |

The table reports attack success rate (ASR) in adaptive black-box red-teaming against GPT-6 Astra and GPT-6.2 Astra, aggregated across six attacker models. Lower ASR is better.

> ❌ **Anti-pattern — Relying on static replay alone:** Using a fixed set of previously successful attacks as the only test of a defense. **Why people do it:** The article does not say; a fixed set is repeatable and easy to rerun. **Consequence:** Static tests can miss weaknesses that an adaptive red-teamer would find. **Fix:** Add adaptive evaluations in which automated red-teamers adjust their approach to the system's responses.

### Campaign detection and enforcement
OpenAI has seen multiple unauthorized adversarial distillation campaigns on its platform this year and shared initial findings in its September report. Its weekly enforcement actions against these campaigns have ranged in scale from thousands to tens of thousands of accounts, and the rate increased significantly over this past summer. OpenAI tracks the prevalence of distillation abuse and the time between detection and enforcement. Its objectives are to constrain the volume and value of data distillers can obtain, to increase the cost of distillation campaigns, and to shorten the time needed to respond to new attacks. The article says it continues to improve these defenses through adversarial testing and investigations.

## The national security implications of adversarial distillation
Adversarial distillation also poses a national security risk. A foreign actor that reproduces advanced capabilities from a U.S. model can deploy them without the protections its developer put in place. In February, OpenAI briefed Congress on activity indicating that DeepSeek was attempting to distill U.S. frontier models. In September, it reported disrupting a campaign to extract its models' protected reasoning, and it attributed a core part of that activity to individuals associated with Moonshot AI, the developer of Kimi. OpenAI says it plans to share more soon about recent campaigns it has disrupted.

### Legislation and export controls
OpenAI supports legislation that would give the U.S. government more tools to identify and sanction foreign actors that illicitly extract capabilities from American AI models. It has also told Congress it supports the Chip Security Act, the AI OVERWATCH Act and the MATCH Act. The article's argument is that stronger export controls limit adversaries' access to the hardware needed to train frontier models, while distillation defenses make it harder to acquire some of the same capabilities from models that are already trained. Together, the two efforts help maintain a U.S. lead in frontier AI and give developers more room to slow a release when they need time to test safeguards.

## How others can build on this work
Supporting the safe proliferation of frontier capabilities is an important goal of OpenAI's anti-distillation work. Its experience so far suggests several useful steps for other model developers and platforms:

> 📋 **Checklist — Steps for other model developers and platforms**
> - [ ] Protect private reasoning across the full interaction. Check assistant messages and tool-call arguments for leaks, and restrict how encrypted reasoning can be reused across conversations and models.
> - [ ] Keep successful extraction attack logs found through red-teaming, production monitoring, or responsible disclosure as regression tests. Where appropriate, and consistent with applicable privacy policies, use those examples to improve model training and classifiers. When evaluating updated defenses, replay the original attacks and test whether an adaptive red-teamer can still find a way through those defenses.
> - [ ] Combine request-level mitigations with campaign-level investigation and enforcement.
> - [ ] Coordinate safeguard requirements and enforcement with deployment partners, and share expertise with other AI model developers, to ensure a high floor of distillation safeguards across the industry.

OpenAI has also shared findings through the Frontier Model Forum and through appropriate government information-sharing channels, to help other developers strengthen their defenses while keeping details that attackers could exploit private. It says it will continue working with researchers, developers and partners to share what it learns about emerging attacks and effective defenses. The article closes by saying that as frontier AI becomes broadly available, these defenses aim to reduce the risk that its capabilities are reproduced without adequate safeguards, in support of OpenAI's mission to ensure that artificial general intelligence is deployed safely and benefits all of humanity.

## 🧠 Mental models
### Two complementary levels
**The model:** Request-level safeguards check each interaction, while campaign-level detection looks for patterns across requests and accounts that reveal coordinated extraction. Each level strengthens the other.
**How to apply it:** Pair per-request checks with account- and pattern-level detection, and feed what investigations find back into training and tests.

### Attacks become regression tests
**The model:** Every successful extraction attack is a test case to keep, not a one-time incident.
**How to apply it:** Log successful attacks from red-teaming, production monitoring or responsible disclosure, and replay them whenever a defense changes.

### Check every output channel
**The model:** Leaks can appear anywhere the model writes, including tool-call arguments, not only in the final message.
**How to apply it:** Screen every output channel, both streaming and non-streaming, before content reaches the caller.

## 🔑 Key takeaways
- Adversarial distillation can transfer a frontier model's capabilities to a model without its safeguards, and the article's main example is cyber capability.
- In the internal study, a student trained on exact reasoning traces generally outperformed one trained on reasoning summaries after supervised fine-tuning, and kept its lead at the largest inference budget after reinforcement learning.
- Protection has to cover the whole interaction: output classifiers screen tool-call arguments, and API controls tie reasoning to its conversation history and block reuse across users, workspaces, organizations and model families.
- In adaptive black-box red-teaming, GPT-6.2 Astra's attack success rate fell from 54.0% to 0.2% for extracting reasoning from an earlier request, and from 22.9% to 0.9% for the current request.
- Defense works at two levels, request-level safeguards and campaign-level detection and enforcement, and investigation findings feed back into both training and tests.
- Static replay can miss weaknesses that adaptive red-teamers find, so evaluations should vary the attacker's approach and visibility.
- Shared indicators of compromise and consistent safeguard standards let other providers investigate related abuse on their own services.

## 🏛️ For Kenji
This is your interpretation, not the author's. For an architect working on AI governance, the article suggests that the most important controls sit at the boundaries of model use: inspecting requests, screening outputs on every channel, and setting reuse rules across users, workspaces and model families. Those boundaries map onto where you would place an AI gateway or a policy enforcement point in an enterprise landscape. The question it raises for your architecture work: if your organization fine-tunes or distills from vendor models, which provenance and reuse rules govern the reasoning traces and outputs you keep, and who verifies them?

## 📚 Reading plan
### From the article
- [Combatting adversarial distillation](https://openai.com/index/combatting-adversarial-distillation) — the source article that this note expands, including its figures and the full ordering of its sections.
- The extracted text contains no in-text URLs for the other works it cites, so they appear under "To go further."

### To go further
- September report on the campaign to extract protected reasoning — the earlier findings the article builds on, cited by OpenAI but not linked in the extracted text.
- September disclosure on encrypted reasoning moved across conversations — the attack pattern that drove the cross-boundary API protections.
- Frontier Model Forum — where OpenAI shared findings with other developers to strengthen their defenses.
- Consistent-standards policy — the shared safeguard and enforcement standards OpenAI applies across providers of its models.
- Chip Security Act, AI OVERWATCH Act and MATCH Act — the hardware and export-control measures OpenAI told Congress it supports.
- Anthropic's SWE-bench Verified result on training with full reasoning transcripts versus summaries (footnote 1) — a similar finding in software engineering.

## Concepts
- [distillation](/concepts/distillation.md)

