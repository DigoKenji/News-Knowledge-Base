---
type: Paper Summary
title: 'Realized versus Population Reliability: Anytime-Valid Self-Consistency Certification under Correlated Reasoning'
description: Under correlated rollouts, anytime-valid self-consistency certificates certify a per-prompt claim, not the population
  reliability claim.
resource: https://arxiv.org/abs/2611.26592
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.26592
  title: 'Realized versus Population Reliability: Anytime-Valid Self-Consistency Certification under Correlated Reasoning'
  author: Terence D. Agbeyegbe, Toneukarin G. Agbeyegbe
status: stable
source_url: https://arxiv.org/abs/2611.26592
source: arXiv
source_type: arxiv
published: '2026-11-19'
domain: ai-ml
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d8166ba06e8c67ad1a181
---

## 🎯 In one sentence
Anytime-valid self-consistency certificates are exact for the per-prompt claim that the drawn reasoning favors a given answer, but when rollouts are correlated they over-certify the population claim: on AIME the single-unit rule certifies on 24.0% of prompts even though the estimated population margin is θ̂ = −0.15.

## ❓ The problem
Self-consistency means sampling a language model several times on the same question and taking the majority answer; each full sampled run is a rollout. Recent work (Cordero-Encinar and Duncan, 2025) turns "how many samples are enough" into a stopping rule: keep sampling while an e-process (a running product of evidence factors) accumulates evidence, and stop when it crosses a threshold. The guarantee is anytime-valid Type I error control: the chance of a false "certified" verdict stays at or below ϵ = 0.05 whenever you stop, by Ville's inequality. That guarantee assumes each rollout is an independent draw. Real reasoning streams are not independent: rollouts share a sampled reasoning prefix, and aggregate claims pool prompts of very different difficulty, so rollouts are positively correlated. The paper asks which claim such a certificate is still entitled to make under that correlation.

## 💡 The idea
Correlation splits the single null hypothesis into two claims that coincide only when correlation is zero. The realized claim is about the one prompt and prefix in front of you: is the answer favored for this drawn reasoning? The population claim is about the model's average behavior over its randomness and the task distribution: is the answer favored on average? The standard betting process stays exactly valid for the realized claim, but a team that reads it as a population claim is certifying something it never tested. The authors frame the gap as the cluster-sampling design effect from survey statistics: many draws from one cluster carry much less information than their count suggests.

## ⚙️ How it works

### Dependent evidence model
Each certification unit (one prompt, or one shared prefix) draws a latent state Z once. Given Z, the rollouts are conditionally i.i.d. with conditional mean µ(Z). The score of each held-out rollout is Y_t = 2·1{X_t = c} − 1, where c is the answer chosen on a separate pilot sample and X_t is the held-out answer. Marginally the scores are exchangeable with pairwise correlation ρ, the share of variance that comes from the unit.

```text
Y_t    = 2*1{X_t = c} - 1                   values in {-1, +1}
mu(Z)  = E[Y_t | Z]                          realized margin of this unit
theta  = E[mu(Z)]                            population drift
pi     = P(mu(Z) > 0)                        share of units with positive realized margin
rho    = sigma_B^2 / (sigma_B^2 + sigma_W^2)
```

Here sigma_B^2 is the variance of mu(Z) across units (between-unit variance) and sigma_W^2 is the average within-unit variance. When rho = 0 the latent state carries no information and the model reduces to the independent case.

### Which null the standard certificate controls
The standard certificate multiplies per-rollout factors, M_n = prod over t ≤ n of (1 + λ·Y_t), with the betting fraction λ between 0 and 1/B (here B = 1), and mixes λ over a grid. Under correlation it is still an exact test of the realized null µ(Z) ≤ 0, because conditional on Z the rollouts are i.i.d. with non-positive mean (Theorem 4.2). It is not a valid test of the population null θ ≤ 0 on the observed history, because the data gradually reveal the realized belief. In the limit its crossing probability under the population null is at least π (Proposition 4.5), which can be well above the nominal ϵ.

```text
M_n = prod over t <= n of (1 + lambda * Y_t),   0 <= lambda <= 1/B
P( sup_n M_n >= 1/eps )  <=  eps     under the realized null mu(Z) <= 0
```

Here lambda is the betting fraction, eps is the nominal error level (0.05 in the experiments), and M_n is the running evidence product after n rollouts.

### Information saturation
Drawing more rollouts from one prompt does not fix the problem. Within one unit, the sample mean converges to the realized belief µ(Z), not to θ, and its variance stops falling at sigma_B^2. The effective sample size of the within-unit mean is n_eff = n / (1 + (n − 1)·rho), which tends to 1/rho. With the measured correlations near 0.45, one prompt is worth about two independent rollouts for the population question, however many rollouts you draw (Theorem 5.1). The authors stress that this is a design-effect calculation for that estimator, not a universal information bound for every estimator.

```text
Var(Ybar_n) = sigma_B^2 + sigma_W^2 / n = sigma^2 * (1 + (n - 1) * rho) / n
n_eff       = n / (1 + (n - 1) * rho)     tends to 1/rho as n grows
```

Ybar_n is the mean of the n within-unit rollouts, and sigma^2 = sigma_B^2 + sigma_W^2 is the marginal variance of a single score.

### Prompt-level remedy and allocation
The repair moves the unit of accumulation from rollouts to prompts. For each prompt q, the within-unit mean U_q is one near-independent summary, and the process is M_Q = prod over q ≤ Q of (1 + λ·U_q) across Q prompts. Because the U_q are independent with mean θ, this is a non-negative supermartingale under θ ≤ 0, so Ville's inequality gives exact anytime-valid population control (Theorem 6.2). For a fixed total budget N = Q·n, the variance of the pooled estimator rises with n, so under a pure rollout budget the best choice is one rollout per prompt (Proposition 6.3). Extra rollouts per prompt pay off only to amortize a real per-prompt cost, and then only up to a point that shrinks as correlation grows.

```text
U_q    = within-unit mean of the n_q rollouts for prompt q
M_Q    = prod over q <= Q of (1 + lambda * U_q)
n_star = sqrt( (c_p / c_r) * (1 - rho) / rho )    two-cost model, per the paper's display
```

Here c_p is the fixed per-prompt cost (prefill or encoding), c_r is the cost per rollout, and n_star is the continuous optimum rollout count per prompt.

### Unknown rho and serial dependence
Realized-null validity needs no knowledge of rho, and population validity holds for any fixed λ whatever rho is. What rho affects is efficiency, because the best betting fraction depends on the spread of the unit means. The authors recommend variance-adaptive betting at the unit level, such as empirical-Bernstein betting (Waudby-Smith and Ramdas, 2024) or Catoni-style estimators for heavy-tailed summaries (Wang and Ramdas, 2023). The conditioning argument needs one latent state per unit with conditional independence, so arbitrary serial dependence inside a unit is not covered. The authors name it the principal open direction and point to mixing or block constructions.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| Synthetic: recovering rho (planted rho = 0.09) | Planted 0.091 | Recovered 0.091 | ICC from one-way ANOVA |
| Synthetic: realized null (theta ≤ 0, pi = 0.37) | Nominal ϵ = 0.05 | Single-unit certificate 0.005 | Realized-null Type I error |
| Synthetic: population null, single-unit certificate | Nominal ϵ = 0.05 | 0.32, climbing toward pi = 0.37 | Population-null certification rate |
| Synthetic: population null, prompt-level remedy | Nominal ϵ = 0.05 | 0.033 at theta = 0 | Population-null certification rate |
| Synthetic: saturation | None | Variance limit 0.090 = sigma_B^2; n_eff ≈ 11 ≈ 1/rho | Variance limit and effective sample size |
| GSM8K, 300 prompts x 30 rollouts | Independent rollouts (rho = 0) | rho-hat 0.435 [0.208, 0.619]; theta-hat +0.96; pi-hat 0.99; 1/rho-hat 2.3 | ICC with cluster bootstrap 95% CI |
| AIME, 150 prompts x 30 rollouts | Independent rollouts (rho = 0) | rho-hat 0.480 [0.405, 0.544]; theta-hat -0.15; pi-hat 0.33; 1/rho-hat 2.1 | ICC with cluster bootstrap 95% CI |
| AIME: single-unit certificate | Nominal ϵ = 0.05 | 0.240 per prompt (24.0%) | Population-null certification rate |
| AIME: prompt-level remedy | Nominal ϵ = 0.05 | 0.000 | Bootstrap certification rate |
| AIME by pilot-agreement bin | Nominal ϵ = 0.05 | Single-unit rate 0.00, 0.03, 0.06, 0.73, 1.00 across bins; theta-hat from -0.70 to +0.97 | Certification rate by bin |

The synthetic study confirms the mechanism: the single-unit certificate keeps the realized-null error far below ϵ but certifies the population null at 0.32, while the prompt-level remedy holds at 0.033 when theta = 0. On real rollouts from Claude Haiku 4.5 at temperature 1.0, correlation is present on both benchmarks, but only AIME has a negative population drift. There, the single-unit certificates come entirely from high-agreement prompts: none of the 101 prompts with non-positive held-out means fired, while 49 prompts with positive means fired at a rate of 0.74. The paper reports no ablation over the betting grid or pilot size, and the evidence rests on one model, 150 to 300 prompts, and one seed; the authors state that the prompt-level 0.000 is consistent with validity but does not establish it. The text rounds the GSM8K rho-hat to 0.44, while the table reports 0.435.

## ⚠️ Limitations
- The within-prompt correlation from a shared reasoning prefix is not measured; only the across-prompt rho is.
- Only one model, Claude Haiku 4.5, at temperature 1.0 was run; the authors call an open-model replication the natural check and note it could change the magnitudes and possibly the pattern.
- Arbitrary serial dependence within a unit is outside the theory; the model assumes exchangeable latent-belief structure.
- The work is a correctness result, not an efficiency one, and Type I control is not power: near theta ≈ 0, certifying the population null may need many units.
- pi-hat is a descriptive proxy based on the sign of held-out means, and the prompt-bootstrap crossing rate is not an estimate of the Type I error rate.
- Source code and saved rollouts are supplementary and will be deposited in a public repository only upon publication, so no public artifact link exists yet.
- 🔎 Added context: the 24.0% headline comes from one seed, one model and one answer-extraction regex; the paper does not report how sensitive it is to these choices.
- 🔎 Added context: with 150 AIME prompts, a 0.000 rate has little power to show anything, so it cannot separate a valid but very conservative procedure from a weak one.

## 🏗️ What it means for building systems
This is my interpretation, not the authors' claim. If you use repeated sampling as an automated gate, such as voting over model answers to decide whether to accept an output or stop spending tokens, the certificate you get answers a per-request question, not a deployment-level reliability claim, which the paper says needs evidence pooled across prompts. Plugging this in would mean a sampling service plus an evaluation pipeline run over a representative, independent prompt set, with pilot and held-out rollouts kept strictly separate. The cost is mostly per prompt: with effective sample sizes near two rollouts per prompt, extra rollouts per prompt mainly add spend. To trust it you would need a fresh prompt sample from production traffic and your own estimate of rho, since the paper's numbers come from one model.

## 🔑 Key takeaways
- The guarantee attaches to the claim, not to the method: the standard betting certificate is exact for "this drawn reasoning favors the answer" and needs no knowledge of rho.
- Under correlation the same certificate can cross for the population claim far more often than ϵ = 0.05; on AIME the single-unit rule fires on 24.0% of prompts while theta-hat = -0.15.
- Correlation caps the information a prompt carries: rho-hat of 0.435 on GSM8K and 0.480 on AIME mean a prompt is worth about 2.3 and 2.1 independent rollouts for the population question.
- Under a pure rollout budget, one rollout per prompt across many prompts is the variance-minimizing choice for population validity (Proposition 6.3).
- The prompt-level e-process is the repair: population-null certification falls to 0.033 at theta = 0 in simulation and to 0.000 on AIME, with exact validity under independence across prompts.
- The regime decides whether the gap matters: on GSM8K, theta-hat = +0.96 leaves it dormant; on AIME, with theta-hat = -0.15, it is salient.

## 📚 Reading plan
### From the paper
- [arXiv:2611.26592, this paper](https://arxiv.org/abs/2611.26592) — read Sections 4 and 8 first, then Section 6 for the repair.
### To go further
- Certified self-consistency (Cordero-Encinar and Duncan, 2025), arXiv:2510.17472 — the independent-rollout Martingale Majority Certificate this paper corrects.
- CITE: anytime-valid statistical inference in LLM self-consistency (Ota et al., 2026), arXiv:2605.05873 — certification of the target answer when the answer set is unknown.
- Anytime-valid answer sufficiency certificates (Akter et al., 2025), arXiv:2510.06478 — self-normalized empirical-Bernstein e-processes for LLM generation.
- Game-theoretic statistics and safe anytime-valid inference (Ramdas, Grünwald, Vovk and Shafer, 2023) — the e-process framework behind the proofs.
- Estimating means of bounded random variables by betting (Waudby-Smith and Ramdas, 2024) — the variance-adaptive betting the authors recommend.
- Survey Sampling (Kish, 1965) — the design-effect idea behind the 1/rho cap.
- Training verifiers to solve math word problems (Cobbe et al., 2021), arXiv:2110.14168 — the GSM8K benchmark.
