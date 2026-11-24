---
type: Paper Summary
title: Do Verifier Judgments Survive Abstraction? Measuring Verification Transport for LLM Agent Evaluation
description: Verifier judgments correct on full agent trajectories flip in roughly 20-31% of cases once the trace is abstracted, and restoring provenance recovers 85.5% of them.
resource: https://arxiv.org/abs/2611.18471
tags:
- ai-ml
- ai-agents
- llm-as-a-judge
- multi-agent-orchestration
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://export.arxiv.org/pdf/2611.18471
  title: Do Verifier Judgments Survive Abstraction? Measuring Verification Transport for LLM Agent Evaluation
  author: Disha Sheshanarayana, Sai Linisha Reddy Goli
status: stable
source_url: https://arxiv.org/abs/2611.18471
source: arXiv cs.AI/cs.LG/cs.CL
source_type: arxiv
published: '2026-11-13'
domain: ai-ml
subdomain: ai-agents
concepts:
- llm-as-a-judge
- multi-agent-orchestration
selected_by: discovery
content: open
notion_url: https://app.notion.com/p/41abaf74172d81ffbfd4cd7380001ea9
---

# Do Verifier Judgments Survive Abstraction? Measuring Verification Transport for LLM Agent Evaluation

**Source:** https://arxiv.org/abs/2611.18471
**Authors:** Disha Sheshanarayana, Sai Linisha Reddy Goli (equal contribution)
**Institution:** Manipal University Jaipur
**Published:** 13 Nov 2026 (arXiv v1) | **Venue:** Preprint; workshop paper, "Who Verifies the Agents? Toward Reliable Agent Development," NeurIPS 2026
**Tags:** #agent-evaluation #llm-as-a-judge #verification #observability

---

## 📄 Abstract (in plain language)
LLM verifiers are used to grade agent trajectories, but their reliability is usually measured on one fixed view of the trajectory. In deployed systems, the same run may reach the verifier through composed tools, delegated sub-agents, or compacted history, so it sees less of the underlying evidence. The authors check whether a verdict that is correct on the full view stays correct when the same frozen execution is abstracted. Across four benchmarks, seven backbones, four protocols and three abstraction boundaries, 20-31% of native-correct verdicts fail, and most failures recover when the original provenance is restored.

## 🎯 Core Contribution
The paper defines **Verification Transport** as a reliability criterion: a judgment correct for an execution should stay correct when only the verifier-visible representation of that execution changes. Prior work on [LLM-as-a-judge](/concepts/ai-ml/llm-as-a-judge.md) reliability mostly varies the policy, prompt or context, and assumes the trajectory is shown in a fixed form. The authors hold the trajectory, outcome, ground truth, criterion and verifier fixed and vary only observability. Their claim is that native-view accuracy alone cannot characterize verifier reliability, because a verifier can be accurate on the full view and still fail once evidence is abstracted.

## 🏗️ Methodology

### Problem Formulation
For a trajectory T with label Y, a verifier V receives a view O_c(T) under condition c and returns a verdict or no usable verdict (⊥). A native-correct judgment (C0) fails transport if its abstracted verdict (Ca) is wrong or ⊥. Wrong verdicts and ⊥ are reported separately, because ⊥ is a loss of coverage rather than an incorrect judgment.

### Proposed Method: Abstraction-Restoration Framework
Each execution gets three matched views, C0 → Ca → Ca-R. The restoration condition Ca-R reinserts the original provenance, so only the verifier's evidence changes. Three boundaries are instantiated:

- **Macro abstraction (CM):** low-level operations sit behind a higher-level macro action. Restoration reinserts the exact internal operations.
- **Delegation (CD):** part of the run is hidden behind a sub-agent boundary, and the verifier sees only the delegated result. Restoration reinserts the delegated trajectory.
- **History compaction (CC):** earlier history is replaced by a marker, keeping recent evidence including the final tool-call/result pair. Restoration reinserts the original events.

### Verifier Protocols
All protocols share the backbone, criterion and representation, and differ only in how evidence is used:

- **V-LLM:** single-pass direct verdict.
- **V-EVIDENCE:** names the visible evidence and checks its sufficiency first.
- **V-AGENTIC:** may request more provenance from the same trajectory.
- **V-DELIBERATIVE:** reuses the V-EVIDENCE verdict, then runs a separate evidence audit with no extra provenance. This isolates deliberation from provenance acquisition.

### Design Choices
Views are built deterministically from frozen trajectories before any verifier call, with temperature 0 and structured outputs, so only evidence visibility varies. Pooled rates sum numerators and denominators across cells rather than averaging percentages.

## 🧪 Experimental Setup

### Datasets & Benchmarks
- **AgentProcessBench (APB), 250 trajectories:** step-level process quality [Fan et al., 2026].
- **ATBench, 758 trajectories:** tool-use trajectory safety [Li et al., 2026c].
- **PlanRewardBench (PRB), 441 pairwise comparisons:** trajectory-level preference [Wang et al., 2026].
- **AgentDojo, 754 trajectories:** interactive agents under prompt injection [Debenedetti et al., 2024].

Counts are the instances that met each benchmark's eligibility rules for matched construction.

### Baselines
There are no baselines in the usual sense. The comparison axes are the native view, the three boundaries, the three restoration conditions and the four protocols. Seven backbones are used: Gemini-3.7-Flash, Gemini-2.5-Flash, GPT-OSS-120B, GPT-OSS-20B, Gemma-4-31B, Gemma-4-26B-A4B and Llama-4-Maverick.

### Evaluation Metrics
- **C0 OBA:** native-view operational balanced accuracy.
- **VTFR:** share of native-correct judgments that fail after abstraction. It measures stability of correct judgments, not overall accuracy.
- **RRR:** share of abstraction-induced failures recovered by restoration.
- **RDR:** share of judgments that survived abstraction but are degraded by restoration.

## 📊 Results

### Main Results
Denominator-weighted over all benchmarks, models and protocols:

| Boundary | VTFR | RRR | RDR |
|---|---|---|---|
| Macro abstraction | 31.0% | 87.9% | 5.3% |
| Delegation | 30.9% | 86.9% | 5.4% |
| History compaction | 20.3% | 79.8% | 4.9% |
| **All boundaries** | | **85.5%** | **5.2%** |

VTFR by protocol (macro / delegation / compaction):

| Protocol | Macro | Delegation | Compaction |
|---|---|---|---|
| V-LLM | 16.4% | 17.3% | 14.1% |
| V-AGENTIC | 25.0% | 23.6% | 18.2% |
| V-EVIDENCE | 34.5% | 34.0% | 18.7% |
| V-DELIBERATIVE | 49.3% | 50.2% | 30.7% |

The protocol ranking is about failure modes, not only totals. Under macro abstraction, V-LLM gives 15.8% wrong verdicts and 0.7% no-verdicts, V-EVIDENCE gives 10.6% and 24.0%, and V-DELIBERATIVE gives 9.4% and 39.9%. Lower VTFR can therefore hide lost verdict coverage. The effect holds across backbones but its size varies (Tables 1-2, Figure 3).

### Ablation Studies
- **Severity:** hiding 25%, 50% and 75% of evidence raises VTFR at the endpoints. On APB, Gemini goes from 24.3% to 31.4% and GPT-OSS from 21.5% to 29.6%. On ATBench, the values go from 17.7% to 21.1% and from 16.9% to 22.9%.
- **Provenance fidelity:** recovery follows Requery < LossyR < ExactR. On APB, Gemini's RRR is 22.5%, 42.8%, then 80.0%, and GPT-OSS's is 24.6%, 34.0%, then 82.5%.
- Re-evaluating on the same abstracted evidence recovers the least, so recovery depends on the evidence, not on another verifier call.

## 💡 Key Insights
- Native-view accuracy overstates verifier reliability. A full-view verdict changes for roughly 20-31% of cases, depending on the boundary.
- Abstention is a separate failure mode. Evidence-seeking protocols mostly lose coverage under macro abstraction rather than giving wrong answers.
- Most failures are recoverable. Exact provenance recovers 85.5% overall, while lossy provenance recovers much less (APB Gemini: 42.8% vs 80.0%), so evidence access is the bottleneck more than verifier capability.
- Extra deliberation does not help here. V-DELIBERATIVE has the highest VTFR, at 49.3% and 50.2% under macro and delegation.

## 🔍 Follow-up References

### Foundational Works This Paper Builds On
- 📚 Zheng et al., 2025 — *ProcessBench: Identifying process errors in mathematical reasoning* → Process-level error localization that motivates trajectory-level verification.
- 📚 Lightman et al., 2023 — cited for step-level verification of reasoning. Title not reproduced here.

### Directly Cited Competing or Related Methods
- 📄 Fan et al., 2026 — *AgentProcessBench* ([arXiv:2603.14465](https://arxiv.org/abs/2603.14465)) → One of the four benchmarks (250 trajectories).
- 📄 Weng et al., 2026 — *Beyond accuracy: Policy invariance as a reliability test for LLM safety judges* ([arXiv:2605.06161](https://arxiv.org/abs/2605.06161)) → Closest in spirit; tests judge stability under policy transforms rather than evidence visibility.
- 📄 Zhuge et al., 2025 — *Agent-as-a-Judge: Evaluate agents with agents* (ICML 2025) → Agentic evaluation, the setting V-AGENTIC resembles.
- 📄 Han et al., 2026 — *SkillTV-Bench* ([arXiv:2608.05573](https://arxiv.org/abs/2608.05573)) → Judge evaluation on agent executions, a close neighbor in setting.

### Cited Datasets, Benchmarks, or Tools
- 🔗 ATBench — [arXiv:2604.02022](https://arxiv.org/abs/2604.02022) → Trajectory safety benchmark; 758 trajectories used.
- 🔗 PlanRewardBench — *Aligning agents via planning: A benchmark for trajectory-level reward modeling* (ACL 2026; Wang et al., 2026) → 441 pairwise comparisons.
- 🔗 AgentDojo — [OpenReview](https://openreview.net/forum?id=m1YYAQjO3w) → Prompt-injection environment; 754 trajectories used.

### Suggested External Reading (Not in Paper)
None proposed. Appendix A (extended related work) and Appendix G (limitations) are the closest in-source pointers.

## ⚠️ Limitations Acknowledged by Authors
The main text has no limitations section. Appendix G states that only three controlled boundaries are studied, that views come from frozen trajectories rather than live abstractions, and that restoration assumes original provenance can be reinserted, with availability and fidelity that may vary. It also notes that VTFR conditions on native-correct judgments, so it measures preservation of correct verification rather than overall competence. Results may not transfer to other domains or verifier designs.

---
*Summary generated: 2026-11-16*
