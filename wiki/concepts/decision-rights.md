---
type: concept
title: "Decision Rights for AI Agents"
created: 2026-04-30
updated: 2026-08-22
tags:
  - concepts
  - governance
  - agentic-ai
  - authority
  - accountability
status: developing
scope_axis:
  - sec-of-ai
related:
  - "[[ai-coding-agent-governance]]"
  - "[[scaling-agentic-ai-cios-talk]]"
  - "[[shadow-automation]]"
  - "[[least-agency-principle]]"
  - "[[emerging-cybersecurity-practices-for-agentic-ai-applications]]"
  - "[[non-human-identity]]"
  - "[[ai-agent-layered-council]]"
  - "[[distributed-kill-switch]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[nist-ai-rmf]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
sources:
  - "[[ai-coding-agent-governance]]"
  - "[[.raw/talks/scaling-agentic-ai-cios-2026-05-01.md]]"
---

# Decision Rights for AI Agents

The documented authority of an AI agent to take a class of action without further human approval — specifying *who* approved that authority, *for what scope*, *under what justification*, and *for how long*. The formulation comes from [[knostic|Knostic]]'s framing in [[ai-coding-agent-governance|AI Coding Agent Governance (Knostic, 2025–2026)]]: *"Governance refers to defining who has the authority to act and under what justification."*

Decision rights are the **governance counterpart** to least privilege. Where least privilege answers *"what can the agent access?"*, decision rights answer *"what can the agent decide on its own — and on whose authority?"*

## Distinction from access scoping

Access scoping (D3 of [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]) controls reach: which repos, which APIs, which environments. Decision rights control **authority delegation** within that reach: among the actions the agent *can* take, which can it take *autonomously*, which require notification, which require approval, which are prohibited?

| Concept | Question it answers | CMM domain |
|---|---|---|
| Least privilege | What can the agent access? | D2 + D3 |
| Least agency ([[least-agency-principle\|Least Agency Principle]]) | How autonomous is it? | D3 |
| **Decision rights** | **On whose authority + under what justification?** | **D1 (Governance & Accountability) + D3 (Control & Least-Agency)** |
| Auditability | Was the decision reversible and attributable? | D7 + D9 |

Without decision rights, an agent operates with *apparent authority* — the user delegating to it has implicitly granted it the authority to act in their name, but the **delegation chain is undocumented**. This is the failure mode that turns helpful agents into risk multipliers.

Documenting that delegation chain is what the [[nist-ai-rmf|NIST AI RMF]] GOVERN function requires when it calls for clear lines of accountability over AI systems. A decision-rights matrix is the artifact that makes a GOVERN accountability claim auditable for an agent: it records who holds the authority to act, for what scope, and under what justification, rather than leaving authority implicit.

## A decision-rights matrix (sample)

A decision-rights matrix per agent type / agent-role pair is the artifact that operationalizes this concept. Sample row:

| Agent type | Action class | Decision right | Approver | Justification | Time bound |
|---|---|---|---|---|---|
| code-review-agent | comment on PR | autonomous | n/a | task-defined | always |
| code-review-agent | merge approval | requires approval | named human reviewer | risk-tier=med | per-PR |
| deployment-agent | deploy to staging | autonomous | n/a | CI green | maintenance window only |
| deployment-agent | deploy to production | requires approval | release manager + on-call | change ticket | per-deploy |
| deployment-agent | force-push or branch deletion | prohibited | — | — | — |

A row is **not optional** for any action class an agent can take. "We didn't write a row for this" defaults to *prohibited*.

## Relation to the least-agency action-risk tiers

Maps directly to the four action-risk tiers documented in [[least-agency-principle|Least Agency Principle]], which come from [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] §3.2 rather than from the OWASP ASI Top 10 that names the least-agency principle:

| Action-risk tier | Decision-right wording |
|---|---|
| auto | autonomous |
| notify | autonomous-with-notification |
| confirm | requires approval |
| block | prohibited |

The action-risk tiering is the *mechanism*; decision rights are the *justification record* — who approved the tier assignment, under what risk analysis, with what review cadence.

## Place in the CMM

- **[[agentic-ai-security-cmm-d1-governance|D1]] L3 evidence**: documented decision-rights matrix per registered agent type (sharpening from the Knostic ingest).
- **[[agentic-ai-security-cmm-d3-control-least-agency|D3]] L3+ evidence**: the matrix is operationalized in the PDP (Cedar / OPA policies) — every tool call is mediated by the matrix at runtime.
- **[[agentic-ai-security-cmm-d9-operations|D9]] L4 evidence**: HITL approval-rate metrics per matrix row (rubber-stamp detection).

This maps onto the [[microsoft-zt4ai|Microsoft ZT4AI]] Governance pillar (verify explicitly), whose named accountable owners and re-evaluate-at-use posture supply the authority-and-justification record decision rights formalize (see [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]]).

## Halt-authority is also a decision right

[[distributed-kill-switch|Distributed Kill Switch]] — the practice of putting halt-authority in the hands of every team member in the loop — is itself a decision right that must appear in the matrix. Sample row:

| Agent type | Action class | Decision right | Approver | Justification | Time bound |
|---|---|---|---|---|---|
| any-agent | halt current run | autonomous (any in-loop human) | n/a | safety; logged-not-punished | always |
| any-agent | halt workflow / use case | autonomous (workflow owner) | n/a | safety + outcome | per-use-case |
| any-agent | halt project / line of business | requires approval | [[ai-agent-layered-council\|Council]] (Legal + COO seats) | escalation | per-project |

Without explicit halt rows, the practice has no audit trail and quietly degrades to "the CIO holds the kill switch alone" — the failure mode [[scaling-agentic-ai-cios-talk|the talk]] explicitly warns against.

## Relations

- Foundation: [[ai-coding-agent-governance|AI Coding Agent Governance (Knostic, 2025–2026)]] introduced the framing.
- Mechanism: [[least-agency-principle|Least Agency Principle]] (the action-risk tiers) — decision rights are the justification record for tier assignments.
- Halt-authority: [[distributed-kill-switch|Distributed Kill Switch]] — halt is itself a decision right.
- Cross-functional body: [[ai-agent-layered-council|AI Agent Layered Council]] — the cross-functional body that approves portfolio-level decision-right assignments.
- Containment: [[shadow-automation|Shadow Automation]] — without decision rights, agent inventory is not governance.
- IAM context: [[non-human-identity|Non-Human Identity (NHI)]] — decision rights bind to NHI, not to the human delegating.

<!-- sources:auto -->
## Sources

- [Scaling Agentic AI: A Leadership Guide for CIOs](https://stream.stream-ext.bizzabo.com/U00liQQ00l2Th5l5Bkc302pY02k01IzHU8P3OqHaCDwYzvxw.m3u8)
<!-- /sources -->
