---
type: entity
entity_type: product
title: "CyberSecEval"
created: 2026-06-23
updated: 2026-06-23
tags:
  - entities
  - products
  - benchmarks
  - offensive-security
  - insecure-code
  - meta
status: developing
scope_axis:
  - ai-in-sec-offense
  - sec-of-ai
publisher: "Meta (Purple Llama)"
license: "MIT"
address: c-000233
related:
  - "[[purple-llama]]"
  - "[[llamafirewall]]"
  - "[[agentdojo]]"
  - "[[google-big-sleep-projectzero]]"
  - "[[meta]]"
sources:
  - "[[.raw/articles/purple-llama-github-2026-06-23.md]]"
  - "https://github.com/meta-llama/PurpleLlama"
---

# CyberSecEval

**Sources:** [Purple Llama / CyberSecEval (repo)](https://github.com/meta-llama/PurpleLlama)

Meta's open benchmark suite (MIT-licensed, shipped in [[purple-llama|Purple Llama]]) for measuring the cybersecurity risk a language model carries: both the insecure code it writes and the offensive capability it can supply to an attacker. It is the measurement counterpart to Purple Llama's defensive guardrails, quantifying the `ai-in-sec-offense` risk a defender needs before tuning a `sec-of-ai` control.

## The three generations

| Version | Adds |
|---|---|
| **CyberSecEval v1** | First benchmarks grounded in CWE and MITRE ATT&CK; scores insecure-code suggestions and compliance with malicious requests |
| **CyberSecEval 2** | Code-interpreter abuse, offensive-capability tests, prompt-injection susceptibility; Hugging Face leaderboard |
| **CyberSecEval 3** | Visual prompt injection, spear-phishing assessment, autonomous offensive-operations tests |

## Use in this wiki

CyberSecEval is the recurring external yardstick behind two existing pages. Google Project Zero's [[google-big-sleep-projectzero|Naptime]] agent reached state-of-the-art on **CyberSecEval2** before evolving into Big Sleep; the benchmark is how that line of frontier vulnerability-discovery work measured its offensive uplift. **CyberSecEval3** supplied the manually labeled insecure-code completions (50 per language) used to validate CodeShield in the [[llamafirewall-2025|LlamaFirewall paper]].

The suite sits on the offense side of the same purple-team split that produces [[llamafirewall|LlamaFirewall]]: CyberSecEval measures the capability, the guardrails contain it. For the [[agentic-ai-security-cmm-2026|CMM]], it is a candidate evidence source wherever a domain calls for quantified model-capability risk rather than control presence.

## Distinction from AgentDojo

[[agentdojo|AgentDojo]] is a peer-reviewed *prompt-injection* benchmark for tool-using agents; CyberSecEval is a *model-capability* benchmark (insecure code, offensive uplift, phishing). They answer different questions. AgentDojo asks "does this defense stop the injection"; CyberSecEval asks "how dangerous is this model's raw capability." The LlamaFirewall evaluation draws on both.

## See also

- [[purple-llama|Purple Llama]] — the parent project
- [[llamafirewall-2025|LlamaFirewall paper]] — uses CyberSecEval3 for the CodeShield evaluation
- [[google-big-sleep-projectzero|Naptime / Big Sleep]] — benchmarked against CyberSecEval2
- [[agentdojo|AgentDojo]] — the prompt-injection-specific independent benchmark
