---
type: paper
title: "Wiz AI-APP Launch"
created: 2026-09-30
updated: 2026-09-30
tags:
  - papers
  - wiz
  - ai-app
  - ai-application-security
status: summarized
origin: aggregated
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
year: 2026
publication_date: 2026-03-23
authors:
  - Snegha Ramnarayanan
  - Aviel Erdis
  - Guy Weiss
  - Dan Segev
publisher: "Wiz"
source_url: "https://www.wiz.io/blog/introducing-wiz-ai-app"
archived_copy: ".raw/articles/introducing-wiz-ai-app-2026-09-30.md"
key_claim: "Wiz presents AI-APP as a graph-powered extension of CNAPP that joins AI inventory, cross-layer risk analysis, and runtime detection."
methodology: "Vendor product announcement built around an illustrative chatbot attack path; no evaluation design is reported."
related:
  - "[[wiz]]"
  - "[[wiz-ai-app]]"
  - "[[wiz-ai-spm]]"
  - "[[ai-spm]]"
  - "[[agent-catalog]]"
  - "[[agent-observability]]"
sources:
  - "https://www.wiz.io/blog/introducing-wiz-ai-app"
  - "[[.raw/articles/introducing-wiz-ai-app-2026-09-30.md]]"
verified: 2026-09-30
verified_against:
  - ".raw/articles/introducing-wiz-ai-app-2026-09-30.md"
verified_findings: 0
verified_note: "Launch claims checked against the complete archived announcement and live original; no open findings."
---

# Wiz AI-APP Launch

**Source:** [Wiz — Introducing Wiz AI Application Protection Platform](https://www.wiz.io/blog/introducing-wiz-ai-app) (2026-03-23). Local copy: `.raw/articles/introducing-wiz-ai-app-2026-09-30.md`.

[[wiz|Wiz]] announced [[wiz-ai-app|Wiz AI-APP]] as a graph-powered platform for AI application discovery, risk analysis, and runtime detection. The post presents this as an extension of cloud-native application protection: the Wiz Security Graph joins application components to infrastructure, identity, and data context. The article describes capabilities and an illustrative attack path, rather than a measured deployment or independent evaluation.

## Inventory and risk model

Wiz says its inventory combines managed-platform integrations, SaaS visibility, and code and workload analysis of custom applications. It names AWS Bedrock, Azure AI, Google Vertex AI, OpenAI, and Microsoft Copilot Studio as covered examples. The AI-based Workload Explainer identifies application components from custom implementations, including agents, models, connected tools, and data flows. The proposed platform therefore extends beyond asset enumeration.

The risk example connects a public chatbot endpoint with an authentication bypass, a model with misconfigured guardrails, sensitive training data, and agent tools able to reach that data. Wiz also says it classifies tools by the effects they permit:

- Reading or exposing data.
- Executing code.
- Calling APIs.
- Modifying infrastructure.

The chatbot is a scenario in the launch article. It does not document an observed compromise or show how the platform tests whether each step is exploitable. For [[ai-spm|AI Security Posture Management (AI-SPM)]], the article illustrates how inventory findings can inform attack-path analysis. It also shows why an [[agent-catalog|AI Agent Catalog]] is more useful for risk analysis when it records each agent's reachable tools and effects.

## Runtime and response

Wiz says runtime detection joins model inputs and outputs, workload execution and tool use, and cloud identity and API activity. In the chatbot scenario, those records would connect a manipulated prompt to container code execution and downstream credential use. This is a proposed correlation path for [[agent-observability|Agent Observability]], not a published detection result.

The post names integrations with Cloudflare, TrojAI, and Pillar Security. Cloudflare contributes endpoint protection status; TrojAI and Pillar contribute AI red-team findings that Wiz says it correlates with cloud exposure. Wiz also names Red Agent for exploitable-risk discovery, Green Agent for fixes and ownership, and Blue Agent for investigation. The post says Security Graph context can enter AI development workflows so agents can identify misconfigurations and propose or perform remediation before commit.

## Evidence boundary

The launch article names the signal classes and intended workflow but gives no telemetry schema, detection test, false-positive data, or customer result. Its runtime section describes detection and response prioritization; it does not state that AI-APP blocks a model response or tool action inline. The post also leaves the commercial availability and the packaging relationship to [[wiz-ai-spm|Wiz AI-SPM]] unspecified. Those are questions for product evaluation, not properties established by this announcement.
