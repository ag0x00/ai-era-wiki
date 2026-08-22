---
type: entity
entity_type: product
title: "Lakera Guard"
created: 2026-05-03
updated: 2026-05-03
tags:
  - products
  - prompt-injection
  - content-safety
  - runtime-security
  - cots
status: developing
vendor: "Lakera"
related:
  - "[[llamafirewall]]"
  - "[[indirect-prompt-injection]]"
  - "[[agent-sandboxing]]"
  - "[[agentic-ai-security-reference-architecture]]"
sources:
  - "https://platform.lakera.ai/docs"
---

# Lakera Guard

Lakera Guard is a commercial AI security API from Lakera (Swiss company) that provides real-time detection of prompt injection attacks, jailbreaks, sensitive data leakage, and unsafe content in LLM inputs and outputs. It is deployed as a middleware layer that intercepts requests and responses between the application and the LLM.

## Core detection capabilities

| Detection category | Description |
|---|---|
| **Prompt injection** | Direct and indirect prompt injection attempts in user inputs and retrieved content |
| **Jailbreaks** | Attempts to bypass safety instructions or system prompts |
| **PII / sensitive data** | Credit card numbers, SSNs, API keys, passwords in inputs or outputs |
| **Hate speech / harmful content** | Unsafe output classification |
| **Competitor mentions** | Configurable topic filtering |

The detection models are continuously updated based on Lakera's attack intelligence database, which aggregates novel injection and jailbreak patterns in near-real time from the [Gandalf game](https://gandalf.lakera.ai/) — a public red-teaming platform that has collected millions of adversarial prompts.

## Integration pattern

Lakera Guard operates as a **sidecar API** — the application routes prompts through Lakera before sending them to the LLM, and optionally routes responses through Lakera before returning them to users:

```
User input → Lakera Guard (input scan) → LLM → Lakera Guard (output scan) → User
```

Integration is via REST API; SDKs available for Python, TypeScript, LangChain, and major agent frameworks. Latency is typically 10–50ms per call.

## Comparison with LlamaFirewall

| Dimension | [[llamafirewall\|LlamaFirewall]] | Lakera Guard |
|---|---|---|
| Licensing | Open source (Meta) | Commercial SaaS |
| Deployment | Self-hosted | Cloud API (on-premises available) |
| Prompt injection recall | 97.5% (PromptGuard 2 benchmark) | Not publicly benchmarked at equivalent specificity |
| CoT auditing | Yes (AlignmentCheck) | No |
| Code scanning | Yes (CodeShield) | No |
| Data freshness | Static model releases | Continuous updates from Gandalf intelligence |
| Best for | FOSS/self-hosted deployments; coding agents | SaaS deployments; organizations needing managed service |

## Role in the RA Runtime plane

In the [[agentic-ai-security-reference-architecture|RA]] Runtime plane, Lakera Guard appears in the **Topic / content safety** row alongside NVIDIA NeMo Content Safety NIM, Lasso, and Microsoft Prompt Shields. The content safety row covers a broader capability than the LlamaFirewall row (input filtering / prompt injection detection) — Lakera Guard addresses both, making it a broader-scope alternative to LlamaFirewall for organizations that prefer a managed SaaS approach.

The enterprise recommended stack uses LlamaFirewall (OSS, no license cost) + NVIDIA NeMo NIMs (commercial inference); Lakera Guard is the alternative for teams that want a fully managed, continuously-updated detection service without self-hosting.
