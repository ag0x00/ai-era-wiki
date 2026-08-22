---
type: gap
title: "Agentic AI Security RA: Open Implementation Questions"
created: 2026-05-01
updated: 2026-05-01
tags:
  - gaps
  - reference-architecture
  - implementation
status: open
question: "When implementing the Agentic AI Security Reference Architecture in a specific enterprise, which design choices remain underspecified — and what is the right answer per deployment shape?"
why_it_matters: "The reference architecture defines the planes and the controls. It does not prescribe the implementation choices that distinguish a working deployment from a brittle one. These six questions consistently surface in implementation conversations and have no single canonical answer; they are tradeoffs the architect must own."
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agent-identity-architecture]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "[[.raw/papers/securing-the-autonomous-future.md]]"
---

# Agentic AI Security RA — Open Implementation Questions

Six implementation questions that the [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] does not prescribe. Each is a tradeoff with no universal right answer; the right answer depends on enterprise scale, deployment shape, and existing infrastructure.

## 1. Single-broker vs. mesh

For tool calls and MCP-server access, does the architecture run a **single central broker** (one PDP, one egress proxy, one identity provider) or a **mesh of co-located brokers** (sidecar per agent, sidecar per node)?

**Trade-off.** Single broker simplifies policy, audit, and revocation but creates a hard dependency and a scaling bottleneck. Mesh distributes load and limits blast radius but multiplies the policy-distribution problem and complicates audit aggregation.

**Where it usually lands.** Enterprises with established service-mesh investment (Istio, Linkerd) lean mesh. Enterprises building agentic infrastructure greenfield often start with a single broker and split later.

## 2. Policy decision point (PDP) placement

Where does the PDP that evaluates Cedar/OPA policy sit? Three plausible options:

- **Inline broker** — every tool call passes through it; lowest latency on policy hits but hardest to scale.
- **Sidecar** — co-located with the agent process; per-agent isolation; complicates centralized policy distribution.
- **External service** — clean abstraction, easy to scale; adds a network hop and creates fail-mode questions (see #6).

**Where it usually lands.** Latency-sensitive deployments (chatbots, coding agents) prefer inline or sidecar. Compliance-heavy deployments (financial-services agents, healthcare RAG) prefer external service for centralized audit.

## 3. Warrants vs. existing IAM/PAM

[[credential-proxy-pattern|Capability-based Warrants]] (task-scoped, signed, ephemeral) are the architecture's authorization primitive. How do they interface with the enterprise's existing **IAM** (Okta, Entra, AD), **PAM** (CyberArk, Delinea, Teleport), and **secrets vault** (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault)?

**Trade-off.** Wrapping existing IAM/PAM with a Warrant-issuance layer preserves enterprise investment but adds a second tier of authorization to maintain. Replacing PAM with a native Warrant system is cleaner but a 12-24 month migration for most enterprises.

**Where it usually lands.** Wrapper. Native replacement is rare in 2026.

## 4. Sandboxing posture

[[agent-sandboxing|Per-task isolation]] is the containment plane's last line of defense. Three granularities:

- **Per-agent VM** — strongest isolation, highest overhead, slowest startup
- **Per-task container** — middle ground; per-task lifecycle aligns with `before_model_callback`/`after_tool_use` hook boundaries
- **Per-call function** (e.g., Lambda, Cloud Run) — fastest startup, weakest cross-call state separation; fits stateless tool calls

**Where it usually lands.** Per-task container for general agents; per-call function for highly stateless tool layers (e.g., MCP server farms); per-agent VM for high-trust autonomous agents only.

## 5. Cross-agent communication

Multi-agent meshes need an inter-agent protocol. Two paths:

- **[[a2a-protocol|A2A Protocol]]** (Google, Linux Foundation governance, opacity principle) — the current standards bet
- **Proprietary** — usually faster to implement, often missing security primitives (signed cards, opacity boundary, trust-domain attestation)

**Where it does the opacity boundary land?** A2A's opacity principle says agents collaborate without sharing internal memory or proprietary logic. In implementation: which fields of the agent's state are externally exchangeable, and which stay private? The protocol provides the *frame*; the boundary is per-deployment.

**Where it usually lands.** A2A for new builds where 2+ agent providers must interoperate. Proprietary inside a single vendor's mesh, with A2A as the external interface.

## 6. Fail-mode for the policy engine

What happens when the PDP, identity provider, or credential proxy is **unreachable**?

**Trade-off.** Fail-closed (deny everything until policy reaches the runtime) preserves security but degrades availability — and an outage looks like a security incident to users. Fail-open (allow with cached policy until refresh) preserves availability but creates a window of unaudited / under-policy action.

**Where it usually lands.** Fail-closed for production systems with regulatory exposure. Fail-open with strict cache TTL for developer-facing tooling. Mixed deployments often run **fail-open at runtime, fail-closed for credential issuance** — the agent can keep using already-issued capabilities but cannot get new ones.

## Basis for exclusion from the RA

A reference architecture defines the *target state* and the *required controls*. These six questions are *implementation tradeoffs* — they would either over-prescribe (locking the RA to one enterprise's existing infra) or under-resolve (six pages of conditional advice instead of a clear architecture). Keeping them as a separate gaps page lets the RA stay opinionated where opinion adds value, and admit ambiguity where ambiguity is honest.

## See Also

- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] — the RA these questions implement
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — the maturity model that scores how well an organization is operating the RA
- [[agent-identity-architecture|AI Agent Identity Architecture]] — adjacent: identity-plane decisions (#2, #3 most relevant)
