---
type: entity
entity_type: product
title: "ADR-Bench"
address: c-000213
created: 2026-06-17
updated: 2026-06-17
tags:
  - entities
  - products
  - benchmarks
  - mcp-security
  - detection-engineering
  - ai-in-sec-defense
  - sec-of-ai
status: developing
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
publisher: "Uber / MIT / University of Oxford (MLSys 2026)"
license: "open-source (GitHub)"
canonical_paper: "https://arxiv.org/abs/2605.17380"
related:
  - "[[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]]"
  - "[[agentdojo|AgentDojo]]"
  - "[[defensebench|DefenseBench]]"
  - "[[mcp-security|MCP Security]]"
  - "[[uber|Uber]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
sources:
  - "https://arxiv.org/abs/2605.17380"
  - "[[.raw/papers/adr-agentic-detection-system-2026-05-17.md]]"
---

# ADR-Bench — MCP-Native Agent Security Benchmark

A defender-side benchmark for detecting attacks against AI agents that operate through the [[mcp-security|Model Context Protocol]], released with the [[adr-agentic-detection-system|ADR system paper]] (MLSys 2026). It is derived from real enterprise telemetry at [[uber|Uber]] rather than synthesized from scratch, and is published on GitHub with the ADR sensor and detection framework.[^bench]

## Definition

| Property | Detail |
|---|---|
| Tasks | 302 (42 malicious / 260 benign — 13.9% attack prevalence)[^bench] |
| MCP servers | 133, spanning 14 categories (81.2% benign / 18.8% malicious)[^bench] |
| Tools | 729 distinct; benign servers expose more (median 7) than malicious ones (median 3)[^bench] |
| Threat coverage | 17 techniques across 5 tactics — full 17/17 framework coverage[^bench] |
| Task complexity | 28.5 MCP tool calls per task on average[^bench] |
| Provenance | Adapted benchmarks (MCP-Artifact, RAS-Eval) + public incident disclosures + Uber internal threat intel[^bench] |
| Release | Task specs, runnable MCP registry, evaluation pipeline, and a minimal JSON config; sensitive identifiers replaced with safe stand-ins[^bench] |

## Relevance to this corpus

ADR-Bench's claimed differentiator is **realism under enterprise conditions** that prior agent benchmarks omit: native MCP context, severe class imbalance (benign dominates), the presence of sensitive information, and policy-violation tasks (agent actions not inherently malicious but violating internal policy, scored against a YAML policy store exposed over MCP).[^bench] On the paper's own comparison (Table 1) it is the only listed benchmark with MCP support and full 17/17 technique coverage, where peers cover 3–6 of 17.[^bench]

This places it as the **enterprise, MCP-native comparator** in the agentic-security benchmark landscape, complementary to [[agentdojo|AgentDojo]] (independent prompt-injection, 4/17) and [[defensebench|DefenseBench]] (defender-agent investigation on Splunk BOTSv3). For the [[agentic-ai-security-cmm-2026|CMM]] D7 evidence requirement, it offers an MCP-aware detection benchmark to report alongside AgentDojo.

> [!gap] Single-organization provenance
> ADR-Bench is derived from one enterprise's SOC telemetry and macOS coding-agent deployment. Its task mix and class balance model Uber's environment; external validity to other enterprise agent fleets is asserted by design intent, not yet measured by independent re-evaluation. Cross-vendor leaderboard adoption is the test it has not yet faced.

## Reported results on it

ADR reports perfect precision (1.000), zero false positives, and 0.667 recall (0.800 F1), 2–4x the F1 of the three baselines, which each fire 30–40 false positives across the 260 benign tasks.[^eval]

| Detector | Precision | Recall | F1 | FP (of 260) |
|---|---|---|---|---|
| ADR | 1.000 | 0.667 | 0.800 | 0 |
| ALRPHFS | 0.333 | 0.405 | 0.366 | 34 |
| GuardAgent | 0.231 | 0.214 | 0.222 | 30 |
| [[llamafirewall\|LlamaFirewall]] | 0.167 | 0.190 | 0.178 | 40 |

## See Also

- [[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]] — the system this benchmark validates
- [[agentdojo|AgentDojo]] · [[defensebench|DefenseBench]] — the independent and defender-agent comparators
- [[ai-vuln-discovery-benchmark-landscape|AI Vuln-Discovery Benchmark Landscape]] — the offense-side benchmark stack this mirrors on the defense side

[^bench]: §4 *ADR-Bench* and Table 1, [arXiv:2605.17380](https://arxiv.org/abs/2605.17380): 302 tasks (42/260), 133 servers across 14 categories, 729 tools, median tools per server, 28.5 tool calls/task, three provenance sources, policy-violation tasks and YAML policy store, full 17/17 coverage versus prior benchmarks, and the GitHub release with safe stand-ins.
[^eval]: §5 *Evaluation*, Table 2, [arXiv:2605.17380](https://arxiv.org/abs/2605.17380): precision/recall/F1 and false-positive counts on ADR-Bench for ADR and the three baselines.
</content>
