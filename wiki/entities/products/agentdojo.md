---
type: entity
entity_type: product
title: "AgentDojo"
created: 2026-05-02
updated: 2026-09-28
tags:
  - entities
  - products
  - benchmarks
  - red-team
  - prompt-injection
  - academic
status: developing
scope_axis:
  - sec-of-ai
publisher: "Academic (NeurIPS 2024)"
license: "open-source"
canonical_paper: "https://arxiv.org/abs/2406.13352"
related:
  - "[[source-triangulation-audit-2026-05-02]]"
  - "[[pyrit]]"
  - "[[garak]]"
  - "[[promptfoo]]"
  - "[[mindgard-cart]]"
  - "[[llamafirewall]]"
  - "[[llamafirewall-2025]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
sources:
  - "https://arxiv.org/abs/2406.13352"
  - "https://arxiv.org/abs/2505.03574"
---

# AgentDojo

**Sources:** [AgentDojo benchmark paper](https://arxiv.org/abs/2406.13352), [LlamaFirewall evaluation](https://arxiv.org/abs/2505.03574).

AgentDojo is a peer-reviewed academic benchmark for prompt-injection attacks against tool-using AI agents. Published at NeurIPS 2024, it evaluates realistic agent tasks under attack across multiple language-model targets. It provides an independent test set alongside red-team tools such as [[pyrit|PyRIT]], [[garak|Garak]], [[promptfoo|Promptfoo]], and [[mindgard-cart|Mindgard CART]].

## Benchmark scope

| Measure | Result |
|---|---|
| Test set | [97 tasks and 629 security cases](https://arxiv.org/abs/2406.13352) |
| Attack success against best-performing agents | Below 25% |
| Tool-filtering defense | Attack success reduced to 7.5% |
| Publication | NeurIPS 2024, peer-reviewed |

## External evaluations

On AgentDojo, Meta reports that [[llamafirewall|LlamaFirewall]] PromptGuard 2 reduced attack success from 17.6% to 7.5%, then to 1.75% when combined with AlignmentCheck ([evaluation](https://arxiv.org/abs/2505.03574)).

Uber uses AgentDojo as a public cross-check for [[adr-agentic-detection-system|ADR]]. On a 93-task split, ADR reported perfect recall across 38 attacks, three false alarms among 55 benign tasks, and an F1 score of 0.962.[^adr]

[^adr]: §5 *Evaluation*, Table 2, [arXiv:2605.17380](https://arxiv.org/abs/2605.17380): ADR scores precision 0.927, recall 1.000, F1 0.962 on the 93-task AgentDojo split (38 malicious / 55 benign).

## Evaluation role

Published prompt-injection detection rates for [[anthropic|Anthropic]] Constitutional Classifiers, Meta LlamaFirewall, and [[promptfoo|Promptfoo]] come primarily from vendor self-evaluations. AgentDojo supplies an academic comparator. Meta's use of the benchmark lets vendor-published results be compared with results in independent papers that use the same test set.

The [[agentic-ai-security-cmm-2026|CMM]] [[agentic-ai-security-cmm-d7-observability|D7]] L4 multi-tool red-team criteria specify two testing categories but do not require an independent benchmark. A proposed refinement would add one independent benchmark, such as AgentDojo, InjecAgent, or WASP. Under that proposal, a mature D7 L4 program would report vendor self-evaluation and benchmark results for the same defense.

## Comparison with red-team tools

| Tool | Type | Assessment basis |
|---|---|---|
| [[pyrit\|PyRIT]] | Multi-turn orchestration framework | Organizations run their own attacks |
| [[garak\|Garak]] | Probe library | NVIDIA-published probes |
| [[promptfoo\|Promptfoo]] | Regression suite | Vendor suite, now part of OpenAI |
| [[mindgard-cart\|Mindgard CART]] | Continuous SaaS | Commercial vendor library |
| **AgentDojo** | Academic benchmark | Peer-reviewed at NeurIPS 2024 |

## Related benchmarks

- **InjecAgent** ([arXiv:2403.02691](https://arxiv.org/abs/2403.02691)) tests indirect prompt injection; ReAct GPT-4 was vulnerable in 24% of cases.
- **WASP** ([arXiv:2504.18575](https://arxiv.org/pdf/2504.18575)) tests prompt injection against web agents.
- [[adr-bench|ADR-Bench]] tests MCP-native enterprise detection across 302 tasks, 133 MCP servers, and all 17 of 17 techniques. It provides an enterprise comparator to AgentDojo's prompt-injection focus.

## Related material

- [[source-triangulation-audit-2026-05-02|Source Triangulation Audit 2026-05-02]], Claim 5.
- [[pyrit|PyRIT]], [[garak|Garak]], [[promptfoo|Promptfoo]], and [[mindgard-cart|Mindgard CART]] provide the vendor red-team toolchain for comparison with AgentDojo.
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] D7 L4.
