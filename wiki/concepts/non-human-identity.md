---
type: concept
title: "Non-Human Identity (NHI)"
address: c-000187
created: 2026-04-30
updated: 2026-08-15
tags:
  - concepts
  - identity
  - agentic-ai
  - iam
  - nhi
status: developing
scope_axis:
  - sec-of-ai
complexity: intermediate
domain: identity-and-access-management
source_url: "https://owasp.org/www-project-non-human-identities-top-10/"
aliases:
  - "NHI"
  - "machine identity"
  - "workload identity"
related:
  - "[[agent-identity-architecture]]"
  - "[[nhi-governance-for-agents]]"
  - "[[credential-proxy-pattern]]"
  - "[[identity-credential-coupling]]"
  - "[[what-are-non-human-identities]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[capability-based-authorization]]"
  - "[[tenuo-warrant]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[okta-for-ai-agents]]"
  - "[[oasis-security]]"
  - "[[shadow-automation]]"
  - "[[agent-observability]]"
  - "[[owasp-state-of-agentic-ai-security-governance]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
sources:
  - "[[.raw/papers/securing-the-autonomous-future.md]]"
  - "[[what-are-non-human-identities]]"
---

# Non-Human Identity (NHI)

**Non-Human Identities (NHIs)** are digital credentials assigned to software workloads — services, bots, automation scripts, and AI agents — rather than to human users. In agentic AI, NHIs encompass every API key, service account, certificate, OAuth token, and JWT issued to an agent so it can authenticate to internal or external systems. The [[agent-identity-architecture|AI Agent Identity Architecture]] defines the governance model; the [[agentic-ai-security-reference-architecture|RA]] Identity plane is the implementation surface, and the [[agentic-ai-security-cmm-d2-identity|CMM D2]] deep dive measures maturity.

## On this page

- [[#Aliases and variants]]
- [[#NHI taxonomy]]
- [[#Why NHI matters for agentic AI]]
- [[#Why human-identity controls fail for NHIs]]
- [[#Real-world incident anchors]]
- [[#Relationship to DSPM]]
- [[#Governance capabilities and the platform-native landscape]]
- [[#The per-task authority frontier]]
- [[#Credential Zero problem]]
- [[#Where this sits in the RA and CMM]]

## Aliases and variants

- **Machine identity** — broader term covering all software principals
- **Workload identity** — often used specifically for cloud-native, certificate-based identity (e.g., [[spiffe|SPIFFE]] SVIDs)
- **Service account** — legacy term for IAM principals bound to a software service
- **NHI** — preferred shorthand in recent vendor and analyst discourse

## NHI taxonomy

The wiki enumerates twelve types, each with a distinct rotation, ownership, and detection profile (per [[what-are-non-human-identities|What Are Non-Human Identities? (Oasis Security)]], which lists eleven — the wiki splits Oasis's combined "tokens and certificates" row into OAuth tokens and TLS/mTLS certificates):

| Type                                           | Authentication mechanism                    | Rotation profile                                                                 |
| ---------------------------------------------- | ------------------------------------------- | -------------------------------------------------------------------------------- |
| Service Account                                | Username/password or service-account secret | Decoupled (secret separate from principal)                                       |
| Service Principal                              | Client secret or certificate                | Decoupled                                                                        |
| System Account                                 | OS-issued, typically high privilege         | Decoupled                                                                        |
| IAM Role (AWS-style)                           | Temporary STS session credentials           | Decoupled, short-lived                                                           |
| API Key                                        | Static key                                  | Decoupled                                                                        |
| Machine Identity (VM / container / serverless) | Cloud-issued cert / SVID                    | Decoupled                                                                        |
| OAuth Token                                    | Time-limited bearer                         | Decoupled                                                                        |
| TLS / mTLS Certificate                         | Asymmetric key                              | Decoupled                                                                        |
| Storage Access Key                             | Long-lived, broad-permission                | **Coupled** (see [[identity-credential-coupling\|Identity-Credential Coupling]]) |
| SAS Token (Azure)                              | Time-limited, granular                      | **Coupled**                                                                      |
| Personal Access Token (PAT)                    | Developer-generated                         | Often **coupled** (the token IS the access)                                      |
| Database User                                  | Application-level                           | Decoupled                                                                        |

The **coupled** rows are where the credential string IS the identity — rotation is identity rotation. See [[identity-credential-coupling|Identity-Credential Coupling]] for implications.

## Relevance to agentic AI

AI agent deployments amplify the NHI problem in three ways:

1. **Scale.** Each agent — often each agent task — may require one or more credentials. Large multi-agent systems generate thousands of identities quickly.
2. **Ephemerality.** Agents are spun up and torn down dynamically; their credentials may not be systematically revoked.
3. **Autonomy.** Unlike a human who can be asked to justify an action, an agent may exercise credentials in ways that are opaque without purpose-built tracing.

The combined effect is a rapidly growing NHI estate that is poorly inventoried and a high-value attack surface.

[[owasp-state-of-agentic-ai-security-governance|OWASP's State of Agentic AI Security and Governance]] separates two layers the industry conflates. NHI is an authentication primitive that answers "is this credential valid?" and gates identity at session start. Agent Identity is a governance layer above it that attests provenance, intent, and authority continuously, governing behavior at the moment of each action rather than only at login. NHI is the principal a per-agent credential names; Agent Identity is what makes that principal accountable across a multi-step task. The threat both layers answer is Identity Spoofing and Impersonation (T9) in [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]] — impersonation of agents, users, or services, and theft of a persistent agent identity.

### Scale evidence (triangulated)

| Source                         | Metric                                                                       | Year      | Type |
| ------------------------------ | ---------------------------------------------------------------------------- | --------- | --- |
| CyberArk                       | 82:1 machine-to-human identity ratio                                         | 2025      | Vendor (Vanson Bourne survey, n=2,600) |
| Rubrik Zero Labs               | 45:1 average; up to 100:1 in some orgs                                       | 2025      | Vendor |
| [arXiv 2503.18255](https://arxiv.org/abs/2503.18255) | 50K → 250K machine identities per enterprise (400% growth) | 2021–2025 | Academic preprint |
| SailPoint Horizons of Identity Security | 69% of orgs have more machine than human identities; ~half deploy 10× more | 2025–2026 | Vendor (n=375 IAM decision-makers, methodology disclosed) |
| [Verizon DBIR 2025](https://www.verizon.com/business/resources/reports/dbir/) | Third-party breach involvement doubled YoY (15% → 30%), driven by ungoverned machine accounts | 2025 | Industry survey (annual, methodology disclosed) |
| GitGuardian / Verizon 2025 | 441,780 exposed secrets in public repos; 39% web-app infra (66% JWTs) | 2025 | Industry-data quantitative |
| [IBM 2025 Cost of a Data Breach](https://www.ibm.com/reports/data-breach) | \$4.44M global / \$10.22M US average; explicit recommendation for NHI controls | 2025      | Industry survey (Ponemon methodology) |
| [[enisa\|ENISA]] Threat Landscape 2025 | Confirms identity- and credential-themed risk acceleration | 2025 | EU government |

Sources differ on the exact ratio (45:1 vs 82:1; SailPoint reports 69% of orgs have more machine than human, ~half 10×) but agree on the structural point: NHIs outnumber humans by an order of magnitude or more, and growth is fast. The exact 82:1 figure is **single-vendor** (CyberArk-specific); the directional claim is well-corroborated. The per-figure provenance, vendor-conflict labels, and independent corroboration live in [[source-triangulation-audit-2026-05-02|the Source Triangulation Audit]] §Claim 1, which is the citation home for the vendor rows that carry no public deep link (CyberArk, Rubrik, SailPoint, GitGuardian).

## Failure of human-identity controls

Eight structural differences make HR-driven IAM and credential-storage-focused PAM unsuitable (per [[what-are-non-human-identities|What Are Non-Human Identities? (Oasis Security)]]):

| Property | Human | NHI |
|---|---|---|
| Centralization | IT-managed, single source of truth | Decentralized; created by developers / IT / IaC |
| Ownership | Tied to individual | Often unowned; shared across teams / apps |
| Scale | Linear with headcount | 10–100× human count, growing exponentially |
| Rate of change | HR-driven (joiner/mover/leaver) | Code-pace (per commit / per deploy) |
| Provisioning | IT-mediated | Developer-driven, often invisible to IT |
| Secret expiration | Frequent password rotation | Often never rotated; sometimes no expiration |
| Operational risk of rotation | Low | High (rotation breaks production workflows) |
| Authentication factor diversity | Three-factor + MFA + SSO | Single-factor (the secret); no MFA equivalent |

Legacy IAM/PAM cannot be retrofitted for NHIs at scale; purpose-built tooling is required (per [[what-are-non-human-identities|Oasis Security]]).

## Real-world incident anchors

| Incident                    | Vector                                     | NHI lesson                                                                                                                                              |
| --------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Microsoft AI Storage Breach | Misconfigured SAS token                    | 38TB internal data exposed (passwords + private keys); coupled identity-credential — see [[identity-credential-coupling\|Identity-Credential Coupling]] |
| CircleCI Breach             | OAuth token compromise                     | Mass-rotation across thousands of customer environments — rotation infrastructure must be tested                                                        |
| Mercedes-Benz Breach        | Service accounts with excessive privileges | Long-lived, over-permissioned NHIs are persistent attacker access                                                                                       |
| [[taiwan-ai-agent-government-intrusion\|Taiwan AI-Agent Government Intrusion]] | Multi-agent attacker framework cracked 85 credentials, 98.8% pivoted via SSO | Federation trust with no per-resource step-up; human, not agent, credentials |

## Relationship to DSPM

Insight Partners draw an analogy: **NHI vendors are to agent credentials what [[dspm|DSPM]] (Data Security Posture Management) vendors were to data sprawl**. Just as DSPM helped enterprises discover and govern data that had proliferated across cloud stores without oversight, NHI tooling helps discover, classify, and govern the machine credentials agents accumulate.

> [!note] Framing from source
> "Improper management of credentials related to NHIs is a people and process problem, not so much a technology problem." — Insight Partners, 2025

## Governance capabilities and the platform-native landscape

NHI governance solutions cover: discovery (machine identity inventory across cloud, SaaS, and on-prem); lifecycle management (rotation, expiry, revocation); scoped permissions (least privilege per identity, extending into Identity Security Posture Management / ISPM); and OAuth / API scope governance.

The market shifted during 2026. Per-agent identity is no longer the preserve of specialist vendors: it is **GA platform-native on all three hyperscalers** — [[microsoft-entra-agent-id|Microsoft Entra Agent ID]], AWS Bedrock AgentCore identities, and GCP Agent Identity — with [[okta-for-ai-agents|Okta for AI Agents]] in Early Access. What remains a developing COTS layer is the NHI *governance* posture above bare identity: discovery of unenrolled agents, lifecycle, and least-privilege review, where [[oasis-security|Oasis Security]], Aembit, Astrix, and [[cyberark-conjur|CyberArk Conjur]] compete. Conditional Access for Agent Identities (Entra) adds risk-based step-up where the platform supports it.

Shadow-agent discovery — the [[microsoft-entra-agent-id|Agent 365 Registry]], Okta Agent Discovery — surfaces agents created at developer pace outside the enrollment process, addressing the [[shadow-automation|shadow automation]] (Microsoft's "agent sprawl") problem.

The [[microsoft-zt4ai|Microsoft ZT4AI]] framework extends the Zero Trust principles — verify explicitly, least privilege, assume breach — to these machine identities, with [[microsoft-entra-agent-id|Entra Agent ID]] as the per-agent identity anchor (see [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]]).

## The per-task authority frontier

A verified NHI still carries **ambient authority**: its workload's full standing permissions on every call, far wider than any one task needs (see [[ambient-vs-derived-authority|Ambient vs Derived Authority]]). Workload identity is task-blind. The frontier control is [[capability-based-authorization|capability-based authorization]] — a task-scoped, signed, attenuating artifact that carries its own policy, so a compromised agent cannot exceed the scope minted for its task. The [[tenuo-warrant|Tenuo Warrant]] is the leading vendor-neutral implementation ([[monotonic-attenuation|monotonic attenuation]]: a delegated child grant can only shrink). No hyperscaler ships per-task holder-bound tokens; platform products issue per-*resource* tokens, which is why this sits at the top of the [[agentic-ai-security-cmm-d2-identity|D2 ladder]] (L5+).

## Credential Zero problem

The **Credential Zero problem** is the bootstrap step: an agent must authenticate *to* a secrets vault or IdP before it can retrieve working credentials. Solutions include [[agent-identity-architecture#spiffe--spire-workload-identity-foundation|SPIFFE/SPIRE]] certificates baked into the workload at deploy time, which avoid storing static secrets entirely, and platform credential-less identity models (Azure Managed Identities, AgentCore token vault, GCP auth-manager).

## Placement in the RA and CMM

- **Architecture.** [[agent-identity-architecture|AI Agent Identity Architecture]] frames the layers; the [[agentic-ai-security-reference-architecture|RA]] Identity plane implements them (workload identity, agent/NHI lifecycle governance, credential proxy, action-to-identity tracing).
- **Practice.** [[nhi-governance-for-agents|NHI Governance for AI Agents]] is the operational discipline.
- **Maturity.** The [[agentic-ai-security-cmm-d2-identity|CMM D2]] ladder grades it: per-agent identity + owner + deploy-pipeline lifecycle + coupled/decoupled inventory at L3, zero-credentials-in-context + automated rotation at L4, a unified governance program with shadow-agent discovery at L5, per-task capability tokens at L5+.
- **Monitoring.** [[agent-observability|Agent Observability]] — action-to-identity tracing requires knowing which NHI took which action.

<!-- sources:auto -->
## Sources

- [Non-Human Identity (NHI)](https://owasp.org/www-project-non-human-identities-top-10/)
<!-- /sources -->
