---
type: concept
title: "Oversight Layer (PDP + PEP for Agentic AI)"
created: 2026-05-01
updated: 2026-09-29
tags:
  - concepts
  - oversight-layer
  - pdp
  - pep
  - architecture
  - zero-trust
status: developing
scope_axis:
  - sec-of-ai
complexity: intermediate
domain: agent-security
no_public_url: "Wiki synthesis of agent decision, enforcement, approval, and observation functions; no single canonical external source."
aliases:
  - "Oversight Layer"
  - "AI Oversight"
  - "Policy Decision Point"
  - "Policy Enforcement Point"
  - "PDP+PEP"
related:
  - "[[owasp-ai-exchange]]"
  - "[[threat-modeling-for-ai]]"
  - "[[guardian-agent]]"
  - "[[sentinels-and-operatives]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agent-observability]]"
  - "[[guardian-agent-metagovernance]]"
  - "[[nist-ai-rmf]]"
  - "[[standards-review-eu-ai-act-2026-Q2]]"
  - "[[agent-runtime-protection-canvass-2026-09]]"
sources:
  - "https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-162.pdf"
  - "https://owaspai.org/go/oversight/"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Current OWASP AI Exchange oversight control and NIST ABAC guide checked 2026-09-29."
---

# Oversight Layer (PDP + PEP for Agentic AI)

The oversight layer is this wiki's name for coordinated policy enforcement and human or automated review of an agent's consequential actions. It may span several services and people. Calling a product an “oversight agent” does not show that it can stop an action on the route that matters.

## Decision path

[NIST SP 800-162 §2.4.3](https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-162.pdf#page=23) names the access-control functions used below. The human and observation rows are separate additions for an agent deployment.

| Function | Place in the action path | Evidence |
|---|---|---|
| Policy Information Point (PIP) | Supplies trusted request attributes. | Attribute source and freshness. |
| Policy Decision Point (PDP) | Evaluates the proposed action against policy. | Rule revision and decision record. |
| Human approver | Decides a held action where policy requires confirmation. | Review and decision record. |
| Policy Enforcement Point (PEP) | Applies the decision at the execution boundary. | Route coverage and a refused-call test. |
| Policy Administration Point (PAP) | Publishes and governs policy independently of agent-controlled content. | Approved policy revision. |
| Observer | Records executed and denied actions and routes findings. | Searchable event and alert records. |

An observer may detect a harmful action after it runs. It is a PEP only if it can prevent or stop the action. An approval interface may collect a human decision. The PEP must hold the exact action until that decision is verified. The [OWASP AI Exchange oversight control](https://owaspai.org/go/oversight/) covers automated detection and human review. The table applies NIST's access-control names to that broader operational task.

## Architectural variants

| Placement | Main proof needed |
|---|---|
| In-process runtime control | Policy is outside model-controlled input and workspace; the running mode and version enforce a refusal. |
| Adjacent gateway or sidecar | Every applicable call traverses the point, including orchestration and permissive modes; direct-call tests fail. |
| External action gateway | The gateway sees the executed parameters and can stop the call before side effects. |
| Supplier-held control | Inspectable supplier evidence or a customer test establishes refusal behavior on the production path. |

A supplier console that shows only alerts proves observation, not enforcement. Where a material supplier-held step cannot be tested or evidenced, the CMM records the criterion as unanswerable under the [[agentic-ai-security-cmm-measurement-protocol|Assessor's Handbook]].

## Framework mapping

The NIST terms describe *functions*, not mandatory boxes or products. A PDP and PEP may share one service. Several PEPs may consult one PDP. [[nist-sp-800-162|NIST SP 800-162]] defines their ABAC roles. [[guardian-agent|Guardian Agent]] and [[sentinels-and-operatives|Sentinels and Operatives]] are adjacent product or analyst terms. Those labels alone identify no NIST function. The implementation path determines whether one applies.

## CMM use

| Domain | Oversight evidence it owns |
|---|---|
| [[agentic-ai-security-cmm-d2-identity\|D2]] | Authenticated agent and represented-human binding used in decisions. |
| [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] | Decision and enforcement of each action and approval gate. |
| [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] | Model-call guardrails and their measured behavior. |
| [[agentic-ai-security-cmm-d5-egress-network\|D5]] | Network reach and mediation for remote calls. |
| [[agentic-ai-security-cmm-d7-observability\|D7]] | Attributed records and detection from the action path. |

[[agentic-ai-security-cmm-d9-operations|D9]] operates the human queue and response procedure. The [[agentic-ai-security-reference-architecture|Reference Architecture]] shows the trust boundaries; the CMM deep dives define the evidence threshold for each domain.

<!-- sources:auto -->
## Sources

- [nvlpubs.nist.gov](https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-162.pdf)
- [owaspai.org](https://owaspai.org/go/oversight/)
<!-- /sources -->
