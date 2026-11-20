---
type: Paper Summary
title: 'SoK: Assessing the Evolution of LLM Capability in Software Vulnerability Research'
description: LLM agents now find, validate and patch real vulnerabilities unattended, but fix and attack timelines have not
  yet shifted.
resource: https://arxiv.org/abs/2611.26601
tags:
- security
- llm-as-a-judge
- multi-agent-orchestration
- zero-trust-architecture
- agent-harness
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.26601
  title: 'SoK: Assessing the Evolution of LLM Capability in Software Vulnerability Research'
  author: arXiv
status: stable
source_url: https://arxiv.org/abs/2611.26601
source: arXiv
source_type: arxiv
published: '2026-11-19'
domain: security
concepts:
- llm-as-a-judge
- multi-agent-orchestration
- zero-trust-architecture
- agent-harness
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81d79667c82fd2cada38
---

## 🎯 In one sentence
LLM agents went from prescribed steps to unattended discovery, validation and patching of real software, reaching expert precision for discovery and validation by August 2025, while CVE records rose from 39,638 to 89,749 between January and October of 2025 and 2026 and the time to fix and to attack barely moved.

## ❓ The problem
Finding a software vulnerability is only the first of three jobs. The finding has to be validated, usually by a proof of vulnerability (PoV), meaning an input or procedure that shows the flaw is real, such as a working exploit. The fix then has to remove the root cause without breaking the software. Fuzzers and static analyzers produce many candidates, most of them false, so triage, proof and repair have stayed with human experts. Since 2024 LLM-based agents have reported real vulnerabilities, but the evidence is scattered across papers, vendor reports and disclosures. Most of it comes from the companies that built the systems, and the claims are rarely comparable. Public records such as CVEs, advisories and exploit listings exist, but they had not been linked to what the systems can actually do.

## 💡 The idea
The authors treat the question as a measurement problem. They borrow the SAE levels of driving automation, where each level is defined by the part of the driving task that stays with the human. Here each level is defined by what a system has been shown to do on real targets, separately for each task. A system gets a level only if most of its results hold up. Every claim is also tagged with its correctness, its access tier and the strength of its evidence. The map is then compared with a decade of public records, so expectations about the consequences can be checked against what has actually happened.

## ⚙️ How it works
### Assessment framework: autonomy, correctness, access, evidence
Each result is a pair of an autonomy level and a correctness category, for example A4·C2, dated from the first source that shows both values for the same system. A0 means no contribution; A2 means carrying out steps a human specifies; A3 means planning and running the whole task, with a human still preparing targets; A4 means completing the task on a given target with no human intervention; A5 also chooses its own targets. Correctness is the share of delivered results that hold up (precision): C1 means most hold up, C2 means as often as an expert, and C3 means expert precision on code that experts and fuzzers had examined for years. Access tiers run from T1 (lab only) to T4 (open weights), and evidence grades run from E1 (reported by the builder) to E3 (confirmed by an independent party).

```text
step = (autonomy level, correctness category), dated by the first source
cell = (capability, access tier)        e.g. Patch, T2
```
A cell is one capability at one access tier, and each cell holds a dated staircase of steps. The four capabilities are discover, validate vulnerability, patch and validate patch. Benchmark values (marked with a double dagger) and inferred values (marked with a dagger) are kept apart from direct evidence.

### Evidence pipeline
The authors built a multi-stage pipeline. A systematic search across 67 queries and 105 organizations' reports produced a corpus of 53,057 sources. Two independent agents marked 9,859 as relevant, and 6,604 of these dated from 2024 onward and concerned real software. Agents then read 1,100 of them in full and quoted a sentence for each value. A script confirmed that 99.6% of those quotes appear word for word in the sources, which rules out invented quotes but not weak support. Each cell was scored twice, once from readings and once by searching the corpus and the web. Separate decision agents argued for higher and for lower levels, and a verifier accepted 34 of the proposals and rejected 30. The final map has 128 steps, and two authors reviewed every one of them against its sources.

### Public-record measurement
The second half compares 503 verified statements from labs, maintainers, agencies and researchers with five measured quantities: CVE volume and severity, the age of the vulnerable code, the share of records naming an AI system as finder, the time from report to fix, and the time from record to exploitation. The statements were turned into seven commonly expected consequences, such as "findings will surge" and "exploits will follow fixes within hours". Each one is then set against what has been demonstrated and what the records show.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| Discovery and validation, every tier | Capable (A4 C1) | Expert (A4 C2), reached August 2025 | Autonomy and correctness category |
| Partner tier, Claude Mythos Preview, code audited for years | Expert on general code (A4 C2) | A4 C3, May 2026; 90.6% of 1,752 assessed findings confirmed by independent firms | Share of findings confirmed |
| Public tier, Aikido Attack | None reported | Expert (A4 C2); security firm confirmed 96% of findings | Share of findings confirmed |
| Open-weight GLM 5.3 vs Mythos Preview, end-to-end exploits | Mythos Preview | 12% of 410 attempts vs 14% | Share of attempts succeeding (Anthropic comparison) |
| Patching, partner tier (Defense Factory) | None reported | Expert (A4 C2), September 2026; 0.53% of fixes rolled back | Share of fixes rolled back |
| Patch validation, LLM judge on a benchmark | Careful human reading | Agreed in 85% of cases | Agreement rate |
| Public systems after first A4 system, discover, capable | First system (months 0) | 5.0 months [2.8, 9.9] | Months, with range (Table 4) |
| Open-weight systems after first A4, discover, expert | First system (months 0) | 13.7 months [at least 5.0] | Months, with range (Table 4) |
| CVE records, January to October | 39,638 (2025) | 89,749 (2026) | Published records |
| Share rated Critical or High | 46% (2025) | 56% (July to October 2026) | Share of scored records |
| Bugs 10 years old or more, Critical or High | 2025 monthly rate | 6 to 11 times as often (July to October 2026) | Monthly rate multiple, where age is known |
| Report to fix, Zero Day Initiative | 96 to 119 days, every year 2016 to 2026 | 148 days in the latest quarter (28 cases) | Median days |
| Bulk reports to outside maintainers | Curated reports and OSS-Fuzz: 58 to 94% fixed | 5 to 26% fixed | Share of reports fixed |

The results show that LLM agents reached expert precision on real software within about a year, and that public and open-weight systems followed within months. Two things they do not show are a settled rate of exploitation and the share of the records that is due to LLMs. The ablations matter here. When only E2 and E3 evidence is kept, discovery and validation still reach A4 but in August 2025 instead of June, and patch validation falls to A3 at the public tier. The evidence base is large but uneven: patch validation rests mostly on the developers' own reports.

## ⚠️ Limitations
- Most evidence comes from the organizations that build the systems. Patch validation rests mostly on their own reports (E1), and remediation is the least measured task.
- Benchmark values (‡) and inferred values (†) are reported separately. Open-weight patching is inferred, and the open-weight delays are upper bounds because they date the first documented use.
- Most CVE records name no tool, so counts of AI-found vulnerabilities are lower bounds, and the share of the surge caused by LLMs is not yet known.
- No source tests a mitigation against an LLM-based attacker, so the mitigation grades describe general effectiveness, not effectiveness against these systems.
- Correctness is precision. Recall is rarely reported, so the vulnerabilities a system misses are unknown.
- 🔎 Added context: the classification of every step was done by LLM agents, mostly Claude models, which are also among the systems being rated. The authors' check covered 16 cells, and the paper does not analyze whether this produces a systematic bias.
- 🔎 Added context: the claim that restricting frontier models "delays the spread by months only" rests on a few comparisons, one of them the lab's own comparison of GLM 5.3 with Mythos Preview. The Table 4 ranges are wide (for example 5.0 [2.8, 9.9]).

## 🏗️ What it means for building systems
This is interpretation, not the authors' claim. Discovery agents would plug into CI and security scanning, and patch-validation agents into the review gate before a fix merges. The paper suggests that the bottleneck is now the fix and its deployment, so the first investment is in dependency automation, release cadence, rollback and time-to-deploy measurement. The cost of running agents is not quantified in the text, only plotted in Figure 5. To trust such a pipeline you would want a working proof for each report, confirmed reports only, and your own rollback and confirmation rates, in the spirit of the Defense Factory figure of 0.53%. The paper also treats it as a reason to assume that attackers can reach the same capability, which points to compartmentalization and least privilege as the defense.

## 🔑 Key takeaways
- Unattended agents reached expert precision (A4 C2) for discovery and validation on public systems about nine months after the first system did (Table 4).
- Code audited for years was found at A4 C3 by a partner-tier system in May 2026, with 90.6% of 1,752 assessed findings confirmed.
- Patching reached A4 C1 in October 2025, but expert correctness (C2) came only in September 2026, and patch validation is the least measured task.
- CVE records more than doubled in 2026, and the share rated Critical or High rose from 46% to 56%.
- The time from report to fix (96 to 119 days) and from record to listed attack (2 to 5 days) has not shifted noticeably, and most bulk reports to outside maintainers remain unfixed.
- The best-supported mitigations are established practices: memory-safe languages, a smaller attack surface, compartmentalization and automatic updates.

## 📚 Reading plan
### From the paper
- [Anthropic, Assessing Claude Mythos Preview's cybersecurity capabilities](https://www.anthropic.com/research/mythos-preview) — the partner-tier system behind the A4 C3 result and its 1,752 assessed findings.
- [Anthropic, Measuring LLMs' impact on N-day exploits](https://www.anthropic.com/research/n-days) — the evidence that exploits can be built from public fixes within hours.
- [Levels of autonomy for AI agents (arXiv:2506.12469)](https://arxiv.org/abs/2506.12469) — the autonomy taxonomy that the framework builds on.
- [SAE J3016, Taxonomy and definitions for driving automation](https://saemobilus.sae.org/content/j3016_202104) — the driving-automation levels the autonomy scale is modeled on.
- [CVE List v5 records (CVEProject)](https://github.com/CVEProject/cvelistV5) — the public record source for the volume, severity and age measurements.
- [PatchEval (ByteDance)](https://github.com/bytedance/PatchEval) — the benchmark of real CVE fixes used for patch-validation values.
### To go further
- CyberGym: evaluating AI agents' real-world cybersecurity capabilities at scale — the benchmark the paper uses as complementary evidence.
- Coordinated vulnerability disclosure and the 90-day fix window — the policy that the time-to-fix results are measured against.
- Milk or wine: does software security improve with age? — the classic study of how long vulnerabilities live in code, relevant to the old-bug finding.
