---
type: maturity-model
title: "Agentic SOC CMM D4 Identity and Action Authority"
address: c-000204
created: 2026-06-03
updated: 2026-06-23
tags:
  - maturity-models
  - cmm
  - agentic-soc
  - identity
  - action-authority
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
related:
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[plan-validate-execute]]"
  - "[[non-human-identity]]"
  - "[[agent-identity-architecture]]"
  - "[[cedar]]"
  - "[[decision-rights]]"
  - "[[hooking-coding-agents-with-cedar-talk]]"
  - "[[cyber-poverty-line]]"
sources:
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[plan-validate-execute]]"
---

# Agentic SOC CMM D4 Identity and Action Authority

Companion deep-dive to the [[agentic-soc-cmm|Agentic SOC CMM]]'s D4 domain. D4 measures whether the SOC's own response agents carry per-agent identity that traces to a human owner, hold scoped and revocable permissions, and act under per-action authority tiers with blast-radius limits and a human override path. It is one of the two autonomy gates for **L2** function autonomy (act-with-approval): an agent may not run consequential actions unsupervised until its authority is scoped and revocable. The domain scores the **Identity & Action-Authority plane** and parts of the **Policy & Enforcement plane** of the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]].

D4 is the SOC's instance of the **securing-the-agents shared layer**. A SOC response agent is a non-human identity and is secured like any other; the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] already models per-agent identity (its D2 Identity & Authorization) and scoped authorization with least-agency (its D3 Control & Least-Agency). This page does not restate that material — it cross-references it.

**The difference D4 carries that the application-security pair does not model is response-action authority.** Containment, blocking, account disablement, and host isolation are state-changing operations on the production estate. D4 gates them by blast-radius limits and approval tiers, not by the data-access scopes the application-security model centers on. A wrong containment action is itself an availability incident, so the authority to act on the defended environment is bounded in a way the application-security model has no reason to define.

> [!gap] Wiki-internal calibration
> The level criteria and the gating threshold are wiki-internal calibration synthesized from the CMM design spec and the reference architecture, not an externally ratified standard. The control landscape names real, dated tools; the maturity ladder and the L2 gate are the model's own and will firm up as the SOC pair is stress-tested against practitioner deployments.

D4 also separates from the other autonomy gates by its **enforcement mechanism**. Its controls are realized largely through *deterministic* policy enforcement — policy-as-code, typed tool contracts, and the [[plan-validate-execute|plan-validate-execute]] pattern — which is exhaustively testable. A scoped-permission decision and a blast-radius limit either hold or do not; they are not probabilistic. So D4 leans on change-control and review more than on evaluation. This is the reason determinism raises the safe ceiling for AI delegation generally: where the identity and authority boundary is enforced deterministically, an AI agent can be granted higher delegation than its own decision quality alone would justify, because the consequential action is bounded by a gate the AI cannot exceed.

## Control landscape (dated)

The controls are identity primitives, policy engines, and approval workflows. Most are general-purpose identity and authorization technology applied to the SOC's response agents; the response-authority tiers are usually carried by the SOAR or response platform.

| Layer | What ships today | Status (mid-2026) |
|---|---|---|
| Per-agent identity | [[non-human-identity\|NHI]] platforms and workload identity: [[spiffe\|SPIFFE]]/SPIRE workload SVIDs; Microsoft Entra Agent ID; cloud agent-identity primitives (AWS, GCP). Each agent traces to a human owner field | Entra Agent ID and the hyperscaler agent-identity primitives are GA; SPIFFE/SPIRE is mature and open. See [[agent-identity-architecture\|AI Agent Identity Architecture]] |
| Scoped, revocable authority | OAuth 2.1 token exchange and capability/scoped tokens; per-agent RBAC; time-bounded elevation (JIT) and revocation. Per-task capability tokens remain early-stage | OAuth 2.1 delegation is GA; per-task tokens are pre-GA, an OSS primitive only |
| Policy-as-code enforcement | [[cedar\|Cedar]] and OPA/Rego at a [[oversight-layer\|policy decision point]] outside the model context; typed tool contracts; the [[plan-validate-execute\|plan-validate-execute]] gate ([[hooking-coding-agents-with-cedar-talk\|Cedar as a deterministic reference monitor]]) | GA; Cedar and OPA are production policy engines, applied to agent tool calls as a pattern rather than a single product |
| Response-authority tiers | SOAR / response-platform approval workflows realize the auto / propose / approve / block tiers and pre-authorized containment with named blast-radius limits and a human override path | GA in mainstream SOAR; the per-action authority *tiering* over agents is configuration, not a shipped agent-governance feature |
| Decision rights | [[decision-rights\|Decision Rights for AI Agents]] matrices documenting which action classes each response agent may take, propose, or never take | Pattern-level; assembled from identity-platform inventory plus the response-platform policy |

The response-authority row is the column the application-security stack does not carry. The other four are the shared identity and authorization layer; for a single-stack buyer they often land within existing entitlements (an Entra tenant already issues agent identities; a SOAR platform already runs approval workflows), so the marginal licensing cost of D4 is frequently near zero and the spend is configuration labor.

## Capability levels

Levels are cumulative: Level N assumes every Level N−1 criterion. Each is stated as a capability specific to the SOC's response agents.

- **L1 — Initial.** Response agents act under a shared or human-borrowed identity; permissions are broad and standing; no per-action authority distinction. An agent that can containment-isolate a host can usually also disable accounts and push blocks, with nothing tiering or bounding the action. There is no reliable trace from a response action to the agent or its human owner.

- **L2 — Developing.** Every response agent has its own identity that traces to a named human owner; permissions are scoped to the action classes the agent needs and are revocable. A per-action authority distinction exists in at least coarse form — some actions execute, others require explicit human approval. **This is the rung that unlocks function-autonomy L2 (act-with-approval).** Per the gating rule, a function may run at autonomy L2 only when D4 supports it: the agent must hold scoped, revocable authority so that a consequential action it proposes can be approved and bounded rather than taken blind. Paired with D1 (Telemetry & Data Readiness), D4 at this level is the second of the two L2 gates.

- **L3 — Defined.** Per-action authority tiers are formalized as **auto / propose / approve / block**, applied per action class and enforced deterministically at a policy decision point outside the agent's context (policy-as-code, typed tool contracts, plan-validate-execute). Response actions carry **blast-radius limits** — caps on how many hosts, accounts, or network segments a single agent action may affect before it must escalate — and a documented human override and rollback path. A [[decision-rights|decision-rights]] matrix is maintained per response-agent type. Standing privilege is replaced by time-bounded elevation for actions above the agent's baseline tier. At this level D4 supports a function reaching autonomy L3 (autonomous in-bounds) on the authority axis; the other L3 gates (D3 Evaluation, D5 Observability) must also hold.

- **L4 — Managed.** The authority model is measured and governed quantitatively. Authority-tier coverage, override rates, blast-radius-limit triggers, and revocation latency are reported as metrics; over-broad grants and unused permissions are detected and reaped. Per-agent identity carries cryptographic attestation (for example a SPIFFE JWT-SVID). Segregation of duties is enforced across agents — the agent proposing a containment is not the agent that approves or executes it. Authority changes run through change-control with review, consistent with the deterministic, exhaustively-testable nature of the enforcement. At this level D4 contributes the authority half of the L4 (delegated) gate, alongside D7 and D8.

- **L5 — Optimizing.** The authority model adapts under governance: authority tiers and blast-radius limits are tuned from operational evidence (incident outcomes, false-containment rates) rather than set once, while every change remains exhaustively testable and audited. Per-task capability tokens scope authority to a single approved action where they exist. Identity federates across the SOC's platforms with reconciled inventory, and the response-authority tiering is unified across the agent fleet rather than configured per tool.

- **L5+ — Leading Edge.** All of L5, plus a named contribution to an agent-identity or authorization standard for the response-action case — for example agent-specific extensions to capability-token or workload-identity specifications, or a published reference pattern for blast-radius-bounded autonomous response.

## Right-sizing by org profile

Authority scoping is one of the cheaper domains to reach a defensible level in, because deterministic enforcement does not require an evaluation program behind it. The realistic target rises with the size of the agent fleet and the blast radius it can reach.

| Band | Realistic D4 target | Why |
|---|---|---|
| Solo / small | L2, often via the provider | Near or below the [[cyber-poverty-line\|cyber-poverty line]]. A team borrowing capability through an MSSP/MDR inherits the provider's identity and approval controls; for its few in-house agents, per-agent identity plus a coarse auto/approve split is achievable and sufficient. Containment authority is usually narrow by necessity |
| Mid | L3 | An in-house SOC running selective delegation on high-volume functions needs formal auto/propose/approve/block tiers and blast-radius limits before it can let a triage or response agent act in-bounds. Policy-as-code does the heavy lifting; this is the band where deterministic enforcement earns its keep |
| Enterprise | L4, selective L5 | A full agent fleet with broad containment authority needs measured authority coverage, attested identity, segregation of duties across agents, and reaping of over-broad grants. L5 where the blast radius and fleet size are largest |

A small team at L2 is right-sized, not immature: with a narrow agent footprint and limited containment authority, the marginal blast radius L3 controls bound is small, and the deterministic gate it does run is the load-bearing one.

## Cost model

The cost driver is configuration and change-control labor, not licensing. The identity and policy primitives are largely incumbent entitlements; the spend is scoping each agent's authority correctly and maintaining it.

| Level | Tooling / licensing | Operational labor | Run-rate note |
|---|---|---|---|
| L2 | ~0 (agent identity within an existing tenant; SOAR approval workflow already licensed) | ~0.25 FTE to issue per-agent identities, assign owners, scope initial permissions, and set a coarse auto/approve split | Deterministic; low recurring cost once scoped |
| L3 | ~0 to low (policy engine such as Cedar/OPA is open; response-platform tiering is configuration) | ~0.5 FTE recurring: author and maintain authority tiers and blast-radius limits, the decision-rights matrices, and the override/rollback paths | The labor is policy authorship and upkeep, not tool spend |
| L4 | metrics/identity-attestation tooling where not already present | board/program metrics on authority coverage and override rates; SoD across agents; permission reaping; change-control review | The dominant cost is governance labor over a larger fleet |
| L5 | per-task token tooling where it exists (pre-GA) | continuous tuning of tiers and limits from operational evidence, kept under audit | Highest where fleet size and blast radius are largest |

D4 spend is identity and policy configuration labor, paid down once and maintained, not a licensing line. Because the enforcement is deterministic and exhaustively testable, the recurring cost is change-control discipline rather than the evaluation treadmill that drives the cost of the probabilistic domains.

## Open questions

- Per-task capability tokens — the primitive that would scope a response agent's authority to a single approved action — are pre-GA and exist only as an OSS primitive. The L5 criterion that references them is forward-looking until a platform ships them.
- The blast-radius-limit thresholds (how many hosts or accounts an autonomous response action may affect before escalation) are necessarily org-specific; the model names the control but cannot fix the numbers, and they will vary with estate size and risk tolerance.
- The boundary between D4 (authority to take the action) and D5 (observing and overriding it after the fact) is clean in principle but blurs in tooling, where the same response platform often carries both. The two are scored separately; an implementation may satisfy them with one product.
- The L2 gating threshold — that D4 must be at L2 for a function to run at autonomy L2 — is anchored to the CMM's MDPI ↔ [[soc-cmm|SOC-CMM]] correspondence and is calibratable, not a fixed constant.

## Relations

- The [[agentic-soc-cmm#axis-2--the-eight-maturity-domains|Agentic SOC CMM]] main page (domain D4) and its [[agentic-soc-cmm#the-gating-rule|gating rule]] — D4 is one of the two L2 autonomy gates, paired with D1.
- The [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] — D4 scores its Identity & Action-Authority plane and parts of its Policy & Enforcement plane.
- Shared layer: the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] (its D2 Identity & Authorization and D3 Control & Least-Agency) and the [[agentic-ai-security-reference-architecture|Agentic AI Security RA]] identity/control planes. The SOC's response agents are non-human identities secured like any other; D4 adds response-action authority with blast-radius limits, which the application-security pair does not model.
- Sibling autonomy gates: D1 (Telemetry & Data Readiness, the other L2 gate), D5 (Observability & Oversight, the L3 partner), D7 (Resilience & Agent Supply Chain) and D8 (People & Governance, the L4 partners).
- Domain concepts: [[non-human-identity|Non-Human Identity]], [[agent-identity-architecture|AI Agent Identity Architecture]], [[plan-validate-execute|Plan-Validate-Execute]], [[cedar|Cedar]], and [[decision-rights|Decision Rights for AI Agents]].
