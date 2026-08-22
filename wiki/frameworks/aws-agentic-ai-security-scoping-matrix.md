---
type: framework
title: "AWS Agentic AI Security Scoping Matrix"
address: c-000001
created: 2026-05-07
updated: 2026-08-21
tags:
  - frameworks
  - aws
  - agentic-ai
  - autonomy
  - agency
  - maturity-models
status: developing
scope_axis:
  - sec-of-ai
authoring_organization: "[[aws|AWS]]"
authors:
  - "[[aaron-brown|Aaron Brown]]"
  - "[[matt-saner|Matt Saner]]"
publication_date: 2025-11-21
related:
  - "[[aws|AWS]]"
  - "[[least-agency-principle|Least Agency Principle]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
  - "[[csa-maestro|CSA MAESTRO / CSA Agentic Trust Framework]]"
  - "[[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10]]"
  - "[[guardian-agent|Guardian Agent]]"
  - "[[oversight-layer|Oversight Layer]]"
sources:
  - "[[.raw/articles/aws-agentic-ai-security-scoping-matrix-2026-05-07.md]]"
coined_by:
  - "[[aws]]"
---

# AWS Agentic AI Security Scoping Matrix

A four-scope categorization scheme for autonomous AI systems published by [[aws|AWS]] on 2025-11-21, structured around two foundational concepts (agency vs autonomy) and six security dimensions per scope. The matrix extends AWS's earlier Generative AI Security Scoping Matrix to address long-running, function-calling, multi-step agentic systems.

## Two foundational concepts

The framework's load-bearing distinction — both terms are explicitly defined and used as separate axes throughout the matrix:

- **Agency** — *"the scope of actions an AI system is permitted and enabled to take within the operating environment, and how much a human bounds an agent's actions or capabilities."*
- **Autonomy** — *"the degree of independent decision-making and action the system can take without human intervention."*

Agency = what is allowed; autonomy = how independently the agent decides. The distinction matters because a system can have high agency (broad permissions) and low autonomy (every action requires approval), or low agency (read-only) and high autonomy (decides freely within its narrow remit). The wiki carries this distinction on [[least-agency-principle|Least Agency Principle]], which uses the AWS definitions as its terminology anchor.

## The four scopes

| Scope | Name | Initiation | Change capability | Approval pattern |
|---|---|---|---|---|
| **1** | **No agency** | Human-initiated only | Read-only; cannot modify environment | N/A |
| **2** | **Prescribed agency** | Human-initiated | Limited; can modify systems | All consequential actions require explicit human approval (HITL) |
| **3** | **Supervised agency** | Human-initiated | High; can modify multiple systems | None during execution; humans set objectives, agents complete autonomously |
| **4** | **Full agency** | Self-initiated by the agent | Comprehensive; multi-system orchestration | None; humans retain supervisory oversight only |

**Scope 1** — agents are essentially read-only, following predefined execution paths. Generative AI processes data within individual workflow nodes; conditional branching only where explicitly designed. Tool access restricted to predefined workflow steps.

**Scope 2** — bidirectional human interaction for context clarification; agent-initiated requests for additional information; cryptographically signed approval decisions are the canonical implementation pattern.

**Scope 3** — dynamic planning and tool selection during execution. Agents have direct access to external APIs and persistent memory across sessions. Optional human intervention points for trajectory optimization but no per-action approval.

**Scope 4** — self-directed activity initiation based on environmental triggers, learned patterns, or predefined conditions. Capability for recursive self-improvement and capability expansion. Human role shifts to strategic guidance + supervisory override rather than per-execution oversight.

## The six security dimensions

The matrix defines six dimensions whose required controls vary by scope:

1. **Identity context (authN and authZ)** — user authentication, service authentication, agent authentication, identity delegation for autonomous actions, federated authentication, agent identity attestation. Maps onto [[agent-identity-architecture|Agent Identity Architecture]].
2. **Data, memory, and state protection** — local resource permissions → role-based access control → context-aware authorization → behavioral authorization. Maps onto [[agent-memory-isolation|Agent Memory Isolation]] and [[memory-poisoning|Memory Poisoning]] defenses.
3. **Audit and logging** — local activity logs → human decision audit trails → comprehensive action logging with reasoning chain capture → continuous behavioral logging with pattern analysis. Maps onto [[agent-observability|Agent Observability]].
4. **Agent and FM controls** — process isolation + I/O validations → approval gateway enforcement → container isolation + tool invocation sandboxing → behavioral analysis + anomaly detection + automated containment.
5. **Agency perimeters and policies** — fixed execution boundaries → approval-based boundary modification → dynamic boundary adjustment → self-adjusting boundaries with context-aware constraints.
6. **Orchestration** — simple workflow orchestration → multi-step approval-gated → dynamic tool orchestration → autonomous multi-agent orchestration with cross-session learning.

The progression across the six dimensions is the matrix's primary content. Implementation depth at each dimension increases as scope advances; AWS's per-scope security-focus narrative recommends specific controls per (scope, dimension) cell.

## Key architectural patterns

The matrix names five patterns that apply across scopes:

- **Progressive autonomy deployment** — start at Scope 1 or 2; advance as confidence and security capabilities mature. Echoes the wiki's [[agentic-ai-security-cmm-2026|CMM]] level-based progression discipline.
- **Layered security architecture** — defense-in-depth across network, application, agent, and data layers. Cites the **confused deputy problem** as a load-bearing reason that machine and human identity must both be addressed (a service or human with lesser permissions elevates permissions through an agent that has more).
- **Continuous validation loops** — automated systems that continuously verify agent behavior against expected patterns with escalation procedures for detected deviations.
- **Human oversight integration** — meaningful oversight through strategic checkpoints. The matrix makes an explicit point that human requirements **shift focus rather than diminish** as scope advances: instantiation and approval demands decrease, but audit, assessment, validation, and complex-control requirements increase.
- **Graceful degradation** — automatic autonomy reduction on security events. If agents act beyond intended bounds, detective controls inject tighter restrictions (more HITL, reduced available actions, or full disable). Maps onto the wiki's [[distributed-kill-switch|Distributed Kill Switch]] and [[behavioral-anomaly-detection-for-agents|Behavioral Anomaly Detection]].

## Cross-walk to wiki ladders

Several existing ladders sit in this conceptual space. Mapping the AWS scopes against them:

| AWS Scope | Wiki [[agentic-ai-security-cmm-2026\|CMM]] level | [[csa-maestro\|CSA ATF]] stage | Gating tier for consequential actions ([[least-agency-principle\|Least Agency]]) |
|---|---|---|---|
| **1** No agency | L1–L2 (Initial / Repeatable) | **Intern** (Observe + Report) | **Block** on every consequential action; reads run at auto |
| **2** Prescribed agency | L2–L3 (Repeatable / Defined) | **Junior** (Recommend + Approve) | **Confirm** (HITL on consequential actions) |
| **3** Supervised agency | L3–L4 (Defined / Managed) | **Senior** (Act + Notify) | **Notify** (autonomous with notification) |
| **4** Full agency | L4–L5+ (Managed / Optimizing / Leading-edge) | **Principal** (Autonomous) | **Auto** (autonomous within agency bounds) |

**AWS scopes and CMM levels measure different things.** The AWS matrix axes are **agency + autonomy** (what the agent is allowed and how independently it decides). The wiki's [[agentic-ai-security-cmm-2026|CMM]] axis is **organizational maturity** (how systematically the security program is designed, measured, and improved). They are orthogonal — a Scope-4 deployment can be at CMM L1 if its security program is ad-hoc, and a Scope-1 deployment can be at CMM L5 if the organization has implemented the full nine-domain control set even though it never lets agents act. The crosswalk above gives the *typical* CMM levels at which each scope is responsibly deployable, not an equivalence. The action-risk tier column is a third kind of object again: a scope describes a deployment, while a tier describes one action, so a Scope-3 deployment still holds auto-tier reads and block-tier prohibitions alongside the notify-tier default the row names.

## Stickiness assessment (2026-05-07, ~6 months post-publication)

The user flagged the matrix's nomenclature as potentially "sticky." Survey of adjacent literature six months out:

- **Agency vs autonomy distinction**: **sticky**. The two-axis framing (what the agent may do vs how independently it decides) appears in CSA ATF (which separates "scope of action" gating from "stage of autonomy"), in [[breaking-the-lethal-trifecta-talk|Bullen's Stripe talk]] (where "agency" implicitly means action-side capability separate from autonomy), and in OWASP's Least Agency framing. AWS's contribution is naming the distinction explicitly and giving it formal definitions.
- **The four-scope ladder structure**: **moderately sticky**. The 4-tier structure is convergent — every major framework now ships a 3–5 tier autonomy ladder (CSA ATF: 4 tiers; the least-agency action-risk tiers: 4; the wiki's CMM: 5 tiers). The *shape* is sticky; AWS's specific *names* (No / Prescribed / Supervised / Full Agency) less so — CSA ATF's Intern/Junior/Senior/Principal naming has gained more practitioner traction.
- **"Scoping Matrix" framing**: **AWS-specific**. The framing is inherited from the GenAI Scoping Matrix and is unlikely to be adopted by competing frameworks who have their own terminology. Wiki should use "AWS Agentic AI Security Scoping Matrix" as the full name when citing.
- **The six security dimensions**: **moderate**. The dimension names (especially "Agency perimeters and policies" and "Identity context") are AWS-flavored; the *content* of each dimension overlaps heavily with the wiki's [[agentic-ai-security-cmm-2026|CMM]] D2–D6 per-plane domains and OWASP control families. Watch whether the names propagate.
- **"Confused deputy problem"**: **established**. Not novel to AWS — this is 1980s computer-security terminology (Hardy, 1988) that the matrix references rather than coins.
- **"Graceful degradation"**: **established**. Standard reliability-engineering term; the matrix's contribution is its application to autonomy reduction on security events.

## Limitations

- **Maturity ≠ scope**: see the key-insight callout above. The matrix does not prescribe organizational maturity; it characterizes deployment scope.
- **No threat model**: the matrix names *security focus* per scope but does not enumerate threats systematically. For threat coverage, pair with [[mitre-atlas|MITRE ATLAS]] (`AML.T####` techniques) and [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10]] (ASI01–ASI10).
- **No compliance mapping**: the matrix is not mapped to NIST AI RMF, ISO 42001, EU AI Act, or other compliance frameworks. Users who need a compliance crosswalk must do their own.
- **AWS-centric examples**: implementation guidance assumes AWS infrastructure. Cross-cloud or on-prem implementations need translation.

## Use cases

- **Scope assessment** — useful as a first-pass classifier when intake-reviewing an agentic deployment. Answers "what scope are we in?" before deeper threat-modeling.
- **Progressive-deployment roadmaps** — anchor the discipline of starting at Scope 1 or 2 and earning advancement to Scope 3 or 4.
- **Cross-vendor terminology** — pair with the crosswalk above when reading vendor documentation that uses different ladder names for adjacent concepts.

## Provenance

Authored by [[aaron-brown|Aaron Brown]] and [[matt-saner|Matt Saner]], AWS Security; published on the AWS Security Blog 2025-11-21. Source captured at [[aws-agentic-ai-security-scoping-matrix-blog|the source-summary page]]; original article URL recorded in `sources:` frontmatter.

<!-- sources:auto -->
## Sources

- [The Agentic AI Security Scoping Matrix: A Framework for Securing Autonomous AI Systems](https://aws.amazon.com/blogs/security/the-agentic-ai-security-scoping-matrix-a-framework-for-securing-autonomous-ai-systems/)
<!-- /sources -->
