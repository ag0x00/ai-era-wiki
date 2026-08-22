---
type: entity
entity_type: product
title: "Purple Llama"
created: 2026-06-23
updated: 2026-06-23
tags:
  - entities
  - products
  - guardrails
  - benchmarks
  - open-source
  - meta
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-offense
homepage: "https://github.com/meta-llama/PurpleLlama"
publisher: "Meta"
org_type: vendor
parent_org: "[[meta]]"
address: c-000232
related:
  - "[[llamafirewall]]"
  - "[[llamafirewall-2025]]"
  - "[[cyberseceval]]"
  - "[[meta]]"
  - "[[prompt-injection]]"
sources:
  - "[[.raw/articles/purple-llama-github-2026-06-23.md]]"
  - "https://github.com/meta-llama/PurpleLlama"
---

# Purple Llama

Meta's umbrella project for open trust-and-safety tooling around generative AI. The name signals a "purple team" stance — pairing offensive (red) measurement with defensive (blue) guardrails in one repository ([github.com/meta-llama/PurpleLlama](https://github.com/meta-llama/PurpleLlama), MIT for evals and Code Shield, Llama Community License for the safeguard models). It is the distribution home for [[llamafirewall|LlamaFirewall]] and the [[cyberseceval|CyberSecEval]] benchmark suite, which the wiki tracks as separate pages.

## Contents

### Safeguards (defensive)

| Component | Function |
|---|---|
| **Llama Guard** (3-8B, 3-1B, 3-11B-vision) | Input/output moderation against the MLCommons hazard taxonomy; 128k context, multilingual, image-capable variant |
| **Prompt Guard** | Classifier for [[prompt-injection\|prompt injection]] and jailbreak inputs (the lineage behind LlamaFirewall's PromptGuard 2) |
| **Code Shield** | Filters insecure LLM-generated code; the static-analysis engine reused as LlamaFirewall's CodeShield |
| **[[llamafirewall\|LlamaFirewall]]** | System-level guardrail framework composing PromptGuard 2 + AlignmentCheck + CodeShield |

### Evaluations (offensive measurement)

[[cyberseceval|CyberSecEval]] v1–v3 — benchmarks quantifying a model's insecure-code generation, compliance with malicious requests, prompt-injection susceptibility, and (in v3) autonomous offensive-operations and spear-phishing capability. This is the `ai-in-sec-offense` half of the project: measuring the uplift a model gives an attacker.

## Basis for both axes

Purple Llama spans two of the wiki's scope axes. The safeguards are `sec-of-ai` (defending the agent against injection and unsafe code). CyberSecEval is `ai-in-sec-offense` measurement — it scores how much offensive cyber capability a model supplies, the same quantity Google's [[google-big-sleep-projectzero|Naptime/Big Sleep]] work benchmarked against (Naptime reached state-of-the-art on CyberSecEval2). The two halves share the purple-team framing: tuning a defense requires a measurement of the offense it must withstand.

## See also

- [[llamafirewall|LlamaFirewall]] · [[llamafirewall-2025|the paper]] — the flagship guardrail framework
- [[cyberseceval|CyberSecEval]] — the benchmark suite
- [[meta|Meta]] — the publisher
- [[agentdojo|AgentDojo]] — the independent benchmark Meta pairs with its own CyberSecEval numbers
