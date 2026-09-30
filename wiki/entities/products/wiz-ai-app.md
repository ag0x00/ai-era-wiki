---
type: entity
entity_type: product
title: "Wiz AI-APP"
created: 2026-09-30
updated: 2026-09-30
tags:
  - products
  - wiz
  - ai-app
  - cnapp
status: developing
origin: aggregated
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
vendor: "Wiz"
role: "Graph-based AI application inventory, cross-layer risk analysis, and runtime threat detection"
homepage: "https://www.wiz.io/blog/introducing-wiz-ai-app"
first_mentioned: "[[wiz-ai-app-launch]]"
related:
  - "[[wiz]]"
  - "[[wiz-ai-app-launch]]"
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
verified_note: "Product claims and evidence limits checked against the complete launch article; no open findings."
---

# Wiz AI-APP

**Sources:** [Wiz AI-APP announcement](https://www.wiz.io/blog/introducing-wiz-ai-app) · [[wiz-ai-app-launch|Wiz AI-APP Launch]].

[[wiz|Wiz]] presents AI-APP as a graph-powered platform that connects AI application discovery, cross-layer risk analysis, and runtime detection. Its launch article describes AI-APP as an extension of Wiz's cloud-native application protection platform (CNAPP). The article establishes Wiz's product claims; it does not report deployment outcomes.

## Discovery and graph context

Wiz says AI-APP discovers managed AI services through cloud integrations, SaaS applications through platform visibility, and custom applications through code and workload analysis. The AI-based Workload Explainer interprets custom workloads to identify models, agents, tools, and data flows. An assessor can use the resulting graph to ask which agent is exposed, what it can call, and what data it can reach. This overlaps with the discovery function of an [[agent-catalog|AI Agent Catalog]].

Wiz says the graph joins an exposed endpoint, authentication weakness, model guardrail configuration, sensitive data, and tool permissions into an attack path. The launch article illustrates that join with a hypothetical chatbot. It classifies agent tools by data access, code execution, API use, and infrastructure modification; it does not publish an exploitability test for that example.

## Runtime signals and action

AI-APP's announced detection path correlates model input and output, workload and tool execution, and cloud identity and API activity. These are signal classes used in [[agent-observability|Agent Observability]]. Wiz says the application context helps teams assess exploitation and prioritize response, but the announcement gives no collection mechanism or detection measurement.

Wiz says Cloudflare contributes AI endpoint protection status and TrojAI and Pillar Security contribute red-team findings. The same launch article assigns exploitable-risk identification to Red Agent, fix and owner determination to Green Agent, and threat investigation to Blue Agent. It also describes Security Graph context supplied to AI development workflows for pre-commit remediation. The article gives no permission model for those agent actions.

## Relationship to posture management

[[wiz-ai-spm|Wiz AI-SPM]] is Wiz's existing AI inventory and posture module. The AI-APP announcement describes a graph-powered platform spanning inventory, attack-path analysis, and runtime detection. It does not define whether AI-SPM is a separate SKU, an AI-APP component, or a renamed capability. [[ai-spm|AI Security Posture Management (AI-SPM)]] remains the practice coordinate for the posture functions.

## Evaluation boundary

The announcement describes runtime detection and investigation, but does not claim inline blocking of model output or tool actions. It gives no availability state, detection metrics, or independently observed attack-path result. A buyer would need those details before treating the stated coverage as an enforced control.
