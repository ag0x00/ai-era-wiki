---
type: entity
entity_type: product
title: "Onyx Platform (Onyx AI Control Plane)"
homepage: "https://onyx.security/platform"
created: 2026-05-03
updated: 2026-09-18
tags:
  - products
  - ai-control-plane
  - guardian-agent
  - ai-spm
  - ai-gateway
  - cots
status: developing
vendor: "Onyx Security"
related:
  - "[[onyx-security]]"
  - "[[guardian-agent]]"
  - "[[onyx-platform-open-questions]]"
  - "[[oversight-layer]]"
  - "[[ai-spm]]"
  - "[[agent-observability]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[wiz-ai-spm]]"
  - "[[palo-alto-prisma-airs]]"
  - "[[microsoft-agent-365]]"
sources:
  - ".raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md"
  - "https://onyx.security/platform"
verified: 2026-09-18
verified_against:
  - ".raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md"
verified_findings: 0
verified_note: "Guardian Agent scale figures now footnoted to the vendor page; callout-to-prose conversion preserves the vendor-claim framing."
---

# Onyx Platform (Onyx AI Control Plane)

**Sources:** [Onyx Platform (homepage)](https://onyx.security/platform)

The Onyx Platform is a unified **AI control plane** product positioned as a single console for observability, security, governance, orchestration, and ROI tracking across enterprise AI deployments. Marketing language calls it a "Secure AI Control Plane for Enterprises." The platform is built around a flagship "Onyx Guardian Agent" that operates across the platform's domains.

The product fits the [[guardian-agent|Guardian Agent]] vendor category as defined by [[gartner|Gartner]] (Feb 2026 Market Guide) — supervisory AI that monitors and governs other AI agents.

## Five product surfaces

Per Onyx's marketing site, the platform is organized into five concurrent capability surfaces, each presented as a co-equal pillar of the offering:

| Surface | What's claimed |
|---|---|
| **AI Observability** | Real-time visibility into prompts, responses, agent interactions; full session replay; shadow AI detection; multi-cloud / multi-agent unified view; anomaly detection and behavioral baselining |
| **AI Security** | AI-SPM; supply-chain risk for agents/MCP/models/AI assets; automated red teaming; real-time prompt/response/action protection; SIEM/SOAR integration |
| **AI Governance** | Policy templates aligned to MITRE / NIST / OWASP / EU AI Act; natural-language policy creation; tool sanctioning + MCP server access control |
| **AI Orchestration** | Centralized AI traffic on a fully-managed OSS AI gateway; smart LLM routing for cost / latency / accuracy; inline MCP gateway; A/B testing; cost optimization |
| **AI ROI** | Adoption tracking by department/team/individual; productivity metrics; cost-benefit analysis; executive dashboards |

The combined-surface positioning is broader than any single specialist competitor. Closest single-vendor analogues: [[palo-alto-prisma-airs|Prisma AIRS]] (security + posture + red-team in one), [[wiz-ai-spm|Wiz AI-SPM]] (posture + observability), [[agentgateway|AgentGateway]] (orchestration). Onyx claims to span the union of those scopes plus governance + ROI tracking.

## Onyx Guardian Agent

The product's centerpiece is the **Onyx Guardian Agent** — described as a "supervisory AI that continuously works across the platform to identify risks and remediate issues." Per Onyx's marketing claims as of 2026-05-03:

- 137,000+ agents secured across enterprise deployments[^guardian-stats]
- 593,000+ employees covered across deployments[^guardian-stats]
- 10M+ sessions analyzed for threats in real-time[^guardian-stats]

These numbers should be treated as vendor-published claims pending independent triangulation.

## Deployment

| Property | Detail |
|---|---|
| Deployment options | Cloud, hybrid, or self-hosted (advertised: AWS VPC, Bedrock Gateway, custom proxy configurations) |
| Time to deploy | "Hours" per marketing copy |
| Integrations | "100+ pre-built" — claims AWS, GCP, Azure, OpenAI, Anthropic, browser, AI platforms, CNAPP, SASE, EDR sources |

## Role in the RA

If Onyx delivers all five surfaces as advertised, the product would touch every plane in the [[agentic-ai-security-reference-architecture|Agentic AI Security RA]]:

| Plane | Onyx claim |
|---|---|
| **Identity** | Discovery integration with browser, AI platforms, CNAPP, SASE, EDR sources |
| **Control** | Natural-language policy creation; tool sanctioning; MCP server access control (governance / posture-side, not PDP enforcement) |
| **Runtime** | **Runtime protection** — real-time prompt / response / action interception; Guardian Agent intervention on detected risks |
| **Egress** | Managed OSS AI gateway; inline MCP gateway; per-request logging and guardrails |
| **Data** | Supply-chain risk for agents / MCP / models / AI assets |
| **Observability** | Session replay, audit trail, anomaly detection, behavioral baselining; SIEM/SOAR integration |

This breadth is its competitive positioning **and** its primary skepticism vector — single-vendor coverage of all six planes is unusual; specialist tools typically dominate any individual plane. Note the precision distinction: Onyx's strongest specific claim is **runtime protection** (in-line interception of prompts/responses/actions), which is much narrower than the generic "AI control plane" framing on the marketing site. The latter is positioning language; the former is the load-bearing technical capability.

## Comparison with peers

| Comparison | Onyx Platform | Alternative |
|---|---|---|
| vs [[palo-alto-prisma-airs\|Prisma AIRS]] | Broader surface (adds orchestration + ROI); newer/smaller vendor | More mature; backed by PA portfolio integration |
| vs [[wiz-ai-spm\|Wiz AI-SPM]] | Adds runtime protection + orchestration | Deeper graph + multi-cloud coverage |
| vs [[agentgateway\|AgentGateway]] | Includes a managed AI gateway and an MCP gateway as one of five surfaces | OSS, Linux Foundation governance, narrower scope |
| vs Single guardian-agent products | Combines guardian-agent role with AI gateway and ROI tracking | Most guardian-agent-only products focus narrowly on supervision |

## Critical assessment

**Independent validation of Onyx's claims is limited.** The Onyx product page is the primary public source for capability claims; independent third-party validation — analyst write-ups, customer case studies, security research — is limited as of 2026-05-03. The five-surface positioning is the vendor's ambition rather than confirmed delivery: a buyer needs to check which surfaces are GA versus roadmap, the depth behind the "100+ integrations" claim, whether the Guardian Agent's capability matches its marketing description, and the customer-count and session-volume figures above.[^guardian-stats]

**The "AI control plane" label is marketing scaffolding, not a settled category.** Onyx's marketing uses "AI Control Plane" as the umbrella positioning and "Guardian Agent" as the centerpiece, but neither term is Onyx's own coinage. "Guardian Agent" tracks [[gartner|Gartner]]'s [[guardian-agent|Guardian Agent]] vendor category from the Feb 2026 Market Guide; reaching for an analyst category rather than a company-specific one signals that the positioning is still being defined. The architecturally precise label for what Onyx does is closer to **runtime protection + AI-SPM + AI gateway**, layered together: the product capability claims (per-prompt/response/action interception, MCP gateway, behavioral baselining) are the load-bearing description, and the "AI control plane" wrapper may not survive into mature category language.

Founding team and funding details, per-surface GA dates, pricing, named customer references, and the identity of the upstream OSS AI gateway are not in the marketing source clipped to `.raw/`; [[onyx-platform-open-questions|Onyx Platform: Open Questions]] tracks these.

## CMM positioning

If the platform delivers as advertised, an Onyx-anchored deployment would target:
- **[[agentic-ai-security-cmm-d2-identity|D2 (Identity & Authorization)]] L3+** via discovery and policy enforcement
- **[[agentic-ai-security-cmm-d3-control-least-agency|D3 (Control & Least-Agency)]] L3** via the Guardian Agent's per-action interception and human oversight
- **[[agentic-ai-security-cmm-d4-runtime-guardrails|D4 (Runtime & Guardrails)]] L3+** via real-time prompt, response, and action protection
- **[[agentic-ai-security-cmm-d7-observability|D7 (Observability & Detection)]] L4** via session replay, behavioral baselining, and a SIEM/SOAR-integrated audit trail
- **[[agentic-ai-security-cmm-d8-supply-chain|D8 (Supply Chain & AI-BOM)]] L3** via supply-chain risk coverage

The product is a candidate for the **enterprise recommended stack** as a "single-pane-of-glass" alternative to assembling Wiz AI-SPM + Prisma AIRS + AgentGateway separately. Validation of that positioning requires independent assessment beyond the marketing source.

## Notes

[^guardian-stats]: [Onyx — Platform: Secure AI Control Plane for Enterprises](https://onyx.security/platform) (retrieved 2026-05-03). Vendor-published Guardian Agent scale claims (agents secured, employees covered, sessions analyzed), unaudited as of the clip date. Local copy: `.raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md`.
