---
type: entity
entity_type: product
title: "Microsoft Entra Agent ID"
created: 2026-05-03
updated: 2026-09-18
tags:
  - products
  - identity
  - nhi
  - agent-lifecycle
  - cots
  - microsoft
  - agent-platform
status: developing
scope_axis:
  - sec-of-ai
origin: aggregated
vendor: "Microsoft"
homepage: "https://learn.microsoft.com/en-us/entra/agent-id/what-is-microsoft-entra-agent-id"
ga_date: "2026-05-01"
related:
  - "[[microsoft-agent-365]]"
  - "[[okta-for-ai-agents]]"
  - "[[agent-identity-architecture]]"
  - "[[nhi-governance-for-agents]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[microsoft-zt4ai|Microsoft ZT4AI]]"
  - "[[microsoft-secure-agentic-ai-end-to-end|Secure Agentic AI End-to-End]]"
  - "[[microsoft-entra-agent-id-security-governance]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
  - "[[microsoft-rai|Microsoft Responsible AI Standard (RAI)]]"
  - "[[agentic-cmm-regulated-fi-stress-test|Agentic AI CMM: Regulated-FI Stress Test]]"
  - "[[crowdstrike-agentic-identity-provider]]"
  - "[[ping-enterprise-personal-agent-access]]"
  - "[[non-human-identity]]"
  - "[[microsoft]]"
  - "[[microsoft-security-copilot]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
sources:
  - "https://learn.microsoft.com/en-us/entra/agent-id/what-is-microsoft-entra-agent-id"
  - ".raw/articles/microsoft-entra-agent-id-what-is-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-identities-overview-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-id-owners-sponsors-managers-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-id-whats-new-2026-09-18.md"
  - ".raw/articles/microsoft-entra-conditional-access-for-agents-2026-09-18.md"
  - ".raw/articles/microsoft-entra-id-protection-for-agents-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-id-design-patterns-2026-09-18.md"
  - ".raw/articles/datadog-entra-agent-id-blueprint-blast-radius-2026-09-18.md"
  - "[[.raw/articles/microsoft-secure-agentic-ai-end-to-end-2026-05-07.md]]"
  - "[[.raw/articles/microsoft-entra-agent-id-security-for-ai-2026-05-25.md]]"
  - "[[.raw/articles/microsoft-entra-agent-id-governance-2026-05-25.md]]"
verified: 2026-09-18
verified_against:
  - ".raw/articles/datadog-entra-agent-id-blueprint-blast-radius-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-id-design-patterns-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-id-governance-2026-05-25.md"
  - ".raw/articles/microsoft-entra-agent-id-owners-sponsors-managers-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-id-what-is-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-id-whats-new-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-identities-overview-2026-09-18.md"
  - ".raw/articles/microsoft-entra-conditional-access-for-agents-2026-09-18.md"
  - ".raw/articles/microsoft-entra-id-protection-for-agents-2026-09-18.md"
verified_findings: 0
verified_note: "Sponsor-succession absence claim was refuted by this page's own governance source; the manager is now named and footnoted. Access-pattern paragraph rewritten from process commentary to the sourced orthogonality."
---

# Microsoft Entra Agent ID

**Sources:** [Microsoft Learn — What is Microsoft Entra Agent ID?](https://learn.microsoft.com/en-us/entra/agent-id/what-is-microsoft-entra-agent-id) · [Microsoft Learn — Agent identities](https://learn.microsoft.com/en-us/entra/agent-id/agent-identities) · [[microsoft-entra-agent-id-security-governance|Entra Agent ID: Security and Governance Model]]

Microsoft Entra Agent ID is "an identity and security framework that extends Microsoft Entra capabilities to AI agents".[^what-is] It gives an agent a directory identity, the credentials behind that identity, and the policy surface that governs what the identity may reach. [[microsoft-agent-365|Microsoft Agent 365]] is the licensed control plane built over it, and it carries the registry and the administration that spans Defender, Intune and Purview; this page covers the identity layer alone.

Entra Agent ID became generally available on 2026-05-01, alongside Agent 365.[^whats-new] Availability splits from entitlement. Microsoft states that "Agent ID is available for all Microsoft Entra customers", and on the same page that "extending Microsoft Entra security features to agents requires Microsoft Agent 365".[^what-is] Conditional Access for agents needs Microsoft Entra ID P1 or P2 **and** an Agent 365 licence for each user, with enforcement of that licensing still to come, and network controls for agents need Microsoft Entra Internet Access.[^cond-access] So the platform is open to every tenant while the controls that make it a security product are not.

## Object model

Four object types carry the model, in Microsoft's names.[^agent-identities][^sponsors][^design-patterns]

| Object | Role |
|---|---|
| Agent identity blueprint | Template and authentication foundation for one or more agent identities |
| Agent identity blueprint principal | The Entra object created when a blueprint is added to a tenant |
| Agent identity | The runtime identity for one agent, a special service principal |
| Agent's user account | Optional account paired 1:1 with an agent identity |

The blueprint holds the credentials and the agent identity holds none. Microsoft states that "agent identities don't have credentials of their own", that they authenticate only through federated identity credentials issued by the blueprint, and that "credentials do not reside on the agent identity".[^agent-identities] The blueprint principal is what "actually acquires tokens, creates agent identities, and appears in audit logs on behalf of the blueprint".[^design-patterns] An agent's user account exists for systems that require a user object, such as Exchange or Teams, and the 1:1 relationship between it and an agent identity is fixed.[^agent-identities]

Policy follows the same hierarchy. "Applying a policy at the blueprint level automatically covers all agent identities derived from it, including any new ones added in the future", so an administrator can apply one Conditional Access policy to a whole agent class, disable the class, or revoke a permission grant across it in a single operation.[^cond-access][^agent-identities] The inheritance has an audit cost: Datadog Security Labs reports that delegated scopes granted through a blueprint "are applied when a token is issued and cannot be seen on the agent identities inheriting them", and that one blueprint carries up to 250 agent identities per tenant.[^datadog] The inheritance also concentrates risk, because a credential added to a blueprint authenticates as any agent identity beneath it.[^datadog]

## Access patterns

Microsoft calls the three flows **agent access patterns** and warns that "on-behalf-of" names an authentication flow rather than a kind of agent.[^cond-access] The pattern determines which object a Conditional Access policy must target.

| Access pattern | Token subject | Policy target |
|---|---|---|
| Acting on behalf of a user | The signed-in user | Users and groups, not agent identities |
| Acting as an application | The agent identity, using blueprint-managed credentials | The agent identity or its blueprint |
| Acting as a user | The agent's user account | The agent's user account |

These are not the agent *types*. Microsoft names assistive, autonomous and user-like agents as a separate taxonomy on the Entra Agent ID overview, and [[microsoft-entra-agent-id-security-governance|the security and governance summary]] treats it as such.[^what-is] The two taxonomies are orthogonal: "any agent can use" the on-behalf-of flow when a signed-in user is present, and "all types of agents might use" the application flow.[^cond-access]

## Accountability roles

Three roles attach to an agent identity, and each carries different authority.[^sponsors] Owners hold technical administration. Sponsors are "business representatives accountable for the agent's purpose and lifecycle decisions, including access reviews and agent retention, without technical administrative access", and at least one sponsor is required for every agent identity and every blueprint. Managers sit in the organizational hierarchy and "don't have authorization to modify or delete agents".

A sponsor is not necessarily a named human. Groups can be assigned, and "when a group is assigned, all members of the group have sponsor rights over the Agent ID object"; an agent identity or blueprint takes up to 100 sponsors with no more than five groups, and an agent's user account up to five.[^sponsors] Group sponsorship weakens the accountability chain that a per-agent human owner would give, because the accountable party becomes a membership list.

Succession runs through Entra ID Governance, and the successor is named. The identity-governance documentation states that "if the sponsor is leaving the organization, sponsorship of the agent identities is automatically transferred to their manager", which keeps a human accountable for the agent's access and lifecycle.[^governance] The concept documentation carries the requirement alone, that "sponsorship should be maintained to ensure succession when an employee who's a sponsor moves or leaves".[^sponsors] The mechanism ships as two lifecycle workflow templates for "notifying managers and cosponsors, and automatically transfer sponsorship when an agent identity sponsor changes roles or leaves the organization, to prevent orphaned agents", which an administrator deploys; that release note names no recipient for the transfer.[^whats-new]

## Risk detection and access control

Identity Protection scores agent risk through eight detections, among them early-life malicious activity, suspicious credential usage and directory reconnaissance; at the time of the page read all agent risk detections were offline rather than real-time.[^id-protection] Attribution has a documented gap: "in on-behalf-of (OBO) flows, where an agent acts using a user's delegated permissions, risky activity is attributed to the user rather than the agent".[^id-protection]

Blocking a risky agent is an administrator's deployment, not a default. Microsoft ships it as a Conditional Access template, "Block access for high-risk agent identities", which blocks sign-ins from risky agent identities once an administrator creates the policy from it.[^whats-new] Three further blind spots are documented on the Conditional Access page: policies do not apply where an agent uses an API key, because that path bypasses Entra token issuance; "policies targeting all users don't include agent's user accounts"; and a policy targeting agent identities "won't apply to the agent's user account".[^cond-access]

## Reach over agents built elsewhere

An agent built outside Microsoft acquires an Entra Agent ID through the Microsoft Entra ID Auth SDK, used as a sidecar, or through workload identity federation; Microsoft names AWS Bedrock and n8n as platforms integrated this way.[^what-is] The platform supports OAuth 2.0, the Model Context Protocol and agent-to-agent communication.[^what-is] Registry-level reach over third-party agents belongs to [[microsoft-agent-365|Agent 365]] rather than to this layer.

## Comparison with Okta for AI Agents

| Dimension | [[okta-for-ai-agents\|Okta for AI Agents]] | Microsoft Entra Agent ID |
|---|---|---|
| General availability | 2026-04-29[^okta-ga-cmp] | 2026-05-01[^whats-new] |
| Best fit | Okta-as-IdP organizations | Microsoft 365 and Azure-native organizations |
| Entitlement | Okta tenant | Open to all Entra customers; security features need Agent 365[^what-is] |
| Registry | Okta Universal Directory and Agent Discovery | Agent Registry, in the Microsoft 365 admin center |
| Audit integration | Okta System Log | Microsoft Entra sign-in and audit logs |
| Agents built elsewhere | IdP-agnostic | Auth SDK sidecar or workload identity federation[^what-is] |

Two more vendors entered the category in September 2026: [[crowdstrike-agentic-identity-provider|CrowdStrike's Agentic Identity Provider]], from the endpoint-security market, and [[ping-enterprise-personal-agent-access|Ping's Enterprise Personal Agent Access]], from an established IAM vendor.

## CMM positioning

Organizations adopting Entra Agent ID reach **L3** on the [[agentic-ai-security-cmm-d2-identity|D2 identity and authorization track]], on the same footing as [[okta-for-ai-agents|Okta for AI Agents]]: a verifiable per-agent directory identity, minted from a blueprint and governed by policy at the class level.

Token scope bounds what that rung buys. [[agentic-cmm-regulated-fi-stress-test|The regulated-FI CMM stress test]] records that Agent ID issues tokens scoped to an agent identity or its blueprint rather than to a task, which leaves per-task holder-bound capability grants of the [[tenuo-warrant|Tenuo Warrant]] class as an off-stack fill for an all-Microsoft buyer. The [[agentic-ai-security-cmm-d7-observability|D7 observability]] rung a buyer can reach depends on Purview action tracing, which is an [[microsoft-agent-365|Agent 365]] entitlement rather than an Entra Agent ID one, so the identity layer alone does not carry it. Sponsor accountability and the orphaned-agent workflows are the mechanism behind [[agentic-ai-security-cmm-d9-operations|D9]] decommission evidence, weakened where sponsorship sits on a group.

## Notes

[^what-is]: Microsoft, [*What is Microsoft Entra Agent ID?*](https://learn.microsoft.com/en-us/entra/agent-id/what-is-microsoft-entra-agent-id), retrieved 2026-09-18. Local copy: `.raw/articles/microsoft-entra-agent-id-what-is-2026-09-18.md`.
[^whats-new]: Microsoft, [*What's new in Microsoft Entra Agent ID*](https://learn.microsoft.com/en-us/entra/agent-id/whats-new-agent-id), page date 2026-05-01, retrieved 2026-09-18. Local copy: `.raw/articles/microsoft-entra-agent-id-whats-new-2026-09-18.md`.
[^agent-identities]: Microsoft, [*Agent identities*](https://learn.microsoft.com/en-us/entra/agent-id/agent-identities), retrieved 2026-09-18. Local copy: `.raw/articles/microsoft-entra-agent-identities-overview-2026-09-18.md`.
[^design-patterns]: Microsoft, [*Microsoft Entra Agent ID design patterns*](https://learn.microsoft.com/en-us/entra/agent-id/concept-agent-id-design-patterns), retrieved 2026-09-18. Local copy: `.raw/articles/microsoft-entra-agent-id-design-patterns-2026-09-18.md`.
[^sponsors]: Microsoft, [*Agent owners, sponsors, and managers*](https://learn.microsoft.com/en-us/entra/agent-id/agent-owners-sponsors-managers), retrieved 2026-09-18. Source of the role definitions and of the sponsor limits (100 per agent identity or blueprint with at most five groups; five per agent's user account). Local copy: `.raw/articles/microsoft-entra-agent-id-owners-sponsors-managers-2026-09-18.md`.
[^cond-access]: Microsoft, [*Conditional Access for agents*](https://learn.microsoft.com/en-us/entra/identity/conditional-access/agent-id), retrieved 2026-09-18. Source of the three access patterns, the licensing requirement and the three documented blind spots. Local copy: `.raw/articles/microsoft-entra-conditional-access-for-agents-2026-09-18.md`.
[^id-protection]: Microsoft, [*Risky agents in Microsoft Entra ID Protection*](https://learn.microsoft.com/en-us/entra/id-protection/concept-risky-agents), retrieved 2026-09-18. Eight agent risk detections, all offline at the date of the read. Local copy: `.raw/articles/microsoft-entra-id-protection-for-agents-2026-09-18.md`.
[^datadog]: Datadog Security Labs, [*Agent ID blueprint blast radius*](https://securitylabs.datadoghq.com/articles/agent-id-blueprint-blast-radius/), retrieved 2026-09-18. The 250 figure is the documented Entra directory limit on agent identities associated with one blueprint per tenant. Local copy: `.raw/articles/datadog-entra-agent-id-blueprint-blast-radius-2026-09-18.md`.
[^governance]: Microsoft, [*Governing agent identities*](https://learn.microsoft.com/en-us/entra/id-governance/agent-id-governance-overview), retrieved 2026-05-25. Source of the automatic transfer of sponsorship to a departing sponsor's manager. Local copy: `.raw/articles/microsoft-entra-agent-id-governance-2026-05-25.md`.
[^okta-ga-cmp]: Okta, [*Okta for AI Agents is now generally available*](https://www.okta.com/blog/ai/okta-for-ai-agents-general-availability/) (2026-04-29), retrieved 2026-09-18. See [[okta-for-ai-agents|Okta for AI Agents]].
