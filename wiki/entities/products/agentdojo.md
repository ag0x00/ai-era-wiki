---
type: entity
entity_type: product
title: "AgentDojo"
created: 2026-05-02
updated: 2026-05-02
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
sources:
  - "https://arxiv.org/abs/2406.13352"
  - "https://arxiv.org/abs/2505.03574"
---

# AgentDojo — Independent Prompt-Injection Benchmark

A peer-reviewed, **independent** benchmark for prompt injection against tool-using AI agents. Published at NeurIPS 2024 (arXiv:2406.13352). Distinguishes itself from [[pyrit|PyRIT]] / [[garak|Garak]] / [[promptfoo|Promptfoo]] / [[mindgard-cart|Mindgard CART]] by being **academic and venue-validated**, not vendor self-evaluation.

## Definition

| Property | Detail |
|---|---|
| Scope | 97 tasks / 629 security cases for tool-using agent prompt injection |
| Methodology | Realistic agent tasks under attack across multiple LLM targets |
| Headline finding | Best agents <25% attack success; tool-filtering defense drops ASR to 7.5% |
| Use by vendors | Meta uses AgentDojo to evaluate [[llamafirewall\|LlamaFirewall]] PromptGuard 2 (ASR 17.6% → 7.5%; combined with AlignmentCheck 1.75%); Uber uses it as the public cross-check for [[adr-agentic-detection-system\|ADR]] (perfect recall on all 38 attacks, 3 false alarms of 55 benign, 0.962 F1)[^adr] |
| Venue | NeurIPS 2024 (peer-reviewed) |
| URL | [arxiv.org/abs/2406.13352](https://arxiv.org/abs/2406.13352) |

[^adr]: §5 *Evaluation*, Table 2, [arXiv:2605.17380](https://arxiv.org/abs/2605.17380): ADR scores precision 0.927, recall 1.000, F1 0.962 on the 93-task AgentDojo split (38 malicious / 55 benign).

## Relevance to this corpus

The wiki's prompt-injection detection-rate citations are mostly vendor self-evaluation: [[anthropic|Anthropic]] Constitutional Classifiers, Meta LlamaFirewall, [[promptfoo|Promptfoo]] regression numbers. **AgentDojo is the cleanest third-party comparator** — Meta's own evaluation uses it, which means the same benchmark numbers appear in vendor-published evaluations and in independent papers, making cross-comparison defensible.

For the wiki's [[agentic-ai-security-cmm-2026|CMM]] D7 L4 evidence requirement (multi-tool red-team eval), AgentDojo serves as the **independent benchmark anchor** that vendor self-evals are compared against. Mature D7 L4 programs should report both vendor-self-eval and AgentDojo numbers for the same defense.

## Distinction from vendor red-team tools

| Tool | Type | Self-eval bias |
|---|---|---|
| [[pyrit\|PyRIT]] | Multi-turn orchestration framework | DIY — orgs run their own attacks |
| [[garak\|Garak]] | Probe library | NVIDIA-published probes |
| [[promptfoo\|Promptfoo]] | Regression suite | Vendor (now part of OpenAI) |
| [[mindgard-cart\|Mindgard CART]] | Continuous SaaS | Commercial vendor library |
| **AgentDojo** | **Academic benchmark** | **Peer-reviewed; venue-validated** |

The wiki's CMM D7 L4 should require *at least one* independent benchmark (AgentDojo or InjecAgent or WASP) alongside the four-quadrant tool coverage to count as L4 evidence.

## Related benchmarks

- **InjecAgent** ([arXiv:2403.02691](https://arxiv.org/abs/2403.02691)) — indirect-prompt-injection benchmark; ReAct GPT-4 vulnerable in 24% of cases
- **WASP** ([arXiv:2504.18575](https://arxiv.org/pdf/2504.18575)) — web-agent security benchmark for prompt injection
- [[adr-bench|ADR-Bench]] — MCP-native, enterprise-realistic detection benchmark (302 tasks, 133 MCP servers, full 17/17 technique coverage); the enterprise comparator to AgentDojo's prompt-injection focus

## See Also

- [[source-triangulation-audit-2026-05-02|Source Triangulation Audit 2026-05-02]] — Claim 5
- [[pyrit|PyRIT]] · [[garak|Garak]] · [[promptfoo|Promptfoo]] · [[mindgard-cart|Mindgard CART]] — vendor red-team toolchain (AgentDojo is their independent comparator)
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] D7 L4
