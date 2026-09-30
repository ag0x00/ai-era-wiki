---
type: entity
entity_type: product
title: "Wiz AI-SPM"
homepage: "https://www.wiz.io/solutions/ai-spm"
created: 2026-05-03
updated: 2026-09-30
tags:
  - products
  - ai-spm
  - cnapp
  - data-plane
  - observability-plane
  - cots
status: developing
scope_axis:
  - sec-of-ai
vendor: "Wiz (Google Cloud)"
related:
  - "[[wiz]]"
  - "[[ai-spm]]"
  - "[[ai-bom]]"
  - "[[wiz-ai-app]]"
  - "[[wiz-ai-app-launch]]"
  - "[[onyx-platform]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
sources:
  - "https://www.wiz.io/solutions/ai-spm"
  - "https://www.wiz.io/blog/ai-security-posture-management"
  - "https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents"
  - "https://www.wiz.io/blog/wizdom-product-launches-2025"
  - "https://www.wiz.io/blog/introducing-wiz-ai-app"
  - "https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/wiz-acquisition/"
  - "https://onyx.security/platform"
  - ".raw/articles/introducing-wiz-ai-app-2026-09-30.md"
  - ".raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md"
verified: 2026-09-30
verified_against:
  - ".raw/articles/introducing-wiz-ai-app-2026-09-30.md"
  - ".raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md"
verified_findings: 0
verified_note: "AI-SPM, AI-APP, Onyx comparison and CMM evidence requests checked against opened sources and official product pages; no product level assigned."
---

# Wiz AI-SPM

**Sources:** [Wiz AI-SPM](https://www.wiz.io/solutions/ai-spm) · [2023 AI-SPM launch](https://www.wiz.io/blog/ai-security-posture-management) · [2025 agent coverage](https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents) · [Wiz AI-APP announcement](https://www.wiz.io/blog/introducing-wiz-ai-app)

[[wiz|Wiz]] AI-SPM is an [[ai-spm|AI Security Posture Management (AI-SPM)]] offering within the Wiz cloud security platform. It combines AI asset discovery, configuration checks, sensitive-data findings, and attack-path analysis in the Wiz Security Graph. Wiz announced the AI-SPM capabilities on [November 16, 2023](https://www.wiz.io/blog/ai-security-posture-management). [Google completed its acquisition of Wiz on March 11, 2026](https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/wiz-acquisition/); Wiz joined Google Cloud.

## Documented capabilities

The [2023 launch](https://www.wiz.io/blog/ai-security-posture-management) described four posture functions and an AI Security Dashboard:

- **Inventory:** Agentless discovery of AI services, libraries, and SDKs, presented as an [[ai-bom|AI-BOM: AI Bill of Materials]] in Wiz Inventory and the Security Graph.
- **Configuration checks:** Built-in rules for AI services, including encryption and network exposure checks.
- **Data posture:** Discovery of sensitive training data and related exposure findings.
- **Attack paths:** Graph correlation across identity, workload, vulnerability, secret, data, and Internet exposure findings.
- **Dashboard:** A view of AI inventory, prioritized issues, and findings for AI developers and data scientists.

Wiz [expanded AI-SPM coverage in 2025](https://www.wiz.io/blog/wizdom-product-launches-2025) to agents and Model Context Protocol (MCP) connections. Wiz stated that agent posture management and MCP discovery were generally available at the November 2025 announcement. Its [agent coverage description](https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents) adds an Agent Inventory View, exposed-endpoint scanning, guardrail configuration checks, runtime monitoring for activity and baseline drift, and response through tickets or remediation workflows. These are vendor-described functions; their coverage in a particular deployment depends on connected environments and available telemetry.

## Relationship to Wiz AI-APP

The [[wiz-ai-app|Wiz AI-APP]] announcement describes a wider graph-based view of AI applications across managed services, SaaS, custom workloads, and runtime signals. It joins model activity, workload execution, and cloud events to support threat detection and response. [[wiz-ai-app-launch|Wiz AI-APP Launch]] records the announcement in detail. [The announcement](https://www.wiz.io/blog/introducing-wiz-ai-app) does not specify the product or entitlement boundary between AI-APP and AI-SPM; an assessment should identify the capabilities enabled in the deployed tenant.

The current [AI-SPM solution page](https://www.wiz.io/solutions/ai-spm) lists “AI Runtime Protection” and describes detection of prompt injection, rogue agents, and malicious behavior. The [agent coverage description](https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents) describes monitoring and response workflows. These pages establish the advertised detection scope but do not by themselves establish which events an inline control can block in a given deployment.

## Deployment evidence for the CMM

The [[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]] and [[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Engineering and Supply Assurance]] domains grade a deployment and its evidence, not a supplier's product description. Wiz's inventory, graph, and monitoring claims identify evidence to request; they do not assign a CMM level to Wiz AI-SPM.

| Criterion | Evidence to inspect in the deployment |
|---|---|
| D7-LOG | Searchable or exportable records for sampled agent tool calls and answers, with event time and accountable owner. |
| D7-SPANS | Traces that correlate sampled inference, tool execution, retrieval, and agent invocation with the agent and session. |
| D7-BASELINE | Each tool-using agent's tool-call baseline, its running detection rule, and an alert or test that shows a departure. |
| D8-AIBOM | A pipeline-generated, machine-readable release AI-BOM with the deployment's component versions and digests. |
| D8-AIBOM-RUNTIME | Recorded comparisons between observed runtime components and the approved release AI-BOM. |

Wiz's advertised AI-BOM is a discovered asset inventory. For D8-AIBOM, the assessor needs the release artifact and the pipeline record that generated it; for D8-AIBOM-RUNTIME, the assessor needs the recorded reconciliation. Similarly, a runtime monitoring claim requires sampled D7 logs, traces, rules, and alerts before it supports a criterion determination.

## Comparable product evidence

[[onyx-platform|Onyx Platform (Onyx AI Control Plane)]] is another vendor offering that [advertises AI-SPM configuration hardening](https://onyx.security/platform). Onyx also advertises prompt, response, and action protection and an inline MCP gateway. Wiz's [AI-SPM documentation](https://www.wiz.io/solutions/ai-spm) emphasizes agentless discovery, cloud configuration findings, and graph-based attack paths, while its [agent coverage description](https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents) includes runtime monitoring. These vendor pages describe different product scopes; a deployment test is needed to compare detection coverage or enforcement behavior.
