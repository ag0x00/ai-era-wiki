---
type: entity
title: "Keycard"
created: 2026-05-03
updated: 2026-06-21
tags:
  - entities
  - organization
  - vendor
  - identity
  - agentic-ai-security
  - seed-funded
status: seed
entity_type: organization
org_type: vendor
role: "Identity and access platform for AI agents — real-time, contextual guardrails for agent-to-system access"
related:
  - "[[okta-for-ai-agents]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[non-human-identity]]"
  - "[[capability-based-authorization]]"
  - "[[tenuo-warrant]]"
homepage: "https://www.keycard.com"
sources:
  - "https://www.globenewswire.com/news-release/2025/10/21/3170297/0/en/Keycard-Launches-to-Solve-the-AI-Agent-Identity-and-Access-Problem-With-38-Million-in-Funding-From-Andreessen-Horowitz-Boldstart-Ventures-and-Acrew-Capital.html"
  - "https://siliconangle.com/2025/10/21/ai-agent-security-startup-keycard-reels-38m/"
  - "https://boldstart.vc/news/keycard-solving-the-ai-agent-identity-and-access-problem-welcome-to-boldstart/"
  - "https://www.upstartsmedia.com/p/this-startup-raised-38m-to-secure"
---

# Keycard

**Sources:** [Keycard (homepage)](https://www.keycard.com) · [Launch announcement (GlobeNewswire)](https://www.globenewswire.com/news-release/2025/10/21/3170297/0/en/Keycard-Launches-to-Solve-the-AI-Agent-Identity-and-Access-Problem-With-38-Million-in-Funding-From-Andreessen-Horowitz-Boldstart-Ventures-and-Acrew-Capital.html) · [boldstart investment thesis](https://boldstart.vc/news/keycard-solving-the-ai-agent-identity-and-access-problem-welcome-to-boldstart/) · [SiliconANGLE coverage](https://siliconangle.com/2025/10/21/ai-agent-security-startup-keycard-reels-38m/)

> Homepage URL above is unverified — re-validate when Keycard publishes a public site.

## Description

Identity-and-access platform for AI agents. Provides **real-time, contextual guardrails** that let enterprises grant AI agents controlled access to internal systems, with the explicit pitch of replacing static, human-driven workflows with **machine-driven, autonomous, agentic applications** built on top.

Founding team has unusually deep identity-platform pedigree:
- **Ian Livingstone** and **Matt Creager**: previously at **Manifold** (acquired by Snyk); helped scale Snyk
- **Jared Hanson**: creator of **Passport.js**, former Chief Architect at **Auth0**

(Source: [boldstart](https://boldstart.vc/news/keycard-solving-the-ai-agent-identity-and-access-problem-welcome-to-boldstart/), [SiliconANGLE](https://siliconangle.com/2025/10/21/ai-agent-security-startup-keycard-reels-38m/))

## Funding

**\$8M seed round** led by **a16z** and **boldstart ventures** (round closed prior to public launch, exact date not disclosed; predates the \$30M Series A).

**\$30M Series A, October 21, 2025**, led by **Acrew Capital**.

**\$38M total**, launch-day announcement.

The seed itself is sixth-largest agentic-AI-security seed in the 12-month window; total funding exceeds every other seed-stage agent-security startup at launch.

## Relevance

Maps cleanly to the [[agentic-ai-security-reference-architecture|RA]] **Identity** plane (primary) with strong overlap into **Control** (least-agency policy enforcement). In the [[agentic-ai-security-cmm-2026|CMM]], evidence supports **D2 L3-L4** (identity & authorization) and **D3 L3-L4** (control & least-agency).

Direct competitor positioning to [[okta-for-ai-agents|Okta for AI Agents]] and [[microsoft-entra-agent-id|Microsoft Entra Agent ID]], but earlier-stage and **developer-platform-shaped**, not enterprise-IDP-shaped. For organizations that don't already run on Okta or Microsoft, Keycard becomes a credible third commercial option for the agent-IAM slot.

Adjacent to [[capability-based-authorization|capability-based authorization]] research ([[tenuo-warrant|Tenuo Warrant]]) but in a different lane: Keycard is **identity + access-policy** (PDP/PEP-shaped), not artifact-carrying-policy capability tokens.

## Product

Per launch coverage:

- **Identity for agents**: agent-as-principal model, distinct from human identities
- **Real-time, contextual guardrails**: policy decisions made at access time, not statically bound at deploy
- **Foundations for trusted agentic applications at scale**: developer-platform pitch
- Integrates with enterprise systems as the access broker

> [!gap]
> No public technical documentation as of May 2026 on the access-control model (RBAC vs ABAC vs capability-based vs hybrid), the policy language used, or whether Keycard implements the [[oversight-layer|PDP / PEP / PIP / PAP]] split explicitly.

## Notable Statements

- The Hanson + Manifold + Snyk lineage signals Keycard is built by the **identity platform-builder community**, not the AI-research community. It inherits the Auth0 / IdP playbook applied to NHIs and agents. This contrasts with the [[capability-based-authorization-talk|Niyikiza / Tenuo]] research-grade capability-token approach.

## See Also

- [[comprehensive-agentic-ai-security-landscape-2026|Comprehensive Agentic AI Security Landscape]] — the funding-landscape pass covering this cohort
- [[okta-for-ai-agents|Okta for AI Agents]], enterprise primary
- [[microsoft-entra-agent-id|Microsoft Entra Agent ID]], M365/Azure primary
- [[non-human-identity|Non-Human Identity]]
