---
type: concept
title: "Tiered Detection Cascade"
address: c-000215
created: 2026-06-17
updated: 2026-08-18
tags:
  - concepts
  - detection-engineering
  - agentic-soc
  - cost-economics
  - ai-in-sec-defense
status: developing
scope_axis:
  - ai-in-sec-defense
origin: aggregated
complexity: intermediate
domain: detection-engineering
aliases:
  - "Cost-ordered detection"
  - "Triage-then-reason"
  - "Two-tier detection"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]]"
  - "[[syara-semantic-detection-talk|SYARA — Semantic Detection]]"
  - "[[1-8m-prompts-30-alerts-talk|1.8M Prompts, 30 Alerts]]"
  - "[[prompt-injection-containment|Prompt Injection Containment]]"
  - "[[agent-observability|Agent Observability]]"
  - "[[llm-as-a-judge|LLM-as-a-Judge]]"
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
  - "[[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]]"
sources:
  - "[[.raw/papers/adr-agentic-detection-system-2026-05-17.md]]"
  - "https://arxiv.org/abs/2605.17380"
---

# Tiered Detection Cascade

## Definition

A detection architecture that orders stages by cost so that a cheap, high-recall pre-filter handles most events and gates a more expensive, high-precision stage that runs only on what the pre-filter flags. The economic premise is that running the most accurate detector (typically an LLM reasoning pass) on every event is unaffordable at agent-traffic volume, while running only a cheap detector misses sophisticated attacks. Cascading the two recovers most of the accuracy at a fraction of the cost.

## Timing of its emergence

Agentic traffic is both high-volume and heavily skewed toward benign activity, and the accurate detector is an LLM whose per-event cost is non-trivial. Those two facts make blanket LLM inspection economically impossible: production systems report tens of thousands of sessions daily and millions of prompts. The cascade is the standard response, spending almost nothing on the common case and real money only on the suspicious minority.

## The pattern

| Stage | Goal | Typical implementation | Tuned for |
|---|---|---|---|
| Pre-filter / triage | Catch everything suspicious cheaply | Regex, entropy, embeddings, or a small/cheap LLM | High recall, low cost |
| Deep stage | Decide precisely on flagged events | Reasoning LLM with context (source code, policy, threat intel) | High precision |

The design discipline is to keep the pre-filter conservative, escalating whenever in doubt, so that false negatives stay low and the precision burden moves to the deep stage. The cost lever is the escalation rate: the fraction of traffic the pre-filter passes upward sets both the spend and the precision/recall trade-off.

The discipline reaches a limit set by the pre-filter's mechanism rather than by its threshold. The [[owasp-ai-exchange|OWASP AI Exchange]] states that detection of statistically odd input is often ineffective against adversarial input, because such samples are designed to resemble normal input by definition, and it names embedding distance and cosine similarity among the measures such a detector uses.[^aix-anomalous] An embedding-similarity pre-filter therefore has recall bounded by construction against an adversary who knows it is there: escalating whenever in doubt requires doubt, and the attack is built to produce none. The bound applies to that detector class rather than to the cascade, since regex and small-LLM triage stages fail differently, and it changes what the escalation rate can be read as. A low escalation rate means a cheap common case where the adversary is not adapting to the pre-filter, and means very little where the adversary is. The Exchange also states the reverse case plainly: not all anomalous input is malicious, and not all malicious input is anomalous.[^aix-anomalous]

## Occurrences across independent sources

- **[[adr-agentic-detection-system|ADR]] (Uber, MLSys 2026).** Tier 1 is a high-recall LLM triage screen; Tier 2 is a Claude Sonnet 4 reasoning agent that queries MCP context. Triage resolves 40.7% of tasks at \$0.017 each; the reasoning path costs \$0.029. Removing the triage layer raises recall but adds false positives and increases cost 29% and latency 52% — quantifying the cascade's value directly.[^adr]
- **[[syara-semantic-detection-talk|SYARA]] (Palo Alto, Unprompted 2026).** Four matchers (string, similarity, classifier, LLM) execute in **cost order**, so a cheap pre-filter gates the expensive LLM call, reported as order-of-magnitude cost and latency reductions over an LLM-only approach.
- **[[1-8m-prompts-30-alerts-talk|Agentforce telemetry]] (Salesforce, Unprompted 2026).** A three-level ensemble of [[behavioral-anomaly-detection-for-agents|behavioral anomaly detection]] reduces 1.8M daily prompts to ≤30 actionable alerts, a cascade tuned to collapse volume before human review.

## Relationship to adjacent patterns

The cascade is an economic axis, distinct from the layered defense in [[prompt-injection-containment|Prompt Injection Containment]] (network → input-detection → execution-containment), which is organized by *where* enforcement sits rather than by *cost order*. A system can be both: ADR's Tier 1/Tier 2 is a cost cascade, while its sensor-plus-inline-hooks split is a containment layering. The deep stage frequently uses an [[llm-as-a-judge|LLM-as-a-judge]], inheriting that pattern's [[recursive-prompt-injection|recursive-injection]] exposure.

> [!gap] Escalation policy is the unsolved knob
> Each system sets the pre-filter's escalation threshold by hand, and the right operating point differs by enterprise (cost tolerance, class imbalance, miss cost). There is no shared methodology for choosing it; ADR notes only that it uses a precision-first setting to keep alert volume manageable under extreme class imbalance.

## See Also

- [[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]] — the two-tier production instance
- [[syara-semantic-detection-talk|SYARA]] — cost-ordered matchers
- [[agent-observability|Agent Observability]] — the telemetry the cascade consumes
- [[evaluating-ai-soc-agents|Evaluating AI SOC Agents]] — cost-per-outcome as a buyer criterion

[^adr]: §3.2 and §5.2–5.3, [arXiv:2605.17380](https://arxiv.org/abs/2605.17380): Tier 1 triage / Tier 2 reasoning, 40.7% of tasks at \$0.017 vs \$0.029 for the reasoning path, and the w/o-Triage ablation (recall and F1 rise; precision falls; cost +29%, latency +52%).
[^aix-anomalous]: [OWASP AI Exchange — ANOMALOUS INPUT HANDLING](https://owaspai.org/go/anomalousinputhandling/), retrieved 2026-08-18.

<!-- sources:auto -->
## Sources

- [ADR: An Agentic Detection System for Enterprise Agentic AI Security](https://arxiv.org/abs/2605.17380)
<!-- /sources -->
