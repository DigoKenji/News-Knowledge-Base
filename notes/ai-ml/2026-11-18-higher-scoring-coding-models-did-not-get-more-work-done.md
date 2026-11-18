---
type: Paper Summary
title: Higher-Scoring Coding Models Did Not Get More Work Done
description: A 19-day forced switch away from Fable 5 and five later upgrades left merge rates flat despite benchmark gains.
resource: https://arxiv.org/abs/2611.23216
tags:
- ai-ml
- mlops
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.23216
  title: Higher-Scoring Coding Models Did Not Get More Work Done
  author: Yining Hua
status: stable
source_url: https://arxiv.org/abs/2611.23216
source: arXiv
source_type: arxiv
published: '2026-11-17'
domain: ai-ml
subdomain: mlops
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d810d8db5f294db6841c7
---

## 🎯 In one sentence
Losing Claude Fable 5 for 19 days, and then five ordinary model upgrades, left the 7-day merge rate of the same developers flat: Fable 5 access moved it by +1.4 percentage points (95% CI −1.4 to +4.3), even though Fable 5 led Opus 4.8 by 10.8 points on SWE-Bench Pro.

## ❓ The problem
New coding models are sold on benchmark gains. SWE-Bench Pro is a benchmark whose tasks come from real bug fixes in public repositories, and a task counts as solved when the code change passes the tests from the original fix. The paper asks whether a higher score means more finished work. Real work is different: requests are vague, the code has to match the project's style, and someone has to accept the change. On GitHub, that acceptance is a merge of a pull request (PR), which is a proposed code change. Comparing developers who use different models is hard, because early adopters may be more experienced, they may give their hardest tasks to the strongest model, and each repository has its own merge habits.

## 💡 The idea
Instead of comparing models across people, the authors use an outside event. On June 12, 2026, a U.S. export control directive removed access to Fable 5 for 19 days, so its users were forced onto lower-tier models, and some switched back when access returned. The people and projects stayed the same and only the model changed, which the authors call a natural experiment. They compare the same developers' merge rates across phases against a control group that used only Opus 4.8, and then check the result against five model upgrades where developers chose to switch.

## ⚙️ How it works
### Data and outcome
The data are public GitHub PRs that Claude Code helped create. Claude Code adds a co-author line naming the model to commits, and the authors read the model from the PR description. A PR is kept only if its description names exactly one model. The 7-day merge rate is the share of PRs merged within 7 days of creation. In a check of 200 PRs, the model named in the description matched the commits in 96% to 99% of cases.

### Natural experiment (suspension analysis)
The treated group is 297 developers who opened at least one PR naming Fable 5 during the three days it was available. The control group is 8,908 developers who used Opus 4.8 and never named Fable 5. Each PR is one observation in a linear probability model:

```text
Y_it = α_i + γ_t + β0·P0_it + β1·P1_it + β3·P3_it + ε_it
```

Y_it is 1 if PR p of developer i, opened on day t, was merged within 7 days, and 0 otherwise. α_i is the developer fixed effect, which absorbs stable differences between developers. γ_t is the calendar-day fixed effect, which absorbs changes that hit everyone on the same day. P0, P1 and P3 flag the treated group's PRs in the before-release, available and restored phases, with the suspension P2 as the reference. The main estimate is (β1 + β3)/2, the average difference between the two phases with Fable 5 available and the suspension; β0 is a placebo.

### Upgrade analysis
The five pooled upgrades are Opus 4.6 to 4.7, Opus 4.7 to 4.8, Opus 4.8 to 5, Opus 5 to 5.5, and Sonnet 5 to 5.5. Developers who switched had PRs with the old model in the week before and the new model in the week after. For each upgrade, Y_ip = α_i + δ·New_ip + ε_ip, where New_ip is 1 if PR p was opened with the new model and δ is the change in merge rate. The five estimates are pooled by inverse-variance weighting. Developers chose their models themselves, so this part is an association, not a randomized effect.

```text
Y_ip = α_i + δ · New_ip + ε_ip
```

### Benchmark context
Anthropic's reported SWE-Bench Pro scores are 80.0% for Fable 5 and 69.2% for Opus 4.8, a gap of 10.8 points. For six of the upgrades both scores were reported, and the newer model scored 5 to 18 points higher. The paper uses these gains as the benchmark side of the comparison.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| Fable 5 available vs suspended, 297 treated developers | Suspension period on Opus 4.8 | +1.4 percentage points (95% CI −1.4 to +4.3) | 7-day merge rate |
| Five model upgrades pooled, 1,208 developers who switched | Same developers on the old model | −0.9 percentage points (95% CI −1.9 to +0.2) | 7-day merge rate |
| Six upgrades, against developers who did not switch | Developers who did not switch | +2.4% (95% CI −4.9% to +10.2%) | Weekly merged PRs |
| Five upgrades pooled, PRs that ran checks | Old model | +0.3 percentage points (95% CI −1.4 to +2.0) | Failed automated check |
| Opus 4.7 to 4.8, single upgrade | Opus 4.7 | +5.4 (95% CI +1.1 to +9.7) | Failed automated check |
| Benchmark gap | Opus 4.8: 69.2% | Fable 5: 80.0%, a 10.8-point lead | SWE-Bench Pro score |

The suspension estimate has wide intervals because the window was only 19 days. The paper rules out gains above about 4 points from Fable 5 access. The five upgrades give tighter intervals and the same direction: all five point estimates were negative and each interval included 0. Almost all PRs were already merged before the upgrades (94.5% within 7 days in the week before), so the outcome had little room to rise.

## ⚠️ Limitations
- The suspension lasted only 19 days, so its estimate is noisy, and the upgrade analysis is observational because developers picked their own switches.
- The main estimate uses only PRs whose description names a model, about 1% of merged Claude Code PRs, and that share fell during the suspension. A refit on all PRs gave the same direction (+0.5 points, −2.3 to +3.3).
- A merge shows that the developer accepted the change, not that the change is correct: 95% of sampled PRs were merged by their own author, and 90% had no approval from another account.
- Only Claude Code and only public GitHub PRs were studied, and only Fable 5 had an outside-imposed switch.
- The restoration phase overlaps with the Sonnet 5 release and a usage rule that limited Fable 5 use until July 7, so some developers may have kept Fable 5 for certain tasks.
- 🔎 Added context: the 297 treated developers were early adopters who started using Fable 5 in its first three days. The paper's equal-weight-per-developer estimate was +3.7 (−5.5 to +12.9), so the main result describes heavy PR openers more than a typical developer.
- 🔎 Added context: the comparison is between two adjacent frontier models, Fable 5 and Opus 4.8. The paper says little about gains against much older models.

## 🏗️ What it means for building systems
This is interpretation, not the authors' claim. If your team runs AI coding agents in a delivery pipeline, the paper suggests a model's benchmark score is a weak signal of whether its output gets accepted. A practical version would tag each agent-produced change with the model that made it, as Claude Code's co-author line does, and then compare acceptance and rework on your own repositories before and after a model swap, rolling out by team so that a team not yet switched serves as a natural holdout. The cost is telemetry plus several weeks of data per team. You would trust the result only if team membership and review habits stayed stable during the window, and if you also measured review and post-merge defects, which this paper cannot measure.

## 🔑 Key takeaways
- A 19-day forced switch from Fable 5 to Opus 4.8 changed the same developers' 7-day merge rate by +1.4 percentage points, which is not statistically significant.
- Five model upgrades pooled gave −0.9 percentage points (95% CI −1.9 to +0.2), also not significant.
- Almost all PRs were already merged before the upgrades, so the merge rate had little room to move. Weekly merged PRs changed by +2.4%, not significant.
- A 10.8-point lead on SWE-Bench Pro did not show up as more merged work in this sample.
- Merges measure acceptance, not quality. Reverts did not change, and failed checks rose only for Opus 4.7 to 4.8.
- Test a model's benchmark claim against outcomes from your own work before you change tools.

## 📚 Reading plan
### From the paper
- [Anthropic, Statement on the US government directive to suspend access to Fable 5 and Mythos 5](https://www.anthropic.com/news/fable-mythos-access) — the event that creates the natural experiment.
- [Peralta et al., Why are agentic pull requests merged or rejected?](https://arxiv.org/abs/2605.22534) — what makes agent PRs get merged.
- [Pinna et al., Comparing AI coding agents: a task-stratified analysis of pull request acceptance](https://arxiv.org/abs/2602.08915) — merge rates across agents and task types.
- [METR, Many SWE-bench-passing PRs would not be merged into main](https://metr.org/notes/2026-03-10-many-swe-bench-passing-prs-would-not-be-merged-into-main/) — why benchmark passes overstate mergeable work.
- [Robbes et al., Promises, perils, and (timely) heuristics for mining coding agent activity](https://arxiv.org/abs/2601.18345) — caveats for mining agent PRs.
- [benchmarks-vs-merges](https://github.com/ningkko/benchmarks-vs-merges) — the released dataset and reproduction code.

### To go further
- Designing benchmarks for knowledge work (Hua, Na, Ayubcha, Lian, 2026) — why benchmark tasks differ from real work.
- The effects of generative AI on high-skilled work: evidence from three field experiments with software developers (Cui et al., Management Science, 2026) — randomized comparisons of AI access.
- Difference-in-differences with fixed effects and clustered standard errors — the statistics behind the main estimate.
- Terminal-Bench (Merrill et al., ICLR 2026) — the hard-task benchmark the paper compares against.
