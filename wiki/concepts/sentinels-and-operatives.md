---
type: concept
title: "Sentinels and Operatives"
created: 2026-05-01
updated: 2026-05-01
tags:
  - concepts
  - guardian-agent
  - runtime-architecture
  - observability
status: developing
scope_axis:
  - sec-of-ai
complexity: intermediate
domain: runtime-architecture
no_public_url: "Gartner Market Guide terminology; source is paywalled with no stable public canonical URL (gated vendor reprints only)."
aliases:
  - "Sentinels"
  - "Operatives"
  - "Guardian Agent runtime split"
related:
  - "[[guardian-agents-market-guide]]"
  - "[[guardian-agent]]"
  - "[[agent-observability]]"
  - "[[agentic-ai-security-reference-architecture]]"
sources:
  - "[[.raw/articles/gartner-market-guide-for-guardian-agents-2026-05-01.md]]"
coined_by:
  - "[[gartner]]"
---

# Sentinels and Operatives

A runtime architectural split introduced in Gartner's Figure 1, separating the **assurance/posture** surface from the **enforcement** surface within a [[guardian-agent|guardian-agent]] deployment.

## The split

| Role | Function | Outputs |
|---|---|---|
| **Sentinels** | Provide environmental context, posture assessment, situational awareness | Continuous signals: agent inventory, behavioral baselines, risk scores, threat intel, compliance state |
| **Operatives** | Act at runtime to identify risks/threats and prioritize responses | Interventions: blocks, redirects, downgrades to lower autonomy tier, escalations to humans, autoremediation |

Sentinels feed Operatives. The data flow is one-way at runtime; Operatives may emit decisions back into the catalog/audit trail that Sentinels then incorporate into next-cycle posture.

## Significance of the split

Without it, three failure modes are common in guardian-agent deployments:

1. **Monolithic monitoring + enforcement.** A single agent that does both assurance and runtime enforcement is harder to reason about, harder to scale, and harder to govern. Gartner's split is the same separation-of-concerns pattern that EDR (telemetry collector + policy engine) and SIEM/SOAR (signal aggregator + automated response) already use; applying it to AI agent oversight is the natural extension.
2. **Latency budget mismatch.** Assurance needs continuous, broad-context evaluation (slow, comprehensive). Enforcement needs sub-second decisions (fast, narrow). Forcing both into one component compromises one of them. The split lets each operate at its native cadence.
3. **Failure-isolation.** If the Sentinel layer goes down, Operatives can fall back to last-known posture. If an Operative misfires, the Sentinel can detect and route to a different Operative or escalate to a human. Single-component deployments have no equivalent fallback.

## Mapping to the wiki's RA

The wiki's [[agentic-ai-security-reference-architecture|six-plane RA]] already separates the Observability plane from the Runtime+Control planes. Sentinels and Operatives **refine** that split at the implementation layer:

| Wiki RA plane | Sentinel role | Operative role |
|---|---|---|
| Identity | Catalog + agent cards | Identity-based revocation |
| Control | (none) | PDP — issues/denies capability tokens at runtime |
| Runtime | (none) | Lifecycle hooks; in-line guardrails (LlamaFirewall etc.) |
| Egress | (none) | AgentGateway — broker decisions per tool call |
| Data | Posture: AI-BOM, lineage, classification | Memory-poisoning blocks; RAG output filtering |
| Observability | Behavioral baselines; AI-SPM posture | (none — observability is Sentinel-side) |

Said differently: **Sentinels live in the Identity, Data, and Observability planes**. **Operatives live in the Control, Runtime, and Egress planes.** This is a useful design rule: when you're choosing where a new control belongs, ask "is this Sentinel work (telling Operatives what's true) or Operative work (deciding what to do)?"

## Common implementations

| Sentinel implementations (2026) | Operative implementations (2026) |
|---|---|
| Wiz AI-SPM | AgentGateway (Linux Foundation) |
| Palo Alto Prisma AIRS | LlamaFirewall PromptGuard 2 / AlignmentCheck / CodeShield |
| Vectra AI (agent behavioral monitoring) | NVIDIA NeMo Guardrails |
| Microsoft Defender for Cloud Apps (memory-injection detection) | Microsoft Agent 365 (real-time policy enforcement) |
| Miggo Security DeepTracing (behavioral baseline) | AWS Bedrock Guardrails |
| OWASP Agent Observability Standard | Google ADK `before_model_callback` / `before_tool_callback` |

A complete guardian-agent deployment typically uses 2–4 Sentinel products and 2–4 Operative products, integrated via the gateway or orchestration layer.

## Implications for the CMM

[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] D7 (Observability & Detection) and D4 (Runtime & Guardrails) already split assurance from enforcement. Adopting Sentinels/Operatives terminology in the CMM:

- **D7 L3+ evidence** should explicitly cite which Sentinels are deployed and what signals they emit
- **D4 L3+ evidence** should explicitly cite which Operatives consume those signals and what actions they take
- **D7→D4 integration** becomes a measurable: are Sentinel signals actually feeding Operative decisions, or are they collected and ignored?

This sharpens the L3 vs L4 distinction at both domains.

## Open question

The wiki's existing [[agent-observability|Agent Observability]] page documents a wide range of telemetry primitives (hooks, OTel, Cedar, behavioral monitoring) without explicitly naming the Sentinel/Operative roles. A future revision should adopt this terminology — likely as a top-level framing section before the existing §1–§8.

## See Also

- [[guardian-agent|Guardian Agent]] — the abstraction Sentinels and Operatives implement
- [[guardian-agents-market-guide|Gartner Market Guide for Guardian Agents (Feb 2026)]] — primary source (Figure 1)
- [[agent-observability|Agent Observability]] — Sentinel-side telemetry
- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] — six-plane decomposition
