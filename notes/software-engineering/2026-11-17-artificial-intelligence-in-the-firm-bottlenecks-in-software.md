---
type: Paper Summary
title: 'Artificial Intelligence in the Firm: Bottlenecks in Software Production'
description: AI coding agents raised code output but software output rose only 13.7%, with code review the bottleneck.
resource: https://fion.ac/jellyfish.pdf
tags:
- software-engineering
- engineering-leadership
- llm-as-a-judge
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://fion.ac/jellyfish.pdf
  title: 'Artificial Intelligence in the Firm: Bottlenecks in Software Production'
  author: Fiona Chen, James Stratton
status: stable
source_url: https://fion.ac/jellyfish.pdf
source: Harvard job market paper (forwarded)
source_type: arxiv
published: '2026-11-14'
domain: software-engineering
subdomain: engineering-leadership
concepts:
- llm-as-a-judge
selected_by: forwarded
content: handoff
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81548bd3f03f417d3791
---

# Artificial Intelligence in the Firm: Bottlenecks in Software Production

**Source:** https://fion.ac/jellyfish.pdf
**Author:** Fiona Chen, James Stratton
**Date:** 2026-11-14
**Concepts:** [LLM-as-a-judge](/concepts/llm-as-a-judge.md)

---

## 🎯 In one sentence
After firms adopted AI coding agents, commits rose 36.0% and pull requests opened rose 35.7%, but Jira issues resolved rose only 13.7% (marginally insignificant), because code review became the bottleneck: the time from review submission to approval rose 56.1%.

## ❓ The problem
Software teams now use AI coding assistants, which suggest code as the developer types, and AI coding agents, which carry out longer multi-step coding tasks. These tools clearly increase the amount of code written, but shipping a feature requires more steps than writing: planning, human review, testing and deployment. A step that cannot speed up, or that gets harder when AI output arrives, limits the output of the whole line, and the paper calls such a step a "bottleneck." Finding where the bottleneck sits is hard because it requires highly granular data on a firm's internal process, which most firm-level datasets do not have. In this paper, a pull request (PR) is a submitted set of code changes waiting for review, "merged" means approved and added to the shared codebase, and a Jira issue counts as resolved only once its code is deployed to production.

## 💡 The idea
Software output is limited by the slower of two complementary steps: writing code and reviewing it. The authors model these steps as used in fixed proportion, which economists call Leontief production, so a coding speedup adds nothing to shipped output once review is saturated. Faster coding builds a queue of unreviewed code, and the wait grows with the queue, while the review itself takes the same time per change. Review cannot be staffed freely either, because reviewers mostly work on code they already know and senior approval is often required for complex or AI-written changes. The paper's central evidence is the queue appearing after agent adoption, while the output gain stays small.

## ⚙️ How it works
### Data and treatment timing
The data come from Jellyfish, an analytics platform that firms use to monitor engineering teams. It records 445 million work events from GitHub (commits and PRs), Jira (issues and epics), Google Calendar (for a subset of 192 firms) and AI tool usage, covering 720,542 workers at 655 firms. Adoption of assistants is the earliest seat activation in the GitHub Copilot or Cursor APIs. Adoption of agents is the earliest signal from the Claude Code API or from GitHub bot accounts and commit or PR signatures. Adoption timing varies across firms because procurement steps (vendor evaluation, budget, legal, security, pilots) take different lengths, and the authors argue this variation is plausibly unrelated to firm growth.

### Staggered difference-in-differences
The design compares firms that have adopted with firms that have not yet adopted, with firm and month fixed effects and controls for baseline size and months since onboarding:

```text
Activity_{f,m} = γ_f + δ_m + Σ_{k≠−1} β_k · AI^k_{f,m} + θ_m · X_{f,m} + ε_{f,m}
```

Here f is a firm and m is a month. Activity is an outcome per worker, such as lines of code, commits, PRs, or Jira issues. γ_f is a firm fixed effect that absorbs stable firm traits, and δ_m is a month fixed effect that absorbs shocks common to all firms. AI^k_{f,m} equals 1 when month m is k months after the firm's adoption month, so β_k is the effect k months after adoption, and the paper reports k = 12. Standard errors are clustered by firm.

### Structural model of production
The model has two stages, coding and review, with output set by the scarcer one:

```text
Y = (1/n) · min{C, R}     output, with n approved changes per unit of software
C^P = A_C · L_C           code produced: coding productivity times coding labor
R^P = A_R · L_R           reviews completed
A_R = (1 − π) · Ã_R       effective review productivity, net of the bug rate π
W = B / R^P               wait for review: backlog B divided by review capacity
```

A_C is coding productivity, L_C and L_R are the labor shares in coding and review, and π is the probability that review finds a bug and sends code back. A senior-only constraint limits review labor to a ceiling, so a share of reviews can only be done by senior engineers. Because coding and review are complements, the optimal allocation balances the two capacities, and any coding gain beyond review capacity builds a backlog rather than output.

### Estimation and long-run simulation
The authors estimate the five parameters θ = (dlnA_C, dlnA_R, dlnL_C, dlnL_R, dln n) by classical minimum distance, choosing θ̂ to match six event-study moments as closely as possible with a weight matrix built from the inverse squared standard errors. Six moments and four free parameters leave two over-identifying restrictions, which are tested with a J statistic of 2.91 (p = 0.233). For simulation, demand is linear, P = a − bY, and the paper defines the demand elasticity ε at the pre-adoption optimum. The firm's total labor rises only if ε exceeds 3.68. Coding productivity is projected as an exponential trend fitted through the two estimates (1.35 for assistants, 2.34 cumulative with agents), following the doubling rate of frontier-model task horizons from Kwa et al. (2025).

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| AI assistants, 12 months after adoption | 3,445.3 | +1,045.9 (+30.4%), marginally insignificant | Lines of code added per worker-month |
| AI agents, 12 months after adoption | 4,806.1 | +2,113.6 (+44.0%), marginally insignificant | Lines of code added per worker-month |
| AI assistants | 18.55 | +3.11 (+16.8%), significant | Commits per worker-month |
| AI agents | 26.43 | +9.51 (+36.0%), significant | Commits per worker-month |
| AI assistants | 3.52 | +0.734 (+20.9%), significant | Pull requests opened per worker-month |
| AI agents | 4.83 | +1.73 (+35.7%), significant | Pull requests opened per worker-month |
| AI assistants | 1.80 | −0.027 (−1.5%), not significant | Jira issues resolved per worker-month |
| AI agents | not reported | +13.7%, marginally insignificant | Jira issues resolved |
| AI assistants | 0.095 | +0.003 (+3.4%), not significant | Jira epics resolved per worker-month |
| AI agents | not reported | +18.0%, marginally insignificant | Jira epics resolved |
| AI assistants | not reported | +3.0%, rules out changes below −4.7% | Active engineers (Jellyfish) |
| AI agents | not reported | −2.6%, rules out changes below −15.6% | Active engineers (Jellyfish) |
| AI agents | 3.56 days | +1.99 days (+56.1%) | Days from PR submission to merge |
| AI agents | 2.78 days | +1.67 days (+59.9%) | Days in backlog before first comment |
| AI agents | 3.98 days | +2.33 days (+58.5%) | Days in active review |
| AI agents | 35.17% | +4.61 points (+13.1%), marginally insignificant | Share of PRs with at least one comment |
| AI agents | 1.63 | +0.629 (+38.6%) | Review comments per PR |
| AI agents, structural estimate | not applicable | Coding productivity +73.7%; review productivity −7.0%, not significant | Minimum-distance model |

The estimates come from 655 firms, with agent effects on lines of code ranging from 38.3% to 77.6% across the robustness variants. The commit and PR effects are significant, but the lines-of-code, issue and epic effects are not, and the paper's introduction calls the agent effect on issues "borderline significant" while Section 4.5 calls it marginally insignificant. The paper also reports that 10.8% of PRs receive an AI-generated comment in the text, but Figure 8 shows 13.2% for the same measure. Interviews with 21 people and a survey of 100 engineers illustrate the mechanism but do not test it.

## ⚠️ Limitations
- The authors measure effects only over the first 12 to 16 months after adoption, and employment may respond slowly.
- The design identifies partial-equilibrium effects, so output gains at adopters could partly come from business-stealing and fall elsewhere.
- The sample is mostly medium-to-large, established firms (the average firm had 1,676.6 total workers in January 2023), so effects may differ for start-ups or other knowledge work.
- Take-up is measured from the data and is a lower bound, and the IV per-worker effects are an upper bound on person-level productivity.
- The scenario in which review productivity grows with coding productivity is an upper bound, and the model leaves out specification, testing and customer coordination, which could be further bottlenecks.
- 🔎 Added context: the review-productivity estimate is −7.0% with a standard error of 0.051 in log points and is not significant, yet the reported split of the pass-through (62.4% bottleneck, 37.6% fragmentation) depends on it. The model also treats a PR receiving a comment as a proxy for a bug, which is an assumption rather than a measured defect rate.
- 🔎 Added context: the model's fit test has two degrees of freedom and p = 0.233, which is weak support for the structure rather than confirmation. The long-run paths extrapolate an exponential trend from two estimates, so the 2031 numbers are sensitive to that choice.

## 🏗️ What it means for building systems
This is interpretation, not the authors' claim. In a PR-based delivery pipeline, the review queue is where agent output lands, so that is where capacity matters: reviewer assignment rules, change size limits, and any AI reviewer that pre-screens changes before a human sees them. The paper's median spend of $178 per worker per month on agents (mean $808) is small beside salary costs, so the larger cost is probably senior reviewer time, which I would expect to be the scarce resource. To trust a rollout, you would track time-to-merge, queue length and comments per PR for each team before and after adoption, since the model predicts a growing queue when review capacity is fixed. The interviews also suggest that a rule requiring senior approval for AI-generated code can create its own queue.

## 🔑 Key takeaways
- Code volume overstates output: after agent adoption, commits rose 36.0% and PRs opened rose 35.7%, while Jira issues resolved rose 13.7%, marginally insignificant.
- Review is the constraint that grew: the time from review submission to approval rose 56.1%, with backlog wait up 59.9% and active review up 58.5%.
- Review capacity is concentrated: 82.8% of reviews are on code the reviewer had already worked in, and senior engineers do 75.7% of reviews on senior-written code.
- There is no measurable employment change yet: the estimates are +3.0% for assistants and −2.6% for agents, with wide intervals.
- The long-run result depends on review productivity: by 2031, output rises 57.8% and employment falls 10.3% if review is fixed, versus output +341.4% and employment −76.2% if review keeps pace.
- The results are short-run and firm-level, measured over 12 to 16 months in mostly large firms.

## 📚 Reading plan
### From the paper
The paper gives no code or data repository, and its cited works appear in the text only as author and year, so there are no links to list.

### To go further
- Kwa et al. (2025), on measuring the task time horizon of frontier AI models, which the authors use to extrapolate coding productivity.
- Demirer, Musolff, & Yang (2026), the closest prior study: in open-source repositories, Copilot raised lines and commits much more than PRs and releases.
- Little (1961), on Little's law, which the model uses to convert backlog into review wait time.
- So & Tang (1995) and Smith & Eppinger (1997), on how lower-quality output and rework slow a production stage.
- Borusyak et al. (2024) and Callaway & Sant'Anna (2021), on staggered difference-in-differences estimators, which the authors use as robustness checks.
- Brynjolfsson, Chandar, & Chen (2025), on junior employment losses from AI, which the authors contrast with their null employment result.
