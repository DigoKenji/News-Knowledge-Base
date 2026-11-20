---
type: Article Summary
title: Prompting Is Now the Slowest, Buggiest Way to Code with AI. Here's the Fast, Clean One
description: Prompts cannot recover hidden business rules; AI coding needs explicit, context-dependent specifications and
  independent verification to deliver reliable software.
resource: https://medium.com/ai-advances/the-top-1-of-ai-coders-barely-prompt-they-do-this-instead-0015703c6064
tags:
- software-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/ai-advances/the-top-1-of-ai-coders-barely-prompt-they-do-this-instead-0015703c6064
  title: Prompting Is Now the Slowest, Buggiest Way to Code with AI. Here's the Fast, Clean One
  author: AI Advances
status: stable
source_url: https://medium.com/ai-advances/the-top-1-of-ai-coders-barely-prompt-they-do-this-instead-0015703c6064
source: Medium (AI Advances)
source_type: medium
published: '2026-10-30'
domain: software-engineering
concepts: []
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81ae803ec99e26c14f1a
---

## 🎯 Core thesis
Prompts alone cannot make AI-written software correct, because the right implementation depends on business context the prompt does not carry, so that context has to be made explicit and checked independently.

## 🧭 Why the author wrote it
The article starts from the corporate fantasy of the "10x developer" and the promise that AI will let one engineer replace a whole team. The author argues the evidence shows AI speeds up code generation without reliably producing valuable, correct software. If he is right, the fix is engineering discipline (explicit requirements plus independent verification), not cleverer prompts.

## Introduction: the mythical 10x developer
> 🔎 Added context: the captured text has no heading before this opening, so the title above is a descriptive label I gave it, not the author's.

### Productivity as a headcount argument
The author describes cost-cutting pressure in which management asks for more output from fewer developers. He points to the "mythical 10x developer" as the figure executives reach for. He also notes that Frederick P. Brooks Jr.'s *The Mythical Man-Month* is often reduced to the claim that adding developers only slows projects, which he treats as a distortion of the book.

### Coordination cost
Adding developers helps when their tasks are independent, because output grows. When they work on tightly coupled code, coordination costs can consume the gain. Too few developers and the project crawls while costs accumulate. The author's conclusion is that there is no magic team size, and a wrong choice wastes months before anyone notices. He cites Microsoft research showing that organizational structure and coordination affect software quality, but the captured text gives no figures.

## Is AI Making This Bloody Mess Even Worse?
### The one-architect, many-agents vision
The author describes the executive dream: one developer directing a fleet of AI coding agents under a single architecture, with the output of a whole team and no coordination overhead. He accepts that this is the promise and then argues that reality does not match it.

### AI inherits the coordination problem
His claim is that AI agents create the same coordination mess as human developers, only faster. He cites Google's DORA research as showing that AI amplifies both the strengths and weaknesses of existing engineering practices. His summary is blunt: give AI a broken engineering process and you get the same disaster at machine speed.

## The AI Coding Productivity Disaster: Just 2% Report 10x Gains
### Self-reported productivity
The article says only around 2% of developers reported gains of 10x or more, which means about 98% did not report reaching that level. The author stresses that even this 2% is self-reported and not measured as increased delivered software. Generating a lot of code and delivering valuable software are different things, and the self-reported figures measure the first.

> ❌ **Anti-pattern — Judging productivity by self-report:** Taking developers' estimates of their own speed as evidence of delivered value. **Why people do it:** It is cheap to collect and flattering to report. **Consequence:** Gains look large while the software that reaches users does not change. **Fix:** Measure delivered, working output (releases, accepted features, defects) rather than activity.

### The productivity paradox: commits versus releases
The article's chart shows 240% more commits but only 30% more releases. Code production rises far faster than delivery. The author calls this the AI productivity paradox: mountains of code without matching gains in shipped software. Getting software into production still does not guarantee it is worth anything, because the market decides.

### Generated software and the market
A second chart shows seven times more apps reaching the market, while the typical subscription app earns $72 a month after its first year. The author is careful here: these figures do not prove that AI causes commercial failure. They show that making software cheap to generate does not make it reliable, useful or profitable.

### The mind-reading assistant
The author says many developers imagine a "digital homunculus" inside the machine that reads their minds and understands what they meant. He asks readers to drop that picture. AI cannot reliably recover business rules or hidden requirements it has never been given, and his line for this is: you can't prompt your way out of a broken specification.

> ❌ **Anti-pattern — The mind-reading assistant:** Assuming the AI infers what you meant from a short request. **Why people do it:** Fluent, plausible code looks like understanding. **Consequence:** The code satisfies the prompt but not the rules nobody wrote down. **Fix:** Write the requirements, constraints and invariants where the agent and the checks can read them.

## The Science Behind the Top 1%: Stop Prompting, Start Engineering
### Engineering versus estimating
The author claims the top 1% of AI developers do not estimate their productivity; they engineer it. The article supplies this as a contrast and gives no measurement of that group. Serious gains, in his framing, come from understanding what AI cannot know, what it can only guess, and what you can force it to get right.

### Magic prompts and prompting frameworks
He sets aside "magic prompts", cure-all prompting frameworks and tutorials that promise overnight 10x results.

> ❌ **Anti-pattern — Prompting as the strategy:** Treating prompt templates and prompting frameworks as the main route to professional software. **Why people do it:** Prompts are quick to try and easy to share. **Consequence:** Output varies with context that is never stated, and nobody can check it against requirements. **Fix:** Use the prompt as one preliminary step inside a specified, verified engineering process.

## The Hidden-Context Trap: Your AI Doesn't Know What You Forgot to Tell It
### The refund example
The author's toy example is a refund function for a payment system. The AI returns sensible code with error handling and a few tests, and everything passes. Then a customer is refunded twice. The assistant was never told that the same payment must not be refunded twice, and nothing was said about partial refunds, concurrent requests, or a network drop in the middle of a financial operation. He calls this missing semantic context: the business rules and dependencies that decide whether code is correct.

### Token models and hidden semantics
The author accepts that today's token-based generative models can reason about context. His point is that they cannot reliably reconstruct or verify the hidden semantics that professional software depends on. The implementation you need depends on more than the prompt.

```javascript
y = F(x, z)
```

**What this does:**
- `x` is the prompt, the natural-language request given to the AI.
- `z` is the relevant context: business rules, invariants and dependencies that the prompt may not carry.
- `y` is the implementation the business actually needs, so correctness is defined by the pair (x, z), not by x alone.
- The notation is the author's model, not executable code. The `javascript` tag matches the source fence.

### Same prompt, different correct answers
Two payment systems receive the same refund prompt. One permits multiple partial refunds and the other allows a single refund per transaction. Both need different implementations.

```javascript
F(x, z₁) ≠ F(x, z₂)
```

**What this does:**
- `x` is the same prompt in both cases, while `z₁` and `z₂` are two different business contexts.
- The inequality says the two correct implementations differ, although the request is identical.
- An AI given only `x` has no basis for choosing the right one.
- The author uses this to justify making `z` explicit and checkable.

### Nested semantic contexts
The author extends the point to commercial software involving insurance premiums, bank transfers, customer contracts and warehouse inventory. There, business rules depend on regulation, database state and exceptions buried several layers deep. One rule can change the meaning of another. Some rules are in code or documents, and others live in the heads of people who understand the business. Until those rules are dug out and written as explicit requirements, AI agents repeat the same mistakes.

## The Most Expensive AI Coding Mistake: Letting AI Judge Your Business Requirements
### The self-certifying pipeline
The author describes the failure mode: AI writes the code, generates the tests, reviews its own work and reports that everything passes. If the developer misunderstood or forgot a requirement, every test can be green while the software is wrong. The author notes that this is a toy example, but the same principles apply to real systems.

> ❌ **Anti-pattern — Self-certified correctness:** Letting the same AI write the code, the tests and the review, then accepting the green result as proof. **Why people do it:** Green tests look like completion, and the loop is fast. **Consequence:** A consistent but wrong system, in which every test passes except the one that mattered. **Fix:** Keep the requirement owner separate from the generator, and verify the output independently against rules that have been written down and reviewed.

### Who decides what correct means
The author's rule is: let the AI write the code, but make sure someone other than the machine decides what correct software means. Rules that exist only in code, documents or people's memories must be extracted and turned into explicit requirements before agents can be trusted with them.

## Transformative vs. Transportive AI: Stop Guessing What You Can Preserve
The author says he developed the distinction in earlier work, after struggling with AI coding on real projects. Transformative AI produces code from learned patterns and whatever context it can reach, often guessing at relationships outside that reach. Transportive AI carries meaning explicitly through the whole development process. He says it goes beyond cleverer prompts.

### Same Token, Different Meaning
A variable's meaning depends on where it appears, which binding it refers to and the language's rules. The same identifier can mean different things inside a closure, a function, a class or a module. Compilers resolve these relationships precisely, while AI assistants with incomplete context may have to guess what the compiler already knows. The first principle of transportive AI follows: carry the semantic information the programming system already has, instead of rebuilding it from tokens.

> 🔎 Added context: a binding is the declaration a name refers to at a given point in the code. Language tools use scope rules to decide which binding each use refers to.

### Same Concept, Different Business Geometry
The author moves from programming-language semantics to business rules and uses an analogy comparing two legal systems. The same offense may lead to similar outcomes in one system and very different ones in another, because layers of law and religion change the result. Software works the same way: one contract gives a full refund and another gives only part of it. Both implementations are valid, and neither can be swapped for the other.

The author names two cases. In contextual agreement, two contexts give the same result for an operation. In contextual divergence, they give different results. He writes I(o,c) for what an operation o means under a context C, so two contexts can agree on one rule and disagree on another. The formulas appear as images that the source text does not include, so this note uses only the terms the text gives. The author's point is that geometry helps visualize these differences, but the engineering work is making them explicit rules that software can check.

## Stop Burying Business Rules in Prompts. Give Them a Structure.
The practical question is how to deal with nested semantic contexts when building commercial software with AI. Explaining the same rules to every agent that touches the code, APIs or database is expensive and error-prone. The author proposes a semiformal specification that engineering tools can understand and check.

### Semiformal specification
The author maps the geometric concepts from earlier to software counterparts, in a table the captured text does not include. The aim is a structure that AI coding tools can work with, with the business rules, the contexts where they apply, and the constraints that must hold everywhere.

## How Your YAML Represents the Geometry
### The refund specification in YAML
The author gives a simplified YAML specification for the refund operation. The figure is not in the captured text. The example uses it to show that one contract allows a full refund, another allows only a prorated one, and neither permits duplicate execution. Different contexts produce different behavior, and some rules hold in both.

### A file is not enforcement
Writing the rules in YAML does not make the software obey them. Someone must define precisely what each rule means, and a context engine must reject missing or conflicting requirements instead of letting AI guess. Independent checks are still needed to catch an implementation that breaks a rule.

> ❌ **Anti-pattern — Declaring the rule in a file:** Writing `duplicate_execution: Forbidden` and assuming the software now respects it. **Why people do it:** A declarative file feels like a control. **Consequence:** Nothing stops an implementation from breaking the rule, and the refund still happens twice. **Fix:** Have a context engine reject missing or conflicting requirements, and run independent checks against the implementation.

### The context engine
The context engine's job is to work out which rules apply in a given context, how they interact, and whether anything is missing or contradictory. Its output feeds the code generation step and gives the checks a precise target.

## From Semantic Geometry to a Working AI Coding Machine
### The machine
The author's full pipeline runs in four steps. You write a semiformal specification of business rules and their contexts. The context engine decides which rules matter and flags gaps or contradictions. AI agents generate the code. Independent verification checks whether the result satisfies the requirements. The diagram is not in the captured text, but its caption gives the same sequence.

### What a passing check means
The author warns against reading the machine as a guarantee. If the code passes, it has met the checks for the business rules that were specified, and nothing more. The system also records which rules were used, which context applied and which checks passed. When requirements change, that record shows what to review again.

### Transformations between contexts
Each context C has its own semantic specification Sc, and moving between contexts requires a transformation. When several contexts are involved, the transformations must follow defined composition rules. Some rules survive the move, others change, and incompatible contexts may make the transformation impossible. These formulas are also images that the captured text does not include. The author's conclusion is that the engineer moves from describing contextual meaning to computing with it.

## 🧠 Mental models
### Coordination cost follows coupling
**The model:** Adding people or agents helps when work is independent and hurts when work is tightly coupled, so coordination cost grows with coupling rather than headcount.
**How to apply it:** Before you add agents to a codebase, check how many modules share state and interfaces. Split work along seams that do not change each other.

### The prompt is not the specification
**The model:** The correct implementation is a function of the prompt and the context, y = F(x, z), and the same prompt under different contexts has different correct answers.
**How to apply it:** When a request is ambiguous, write down the context it depends on, such as limits, invariants and contracts, before asking for code.

### Contextual agreement and divergence
**The model:** An operation's meaning depends on the context it is evaluated in, so two contexts may agree on one rule and disagree on another.
**How to apply it:** Compare the rules across the contracts or markets you serve, and mark each rule as shared or context-specific before you build a single implementation.

### Do not let the generator grade itself
**The model:** The entity that writes the code must not be the only judge of whether the code meets the requirement.
**How to apply it:** Write requirements and acceptance checks in a form that a human has reviewed, and run them against output the generator did not write.

### Transportive, not transformative
**The model:** Carry the meaning that tools and specifications already hold through the process, instead of asking the model to rebuild it from tokens each time.
**How to apply it:** Feed agents the compiler's resolution results, the rule specification and the context identifiers, and check the outputs against them.

## 🔑 Key takeaways
- Only around 2% of developers reported 10x productivity gains, and the author notes even that figure is self-reported rather than measured as delivered software.
- The article's chart shows 240% more commits but only 30% more releases, which the author calls the AI productivity paradox.
- The typical subscription app earns $72 a month after its first year, while the market has seven times more apps. The author says this does not prove AI causes commercial failure.
- The same refund prompt has different correct implementations under different contexts, shown as F(x, z₁) ≠ F(x, z₂), so the business context must be part of the input.
- An AI that writes the code, the tests and the review can certify a wrong system, so requirements must be owned and checked by someone other than the generator.
- Business rules should live in a semiformal specification that a context engine validates for gaps and conflicts, and that independent checks test against the implementation.
- A passing check means only that the specified rules were met; the system should record which rules, contexts and checks applied so that changes can be traced.

## 🏛️ For Kenji
> This is your interpretation, not the author's.

For an architect, the article turns business rules into a first-class artifact that AI agents must consume and that verification must test, which fits architecture governance. Refund-like rules that differ by contract or jurisdiction would be specified per context, with the record of which context applied. A question it raises: in your systems, which business rules currently exist only in people's heads or in legacy code, and what would it take to write them down where an agent and a check can read them?

## 📚 Reading plan
### From the article
- None. The captured text contains no links. It names Brooks's *The Mythical Man-Month* (1975), Microsoft's research on organizational structure and software quality, and Google's DORA research without URLs.

### To go further
- Frederick P. Brooks Jr., *The Mythical Man-Month* — the source of the coordination argument and the surgical-team model the author revisits.
- Google's DORA research reports — the author's basis for saying AI amplifies existing engineering strengths and weaknesses.
- Microsoft research on organizational structure and software quality — the article cites it for coordination effects, but gives no title.
