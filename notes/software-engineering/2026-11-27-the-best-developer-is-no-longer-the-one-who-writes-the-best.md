---
type: Article Summary
title: The Best Developer Is No Longer the One Who Writes the Best Code
description: Spec-driven development shifts developer value from coding speed to precise intent and rigorous verification,
  raising the bar.
resource: https://medium.com/gitconnected/the-best-developer-is-no-longer-the-one-who-writes-the-best-code-996e8ed0869b
tags:
- software-engineering
- spec-driven-development
- context-engineering
- ai-agent
- multi-agent-orchestration
- verification-tax
- technical-debt
- version-control-code-review
- software-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-27T06:47:00-03:00'
sources:
- resource: https://medium.com/gitconnected/the-best-developer-is-no-longer-the-one-who-writes-the-best-code-996e8ed0869b
  title: The Best Developer Is No Longer the One Who Writes the Best Code
  author: unknown
status: stable
source_url: https://medium.com/gitconnected/the-best-developer-is-no-longer-the-one-who-writes-the-best-code-996e8ed0869b
source: Level Up Coding
source_type: medium
domain: software-engineering
concepts:
- spec-driven-development
- context-engineering
- ai-agent
- multi-agent-orchestration
- verification-tax
- technical-debt
- version-control-code-review
- software-engineering
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/425baf74172d81d48210dfe13bd4b286
---

## 🎯 Core thesis
In Spec-Driven Development, a developer's value shifts from how fast they turn ideas into code to how precisely they state intent and how rigorously they verify the result, and that raises the bar.

## 🧭 Why the author wrote it
The author starts from a felt change: AI assistants produce code in minutes, but verification, integration and accountability consume the rest of the day. If the thesis is right, the scarce skill is no longer typing code. It is defining what "correct" means and proving it, and teams that keep measuring developers by coding speed will misjudge both people and risk.

## Introduction
The article opens with a comparison of two workdays. In 2019 a developer spent eight hours in the editor, debugging line by line through miles of boilerplate. Today the same developer spends an hour writing a specification, runs three agent instances in the background, and spends the rest of the day reading and accepting what they generated. The author's point is that this is "not less work, but different work." The opening also cites Faros AI data from more than 10,000 developers across 1,255 teams: in teams with high AI adoption, pull request review time grew by nearly 91%.

> 🔎 Added context: Faros AI is a vendor of engineering-productivity analytics, so its numbers measure its own customer dataset rather than the industry as a whole.

## The Takeaway You Can Walk Away With Right Now
In Spec-Driven Development (SDD), the bottleneck has moved from writing code to defining intent and verifying the outcome. Coding is not dying; the developer's value migrates from typing speed to how precisely they can say what they want and how rigorously they check it.

The author's analogy: you stop being a bricklayer and become an architect supervising construction. You produce the blueprint (the specification), commission the build to a team of AI agents, and sign off on technical acceptance. Code is one possible realization of the design, not the design itself.

The critical trap sits in one sentence: "all tests passed" is not the same as "matches my intent." Closing that gap is the new and harder work. The practical consequence is uncomfortable. If you save an hour generating code but cannot verify that it does exactly what you intended, you have not gained time. You have deferred the work into invisible debt that returns with interest.

> ❌ **Anti-pattern — Verification by green tests alone:** accepting generated code because the test suite passes. **Why people do it:** passing tests feel like proof, and the code looks plausible. **Consequence:** the code can diverge from intent without anyone noticing, and the cost returns later as debt. **Fix:** check the output against the specification you wrote, not only against the tests.

## Traditional Role vs. New Role
The center of gravity has moved from translating logic into code toward formulating intent. The author presents this as an observable shift, not a vendor slogan. Until recently, the main cost of engineering was converting business logic into syntax. Frontier models made first drafts of code cheap, so value moved elsewhere.

In the new arrangement you become an **intent author**, whose main output is a precise, machine-interpretable specification rather than code. The AI generates and implements the code. The most radical version is called spec-as-source: you edit only the specification, and the generated code is marked "do not edit manually." The author notes that most teams live between spec-first and that extreme.

Underneath is a philosophical inversion. For years the assumption was that code is the source of truth, the only trustworthy description of what a system does. SDD makes intent the source of truth and code its generated artifact. A good specification then functions as a contract that defines "correct" before anything is built.

The author also reports scale. The share of code co-authored by AI grew from a few percent in 2023 to tens of percent today, and more than half of professional engineers use AI tools daily. Sean Grove of OpenAI argued that writing code is about 10 to 20 percent of an engineer's value, and the remaining 80 to 90 percent is "structured communication," meaning organized articulation of intent. The author says the exact proportions can be argued, but the direction is hard to dispute: less code does not mean less work.

*Figure (caption only in the source):* The center of gravity of work shifts from writing code toward defining intent and accepting the outcome.

## Skills That Just Gained in Value
The author describes a polarization of competencies, not "the end of coding." Kent Beck said that ninety percent of his skills lost value and ten percent gained a thousandfold. The author calls that a deliberate exaggeration that still captures the asymmetry.

Rising in value:
- **Architectural thinking:** designing system boundaries and making decisions the agent will not make for you.
- **Requirements precision:** turning a vague "we want it to work" into an unambiguous, testable specification.
- **Verification and judgment** (Beck's word: "taste"): assessing whether an outcome is genuinely good, not just technically correct.
- **Context engineering:** giving the agent the right context at the right moment.
- **Agent orchestration:** coordinating several parallel instances working on different parts of a task.

### Context engineering
The author calls this the least obvious skill. An agent is only as good as the context you give it. Mechanically, you supply the specification, constraints, examples and relevant fragments of the existing system, in the right order and quantity, so the model reasons from the right material. Without this, the agent satisfies the letter of the prompt rather than your intent, and that is the source of most bugs caught in review.

> 🔎 Added context: "Context" here means everything the model reads at a call, including instructions, examples and retrieved code, not only the user's question.

Declining in value: boilerplate, manual translation of requirements into code, line-by-line debugging, and memorizing syntax and APIs. That knowledge does not become useless, since you cannot judge the agent's output without it. Raw fluency in typing it is simply no longer an edge.

The author's team rule is: don't commit code you can't explain.

> ❌ **Anti-pattern — Accepting what looks fine:** merging a few hundred lines that look reasonable and pass the tests. **Why people do it:** the volume is large and review feels like a formality. **Consequence:** code nobody on the team understands ends up in production. **Fix:** the author's rule, "don't commit code you can't explain."

*Figure (caption only in the source):* Competency map: where to invest, and what is no longer a competitive edge.

## What Stays, What Goes
The bottleneck has not disappeared; it has moved to verification. When code generation rises from hundreds to thousands of lines a day, writing stops being the constraint and review, integration and accountability take over. Simon Willison observed that the whole software lifecycle was designed on the assumption that producing several hundred lines takes a day, and that assumption no longer holds. The developer's job is still to ship code they have proven works. AI moved the proving to the center of the profession.

The author states one hard rule: whoever clicks "merge" owns the responsibility. A machine cannot be held accountable for an outage, so the more code AI generates, the more valuable the human act of taking the consequences becomes.

### Does AI actually speed us up?
The author argues the data looks contradictory only until you check the conditions. AI speeds up simple and greenfield tasks, and it can slow experienced engineers on large existing systems (brownfield). These are different conditions with different results, not the same conditions with opposite outcomes.

> 🔎 Added context: greenfield means building from scratch with no existing codebase to respect; brownfield means changing a large existing system.

The evidence the article gives:
- In a controlled experiment on a simple HTTP server, developers with an assistant finished faster (Peng et al., n=95, greenfield, arXiv:2302.06590).
- In a controlled field experiment across nearly 5,000 developers, merged pull requests grew by about 26%.
- METR studied 16 experienced open-source developers on their own large repositories (brownfield, millions of lines). They were 19% slower with AI, although they believed they were 20% faster. METR later revised that study because of a sample selection error, so the author treats it as one signal, not decisive proof.

Then the trust paradox: AI co-authors a significant share of production code, yet 76% of developers report frequent hallucinations and low confidence in AI code (Qodo, State of AI Code Quality 2025). The gap between the scale of delegation and the level of trust is where verification debt is born. Time saved writing returns as a tedious audit of someone else's code.

The author also corrects an earlier belief: that the specification is simply the new code, so writing good specs would dissolve the problem. Even Guy Podjarny, a prominent advocate of spec-centric work, shifted his narrative from "spec" toward "context" (Tessl podcast, episode 101). Natural language is too ambiguous to fully replace formal precision, and better models will not fix that, because the ambiguity is a property of language itself.

> ❌ **Anti-pattern — Spec as a cure-all:** believing that a good enough specification removes the need for verification. **Why people do it:** the spec feels like the contract that settles correctness. **Consequence:** natural-language ambiguity survives into the code and the review burden stays. **Fix:** plan verification as a permanent step, even with good specs.

The failure whose cost nobody budgeted for usually looks the same: AI-generated code ships to production without review by an experienced engineer, and hours later someone is counting losses. The author's conclusion is not "against AI" but "plan the process with verification time built in from the start," because saving on writing without verifying is an illusion.

*Figure (caption only in the source):* Same tools, different conditions, which is why productivity data seems contradictory until you look at context.

## How to Prepare Yourself and Your Team
Success depends less on whether you deploy the tool than on whether team members become **active integrators** rather than passive delegators. That is a leader's job, not a configuration task. You can mandate AI use; you cannot mandate trust or judgment. The difference decides whether AI builds the team's competencies or erodes them.

At the individual level the decision is: do you treat the agent as a thinking partner or as a generator you approve? The author cites a pilot study by Shen and Tamkin (Anthropic, 2026) with about fifty participants learning a new library. Developers who used AI to investigate "why" scored significantly higher on comprehension tests than those who delegated generation and clicked "accept." Active use builds understanding; passive delegation erodes it.

> ❌ **Anti-pattern — Passive delegation:** asking the agent for code and clicking "accept." **Why people do it:** it is the fastest path to a finished diff. **Consequence:** comprehension drops, and the developer cannot evaluate the next output. **Fix:** use the agent to explain the "why" behind the code before merging.

At the team level, the author recommends five deliberate levers:
- **Explicit AI use policy:** where autonomous delegation is allowed and where human review is required.
- **Mandatory review of critical and security-related code:** not a place for auto-accept.
- **Small batches and fast feedback loops:** catch an agent's mistake before it propagates.
- **Evaluating specification quality, not line counts:** measure what is now the real bottleneck.
- **Regular "no-AI days":** so the core skill set does not rust.

The author warns about a trap even well-run teams fall into: a top-down "use AI" mandate without cultural scaffolding can do more harm than good. Cat Hicks's research on what she calls AI Skill Threat, meaning stress and uncertainty about professional identity, affects 43 to 45 percent of engineers (a study of more than 3,200 developers). A culture of learning and belonging reduces that threat, while pressure based on competition and "innate talent" amplifies it. A leader who builds a learning culture lowers resistance; one who only applies pressure raises it.

The concrete Monday plan the author proposes: for yourself, the rule "I understand before I merge"; for the team, the five levers above.

> ❌ **Anti-pattern — Mandate without scaffolding:** ordering "use AI" with no training, norms or safe space to learn. **Why people do it:** adoption is easy to measure and mandate. **Consequence:** stress and identity threat rise, and resistance or passive use follows. **Fix:** pair the mandate with a learning culture and clear review rules.

*Figure (caption only in the source):* Two paths through the role change; only one builds active integrators.

## What This Means for Your Career
The article opens with Eric Hoffer's line: in times of change, learners inherit the earth, while the learned are beautifully equipped for a world that no longer exists (Reflections on the Human Condition, 1973).

The market is polarizing and the bar is rising, not falling. The number of simple junior roles at companies with deep AI adoption is shrinking, and the wage premium for AI and architecture skills is growing. Some market forecasts still show net employment in the industry growing. Both signals can be true depending on which layer of the market you look at.

The author names the psychological cost directly. Moving from a creator who "shapes code like clay" to a reviewer of someone else's work strikes at an identity built on craftsmanship. Anxiety about one's role is a normal response to real change, not a sign of weakness, and silence on the subject deepens it.

History gives a reality check. Each layer of abstraction arrived with the prophecy that it would eliminate the developer role, and each time the prophecy was wrong. Compilers were supposed to let business analysts write programs without specialists. Fourth-generation languages and CASE tools promised application generation from specifications. DevOps was supposed to eliminate the database administrator, who became a platform engineer instead. The pattern: a lower barrier to entry increases the volume of software, which increases demand for more advanced engineers. SDD, in the author's view, is the next layer, not the first "end."

The winners are not those who mastered the old craft best, but those who treat the new role as learning from scratch. Clinging to "I am the one who writes the fastest code" is a poor identity for what is coming. Adopting the identity of an intent author and verifier means inheriting the future.

*Figure (caption only in the source):* Every abstraction layer foretold the end of developers, and each time it raised the bar instead.

## Summary
The difference between the 2019 developer and today's is not that one works less. Their value has shifted from coding speed to precision of intent and rigor of verification. The author reduces the whole thesis to that sentence.

The five concrete actions:
1. Rising in value: architecture, requirements precision, judgment and orchestration. Declining: boilerplate, line-by-line debugging and syntax memorization.
2. The bottleneck has shifted, not disappeared. "Tests passed" is not "matches intent," and savings without verification are debt, not gain.
3. Build verification time into estimates from the start, rather than treating code generation as the end of the work.
4. As a leader, build a learning culture, not just a mandate. The author says active integrators outperform passive delegators on every metric that matters.
5. The profession is not ending. The bar is rising, and historical precedent supports that.

The author closes with the reminder that the work of an intent author is harder than that of an executor but also more interesting. Less time goes to typing what you already knew how to write, and more to what requires thinking: what we want to build, and how we prove we built it.

## Final Note
The author asks readers to share one change they have noticed in their own daily work, and to send the piece to a teammate who is anxious about the change. The message repeats: the bar is rising, and that is good news for people who keep learning.

## 🧠 Mental models
### Intent author and construction oversight
**The model:** You are the architect who writes the blueprint, not the bricklayer. Your output is a specification, and AI agents build the code under your supervision.
**How to apply it:** Before generating code, write down what "correct" means as a testable contract. At acceptance, check the build against that contract, not only against the test suite.

### Verification debt
**The model:** Time saved on writing code that is not verified is not saved; it is deferred into invisible debt that returns with interest.
**How to apply it:** Estimate verification time as a line item from the start. If you cannot verify a generated change, count it as unfinished.

### Whoever clicks merge owns it
**The model:** Accountability cannot be delegated to a machine. The person who merges is responsible for the outcome.
**How to apply it:** Require a human reviewer who can explain critical or security-related changes before they merge.

### Context, not just specs
**The model:** The agent's output is only as good as the context it receives: specification, constraints, examples and relevant system fragments, in the right order and quantity.
**How to apply it:** When an agent misses intent, first check what context it was given before rewriting the prompt.

## 🔑 Key takeaways
- The bottleneck in SDD has moved from writing code to defining intent and verifying outcomes. "All tests passed" does not prove the code matches intent.
- Teams with high AI adoption saw pull request review time grow by nearly 91% (Faros AI, more than 10,000 developers and 1,255 teams), so review capacity becomes the constraint.
- The productivity evidence depends on context: AI sped up greenfield tasks in controlled experiments, while METR found experienced developers 19% slower on large brownfield repositories, though they believed they were faster.
- 76% of developers report frequent hallucinations and low confidence in AI code (Qodo, 2025), and this trust gap is where verification debt forms.
- Active use of AI to understand code builds comprehension; passive acceptance erodes it (Shen and Tamkin pilot, about 50 participants).
- A learning culture lowers AI Skill Threat, which affects 43 to 45 percent of engineers according to Cat Hicks's research, while competitive pressure raises it.

## 🏛️ For Kenji
*This is your interpretation, not the author's.* For a Staff Architect path, the article points to a role where you define system boundaries, requirements contracts and acceptance criteria that agents then realize, and where your governance background matters because "tests passed" is not evidence of intent. The question it raises for your work: which architecture decisions in your domain need a written, verifiable intent contract before any agent touches them, and who owns acceptance when several agents build in parallel?

## 📚 Reading plan
### From the article
- The article text contains no hyperlinks. Its sources are named in the "To go further" list below, and the only identifier given is arXiv:2302.06590.

### To go further
- Peng et al., controlled HTTP server experiment (arXiv:2302.06590) — the greenfield productivity evidence the article leans on.
- METR study of experienced open-source developers on large repositories — the brownfield counter-evidence, read with the author's note about the later sample-selection revision.
- Shen and Tamkin, Anthropic Research pilot on learning a new library with AI — the "investigate why" vs "accept" comprehension result.
- Qodo, State of AI Code Quality 2025 — the 76% hallucination and low-trust figure.
- Faros AI productivity data (more than 10,000 developers) — the 91% review-time growth.
- Cat Hicks, research on AI Skill Threat — the identity and learning-culture argument for leaders.
- Guy Podjarny, Tessl podcast episode 101 — the shift from "spec" to "context."
- Sean Grove (OpenAI), on structured communication as most of an engineer's value — the intent-as-source argument.
- Eric Hoffer, Reflections on the Human Condition (1973) — the closing quotation on learners and change.

## Concepts
- [spec-driven-development](/concepts/software-engineering/spec-driven-development.md)
- [context-engineering](/concepts/ai-ml/context-engineering.md)
- [ai-agent](/concepts/ai-ml/ai-agent.md)
- [multi-agent-orchestration](/concepts/ai-ml/multi-agent-orchestration.md)
- [verification-tax](/concepts/software-engineering/verification-tax.md)
- [technical-debt](/concepts/software-engineering/technical-debt.md)
- [version-control-code-review](/concepts/software-engineering/version-control-code-review.md)
- [software-engineering](/concepts/software-engineering/software-engineering.md)
