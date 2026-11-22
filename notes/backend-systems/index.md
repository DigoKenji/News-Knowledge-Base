# Notes — Backend & Systems Engineering

- [Muru](/notes/backend-systems/2026-11-22-muru.md) — Muru is an AI-assisted Rust server for Python web apps, and its author's judgment, not the AI, sets the quality bar.
- [Uber Cuts Translation Cache Memory by up to 90% While Keeping P99 Latency Below 1 MS](/notes/backend-systems/2026-11-21-uber-cuts-translation-cache-memory-by-up-to-90-while-keeping.md) — Uber cut its Go translation cache's initial memory over 90% by keeping keys and hot values in RAM, spilling the rest to disk.
- [LinkedIn Cuts Daily Spark Shuffle Fetch Failures from 750 to 20 with Apache Celeborn](/notes/backend-systems/2026-11-19-linkedin-cuts-daily-spark-shuffle-fetch-failures-from-750-to.md) — Moving shuffle storage to dedicated Celeborn workers cut LinkedIn's daily Spark shuffle fetch failures from 750 to 20.
- [Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity](/notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md) — Refusal-driven dispatch with per-replica caps keeps LLM fleets busy as capacity shifts, serving a classifier on 43% fewer H100s.
- [15M web servers on one VM – smoltcp, DPDK, AF_XDP](/notes/backend-systems/2026-11-19-15m-web-servers-on-one-vm-smoltcp-dpdk-af-xdp.md) — Kernel socket memory and the 16-bit port space, not CPU, cap servers per VM; userland networking removes most of that cost.
- [How we raised Databricks cluster rate limits by 7.5×](/notes/backend-systems/2026-11-18-how-we-raised-databricks-cluster-rate-limits-by-7-5.md) — Fixed request-rate limits cannot protect long-running work, so control how many operations run as health changes.
- [Deterministic Simulation Testing](/notes/backend-systems/2026-11-17-deterministic-simulation-testing.md) — Distributed bugs become reproducible when a single-threaded core controls clock, network and disk progress through tick calls.
- (empty)
