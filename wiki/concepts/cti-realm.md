---
type: concept
title: "CTI-REALM Benchmark"
address: c-000098
created: 2026-05-23
updated: 2026-05-23
tags:
  - concepts
  - benchmarks
  - detection-engineering
  - ai-in-sec-defense
  - evaluation
status: developing
scope_axis:
  - ai-in-sec-defense
aliases:
  - "CTI-REALM"
coined_by:
  - "[[microsoft]]"
related:
  - "[[cybergym]]"
  - "[[exploit-benchmarks]]"
  - "[[ai-vuln-discovery-benchmark-landscape]]"
  - "[[mdash]]"
  - "[[microsoft]]"
  - "[[osint-to-knowledge-graph-talk]]"
sources:
  - "https://www.microsoft.com/en-us/security/blog/2026/03/20/cti-realm-a-new-benchmark-for-end-to-end-detection-rule-generation-with-ai-agents/"
  - "https://arxiv.org/pdf/2603.13517"
  - "https://ukgovernmentbeis.github.io/inspect_evals/evals/cybersecurity/cti_realm/index.html"
---

# CTI-REALM Benchmark

**CTI-REALM** is Microsoft Research's open-source benchmark for **end-to-end detection-rule generation** — the defender-side counterpart to the offense-leaning exploit benchmarks. Published 2026-03-20 ([arXiv 2603.13517](https://arxiv.org/pdf/2603.13517)), it measures whether an AI agent can convert cyber threat intelligence into *validated* detections, not merely answer a quiz. (Confidence: **high** on structure; **medium** on exact per-model scores — the blog reports ranges, not a full table.)

## Measurement scope

The benchmark replicates a detection engineer's full workflow: read a CTI report, explore telemetry, write and iteratively refine KQL queries, and produce validated detection logic (Sigma rules + KQL). It scores intermediate decision quality at checkpoints — CTI comprehension, MITRE ATT&CK technique mapping, data-source identification, query refinement — rather than only the final rule. Emulated attacks span Linux, cloud, and Azure Kubernetes Service, with ground-truth labels.

Two tiers exist — **CTI-REALM-25** and **CTI-REALM-50** (25 and 50 tasks, atomic attacks through multi-step intrusions); the published results use CTI-REALM-50, built from 37 curated public CTI reports. The metric is a reward score on a 0–1 scale.

## Results

Across 16 frontier models, [[microsoft|Microsoft]] reports that **Claude occupies the top three positions, with reward 0.624–0.685**; [[mythos|Claude Opus 4.6]] (High) leads at roughly **0.637**, with GPT-5 variants following. The blog gives ranges rather than a full per-model table.

> [!gap] Per-model table not yet sourced
> The blog publishes only the top range (0.624–0.685). The arXiv paper and the [UK AISI inspect_evals harness](https://ukgovernmentbeis.github.io/inspect_evals/evals/cybersecurity/cti_realm/index.html) — which integrates CTI-REALM — are the candidates for the full leaderboard.

## Significance

CTI-REALM fills a quadrant the exploit benchmarks miss: **defensive detection engineering**. Where [[cybergym|CyberGym]] measures vulnerability reproduction and [[exploit-benchmarks|ExploitBench/ExploitGym]] measure exploit construction (both offense-leaning), CTI-REALM measures whether agents can convert intelligence into deployed defenses — the SOC-automation capability behind [[mdash|MDASH]] and the agentic-SOC thesis. Microsoft open-sourced it so the industry can test models against a shared detection-engineering standard. See the [[ai-vuln-discovery-benchmark-landscape|benchmark landscape]] for where it fits. [[osint-to-knowledge-graph-talk|Sun's Palo Alto pipeline]] is a production instance of converting unstructured OSINT threat reports into a queryable, source-cited knowledge graph.

<!-- sources:auto -->
## Sources

- [microsoft.com](https://www.microsoft.com/en-us/security/blog/2026/03/20/cti-realm-a-new-benchmark-for-end-to-end-detection-rule-generation-with-ai-agents/)
- [arxiv.org](https://arxiv.org/pdf/2603.13517)
- [ukgovernmentbeis.github.io](https://ukgovernmentbeis.github.io/inspect_evals/evals/cybersecurity/cti_realm/index.html)
<!-- /sources -->
