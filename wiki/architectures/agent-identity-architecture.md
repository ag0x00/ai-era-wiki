---
type: architecture
title: "AI Agent Identity Architecture"
address: c-000188
created: 2026-04-30
updated: 2026-09-10
tags:
  - architectures
  - identity
  - agentic-ai
  - iam
  - nhi
  - spiffe
status: developing
scope_axis:
  - sec-of-ai
attributed_to: "Insight Partners (George Mathew, Hunter Korn et al.)"
problem_solved: "Authenticating and authorizing AI agents in enterprise ecosystems — both machine-to-machine internal communication and external service access — without creating unmanaged credential sprawl."
components:
  - "Delegated Access Model"
  - "Autonomous Agent Model"
  - "SPIFFE/SPIRE Workload Identity"
  - "Secrets Vault / PAM Layer"
  - "Authorization Policy Layer"
  - "Capability-Token Layer"
  - "Action-to-Identity Trace"
related:
  - "[[non-human-identity]]"
  - "[[nhi-governance-for-agents]]"
  - "[[credential-proxy-pattern]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[capability-based-authorization]]"
  - "[[ambient-vs-derived-authority]]"
  - "[[tenuo-warrant]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[okta-for-ai-agents]]"
  - "[[agent-observability]]"
  - "[[owasp-state-of-agentic-ai-security-governance]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[hugging-face]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[falcon-guardian]]"
  - "[[crowdstrike-agentic-identity-provider]]"
  - "[[ping-enterprise-personal-agent-access]]"
sources:
  - "[[securing-the-autonomous-future]]"
coined_by:
  - "[[insight-partners]]"
---

# AI Agent Identity Architecture

The conceptual identity architecture for AI agents comprises three elements: the identity models available, the layers that authenticate and authorize an agent, and the trace that binds each action to a human. The [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] realizes it as the **Identity plane**, and the [[agentic-ai-security-cmm-d2-identity|CMM D2 Identity & Authorization]] deep dive measures organizational maturity against it.

## On this page

- [[#Problem]]
- [[#Two identity models]]
- [[#Layers]]
- [[#Platform-native landscape]]
- [[#Data and control flow]]
- [[#Trade-offs]]
- [[#Placement in the RA and CMM]]
- [[#See also]]

## Problem

AI agents must authenticate to and be authorized for services inside and outside the enterprise. They differ from human users in three ways: they may be ephemeral, they arrive in large numbers, and they act either on behalf of a human (delegated) or under their own identity (autonomous). Incumbent identity governance, vaulting and PAM capabilities cover the credential half of this, and how far they stretch to an agent population this large and this short-lived is unsettled. Existing protocols such as OAuth 2.0 do not model *who directed an action* — the agent or a human (per [[securing-the-autonomous-future|Securing the Autonomous Future]]).

Two design problems sit underneath: a **principal problem** (every agent needs a verifiable identity that traces to a human) and an **authority problem** (a verified identity still carries workload-wide ambient authority, far wider than any one task needs). The layers below address the first; the [[#Layers|capability-token layer]] addresses the second.

[[owasp-state-of-agentic-ai-security-governance|OWASP's State of Agentic AI Security and Governance]] frames this as the policy anchor for the architecture: [[non-human-identity|NHI]] is the authentication primitive (a valid credential at session start), while Agent Identity is the governance layer that attests provenance, intent, and authority continuously and governs behavior at each action. The architecture defends against the corresponding threats in [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]]: Privilege Compromise (T3), Identity Spoofing and Impersonation (T9), and Insecure Inter-Agent Protocol Abuse (T16), the last exercised at the MCP and A2A boundaries where agent identity is presented to other parties.

## Two identity models

### Delegated access model

The agent acts **on behalf of a human user** using that user's scoped access token, which is the usual arrangement for copilots and AI coding assistants. Governance stays simpler because the human remains the principal of record. [[microsoft-entra-agent-id|Microsoft Entra Agent ID]] calls this the **assistive** pattern (delegated permissions, acts for a user), and [[okta-for-ai-agents|Okta for AI Agents]] implements it as the OAuth 2.1 delegation flow.

### Autonomous agent model

The agent holds a **unique identity** and authenticates independently to carry out tasks. Infrastructure agents, RPA-style workflows, and AI-employee scenarios require this model. Governance is harder here, because identity sprawl runs fast and credentials may be ephemeral. Entra Agent ID calls this the **autonomous** pattern (own identity, client-credentials flow) and defines a third, hybrid shape as well — an agent paired 1:1 with its own user account (mailbox, Teams), for cases where the agent must appear as a directory user.

> [!note] Trend
> Enterprises today lean toward delegated access for productivity use cases. The balance is expected to shift toward autonomous agents as AI-native workflows mature. The platform-native identity products that shipped in 2026 support both models from a single directory object.

## Layers

### SPIFFE / SPIRE workload identity foundation

[[spiffe|SPIFFE]] (Secure Production Identity Framework for Everyone) and SPIRE provide cryptographically verifiable identities to workloads — agents, orchestrators, vector stores, LLM endpoints — without static secrets. Enterprises already run SPIFFE/SPIRE for machine-to-machine workload identity, and the platform-native agent identities build on it: GCP Agent Identity issues SPIFFE-based IDs directly. SPIFFE also closes the **Credential Zero** problem. An agent must authenticate *to* a vault or IdP before it can retrieve any further credential, and a SPIFFE Verifiable Identity Document (SVID) provisioned at deploy time carries that first authentication without a pre-stored secret.

> [!note] Authentication only
> SPIFFE/SPIRE establishes *who* a workload is. An **authorization layer** ([[#Authorization policy layer]] below) must be added to define *what* an authenticated agent may do, and a [[#Capability-token layer]] to bound *which task* a given grant covers.

### Secrets vault and PAM layer

For external service access (API keys, JWTs, OAuth tokens), agents retrieve short-lived credentials from a secrets vault or modern PAM product. The [[credential-proxy-pattern|credential proxy pattern]] keeps these credentials out of the agent's context entirely — the agent holds a proxy token, the proxy injects the real secret at the network layer. A credential-less identity model (Azure Managed Identities, AWS Bedrock AgentCore token vault, GCP auth-manager) achieves this by never issuing the agent a long-lived secret. [[non-human-identity|Non-Human Identity (NHI)]] governance vendors extend PAM to discover and manage the lifecycle of the machine identities AI deployments create.

### Authorization policy layer

After authentication, a [[oversight-layer|Policy Decision Point]] enforces scoped permissions. [[cedar|Cedar]] and [[opa|OPA]]/Rego are the common policy engines; platform-native PDPs now ship (AWS Bedrock AgentCore Policy on Cedar; Microsoft Agent Governance Toolkit). The PDP answers a binary question against the agent's identity and the requested action.

### Capability-token layer

Identity-based authorization is **ambient**: a verified agent carries its workload's full standing authority on every call, far wider than any single task requires (see [[ambient-vs-derived-authority|Ambient vs Derived Authority]]). The capability-token layer makes authority **derived**. It mints a task-scoped, signed, short-lived artifact that *carries its own policy*, so a compromised agent cannot exceed the scope minted for that task. The [[tenuo-warrant|Tenuo Warrant]] implements the layer vendor-neutrally, through [[capability-based-authorization|capability-based authorization]] with [[monotonic-attenuation|monotonic attenuation]], where a delegated child grant can only shrink, never widen. Platform identity products issue per-*resource* (audience) tokens, and no hyperscaler yet ships a per-*task* holder-bound token, which leaves this layer ahead of the shipping landscape.

### Action-to-identity trace

Every action is recorded against the identity that took it and the context that triggered it (human instruction versus autonomous decision). Entra Agent ID **sponsors** bind every agent to a named human whose accountability transfers automatically to their manager on departure, [[microsoft-entra-agent-id|Microsoft Agent 365]] writes the trail to Purview, and the Anthropic Compliance API attributes Claude-generated actions to a deployment identity. The remaining standards gap is capturing the *delegation chain* — who instructed the agent — at the protocol level rather than only in audit logs. The NIST CAISI Concept Paper takes it up in its OAuth 2.1 / OIDC extensions for agents, and the warrant's embedded delegation chain satisfies it cryptographically.

## Platform-native landscape

Per-agent identity moved from emerging to **GA platform-native on all three hyperscalers** during 2026, displacing the earlier picture in which only specialist PAM/NHI vendors served the case:

| Capability | Status (mid-2026) | Reference implementations |
|---|---|---|
| Per-agent identity | GA platform-native; also the security-platform row below | [[microsoft-entra-agent-id\|Entra Agent ID]] (GA Apr 2026); AWS Bedrock AgentCore identities; GCP Agent Identity (SPIFFE-based); [[spiffe\|SPIFFE/SPIRE]] (OSS); [[okta-for-ai-agents\|Okta for AI Agents]] (Early Access, GA expected FY27) |
| Credential-less / vault | GA platform-native | Azure Managed Identities; AgentCore token vault; GCP auth-manager; [[credential-proxy-pattern\|credential proxy]] (OSS/COTS) |
| NHI governance (discovery, lifecycle, posture) | Developing COTS | [[oasis-security\|Oasis Security]], Aembit, Astrix, [[cyberark-conjur\|CyberArk Conjur]], Okta NHI |
| Conditional / risk-based access for agents | MS GA; no AWS/GCP equivalent | Conditional Access for Agent Identities (Entra ID P1); ID Protection for agents |
| Per-task capability tokens | Leading-edge, OSS-only | [[tenuo-warrant\|Tenuo Warrant]] (Ed25519, monotonic attenuation) |
| Per-agent identity from security-platform vendors (non-IdP incumbents) | Announced; one available, one in development | [[crowdstrike-agentic-identity-provider\|CrowdStrike Agentic IdP]] (in development); [[ping-enterprise-personal-agent-access\|Ping Enterprise Personal Agent Access]] (available) |

Two security-platform vendors entered the per-agent identity market in the first week of September 2026, from outside the IdP incumbency the table's first row records. [[crowdstrike-agentic-identity-provider|CrowdStrike's Agentic Identity Provider]] issues a cryptographically verifiable identity at the point [[falcon-guardian|Falcon Guardian]] discovers an agent, brokers short-lived tokens in place of standing credentials, and binds each action to the delegating human or workload. CrowdStrike announced it on 2026-09-02, states the product is in development, and gives no general-availability date. [[ping-enterprise-personal-agent-access|Ping's Enterprise Personal Agent Access]], announced 2026-09-01, delivers agent discovery, secretless just-in-time privileged access and runtime action control through PingOne Privilege; Ping states it is available now. CrowdStrike scopes a token to a task rather than to a workload; Ping scopes access to the resource an agent reaches and the action it takes, and states no task boundary. Neither publishes a holder-binding or attenuation mechanism, so neither reaches the capability-token layer above.

A verifiable per-agent identity is the prerequisite for per-agent egress policy and per-agent behavioral baselining, so the identity layer is built first (the [[agentic-ai-security-cmm-d2-identity|D2→D5 and D2→D7 dependency caps]]).

## Data and control flow

```
Human User
    │  (delegates scope, or triggers an autonomous workflow)
    ▼
Agent Identity (delegated token OR own SPIFFE SVID / platform agent identity)
    │
    ├─► Authorization: PDP (Cedar/OPA) — may a grant of this shape proceed?
    │       └─► Capability token (Tenuo Warrant) — scoped to THIS task, attenuating
    │
    ├─► Internal services (LLM, vector store, orchestrator)
    │       └─ SPIFFE/SPIRE mTLS
    │
    └─► External services (APIs, SaaS, MCP servers)
            └─ Vault-retrieved / proxy-injected credential (agent never holds it)
                    └─ NHI governance tracks lifecycle

All actions → Action-to-Identity Trace (delegation chain, sponsor-attributed)
```

## Trade-offs

| Aspect | Delegated Access | Autonomous Agent |
|---|---|---|
| Governance complexity | Lower (human remains principal) | Higher (own identity, ephemeral) |
| Blast radius if compromised | Limited to that user's scope | Broad if over-provisioned; frozen at grant scope if capability-token-bounded |
| Suitable for | Copilots, coding assistants | Infrastructure agents, AI employees |
| IAM tooling maturity | High (existing IAM/PAM + GA agent identity) | Medium (GA agent identity; NHI governance and per-task tokens still maturing) |

Ambient pod-level identity broadened the autonomous-agent blast radius in the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]]. Lateral movement ran on IAM credentials read from the instance metadata service (IMDS) and on over-permissioned Kubernetes service accounts; at [[hugging-face|Hugging Face]] a single dataset-worker pod reached cluster admin across multiple clusters in under 13 hours (Dalton and Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026, summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]). Pod-level identity is ambient by construction, because the service account attaches to the workload and every job in that pod therefore carries the union of authority any job might need. A single compromise inherits all of it. That is the concrete case for the capability-token layer above — a per-task grant that only attenuates does not widen when the holder is taken — and for treating service-account scope as an identity-plane control rather than a Kubernetes deployment detail.

Ambient federation trust produces the same structural failure on the identity plane's other axis. In the [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]], a multi-agent attacker framework cracked 85 personnel credentials and pivoted 84 of them (98.8%) laterally via SSO, because no per-resource step-up sat between the federated session and the resources it reached (Dream Security, ["Inside a Multi-Agent AI Framework Used to Compromise Government Entities in Asia"](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia), 2026-08-12).

## Placement in the RA and CMM

- **Reference architecture.** The [[agentic-ai-security-reference-architecture|Agentic AI Security RA]] **Identity plane** is the implementation surface of this page: workload identity, agent/NHI lifecycle governance, the credential proxy, action-to-identity tracing, and OAuth 2.1/OIDC delegation, with the capability-token layer split across Identity and Control.
- **Maturity model.** The [[agentic-ai-security-cmm-d2-identity|CMM D2 deep dive]] turns these layers into a graded ladder: per-agent identity + human owner + deploy-pipeline lifecycle at L3, zero-credentials-in-context + automated rotation at L4, a unified governance program with shadow-agent discovery at L5, and per-task holder-bound capability tokens at L5+. Egress and observability cannot exceed D2-L3, which is why that rung is built first.

## See also

- [[non-human-identity|Non-Human Identity (NHI)]] — the credential class this architecture governs, and the NHI governance vendor category
- [[nhi-governance-for-agents|NHI Governance for AI Agents]] — the operational practice
- [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]] — the load-bearing control of the secrets layer
- [[capability-based-authorization|Capability-Based Authorization]] / [[tenuo-warrant|Tenuo Warrant]] — the per-task authority frontier
- [[mcp-security|MCP Security]] — external protocol boundary where agent identity is exercised
- [[agent-observability|Agent Observability]] — how the action-to-identity trace feeds monitoring
- [[owasp-state-of-agentic-ai-security-governance|State of Agentic AI Security and Governance v2]] — OWASP's framing of Agent Identity (a governance layer attesting provenance, intent, authority) as distinct from NHI (an authentication primitive)
