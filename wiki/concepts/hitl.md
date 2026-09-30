---
type: concept
title: "Human-in-the-Loop (HITL) for Agentic AI"
created: 2026-05-03
updated: 2026-09-29
tags:
  - concepts
  - hitl
  - control-plane
  - least-agency
  - agentic-ai
status: developing
scope_axis:
  - sec-of-ai
source_url: "https://owaspai.org/go/oversight/"
sources:
  - "https://owaspai.org/go/oversight/"
  - "https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents"
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
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Current OWASP oversight control and CSA-hosted Agentic Trust Framework checked 2026-09-29."
---

# Human-in-the-Loop (HITL) for Agentic AI

Human-in-the-loop approval is a decision by an authorized person on a proposed agent action before that action executes. It is effective only where the action waits at an enforcement point outside the model's instructions. A person sending an agent-prepared draft can be the gate if the agent cannot send it first.

## Action tiers and approval

[[agentic-ai-security-cmm-d3-control-least-agency|CMM D3]] records each callable action in one of four tiers. The deployment's risk decision assigns the tier; the examples below do not impose a universal classification.

| Tier | Execution behavior | Example |
|---|---|---|
| Auto | Runs under standing authorization. | Bounded read of an approved source. |
| Notify | Runs and informs a named person. | Reversible low-impact write. |
| Confirm | Waits for an authorized person's decision. | Production change or external send where policy requires review. |
| Block | Does not run. | Action outside the permitted task or destination. |

For a confirm action, the review interface should show:

- The actual target and proposed parameters.
- The expected impact and reversibility.
- Prior steps that affect the decision.

The enforcement point must use the parameters the person reviewed. A refusal, expiry, or unavailable approval route leaves the action unrun. [OWASP AI Exchange's oversight control](https://owaspai.org/go/oversight/) recommends infrastructure gates, informed review, and risk-selected human involvement. A model instruction to “ask first” cannot enforce a held action.

## Operational evidence

The assessor traces a proposed action from classification through approval to execution, then tests:

- A direct tool or alternate autonomy route cannot execute before approval.
- An expired or refused request does not execute.
- Changed target or parameters require a new decision.
- Queue outage does not silently turn confirm into notify or auto.
- Approval volume, queue age, and reviewer behavior remain visible enough to detect fatigue or bypass.

[[agentic-ai-security-cmm-d3-control-least-agency|D3]] requires a stronger request-specific, cryptographically bound approval token at its highest level. That token is a CMM criterion, not the definition of every working human approval gate. [[agentic-ai-security-cmm-d9-operations|D9]] grades queue operation and approval records. A screen that displays a confirmation while the write has already occurred fails the gate regardless of the approver's diligence.

## Source distinctions

The [Agentic Trust Framework article hosted by the Cloud Security Alliance](https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents) is written by **Josh Woodruff of MassiveScale.AI**. Its five gates govern promotion to a broader autonomy level:

- Performance
- Security Validation
- Business Value
- Incident Record
- Governance Sign-off

Those are promotion checks, not five steps in each action approval. The article's Intern-to-Principal labels and example thresholds are its own framework; they are not CMM maturity levels or general requirements for a deployment.

## CMM use

[[agentic-ai-security-cmm-d3-control-least-agency|D3]] tests the action policy and before-execution approval gate, including binding at its highest level. [[agentic-ai-security-cmm-d9-operations|D9]] tests whether the human queue and review process work over time. [[agentic-ai-security-cmm-d2-identity|D2]] identifies the agent and human whose authority the decision uses. The [[agentic-ai-security-reference-architecture|Reference Architecture]] locates the enforcement point; a user interface alone does not establish its coverage.

<!-- sources:auto -->
## Sources

- [Human-in-the-Loop (HITL) for Agentic AI](https://owaspai.org/go/oversight/)
- [cloudsecurityalliance.org](https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents)
<!-- /sources -->
