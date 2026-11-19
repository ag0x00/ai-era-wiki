---
type: concept
title: "SPIFFE / SPIRE"
created: 2026-04-30
updated: 2026-10-06
tags:
  - concepts
  - identity
  - workload-identity
  - spiffe
status: developing
scope_axis:
  - sec-of-ai
source_url: "https://spiffe.io/docs/latest/spiffe-about/spiffe-concepts/"
complexity: advanced
domain: identity-architecture
aliases:
  - "SPIFFE"
  - "SPIRE"
  - "Secure Production Identity Framework for Everyone"
related:
  - "[[agent-identity-architecture]]"
  - "[[non-human-identity]]"
  - "[[nhi-governance-for-agents]]"
  - "[[shadow-agent-admission-control]]"
  - "[[a2a-protocol]]"
  - "[[google-cloud-agentic-security-profile]]"
  - "[[agent-catalog]]"
  - "[[okta-for-ai-agents]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[identity-credential-coupling]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[shadow-automation]]"
sources:
  - "[[.raw/articles/spiffe-spiffe-concepts-2026-10-06.md]]"
  - "[[.raw/articles/spiffe-spire-concepts-2026-10-06.md]]"
  - "[[.raw/articles/spiffe-spire-adopters-2026-10-06.md]]"
  - "[[.raw/articles/cncf-spiffe-spire-graduation-2026-10-06.md]]"
  - "[[.raw/articles/google-cloud-iam-agent-identity-overview-2026-10-06.md]]"
  - "[[.raw/articles/microsoft-entra-workload-identity-federation-spiffe-spire-2026-10-06.md]]"
  - "[[.raw/articles/microsoft-entra-agent-id-configure-third-party-agents-2026-10-06.md]]"
  - "[[.raw/articles/anthropic-claude-api-wif-spiffe-2026-10-06.md]]"
  - "[[.raw/articles/a2a-protocol-specification-v1-0-0-2026-10-06.md]]"
  - "[[.raw/articles/ietf-draft-ietf-wimse-aims-00-2026-10-06.md]]"
  - "[[.raw/articles/ietf-draft-klrc-aiagent-auth-status-2026-10-06.md]]"
  - "[[.raw/articles/ietf-wimse-wg-documents-2026-10-06.md]]"
  - "[[.raw/articles/nist-nccoe-software-ai-agent-identity-authorization-concept-paper-2026-10-06.md]]"
  - "[[.raw/papers/securing-the-autonomous-future.md]]"
  - "https://github.com/spiffe/spire/releases"
  - "https://www.hashicorp.com/en/blog/vault-enterprise-1-21-spiffe-auth-fips-140-3-level-1-compliance-granular-secret-recovery"
verified: 2026-10-06
verified_against:
  - ".raw/articles/a2a-protocol-specification-v1-0-0-2026-10-06.md"
  - ".raw/articles/anthropic-claude-api-wif-spiffe-2026-10-06.md"
  - ".raw/articles/cncf-spiffe-spire-graduation-2026-10-06.md"
  - ".raw/articles/google-cloud-iam-agent-identity-overview-2026-10-06.md"
  - ".raw/articles/ietf-draft-ietf-wimse-aims-00-2026-10-06.md"
  - ".raw/articles/ietf-draft-klrc-aiagent-auth-status-2026-10-06.md"
  - ".raw/articles/ietf-wimse-wg-documents-2026-10-06.md"
  - ".raw/articles/microsoft-entra-agent-id-configure-third-party-agents-2026-10-06.md"
  - ".raw/articles/microsoft-entra-workload-identity-federation-spiffe-spire-2026-10-06.md"
  - ".raw/articles/nist-nccoe-software-ai-agent-identity-authorization-concept-paper-2026-10-06.md"
  - ".raw/articles/spiffe-spiffe-concepts-2026-10-06.md"
  - ".raw/articles/spiffe-spire-adopters-2026-10-06.md"
  - ".raw/articles/spiffe-spire-concepts-2026-10-06.md"
  - ".raw/papers/securing-the-autonomous-future.md"
verified_findings: 0
verified_note: "3 low fixed (Google service scope, Entra RS256 wording now cited to the considerations page, WIMSE practices draft state); A2A spec read at §4.5 and §7, AIMS at header and §6, NCCoE at §2; Vault 1.21, SPIRE releases and Gemini Enterprise release notes checked live"
---

# SPIFFE / SPIRE

SPIFFE (Secure Production Identity Framework for Everyone) is an open specification for naming software workloads and proving those names with short-lived cryptographic documents. A **SPIFFE ID** is a URI of the form `spiffe://<trust domain>/<workload identifier>`. A **SPIFFE Verifiable Identity Document (SVID)** carries one SPIFFE ID as an X.509 certificate (X.509-SVID) or as a JSON Web Token (JWT-SVID), and the **Workload API** delivers SVIDs and trust bundles to a running workload, so the workload stores no long-lived secret.[^spiffe-concepts] SPIRE (the SPIFFE Runtime Environment) is the project's production-ready implementation of the SPIFFE APIs.[^spire-concepts] The Cloud Native Computing Foundation announced the graduation of both projects on 2022-09-20.[^cncf] For AI agents, an SVID answers which workload is calling; [[agent-identity-architecture|AI Agent Identity Architecture]] places it beneath the authorization and task-binding layers that decide what the agent may do.

## Identity model

A **trust domain** is the trust root of one SPIFFE deployment. Every workload in it receives identity documents that verify against the domain's root keys, and the SPIFFE documentation advises separate trust domains for separate physical locations and for environments with different security practices.[^spiffe-concepts] A **trust bundle** holds the root certificates a workload uses to verify another workload's SVID.

The two SVID formats differ in replay exposure. The SPIFFE documentation states that "it is advised to use X.509-SVIDs whenever possible", because a JWT-SVID intercepted in transit can be replayed, and it reserves the JWT format for paths such as an L7 proxy or load balancer between the workloads.[^spiffe-concepts] Microsoft Entra Workload ID and the Claude API both federate with JWT-SVIDs, so the format the documentation ranks second is the one used on cross-boundary paths.

## Attestation and registration entries

SPIRE decides which workload receives which SPIFFE ID through attestation against operator-written records. The SPIRE server holds the trust domain's signing keys and "stores registration entries (which specify the selectors that determine the conditions under which a particular SPIFFE ID should be issued)".[^spire-concepts] A SPIRE agent on each node first proves the node's identity to the server through node attestation, for example with a cloud instance identity document. When a workload calls the Workload API, the agent's workload attestors read the calling process's properties from the operating system or orchestrator, and the agent "determines the workload's identity by comparing the discovered workload selectors to registration entries".[^spire-concepts]

Two consequences follow for agent governance. A registration entry is an inventory record, because a workload that matches no entry receives no SVID; [[shadow-agent-admission-control|Shadow Agent Admission Control]] builds on this property to contain [[shadow-automation|Shadow Automation]]. Attestation binds the credential to the running workload rather than to a copied secret, which is the evidence that D2-IDENTITY-ATTEST in [[agentic-ai-security-cmm-d2-identity|CMM D2]] asks for at L5.

## Issuers and relying parties

SPIRE remains under active release; v1.15.3 was published on 2026-08-21.[^spire-rel] Other products issue SPIFFE identities or accept them from SPIRE:

- **Google Agent Identity** assigns each agent deployed on a supported Google Cloud service, among them Agent Runtime and Cloud Run, "a unique SPIFFE identity and an X.509 certificate", valid for 24 hours, in a trust domain under `system.id.goog`, and uses mutual TLS to Google APIs by default.[^g-aid] The IAM overview states no launch stage. Gemini Enterprise displays an agent's SPIFFE ID on its details page, generally available since 2026-06-25, and shows the Agent Registry resource ID when no SPIFFE ID is published.[^ge-rn]
- **HashiCorp Vault Enterprise 1.21** authenticates AI agents and other non-human workloads with SPIFFE and issues X.509-SVIDs to workloads Vault has authenticated.[^vault]
- **Microsoft Entra Workload ID** exchanges a SPIRE JWT-SVID for an Entra access token. Microsoft supports only RS256-signed external tokens for this exchange, and SPIRE signs JWT-SVIDs with ES256 by default, so the SPIRE certificate authority must be reconfigured to issue RS256 tokens.[^entra] Entra Agent ID's own guidance for third-party agents names GCP Workload Identity and AWS STS as federation sources and does not name SPIFFE.[^entra-3p]
- **The Claude API** accepts SPIFFE workloads through workload identity federation, and "WIF accepts JWT-SVIDs only, not X.509-SVIDs".[^claude]

The SPIRE repository's adopters file lists two groups. End users with notable contributions include Bloomberg, ByteDance, GitHub, Netflix and Uber. A second group, companies that use SPIFFE and SPIRE "to build higher layer products and services", includes Amazon, Anthropic and Google; Anthropic's linked entry documents JWT-SVID federation to the Claude API, not internal SPIRE use.[^adopters]

## Use for AI agents

An SVID gives an agent's workload an identity without a pre-stored secret. That property solves the bootstrapping step that [[agent-identity-architecture|AI Agent Identity Architecture]] calls Credential Zero: the agent's first authentication, to a vault or identity provider, uses an attested SVID, and every later credential is fetched with it.

The SVID names a workload, and three limits follow for agents:

- **Granularity.** Agents that run in one workload share its SPIFFE ID; [[non-human-identity|Non-Human Identity (NHI)]] sets out how an assessor checks a shared identity against the agent inventory.
- **Authorization.** The SVID establishes who calls. The receiving service, a policy decision point or a task-scoped grant decides what the caller may do.
- **Delegation.** The SVID carries no record of the human the agent acts for; a delegated token or a warrant carries that chain.

The text of version 1.0.0 of the [[a2a-protocol|A2A Protocol (Agent-to-Agent)]] specification does not name SPIFFE. It handles identity "at the protocol layer, not within A2A semantics" and offers mutual TLS as one of five security schemes an Agent Card can declare.[^a2a] An A2A receiver that declares mutual TLS and trusts the enterprise's trust bundle accepts X.509-SVIDs from registered agents and refuses callers without one.

## Standards status

Agent identity standards that build on SPIFFE are drafts. In the IETF WIMSE working group, draft-ietf-wimse-aims-00 (2026-09-15), which replaced draft-klrc-aiagent-auth, describes the SPIFFE identifier as "a widely deployed and operationally mature instance of the WIMSE identifier model" and requires that an agent "MUST be assigned exactly one WIMSE identifier, which MAY be a SPIFFE ID".[^aims] The working group's architecture, identifier, credential and token drafts were all at "I-D Exists" on 2026-10-06, and only its workload identity practices draft had been submitted to the IESG, where it was in Area Director evaluation.[^wimse] The NIST NCCoE concept paper on software and AI agent identity, a draft for comment from February to April 2026, lists SPIFFE and SPIRE among the standards its project is considering and describes them as "one way in which agent workloads could be identified and authenticated".[^nccoe]

## Notes

[^spiffe-concepts]: SPIFFE project, [*SPIFFE Concepts*](https://spiffe.io/docs/latest/spiffe-about/spiffe-concepts/), retrieved 2026-10-06. SPIFFE ID, trust domain, SVID formats, Workload API and trust bundle.
[^cncf]: Cloud Native Computing Foundation, [*SPIFFE and SPIRE Projects Graduate from Cloud Native Computing Foundation Incubator*](https://www.cncf.io/announcements/2022/09/20/spiffe-and-spire-projects-graduate-from-cloud-native-computing-foundation-incubator/), 2022-09-20.
[^spire-concepts]: SPIFFE project, [*SPIRE Concepts*](https://spiffe.io/docs/latest/spire-about/spire-concepts/), retrieved 2026-10-06. Server, agent, node and workload attestation, registration entries.
[^spire-rel]: SPIFFE project, [*SPIRE releases*](https://github.com/spiffe/spire/releases), read 2026-10-06; v1.15.3 is the newest tag listed, dated 2026-08-21.
[^g-aid]: Google Cloud, [*Agent Identity overview*](https://docs.cloud.google.com/iam/docs/agent-identity-overview), retrieved 2026-10-06.
[^ge-rn]: Google Cloud, [*Gemini Enterprise release notes*](https://docs.cloud.google.com/gemini/enterprise/docs/release-notes), entry of 2026-06-25, "Gemini Enterprise: View agent identity".
[^vault]: HashiCorp, [*Vault Enterprise 1.21: SPIFFE auth, FIPS 140-3 level 1 compliance, granular secret recovery*](https://www.hashicorp.com/en/blog/vault-enterprise-1-21-spiffe-auth-fips-140-3-level-1-compliance-granular-secret-recovery), 2025-10-23.
[^entra]: Microsoft, [*Federate SPIFFE/SPIRE workloads with Microsoft Entra Workload ID*](https://learn.microsoft.com/en-us/entra/workload-id/workload-identity-federation-spiffe-spire), ms.date 2026-08-12. The RS256 rule is stated in [*Workload identity federation for app considerations*, Supported signing algorithms and issuers](https://learn.microsoft.com/en-us/entra/workload-id/workload-identity-federation-considerations#supported-signing-algorithms-and-issuers), which adds that other algorithms "may work, but haven't been tested".
[^entra-3p]: Microsoft, [*Configure third-party agents with Microsoft Entra Agent ID*](https://learn.microsoft.com/en-us/entra/agent-id/configure-third-party-agents), ms.date 2026-04-29.
[^claude]: Anthropic, [*SPIFFE* workload identity federation provider](https://platform.claude.com/docs/en/manage-claude/wif-providers/spiffe), retrieved 2026-10-06.
[^adopters]: SPIFFE project, [*SPIRE ADOPTERS.md*](https://github.com/spiffe/spire/blob/main/ADOPTERS.md), retrieved 2026-10-06.
[^a2a]: A2A Project, [*A2A Protocol Specification*, §7 Authentication and Authorization](https://a2a-protocol.org/latest/specification/#7-authentication-and-authorization), latest released version 1.0.0, retrieved 2026-10-06; the scheme list is in §4.5.
[^aims]: IETF WIMSE WG, [*draft-ietf-wimse-aims-00: AI Identity Management System*](https://datatracker.ietf.org/doc/draft-ietf-wimse-aims/), 2026-09-15, §6. The replaced individual draft is at [draft-klrc-aiagent-auth](https://datatracker.ietf.org/doc/draft-klrc-aiagent-auth/).
[^wimse]: IETF Datatracker, [*WIMSE working group documents*](https://datatracker.ietf.org/wg/wimse/documents/), read 2026-10-06.
[^nccoe]: NIST NCCoE, [*Accelerating the Adoption of Software and AI Agent Identity and Authorization*, concept paper, draft](https://www.nccoe.nist.gov/sites/default/files/2026-02/accelerating-the-adoption-of-software-and-ai-agent-identity-and-authorization-concept-paper.pdf), February 2026, §2.

<!-- sources:auto -->
## Sources

- [SPIFFE / SPIRE](https://spiffe.io/docs/latest/spiffe-about/spiffe-concepts/)
- [SPIRE Concepts | SPIFFE](https://spiffe.io/docs/latest/spire-about/spire-concepts/)
- [SPIRE ADOPTERS.md](https://github.com/spiffe/spire/blob/main/ADOPTERS.md)
- [SPIFFE and SPIRE Projects Graduate from Cloud Native Computing Foundation Incubator](https://www.cncf.io/announcements/2022/09/20/spiffe-and-spire-projects-graduate-from-cloud-native-computing-foundation-incubator/)
- [Agent Identity overview | IAM | Google Cloud Documentation](https://docs.cloud.google.com/iam/docs/agent-identity-overview)
- [Federate SPIFFE/SPIRE workloads with Microsoft Entra Workload ID | Microsoft Learn](https://learn.microsoft.com/en-us/entra/workload-id/workload-identity-federation-spiffe-spire)
- [Configure third-party agents with Microsoft Entra Agent ID | Microsoft Learn](https://learn.microsoft.com/en-us/entra/agent-id/configure-third-party-agents)
- [SPIFFE | Workload identity federation | Claude Platform Docs](https://platform.claude.com/docs/en/manage-claude/wif-providers/spiffe)
- [Agent2Agent (A2A) Protocol Specification, Latest Released Version 1.0.0](https://a2a-protocol.org/latest/specification/)
- [draft-ietf-wimse-aims-00 - AI Identity Management System](https://datatracker.ietf.org/doc/draft-ietf-wimse-aims/)
- [draft-klrc-aiagent-auth-03 - AI Agent Authentication and Authorization (Datatracker status)](https://datatracker.ietf.org/doc/draft-klrc-aiagent-auth/)
- [WIMSE Working Group Documents (IETF Datatracker)](https://datatracker.ietf.org/wg/wimse/documents/)
- [Accelerating the Adoption of Software and AI Agent Identity and Authorization (NCCoE Concept Paper, Draft)](https://www.nccoe.nist.gov/sites/default/files/2026-02/accelerating-the-adoption-of-software-and-ai-agent-identity-and-authorization-concept-paper.pdf)
- [github.com](https://github.com/spiffe/spire/releases)
- [hashicorp.com](https://www.hashicorp.com/en/blog/vault-enterprise-1-21-spiffe-auth-fips-140-3-level-1-compliance-granular-secret-recovery)
<!-- /sources -->
