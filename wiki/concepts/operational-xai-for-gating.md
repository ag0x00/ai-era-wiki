---
type: concept
title: "Operational XAI for Action Gating"
address: c-000012
created: 2026-05-07
updated: 2026-08-19
tags:
  - concepts
  - explainability
  - human-in-the-loop
  - decision-rights
  - agentic-ai
status: developing
scope_axis:
  - sec-of-ai
aliases:
  - "Action-Gating XAI"
  - "Justification Gating"
  - "Reasoning-Chain Gating"
related:
  - "[[mechanistic-interpretability-for-defense|Mechanistic Interpretability for Defense]]"
  - "[[hitl|Human-in-the-Loop]]"
  - "[[decision-rights|Decision Rights for AI Agents]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]"
  - "[[maais-multilayer-agentic-ai-security|MAAIS Framework]]"
  - "[[plan-validate-execute|Plan-Validate-Execute]]"
sources:
  - "[[.raw/papers/maais-arora-hastings-2025-12-19.md]]"
---

# Operational XAI for Action Gating

The runtime requirement that an agent produces a human-readable justification of its reasoning before executing a high-impact action — and that the justification is structurally evaluable, either by a human reviewer or by a second-stage automated judge, before the action is allowed to proceed. Distinct from [[mechanistic-interpretability-for-defense|mechanistic interpretability]] (which is a researcher's probe of model internals); this is an **operational primitive** that gates production agent actions on the quality of their stated reasoning.

## Distinction from researcher XAI

Two senses of "explainable AI" coexist in the literature:

| Sense | Audience | Use | Wiki home |
|---|---|---|---|
| **Researcher XAI** — feature attribution, attention maps, probing classifiers, mechanistic interpretability of model internals | ML researchers, security researchers, attackers | Understand or attack the model | [[mechanistic-interpretability-for-defense\|Mechanistic Interpretability for Defense]] |
| **Operational XAI** — agent-emitted reasoning chains, justification fields in tool-call schemas, structured "why I am doing this" outputs | Operators, security reviewers, automated judges | **Gate** an action before it executes | This page |

Researcher XAI is a tool. Operational XAI is a **runtime control**. A model can be highly mechanistically interpretable yet produce no operational explanation, and an agent can produce excellent operational explanations from a fully opaque underlying model.

## Mechanisms

### Reasoning-chain capture

The agent's tool-call schema requires a `justification` field with the natural-language reasoning behind the call. Tools refuse calls without a justification. The justification is logged immutably (D7 / MAAIS Layer 7 territory) and surfaces in audit and operator dashboards.

```json
{
  "tool": "send_email",
  "args": {
    "to": "alice@example.com",
    "subject": "Quarterly review",
    "body": "..."
  },
  "justification": "User asked me to summarize Q1 metrics and email Alice; the user explicitly approved the recipient list in turn 3."
}
```

### Threshold-gated HITL

When the agent's confidence in its reasoning falls below a threshold, the action escalates to a human reviewer. Two confidence sources:

- **Self-reported** — agent emits a confidence score alongside the justification. Manipulable by injection but cheap.
- **Externally evaluated** — a second-stage LLM-as-judge scores the explanation quality, factuality, and alignment with the original task. More robust to injection but adds latency + cost.

### LLM-as-second-judge

A separate model (often a smaller, faster, or differently trained one) evaluates the primary agent's justification before the action runs. The judge can:

- Score the explanation against task alignment.
- Detect injection-style content in the reasoning ("system: ignore previous instructions...").
- Check the justification mentions all action-affected entities.

This is the architecture used in [[guardrails-beyond-vibes-talk|Stripe's offline-eval pipeline]] but applied at runtime.

### Plan-Validate-Execute as XAI structure

[[plan-validate-execute|Plan-Validate-Execute]] (Lidzborski / Google Workspace) is a specific instance: the agent first produces a plan (the explanation surface); a separate validator (human or automated) approves the plan; only then does the agent execute. The plan IS the justification, and validation IS the gate.

## Gating points

Not every action needs operational XAI — that would be HITL-fatigue territory. The decision belongs in the [[decision-rights|Decision Rights]] matrix:

| Action class | Justification required? | Who evaluates? |
|---|---|---|
| Read-only / Scope-1 | No | n/a |
| Reversible writes (low impact) | Optional, logged only | n/a |
| Sensitive writes (production data, comms, money) | **Yes** | Automated judge + HITL on low confidence |
| Privilege-elevation requests | **Yes** | Human reviewer always |
| External communications (email, posts, API calls beyond org) | **Yes** | Automated judge; HITL on low confidence |
| Destructive / irreversible operations | **Yes** | Human reviewer always |

The `production_impacting_write` annotation from [[breaking-the-lethal-trifecta-talk|Stripe's `ToolAnnotations`]] is the established mechanism for routing actions into the high-impact class.

## Limitations

- **Justifications can lie.** A compromised agent (via [[prompt-injection|prompt injection]]) can emit a plausible justification for a malicious action. Operational XAI alone is **not a security ceiling**. Pair with structural defenses: capability tokens, [[lethal-trifecta|trifecta]] containment, sensitive-action HITL.
- **HITL fatigue.** If every action requires a justification reviewed by a human, reviewers rubber-stamp. The point of threshold-gating is to keep human attention scarce — only low-confidence justifications surface.
- **Justification quality is hard to evaluate.** "Looks coherent" is not the same as "actually reflects the agent's process." LLM-as-judge can be fooled by adversarially crafted justifications.
- **Latency cost.** Two-stage evaluation (judge before action) adds ~100ms–1s per gated action. Acceptable for sensitive writes; prohibitive for high-frequency operations.

## Relation to wiki

- **CMM D1 (Governance & Accountability)** — the Decision Rights matrix names which actions require justifications and who evaluates them. Operational XAI is the implementation surface for that matrix at runtime.
- **CMM D4 (Runtime & Guardrails)** — the gate itself (judge logic, HITL routing) lives here as L3+ controls.
- **CMM D7 (Observability and Detection)** — justification capture and audit trails belong here.
- **MAAIS Layer 5 (Accountability and Trustworthiness)** — explicitly names "Explainable AI (XAI) Techniques" as an Accountability layer control. This page is the wiki's operational positioning of that control.
- **[[mechanistic-interpretability-for-defense|Mechanistic Interpretability for Defense]]** — adjacent but distinct; covers the researcher-XAI sense of the term.
- **[[plan-validate-execute|Plan-Validate-Execute]]** — concrete pattern; this page generalizes it.
- **[[decision-rights|Decision Rights for AI Agents]]** — the governance counterpart that decides where to gate.

## Provenance

The wiki page was created to disambiguate operational XAI from [[mechanistic-interpretability-for-defense|mechanistic-interpretability-for-defense]] after the [[maais-multilayer-agentic-ai-security|MAAIS]] ingest surfaced "XAI" as an Accountability-layer control without further specification. Concrete patterns (Plan-Validate-Execute, LLM-as-judge, justification fields in tool schemas) are drawn from [[securing-workspace-genai-at-google-talk|Google Workspace]], [[guardrails-beyond-vibes-talk|Stripe Offline Eval]], and the [[breaking-the-lethal-trifecta-talk|`ToolAnnotations`]] schema.

<!-- sources:auto -->
## Sources

- [Securing Agentic AI Systems -- A Multilayer Security Framework](https://arxiv.org/abs/2512.18043)
<!-- /sources -->
