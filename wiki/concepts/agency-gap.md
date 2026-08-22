---
type: concept
title: "Agency Gap"
created: 2026-05-03
updated: 2026-08-21
tags:
  - concepts
  - agentic-ai
  - threat-model
  - rogue-actions
status: developing
no_public_url: "term coined by Nicolas Lidzborski at a non-published conference talk; no public URL"
scope_axis:
  - sec-of-ai
attributed_to: "Nicolas Lidzborski (Google), Unprompted March 2026"
related:
  - "[[orchestration-hijacking]]"
  - "[[least-agency-principle]]"
  - "[[emerging-cybersecurity-practices-for-agentic-ai-applications]]"
  - "[[hitl]]"
  - "[[plan-validate-execute]]"
  - "[[lethal-trifecta]]"
  - "[[securing-workspace-genai-at-google-talk]]"
---

# Agency Gap

The non-deterministic disconnect between a user's actual intent and the autonomous execution performed by an AI agent. Coined / named in this form by [[nicolas-lidzborski|Nicolas Lidzborski]] (Google Workspace) at [[unprompted-conference-march-2026|Unprompted March 2026]] as one of three sub-classes of "rogue action" risk in agentic systems.

## The pattern

Because agent reasoning is probabilistic, minor variations in the prompt — wording, ordering, retrieved context — can lead to substantively different action sequences. The agent isn't malfunctioning in any classical sense; it is following a plausible interpretation of the user's request that diverges from the user's actual intent.

**Lidzborski's worked example.** A user asks the agent to "email John about the contract." There are two Johns in the address book. The agent picks one. *"Just because of a name conflict, just figure out, oh, that's close enough."* In an autonomous flow without human confirmation, the contract goes to the wrong John. Sensitive data is now in unauthorized hands.

The failure mode is **not** [[prompt-injection|prompt injection]]. It is not even a misunderstanding on the agent's part — the agent's reasoning is internally coherent. It is a gap between what the user *meant* and the action sequence the agent generated from a probabilistic interpretation of what the user *said*.

## Basis for the term "agency gap"

The framing is deliberate:

- **Ambiguity** suggests a defect in the user's instruction
- **Agency gap** locates the failure in the agent's autonomy — the freedom to commit to an interpretation without verifying it is the source of risk

This shifts the design question from *"how do we make user prompts unambiguous?"* (impossible at scale) to *"how do we constrain agent autonomy in proportion to the consequences of misinterpretation?"* (tractable, and the foundation of the [[least-agency-principle|Least Agency Principle]] and [[hitl|HITL]]).

## Relation to the action-risk tiers

The agency gap is the structural reason the four action-risk tiers (auto / notify / confirm / block) exist. The tiers come from [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] §3.2, which operationalizes the OWASP ASI least-agency principle, and [[least-agency-principle|Least Agency Principle]] carries them in full:

| Action class | Agency-gap blast radius | Tier |
|---|---|---|
| Read-only, reversible | Low — wrong answer can be discarded | Auto |
| Low-stakes write | Medium — recipient receives unintended content | Notify |
| Sensitive write or sharing | High — irreversible disclosure | Confirm (HITL) |
| Out-of-scope or unconditionally prohibited | N/A | Block |

The confirm tier exists precisely *because* the agency gap cannot be closed by prompting alone. The remedy is to halt agent autonomy at the boundary of an irreversible action and route the interpretation back to the human for verification.

## Defenses

The agency gap is **not** a defendable surface in the model itself. Defenses sit in the orchestration and control planes:

- **[[plan-validate-execute|Plan-Validate-Execute]]** (Google Workspace) — explicit enumeration of intended actions before execution; gatekeeper checks each action against dynamically generated policy and user intent
- **[[hitl|HITL confirm-tier gates]]** for irreversible actions — require explicit human approval before executing
- **Tool-call schema constraints** — typed tool annotations (recipient = expected entity type, contract ID = expected format) catch some agency-gap errors at the policy layer
- **Behavioral baseline drift detection** — repeated misalignment between agent action and user behavior is observable in [[agent-observability|agent observability]] data

Lidzborski is explicit about the **review-fatigue limit**: HITL gates can be subverted by users who learn to rubber-stamp confirmations. The agency gap is therefore a perpetual residual risk; the question is how to size the residual to tolerable bounds.

## Cross-references

- **Distinct from [[orchestration-hijacking|Orchestration hijacking]]:** orchestration hijacking is *adversarial* — an attacker manipulates the planner via injected content. Agency gap is *non-adversarial* — the agent's own probabilistic reasoning produces a divergent interpretation. Defense overlap is real (HITL helps both) but the threat model differs.
- **Relation to [[lethal-trifecta|Lethal Trifecta]]:** the agency gap is more dangerous when the trifecta is in play. A small interpretation slip with private-data scope and external-comms reach becomes a high-impact disclosure event.
- **Confused deputy** (cited by Lidzborski as a separate sub-class) is a related but distinct failure mode where the orchestration *grants* permissions the user didn't authorize. Agency gap is about *which action* the agent picks within authorized permissions; confused deputy is about *whether the agent should have those permissions at all*.

> [!gap]
> No standardized measurement exists for agency-gap incidence in production agents. UEBA-for-agents and behavioral baselines surface clear deviations but not the subtle "wrong John" class. This is an open observability gap.
