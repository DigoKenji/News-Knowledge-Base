---
type: Article Summary
title: 'QCon San Francisco 2026: Scaling Tech by Aligning Humans'
description: Staff-plus engineers can scale impact by aligning people around a short technical strategy, a north star and
  explicit bets.
resource: https://www.infoq.com/news/2026/11/scalling-tech-humans
tags:
- architecture
- architecture-governance
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/scalling-tech-humans
  title: 'QCon San Francisco 2026: Scaling Tech by Aligning Humans'
  author: Daniel Dominguez
status: stable
source_url: https://www.infoq.com/news/2026/11/scalling-tech-humans
source: InfoQ
source_type: blog
published: '2026-11-21'
domain: architecture
subdomain: architecture-governance
concepts: []
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d815c8baac9e9cc093e1d
foundation_candidates:
- technical strategy
- north star
- strategy bet
---

## 🎯 Core thesis
A senior engineer can scale impact by aligning people around a short technical strategy, a north star and a few explicit bets, and this can reach further than any code one engineer writes.

## 🧭 Why the author wrote it
Several product strategies began to share similar technical risks, and without a common technical strategy beneath them, teams optimized locally for different goals. If the thesis is right, a lightweight technical strategy built with people gives those teams one direction faster than a long technical document would.

## Opening: strategy as leverage
At QCon San Francisco 2026, Scott Behrens, Principal Security Engineer at Netflix, presented how staff-plus engineers can define and align a company-wide technical strategy, drawing on Netflix's experience scaling internal AI agents. The core claim is that a strategy which brings people together can often scale impact further than any code an engineer could write alone.

### Staff-plus engineer
In the talk, the staff-plus engineer finds the problem, writes the pitch and leads the team that builds the strategy. > 🔎 Added context: "Staff-plus" is an industry title for senior engineers who influence beyond their own team without managing people; the article does not define it.

### Technical strategy
In Behrens's words, a strategy is simply a set of choices. The article's key point is that a technical strategy can complement a product strategy without waiting for one, so the two run side by side. Behrens argued it should be lightweight, because lightweight strategies deliver faster than long technical documents. Its purpose is to give several teams a shared direction for the technical risks they all face.

## Why the work started
Several product strategies, each with its own direction, began to share similar technical risks. Without a common technical strategy underneath them, teams optimized locally for different goals. Behrens drew three conclusions: a technical strategy can complement product strategy without waiting for it, lightweight strategies deliver faster than long technical documents, and a clear north star does more steering than people expect.

> ❌ **Anti-pattern — Local optimization:** Each team pursues its own direction while sharing technical risk with others. **Why people do it:** Each team is measured on its own goals, so local optimization looks rational. **Consequence:** Teams work at cross purposes on the same risk. **Fix:** Agree a common technical strategy beneath the product strategies, as the talk recommends.
> 🔎 Added context: The causes and consequences in this note's anti-pattern blocks are my reading, since the talk named the failure modes without explaining them.

## The playbook: finding the right problem
Behrens presented the playbook as levels in a video game, and the first level is finding the right problem. He suggested looking at company communications, incident trends, technical debt, conversations with other tech leads and gaps between team charters. The problem is then tested on three questions: is it worth solving now, is it foundational or compounding, and is this the right person to lead it?

### Foundational or compounding problem
The article asks whether a problem is foundational or compounding but does not define either term. > 🔎 Added context: In my reading, a foundational problem is one that other work depends on, and a compounding problem grows more costly the longer it waits. The distinction is used to decide whether a problem deserves strategy effort now.

> 📋 **Checklist — Sources for finding the right problem**
> - [ ] Company communications
> - [ ] Incident trends
> - [ ] Technical debt
> - [ ] Conversations with other tech leads
> - [ ] Gaps between team charters

## The pitch
Once the problem is chosen, the next step is a short problem statement that explains the intent, the expected value, the problems with supporting evidence, why the work matters now and dated next steps. Behrens suggested leaving possible approaches open so the people who join can shape them, and testing the statement early with a few colleagues, including the likeliest dissenter.

### Problem statement
A problem statement is a short written pitch whose job is to bring people into the work. Its parts are intent, expected value, the problems with evidence, timing and dated next steps. Leaving approaches open invites others to shape the work instead of joining a finished plan. Testing it early with the likeliest dissenter exposes weak evidence before the wider group sees it.

> 📋 **Checklist — Problem statement contents**
> - [ ] Intent
> - [ ] Expected value
> - [ ] Problems, with supporting evidence
> - [ ] Why the work matters now
> - [ ] Dated next steps

> ❌ **Anti-pattern — Unclear problem statement:** A pitch that leaves out intent, expected value or evidence. **Why people do it:** A pitch can feel finished before the reasons behind it are written down. **Consequence:** People cannot tell whether to join. **Fix:** Use the five parts above and test the statement early.

## The tiger team
With a team to form, Behrens described a small tiger team whose job is to create the strategy rather than execute it. Its members should mix perspectives, be comfortable with ambiguity, have manager support, and agree on scope, timeline and a clear stop condition from the start. His own first meeting had far too many people and still missed key functions. He emphasized that the most important trait of a good team is making it safe to push back.

### Tiger team
The tiger team is small and exists to create the strategy, so its members do not carry it out. The stop condition, agreed at the start, defines when the team's work ends. Mixing perspectives matters because the early questions have no settled answers, which is why the article also values people who tolerate ambiguity.

> 📋 **Checklist — Setting up the tiger team**
> - [ ] Mix perspectives
> - [ ] Choose people comfortable with ambiguity
> - [ ] Get manager support
> - [ ] Agree scope, timeline and a clear stop condition from the start

> ❌ **Anti-pattern — Oversized first meeting:** Inviting far too many people and still missing key functions, as Behrens's first meeting did. **Why people do it:** A wide invitation feels safer than choosing who matters. **Consequence:** The group is slow to decide and key functions are still absent. **Fix:** Invite a small group that covers the key functions.

## The north star
With a team in place, the next step is a north star: a short aspirational statement that gives direction and helps answer whether a piece of work moves the organization closer to or farther from its goal. Behrens noted that a good north star is harder to write than it looks, so he recommended drafting it with the team and pairing it with key outcomes that define what must be true.

### North star
The north star is short and aspirational, so people can remember it and apply it to new work. Its test is whether a piece of work moves the organization closer to or farther from the goal. The key outcomes paired with it state what must be true for the north star to hold.

> ❌ **Anti-pattern — Vague north star:** A statement too general to answer whether a piece of work helps. **Why people do it:** Writing a sharp one is harder than it looks. **Consequence:** Decisions are not steered. **Fix:** Draft it with the team and pair it with key outcomes.

## Strategy bets
Behrens described the strategy as a set of choices, and each choice as a bet. He suggested writing a bet as a choice against another reasonable choice, naming the tradeoff explicitly, keeping each bet short and revisiting bets over time. If a bet does not feel a little uncomfortable, he said, it is probably not a real bet, and keeping only a small set of active bets keeps the strategy focused.

### Strategy bet
A strategy bet is a choice made against a specific alternative, which is why the tradeoff must be named. The article asks for short bets so they can be read quickly and revisited as the work changes. A bet that feels comfortable has usually not named a real tradeoff.

> ❌ **Anti-pattern — Bets that are not really bets:** Choices with no real alternative, so no tradeoff is named and nothing feels uncomfortable. **Why people do it:** Comfortable choices are easier to agree on. **Consequence:** The strategy has no real direction. **Fix:** Name the alternative and the tradeoff, and rewrite any bet that feels comfortable.

## Communication and feedback
Behrens suggested expanding support gradually from the immediate team to managers, directors and executives, recruiting champions across the organization and openly showing how feedback changed the strategy. He defined success as the moment when you are not in the room and people can still explain the work. He also reviewed common failure modes: unclear problem statements, the wrong team, vague north stars, bets that are not really bets, and strategies that nobody knows exist.

### Champion
The article recommends recruiting champions across the organization without defining the role. > 🔎 Added context: In my reading, a champion is a respected person in another part of the company who carries the strategy into their own discussions, which lets it spread beyond the people who wrote it.

> ❌ **Anti-pattern — Strategy nobody knows exists:** A strategy kept within the team that wrote it, so others cannot explain or act on it. **Why people do it:** Writing for the team feels sufficient at the start. **Consequence:** Nobody outside the team can use the strategy. **Fix:** Expand support gradually and openly show how feedback changed the strategy.

## Closing: keep it simple
The talk ended by urging engineers to keep strategy simple, communicate it widely, use a short north star to steer decisions and adapt the playbook to their own context. Behrens suggested that as organizations launch more initiatives in parallel, especially around AI, the ability of senior engineers to align many teams around one technical direction may become as valuable as the systems they build.

## 🧠 Mental models
### Playbook as ordered levels
**The model:** Behrens framed the work as levels in a video game, taken in order, starting with finding the right problem and then writing the pitch. **How to apply:** Do not write the pitch until the problem has passed the three questions of the first level.

### Success is when you are not in the room
**The model:** The strategy has worked when people who were not part of the strategy work can still explain it. **How to apply:** Test this by asking someone outside the team to explain the north star and the active bets back to you.

### A bet should feel a little uncomfortable
**The model:** If a bet does not feel a little uncomfortable, it probably does not name a real tradeoff. **How to apply:** For each bet, write the reasonable alternative you rejected; if you cannot name one, rewrite the bet.

## 🔑 Key takeaways
- A short technical strategy can scale a senior engineer's impact beyond any code they write, the talk argued.
- Technical strategy can complement product strategy without waiting for one to exist.
- Behrens argued that lightweight strategies deliver faster than long technical documents.
- The pitch should state intent, expected value, evidence, why the work matters now and dated next steps, and leave approaches open.
- The tiger team creates the strategy rather than executing it, with a stop condition agreed from the start and a rule that pushing back is safe.
- Keep only a small set of active bets, each naming its tradeoff, and steer decisions with a short north star.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Business and Technology. Business covers product strategies and how teams are aligned; Technology covers the shared technical risks and the technical strategy. Data and Application are not covered; AI agents appear only as background to Netflix's work. |
| Scope | Enterprise: the talk frames the work as a company-wide technical strategy, although it starts with one small team. |
| Stakeholders and concerns | Staff-plus engineers, tech leads, product teams, managers, directors and executives. Concerns stated or implied: teams optimizing for different goals, unclear problems, choosing the right people to lead, and whether the strategy is understood beyond its authors. |
| Frameworks and notations | none named |

### Decisions and trade-offs
- **Technical strategy alongside product strategy:** The author recommends writing it without waiting for the product strategy. The article names no cost.
- **Lightweight strategy over long technical documents:** The author recommends the short form, which he says delivers faster. The article names no cost.
- **Small tiger team that creates the strategy:** The author recommends a small team with a stop condition. The article names no cost; the implied trade-off is that team members are not executing during this work.
- **Open approaches in the pitch:** The author recommends leaving approaches open for joiners to shape. The article names no cost; the implied trade-off is that the pitch commits to less.
- **A small set of active bets:** The author recommends keeping few active bets. The article names no cost; the implied trade-off is that work outside the active bets gets less direction.

### Foundations it rests on
- [Business Architecture](/foundations/business-architecture.md) — The article relates a technical strategy to product strategies, which stretches the definition's focus on strategies and products to a technical strategy running beside them.
- [Enterprise Architecture](/foundations/enterprise-architecture.md) — The talk aims to steer change across teams toward one direction, but it covers no processes or information, which the definition also includes.
- [Architecture](/foundations/architecture.md) — The article's "strategy" means a set of choices about direction, not the system properties or design principles the definition describes.

## 🏛️ For Kenji
This is your interpretation, not the author's. For a Staff Architect, the talk is a practical account of shaping technical direction across teams without relying only on code, and its focus on explicit tradeoffs is close to the strategy work in an MBA. Question: when several teams already run their own strategies, which shared technical risk would you put forward as the first bet, and what reasonable alternative would you name against it?

## 📚 Reading plan
### From the article
The article cites no links, so there is nothing to follow from it.
### To go further
- TOGAF 9.2 business architecture — to compare the talk's technical strategy with TOGAF's wider view of strategies and products.
- Netflix's internal AI agent work — the experience the talk drew on, although the article names no source.
