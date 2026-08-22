---
type: entity
entity_type: product
title: "Palo Alto Prisma AIRS (AI Runtime Security)"
created: 2026-05-03
updated: 2026-08-21
tags:
  - products
  - ai-runtime-security
  - prompt-injection
  - ai-spm
  - red-teaming
  - cots
status: developing
vendor: "Palo Alto Networks"
ga_date: "2025-04-28"
related:
  - "[[palo-alto-networks]]"
  - "[[wiz-ai-spm]]"
  - "[[llamafirewall]]"
  - "[[lakera-guard]]"
  - "[[mindgard-cart]]"
  - "[[ai-agents-are-here-so-are-the-threats-unit42]]"
  - "[[agentic-ai-security-reference-architecture]]"
sources:
  - "https://www.paloaltonetworks.com/prisma/prisma-ai-runtime-security"
  - "https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-introduces-prisma-airs--the-foundation-on-which-ai-security-thrives"
  - "[[.raw/articles/agentic-ai-threats-unit42-2025-05-01.md]]"
  - "https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-secures-the-ai-agent-revolution-with-the-launch-of-prisma-airs-2-0"
  - "https://pan.dev/airs/"
---

# Palo Alto Prisma AIRS (AI Runtime Security)

Prisma AIRS is [[palo-alto-networks|Palo Alto Networks]]' end-to-end AI security platform protecting the full lifecycle of AI applications and agents — model security, posture management, runtime firewalling, API-based inline guardrails, and AI red teaming. It is branded under the Prisma family alongside Prisma Cloud (CNAPP) and Prisma SASE (Access), positioned as the dedicated AI security pillar.

## Capabilities

| Capability | Description |
|---|---|
| **AI Runtime Firewall + AI Runtime API (API Intercept)** | Inline enforcement against prompt injection, jailbreaks, tool misuse, malicious agent behavior; PII/secret redaction; outbound data-leak prevention |
| **AI Model Security** | Model scanning for malicious models, vulnerabilities, supply-chain risk (from the Protect AI acquisition: Guardian, ModelScan tooling) |
| **AI Red Teaming** | Continuous autonomous adversarial testing of deployed models and agents |
| **Posture Management** | Configuration and access risk for AI services (distinct from Prisma Cloud's AI-SPM, which came from Dig Security) |
| **Agent defense** | Real-time agent behavior monitoring added in 2.0 (October 2025) |

## Deployment

SaaS control plane with multiple enforcement modes:

| Mode | Use case |
|---|---|
| **API-based intercept** | Developer-integrated guardrails via the AI Runtime API (documented at pan.dev/airs); applications POST prompts/responses for inspection before delivery |
| **Network-inline** | Existing PAN-OS NGFW or Prisma Access enforcement points inspect AI traffic |
| **SDK / proxy partner integrations** | [Portkey](https://www.paloaltonetworks.com/blog/2025/08/portkey-fortifies-ai-gateway-with-prisma-airs-platform/) AI Gateway (announced August 2025) and [LiteLLM proxy guardrails](https://docs.litellm.ai/docs/proxy/guardrails/panw_prisma_airs) — Prisma AIRS as a guardrail backend for OSS AI gateways |

## Role in the RA

In the [[agentic-ai-security-reference-architecture|Agentic AI Security RA]], Prisma AIRS appears in multiple planes:

| Plane | Capability | Role |
|---|---|---|
| **Runtime** | Input filtering / prompt-injection detection | Commercial alternative to [[llamafirewall\|LlamaFirewall]] / [[lakera-guard\|Lakera Guard]] |
| **Runtime** | Topic / content safety | PII/secret redaction; output filtering |
| **Egress** | Tool authorization (via API Intercept) | Inline policy enforcement on agent-to-tool communications |
| **Data** | Model scanning + supply-chain | From Protect AI integration |
| **Observability** | AI Security Posture Management | Distinct from Wiz AI-SPM; tighter PA stack integration |
| **Observability** | AI red teaming integration | Continuous CART; competes with [[mindgard-cart\|Mindgard CART]] |

The enterprise recommended stack lists Prisma AIRS for organizations with existing Palo Alto Networks platform commitments (Prisma SASE, Prisma Cloud, Cortex). Its strongest competitive position is **as a unified AI security pillar** integrated into a broader PA portfolio rather than as a best-of-breed point solution.

## Comparison with peers

| Comparison | Prisma AIRS | Alternative |
|---|---|---|
| vs [[llamafirewall\|LlamaFirewall]] (input filtering) | Commercial SaaS; managed updates; broader scope | OSS; self-hosted; published benchmarks |
| vs [[lakera-guard\|Lakera Guard]] (content safety) | Tighter PA portfolio integration; bundled with model scanning + red teaming | Specialist focus; Gandalf-fed continuous detection updates |
| vs [[wiz-ai-spm\|Wiz AI-SPM]] (posture) | Tied to PA stack; built on Dig Security acquisition | Multi-cloud graph; not tied to a runtime stack |
| vs [[mindgard-cart\|Mindgard CART]] (red teaming) | Bundled in 2.0; enterprise scope | Best-of-breed CART specialist |

The strategic positioning: Prisma AIRS is **breadth across the AI lifecycle**, sacrificing depth in any single capability for unified policy and reporting under the PA portfolio.

## Timeline

| Date | Event |
|---|---|
| **April 28, 2025** | Prisma AIRS launched (initial GA) |
| **August 2025** | Portkey AI Gateway integration with Prisma AIRS announced |
| **October 29, 2025** | Prisma AIRS 2.0 GA — Protect AI integration completes; agent-lifecycle protection expanded |
| **2026** | CyberArk acquisition by Palo Alto (~\$25B) — integration with [[cyberark-conjur\|CyberArk Conjur]] expected, bringing identity-side coverage under the Prisma AIRS umbrella |

Marketing positioning at 2.0 launch: "78% of organizations transforming with AI but only 6% have guardrails."

## CMM positioning

- **D4 (Runtime & Guardrails) L3+**: Inline prompt-injection, jailbreak, tool-misuse detection
- **D7 (Observability & Detection) L3**: AI-SPM via Dig Security lineage
- **D7 (Observability & Detection) L4**: Continuous AI red teaming (CART), which supplies the multi-category red-team eval D7 grades at L4
- **D8 (Supply Chain & AI-BOM) L3**: Model scanning from Protect AI integration

> [!note] Two AI-SPM products under Palo Alto
> Palo Alto has two distinct AI-SPM offerings: (1) **Prisma Cloud AI-SPM** — built on the Dig Security acquisition and integrated into Prisma Cloud CNAPP; (2) **Prisma AIRS posture management** — the AIRS-native posture component. Buyers evaluating Palo Alto's AI security portfolio should confirm which AI-SPM their license includes.
