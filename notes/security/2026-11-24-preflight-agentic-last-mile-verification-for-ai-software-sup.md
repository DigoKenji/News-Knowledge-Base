---
type: Paper Summary
title: 'Preflight: Agentic Last-Mile Verification for AI Software Supply Chains'
description: Preflight executes a dependency through the calls a program actually makes, exposing malicious behavior that
  import-only probes miss.
resource: https://arxiv.org/abs/2611.31110
tags:
- security
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.31110
  title: 'Preflight: Agentic Last-Mile Verification for AI Software Supply Chains'
  author: Yaman Jandali, Nojan Sheybani, Farinaz Koushanfar, Tara Javidi
status: stable
source_url: https://arxiv.org/abs/2611.31110
source: arXiv
source_type: arxiv
published: '2026-11-23'
domain: security
concepts:
- ai-agent
- threat-modeling
- zero-trust-architecture
- supply-chain-attack
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d81b8a039eac726c8a074
---

## 🎯 In one sentence
Preflight verifies a Python dependency by running it through the calls the application actually makes, and it returns four malicious verdicts across five real incident contexts, zero false positives on 100 benign packages, and malicious verdicts in 2 of 2 ablation cases where import-only probing found none.

## ❓ The problem
Python applications import third-party packages and run them with the application's own privileges. In AI-adjacent software those dependencies can reach model-provider tokens, cloud credentials, local files and user data, so a compromised release executes inside the application's trust boundary. Current checks answer different questions: provenance (where an artifact came from, proven by attestations), static analysis (whether the code looks suspicious) and package-level behavior (what the package does in general). None of them says how a specific release behaves for the specific program that is about to run it. Malicious code can stay dormant until one function, constructor or plugin path is called, and an application usually touches only a small part of a package's API. The authors call this gap between package-level assurance and application-specific execution the "last mile" of supply-chain verification.

## 💡 The idea
Verify the dependency through the way your program uses it, not through the way the package presents itself. Preflight reads the program, finds the package functions it references, generates a small harness that calls those functions, runs the harness in a restricted sandbox, and compares the observed behavior with a known earlier version. The key point is that an import is not a call: many behaviors appear only when a referenced function runs, so probing the referenced surface sees what import-only probing misses. The paper does not give an analogy for this idea.

## ⚙️ How it works
### Application-surface extraction
Preflight parses the downstream program with Python's abstract syntax tree (AST) and records each imported package and each package symbol the program references, including aliases and module-attribute access. These records are joined with exact versions from requirements.txt, uv.lock or Pipfile.lock, and unpinned dependencies are rejected because the verdict must bind to one version. The output is the referenced dependency surface: the constructors and functions the probes must call. For example, if the program calls requests.get, the probe calls requests.get rather than only importing requests.

### Agentic harness synthesis
For each referenced surface, a language model (gpt-5.4-mini through the OpenAI Responses API, temperature 0, at most 4,096 output tokens, 120-second timeout) receives package-source context, the package identity and version, the referenced symbols and the invocation constraints, and proposes Python code that exercises them. A deterministic validator then accepts or rejects each candidate: it must be valid Python of at most 100 lines, import the target, invoke every referenced symbol, respect fixed constraints and keep symbol coverage after AST instrumentation. A rejected candidate gets feedback and one revision within a budget of two validator rounds; runtime traces are not given to the model. If that fails, the pipeline uses a deterministic fallback harness, but the live-agent evaluation excludes fallback generations. The model's action space is limited to writing and revising harness source; it does not pick packages, sandbox policy, traces or the end of analysis.

### Sandboxed behavioral observation
Each artifact is installed and executed in a restricted Docker container running as a non-root user, with a read-only root filesystem, no-new-privileges, dropped Linux capabilities, bounded resources and a seccomp policy. The sandbox must allow ptrace because strace is used to trace process and system-call activity; network traffic is captured, DNS is logged through an internal sinkhole, and the filesystem is compared before and after execution. Raw traces are noisy, so they are reduced to a canonical behavioral fingerprint: normalized endpoints, sensitive environment reads, persistent writes, spawned executables and syscall categories. The raw evidence stays available for inspection, while the fingerprint allows stable comparison between versions.

### Differential verification
Network access, file writes and subprocesses are normal in benign software, so the authors treat a change between versions, not the presence of a capability, as the main signal. With F(P, A) as the set of canonical capabilities observed when package P is exercised through the surface of application A, the differential is:

```text
ΔA(Pv, Pb) = F(Pv, A) \ F(Pb, A)
```

Pv is the target version, Pb the baseline version and A the application. F returns the observed capability set, and the backslash is set difference, so ΔA is what the target does for this application that the baseline did not. Credential-bearing environment reads, unseen or undeclared endpoints, persistent writes and newly observed subprocesses receive higher severity. When no defensible predecessor exists, the tool runs target-only analysis and makes no claim about newly introduced behavior.

### Verdicts and signed certificates
The verdict policy is simple: at least two critical capability findings imply malicious, one implies suspicious, and none implies clean, unless a critical static finding overrides the result. Bandit, Semgrep and a typosquat checker supply static findings, but high-severity static findings alone are not critical. The result is encoded as an in-toto statement in a DSSE envelope and signed with Ed25519. A separate checker verifies the signature, subject digests, certificate freshness, program hash, lockfile hash and verdict, and permits execution only for a valid clean certificate, so evidence generation is kept apart from a small deterministic enforcement step. An attempt to have gpt-5.4 synthesize verdicts failed for all five contexts because every request exceeded the input-size or context limit, so all final verdicts were deterministic.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| LiteLLM client context | 1.82.6 → 1.82.7 | clean | verdict; 0 critical capability findings |
| LiteLLM proxy context | 1.82.6 → 1.82.7 | malicious | verdict; 14 critical findings (two metadata endpoints, 12 executables) |
| Ultralytics | 8.3.40 → 8.3.41 | malicious | verdict; 6 critical findings (three endpoints, three curl paths) |
| ctx | 0.1.2 → 0.2.2 | malicious | verdict; 3 critical findings (endpoints) |
| DurableTask | target-only (no benign predecessor) | malicious | verdict; 3 endpoints, two of them sandbox infrastructure |
| Benign corpus | 100 pinned AI/data/ML-adjacent PyPI packages | 100 clean, 0 flagged | false-positive rate 0/100 (0%) |
| Ablation, Ultralytics | import-only probe: clean | function-level harness: malicious | 2 of 2 preregistered findings observed (import-only: none) |
| Ablation, ctx | import-only probe: clean | function-level harness: malicious | 1 of 2 preregistered findings observed (import-only: none) |

The LiteLLM pair is the cleanest test: the same two versions get a clean verdict for the client workload and a malicious verdict for the proxy workload, so the verdict follows the surface being exercised. The benign result covers 100 packages and only the workloads that were exercised, and the authors say it does not cover all API paths. The ablation has only two cases and only varies harness mode. Expected-finding coverage is partial: LiteLLM proxy and ctx matched only some of their preregistered findings, and DurableTask matched neither, even though its verdict is malicious.

## ⚠️ Limitations
- Reflection, dynamically constructed imports, callbacks, plugins, input-sensitive branches and environment-dependent behavior can fall outside the extracted or exercised surface.
- Containment can stop later stages of malicious execution only after earlier behavior has been observed, and the canonical fingerprint does not attribute behavior to installation, import or explicit API execution.
- A prior version is a behavioral reference, not proof of benignity, and target-only cases such as DurableTask cannot show what a particular version newly introduced.
- The ablation does not isolate iterative synthesis from single-shot generation, and it does not show that LLM-generated harnesses beat deterministic function-level harnesses with the same API knowledge.
- The synthesis loop closes over validator acceptance, not over runtime behavior; feeding traces back into probe choice is future work.
- The model alias was recorded without an immutable snapshot or seed, and sampling parameters other than temperature were not set.
- Untrusted artifacts run in Docker; the authors note that stronger isolation such as disposable microVMs could be added without changing the architecture.
- 🔎 Added context: Zero flagged out of 100 benign packages gives a 95% upper bound of roughly 3% on the false-positive rate (the rule of three), so this sample cannot show a rate much below that.
- 🔎 Added context: The four incidents have known malicious artifacts with verified provenance, which is an easier test than unknown malware; the harder question is whether the method finds behavior it was not tuned for.

## 🏗️ What it means for building systems
This is interpretation, not the authors' claim. The natural place for this check is a gate in CI or at deployment: on each dependency update, read the lockfile and the application code, run the generated harnesses in a sandbox, and block the release unless a valid signed clean certificate exists for that program hash and lockfile hash. The model cost is small (the paper reports 14 API calls, about $0.027 estimated, for five contexts), so the real costs are sandbox execution, harness validation, and keeping a benign baseline version for each package. To trust such a gate you would need to trust the deterministic validator, the sandbox configuration, the signing keys, and the baseline choice, and you would need to know which referenced surfaces the harness did not exercise.

## 🔑 Key takeaways
- The same package version can get different verdicts for different programs: LiteLLM 1.82.6 to 1.82.7 is clean for the client workload and malicious for the proxy workload.
- Probing the functions the program references exposed malicious behavior in 2 of 2 ablation cases, while import-only probing exposed none.
- The model is confined to writing harness source, and deterministic validation, sandboxing and signing make the verdict.
- Zero of 100 benign PyPI packages were flagged, but only for the workloads the paper exercised, not for all API paths.
- Verdict and expected-finding coverage are recorded separately; DurableTask was malicious while matching neither preregistered category.
- Without a benign predecessor, the tool reports observed behavior but cannot say what a version newly introduced.

## 📚 Reading plan
### From the paper
- [Datadog malicious software packages dataset](https://github.com/DataDog/malicious-software-packages-dataset) — the human-vetted dataset the incident artifacts come from; read it to see how the labels are built.
- [GuardDog](https://github.com/DataDog/guarddog) — the static and metadata-based scanner the paper contrasts with its behavioral approach.
- [OpenSSF Package Analysis](https://openssf.org/package-analysis/) — registry-scale behavioral analysis, the nearest alternative to a program-scoped run.
- [PyPI digital attestations](https://docs.pypi.org/attestations/) — the provenance layer that Preflight complements rather than replaces.
- [SLSA v1.2](https://slsa.dev/spec/v1.2/) — the supply-chain guarantees specification behind the provenance discussion.
- [Differential static analysis for malicious updates (SCORED '23)](https://doi.org/10.1145/3605770.3625211) — the version-differencing idea, applied statically.
- [CoverUp](https://doi.org/10.1145/3729398) — LLM test generation with coverage feedback, the closest analogue to the harness loop.
### To go further
- Duan et al., "Towards measuring supply chain attacks on package managers for interpreted languages" (NDSS 2021) — how these attacks reach package managers.
- Ferreira et al., "Containing malicious package updates in npm with a lightweight permission system" (ICSE 2021) — runtime permission limits for suspicious updates.
- "in-toto: Providing farm-to-table guarantees for bits and bytes" (USENIX Security 2019) — the attestation framework behind the signed statement.
- DySec, ML-based dynamic analysis for malicious PyPI packages (IEEE TIFS 2026) — a learned alternative to rule-based behavioral verdicts.

## 🔗 Concepts
- [ai-agent](/concepts/ai-ml/ai-agent.md)
- [threat-modeling](/concepts/security/threat-modeling.md)
- [zero-trust-architecture](/concepts/security/zero-trust-architecture.md)
- [Supply-chain attack](/concepts/security/supply-chain-attack.md)
