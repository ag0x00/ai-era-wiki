---
type: concept
title: "Human-in-the-Loop (HITL) for Agentic AI"
created: 2026-05-03
updated: 2026-08-21
tags:
  - concepts
  - hitl
  - control-plane
  - least-agency
  - agentic-ai
status: developing
scope_axis:
  - sec-of-ai
source_url: "https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/"
related:
  - "[[least-agency-principle]]"
  - "[[agency-gap]]"
  - "[[emerging-cybersecurity-practices-for-agentic-ai-applications]]"
  - "[[owasp-ai-exchange]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[lethal-trifecta]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[agents-rule-of-two]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[precize-agentic-ai-top10]]"
---

# Human-in-the-Loop (HITL) for Agentic AI

Human-in-the-loop (HITL) is the architectural requirement that an agent pause execution and obtain explicit human approval before taking certain high-impact or irreversible actions. In the context of agentic AI security, HITL is a **control-plane primitive** — a deterministic enforcement gate that runs below the model and cannot be bypassed by prompt injection or model misbehavior, provided it is implemented at the platform layer rather than as a prompt instruction.

The absence of this control is named directly in the [[precize-agentic-ai-top10|Precize Top 10 for Agentic AI Vulnerability]] as AAI012, Checker-out-of-the-Loop Vulnerability: no human operator or automated checker is alerted when an agent operates outside its system limits. AAI012's framing complements the gate model above — it names the *monitoring/alerting* failure (nobody is watching for drift), where the confirm tier below names the *authorization* failure (nobody signed off before the action). AAI012's own analogy is aviation autopilot incidents where pilots were not alerted in time to correct a dangerous flight condition. This page's D9 fatigue discussion makes the same detection-latency argument from the opposite direction: a gate that exists but nobody is monitoring degrades to the same outcome as no gate at all.

## The action-risk tiers

The wiki's action-risk tiers classify actions into four tiers that determine when HITL is required. The tiers are stated in [[least-agency-principle|Least Agency Principle]], which carries their provenance: the OWASP Agentic AI Top 10 introduces least agency as a principle and supplies no tier scheme, and the tier scheme comes from [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] §3.2.

| Tier | Behavior | When used |
|---|---|---|
| **Auto** | Agent acts without notification | Read-only, low-risk, reversible actions |
| **Notify** | Agent acts, then informs the human | Low-risk writes; audit trail sufficient |
| **Confirm** | Agent proposes action; human must approve before execution | High-impact, partially reversible, or out-of-scope actions |
| **Block** | Action is refused regardless of instruction | Unconditionally prohibited action classes |

The confirm tier is the operative HITL gate. An agent reaching a confirm-tier action must halt, surface a structured proposal to the human principal, and wait. Only after explicit approval (not a default timeout) does it proceed.

The tiers classify actions. A second axis, stated by the [[owasp-ai-exchange|OWASP AI Exchange]] under `OVERSIGHT`, classifies the oversight a tiered action receives — fully autonomous execution, soft confirmation, hard confirmation, or mandatory human initiation — and is set out with the tier table in [[least-agency-principle|Least Agency Principle]].[^aix-oversight] The confirm tier above is the wiki's name for the action class; hard confirmation is the mechanism it demands.

## Case for platform enforcement

A common failure mode is implementing HITL as a **prompt instruction** — telling the model "always ask before deleting files." This is bypassable:

- A [[prompt-injection|prompt injection]] in external content can instruct the model to skip the confirmation step
- A jailbreak or goal-drift can cause the model to rationalize that the action is safe
- The model's in-context reasoning can construct a "the user implicitly approved" justification

The [[breaking-the-lethal-trifecta-talk|VS Code CVE-2025-62453 case]] illustrates the failure directly: VS Code's confirmation gates were implemented as UI conventions rather than as platform constraints. Certain tool calls (e.g., `editFile`) auto-saved to disk before the user could approve or reject, creating a TOCTOU window that bypassed the gate entirely.

Platform-enforced HITL means the runtime **does not call the tool** until a cryptographically-linked approval token is received. The model can propose; only the platform can act.

### Approval-token binding

**A token bound to the action's specific parameters, rather than to the approver's intent, closes the gap between what the human saw and what the runtime performs.** The [[owasp-ai-exchange|OWASP AI Exchange]] specifies a unique approval token per request, cryptographically binding the approver's identity, the specific action parameters, and an expiry, with execution rejected where a parameter deviates from the approved value or the token has expired.[^aix-oversight] An approval for a transfer of one amount does not authorize the same call with another. The same entry requires explicit confirmation of key parameters in plain language, logs the approver's understanding alongside the approval signal, applies a mandatory minimum delay before execution for the highest-risk actions, and requires segregation of duties for critical actions — multiple approvers from different roles, assigned by policy rather than by the agent.[^aix-oversight] The audit trail is tamper-evident and records the request, the parameters presented, the approver's identity and authentication method, the decision, and the outcome.[^aix-oversight] High-risk categories are defined in advance along four dimensions the Exchange names: irreversibility, data classification, external parties, and financial thresholds.[^aix-oversight]

The disclosure surface is specified alongside the gate. Agent identity is disclosed at interaction start, and the authorised action scope is presented from a static capability manifest rather than from consent text the agent composes, since an agent must not frame its own consent request.[^aix-oversight] Consent for high-impact actions is granular and task-specific, and the user holds an accessible action log and a genuine mid-task opt-out.[^aix-oversight] The Exchange routes regulatory disclosure obligations away from this control to `CHECK COMPLIANCE` and `AI TRANSPARENCY`.[^aix-oversight] The review interface itself is constrained: it presents the action requested, the stated rationale, relevant prior actions in the session, estimated impact, and a reversibility classification, and it does not pass through agent-generated urgency framing without sanitisation.[^aix-oversight]

## Scope: which actions require confirm-tier HITL

Actions that typically require the confirm tier include any combination of:

- **Irreversibility**: deletes, overwrites, sends (email, message, payment)
- **Scope breach**: acting outside the agent's declared workspace or data scope
- **Cross-system writes**: pushing to production, committing to main, writing configuration
- **Credential use**: any tool call that uses a non-proxied credential
- **Lethal Trifecta trigger**: actions combining private data access + untrusted content provenance + external comms reach

The tier is assigned per action class (not per agent), via policy in the [[agentic-ai-security-reference-architecture|Control plane]] (Cedar or OPA policy). The same agent may auto-execute reads but require confirm for writes.

The [[agency-gap|agency gap]] is why the list holds irreversibility and scope breach rather than a malice test. An agent whose reasoning is internally coherent can still commit to the wrong reading of an ambiguous instruction, so the confirm gate routes the interpretation back to a human at the point where a wrong reading cannot be withdrawn.

## CSA Agentic Trust Framework gates

The [[csa-maestro|CSA Agentic Trust Framework (ATF)]] formalizes HITL into five progressive autonomy promotion gates — preconditions that must be met before an agent can operate at higher autonomy tiers. The gates encode:

1. Task scope definition (what is the agent allowed to do?)
2. Resource access justification (why does it need these tools?)
3. Human oversight checkpoints per interaction class
4. Risk-based step-up (higher-risk actions trigger re-confirmation even mid-task)
5. Revocation capability (can you pull the agent back at any point?)

## Production HITL implementations

### Stripe — three-ring containment

[[breaking-the-lethal-trifecta-talk|Andrew Bullen's talk]] describes Stripe's three-ring containment model, where HITL is the outer ring. Key implementation details:

- HITL gates are enforced by a **policy engine** (not the model) — the agent never decides its own HITL requirement
- Gates are scoped to the action class rather than to the session: re-confirmation is required on each high-risk call even within a single task
- ASR (Attack Success Rate) with HITL enforced: 1.5–6.7% depending on model — *not zero*, but an order-of-magnitude reduction from ungated baselines
- The residual ASR reflects HITL bypass via social engineering of the human approver, not bypass of the gate mechanism itself

### Google Workspace — Plan-Validate-Execute

[[securing-workspace-genai-at-google-talk|Nicolas Lidzborski's Workspace talk]] describes Google's canonical HITL implementation as the **[[plan-validate-execute|Plan-Validate-Execute]]** pattern: the agent enumerates a structured plan; a non-LLM gatekeeper validates the plan against dynamically generated policy and against user intent; only after the gate passes does the agent execute. The pattern explicitly addresses the [[recursive-prompt-injection|recursive-injection]] failure mode by making validation deterministic rather than LLM-based. Lidzborski is also explicit about the **review fatigue / rubber-stamping** UX gap as an unsolved problem.

The oversight interface is itself a threat surface. [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]] names Overwhelming Human-in-the-Loop (T10): inducing decision fatigue or compromising the approval interface so a human rubber-stamps a malicious action. Its Playbook 5 (protecting HITL and preventing decision-fatigue exploits) treats approval-volume throttling, structured high-signal proposals, and tamper-evident approval channels as controls, not UX polish.

The two implementations cover complementary surfaces: Stripe emphasizes egress + tool-policy enforcement (the *data leaving* side); Google Workspace emphasizes the planning + validation interlock (the *deciding to act* side). Real production agentic systems will need both.

## HITL in the CMM

In the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]], HITL is graded across [[agentic-ai-security-cmm-d3-control-least-agency|D3 Control & Least-Agency]], which holds the policy decision point and the per-action-class approval coverage, and [[agentic-ai-security-cmm-d9-operations|D9 Operations & Human Factors]], which holds approval-rate, queue-age, and fatigue measurement. [[agentic-ai-security-cmm-d9-operations|D9]] L3 also grades the high-risk approval record itself — pre-defined categories along the Exchange's four dimensions, a tamper-evident audit trail, the mandatory minimum delay, and segregation of duties for the highest-risk actions. Cryptographic approval tokens are graded at D3 L5 alongside per-task capability tokens, since both are cryptographic binding with no platform-native implementation; the token binds the approval to its parameters, while the D9 L3 record carries what was approved, by whom, and on what understanding. Read the criteria from those two deep dives rather than from this page.

The D9 fatigue criteria — behavioral evidence that gates fire and are not circumvented — have a specific failure mode in agentic coding. Approval fatigue does not usually announce itself as circumvention; it presents as a defensible sequence of allowlisting, then autonomous modes, then suppressed prompts, at the end of which the deployment has changed shape and the gate no longer exists to be bypassed. Measuring approval volume and disposition over time, rather than gate existence, is what [[agentic-ai-security-cmm-d9-operations|D9]] grades. See [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] and [[agents-rule-of-two|Agents Rule of Two]], whose supervision fallback carries the same unstated assumption that the supervising human is present.

## Relation to capability tokens

[[tenuo-warrant|Tenuo Warrants]] and [[capability-based-authorization|capability-based authorization]] provide a complementary control: rather than pausing for approval at execution time, they restrict what the agent can request in the first place. HITL and capability tokens are not alternatives — they are complementary layers:

- Capability tokens: pre-authorize the *scope* of possible actions
- HITL: enforce human approval for high-impact actions *within* that authorized scope

Together they implement defense in depth: an agent that cannot request unauthorized actions (capability tokens) and must obtain approval before taking high-impact authorized ones (HITL).

> [!gap]
> No vendor-neutral, open-source HITL primitive exists with documented integration patterns for common agent frameworks (LangGraph, Google ADK, Anthropic SDK). The Control plane in the RA lists HITL as "Concept — Developing." Stripe's implementation is closed-source. This is an open gap in the reference implementation landscape.

[[red-teaming-capability-framework|The Red Teaming Capability Framework]] states human-in-the-loop review as a required capability at its upper tiers, alongside continuous automated red teaming and behavioral baseline monitoring — the gate is what keeps an automated red-team pipeline from acting on its own findings.

## Notes

[^aix-oversight]: [OWASP AI Exchange — OVERSIGHT](https://owaspai.org/go/oversight/), retrieved 2026-08-19. The oversight-requirement axis, the approval-token specification, the user-facing disclosure requirements, and the review-interface constraints.

<!-- sources:auto -->
## Sources

- [Human-in-the-Loop (HITL) for Agentic AI](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)
<!-- /sources -->
