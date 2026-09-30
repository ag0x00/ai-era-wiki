---
type: framework
title: "AWS Agentic AI Security Scoping Matrix"
address: c-000001
created: 2026-05-07
updated: 2026-09-29
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
verified: 2026-09-29
verified_against:
  - ".raw/articles/aws-agentic-ai-security-scoping-matrix-2026-05-07.md"
verified_findings: 0
verified_note: "Archived AWS summary and original AWS Security Blog read; source and CMM-scope claims corrected."
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

**Scope 2** — bidirectional human interaction for context clarification and agent-initiated requests for information; AWS lists cryptographically signed approval decisions among its implementation considerations.

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

- **Progressive autonomy deployment** — start at Scope 1 or 2; advance when the deployment's authority, oversight, and evidence support the changed scope. The [[agentic-ai-security-cmm-2026|CMM]] provides a separate security-capability profile for each deployment shape.
- **Layered security architecture** — defense-in-depth across network, application, agent, and data layers. Cites the **confused deputy problem** as a load-bearing reason that machine and human identity must both be addressed (a service or human with lesser permissions elevates permissions through an agent that has more).
- **Continuous validation loops** — automated systems that continuously verify agent behavior against expected patterns with escalation procedures for detected deviations.
- **Human oversight integration** — meaningful oversight through strategic checkpoints. The matrix makes an explicit point that human requirements **shift focus rather than diminish** as scope advances: instantiation and approval demands decrease, but audit, assessment, validation, and complex-control requirements increase.
- **Graceful degradation** — automatic autonomy reduction on security events. If agents act beyond intended bounds, detective controls inject tighter restrictions (more HITL, reduced available actions, or full disable). Maps onto the wiki's [[distributed-kill-switch|Distributed Kill Switch]] and [[behavioral-anomaly-detection-for-agents|Behavioral Anomaly Detection]].

## Relationship to the CMM and action tiers

AWS scope describes the authority granted to a deployment and how independently it acts. The [[agentic-ai-security-cmm-2026|AAI-S CMM]] grades the security outcomes evidenced for that deployment across nine domains. No AWS scope implies a CMM level: a full-agency system can have weak controls, while a read-only system can show mature controls for the criteria that apply to it. The assessor records current and risk-selected target levels by domain, with confidence and blockers.

[[least-agency-principle|Action-risk tiers]] describe individual actions. A supervised-agency deployment can permit routine reads automatically, require confirmation for a consequential write, and block another action outright. Scope classification informs the threat model and target selection; the action policy and observed evidence establish whether the selected controls work. [[csa-maestro|CSA ATF]] stages provide a separate autonomy vocabulary and are not CMM score conversions.

## Terminology and boundaries

Agency and autonomy answer different questions: what the agent may do, and how independently it may decide. [[csa-maestro|CSA ATF]] also discusses scope of action and autonomy, but its stages are not conversions of AWS scopes. The CMM's five levels grade security capability, not either of those properties.

The matrix applies established security ideas, including the confused-deputy problem and graceful degradation, to agents. Its distinctive contribution is a four-scope description of permitted action and human oversight, with control considerations for each scope. Use the full AWS framework name when attributing its scope labels.

## Limitations

- **Maturity and scope differ**: the matrix characterizes deployment authority and autonomy. It does not establish a CMM domain level.
- **Threat coverage**: the AWS article's scope descriptions name security concerns but provide no systematic threat inventory. For threat coverage, pair the matrix with [[mitre-atlas|MITRE ATLAS]] (`AML.T####` techniques) and [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10]] (ASI01–ASI10).
- **Compliance mapping**: the article's matrix does not map its scopes to NIST AI RMF, ISO 42001, or the EU AI Act. A compliance claim needs a separate clause-to-control crosswalk.
- **Implementation examples**: the article uses a calendaring workflow and names control properties, but leaves concrete product selection, deployment topology, and evidence tests to the implementer.

## Use cases

- **Scope assessment** — useful as a first-pass classifier when intake-reviewing an agentic deployment. Answers "what scope are we in?" before deeper threat-modeling.
- **Progressive-deployment roadmaps** — anchor the discipline of starting at Scope 1 or 2 and earning advancement to Scope 3 or 4.
- **Cross-vendor terminology** — use the scope and action-tier distinction above when reading vendor documentation that uses different ladder names for adjacent concepts.

## Provenance

Authored by [[aaron-brown|Aaron Brown]] and [[matt-saner|Matt Saner]] of AWS Security and published on the AWS Security Blog on 2025-11-21. The archived summary is [[aws-agentic-ai-security-scoping-matrix-blog|the source-summary page]]. The original article is linked below.

<!-- sources:auto -->
## Sources

- [The Agentic AI Security Scoping Matrix: A Framework for Securing Autonomous AI Systems](https://aws.amazon.com/blogs/security/the-agentic-ai-security-scoping-matrix-a-framework-for-securing-autonomous-ai-systems/)
<!-- /sources -->
