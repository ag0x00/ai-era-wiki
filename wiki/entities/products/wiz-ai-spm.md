---
type: entity
entity_type: product
title: "Wiz AI-SPM"
homepage: "https://www.wiz.io/solutions/ai-spm"
created: 2026-05-03
updated: 2026-08-21
tags:
  - products
  - ai-spm
  - cnapp
  - data-plane
  - observability-plane
  - cots
status: developing
vendor: "Wiz (Google Cloud Security)"
related:
  - "[[wiz]]"
  - "[[ai-spm]]"
  - "[[ai-bom]]"
  - "[[palo-alto-prisma-airs]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[codemender]]"
  - "[[google-cloud-codemender-preview]]"
sources:
  - "https://www.wiz.io/solutions/ai-spm"
  - "https://www.wiz.io/blog/ai-security-posture-management"
  - "https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents"
  - "https://www.wiz.io/blog/wizdom-product-launches-2025"
---

# Wiz AI-SPM

**Sources:** [Wiz AI-SPM (homepage)](https://www.wiz.io/solutions/ai-spm) · [AI Security Posture Management (Wiz blog)](https://www.wiz.io/blog/ai-security-posture-management) · [Wiz AI-SPM secures AI agents (Wiz blog)](https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents)

Wiz AI-SPM (AI Security Posture Management) is a native module of the [[wiz|Wiz]] CNAPP platform that inventories AI assets (models, services, SDKs, libraries, MCP connections), detects misconfigurations, finds attack paths to AI training data and model endpoints, and monitors agent runtime behavior. Wiz claimed first-CNAPP-to-ship-AI-SPM in 2024 and expanded materially through 2025. Following Google's \$32B Wiz acquisition, AI-SPM is now part of Google Cloud Security.

## Capabilities

| Capability | Description |
|---|---|
| **Model / asset inventory** | Agentless discovery of OpenAI, AWS Bedrock, Vertex AI, SageMaker, plus self-hosted models; AI Bill of Materials (AI BOM) for SDKs and libraries |
| **Configuration risk** | Built-in rules for managed AI services; IaC scanning of pipelines feeding AI workloads |
| **Data exposure (DSPM-for-AI)** | Surfaces sensitive training data; identifies and removes attack paths to it via the Wiz Security Graph |
| **Access risk** | Identity/CIEM correlation to AI endpoints; secrets exposure detection |
| **AI agent + MCP coverage** | Added 2025; discovers and inventories agents and MCP servers running in monitored environments |
| **Runtime Monitoring** | Baseline-drift detection on AI agent behavior; complements posture-only view |
| **Threat Correlation** | Ties live agent behavior to cloud resources via the Wiz Security Graph |
| **AI Security Dashboard** | Dedicated developer-facing dashboard for AI-specific risk |

## Role in the RA

In the [[agentic-ai-security-reference-architecture|Agentic AI Security RA]], Wiz AI-SPM appears in three planes:

| Plane | Capability | Role |
|---|---|---|
| **Data** | Supply-chain scanning, AI-BOM discovery | Primary CNAPP-integrated AI-BOM |
| **Observability** | AI Security Posture Management | Primary enterprise COTS for AI-SPM |
| **Observability** | Agent behavioral monitoring (runtime) | 2025-added capability via baseline-drift |

The enterprise recommended stack uses Wiz AI-SPM as the AI-SPM/posture layer for organizations already using Wiz CNAPP across their cloud environment.

## Comparison with peers

| Product | Primary differentiator |
|---|---|
| **Wiz AI-SPM** | Graph-based attack-path correlation; broad cloud coverage (AWS, Azure, GCP, OCI, Alibaba, vSphere, K8s) |
| **[[palo-alto-prisma-airs\|Palo Alto Prisma AIRS]]** (and Prisma Cloud AI-SPM) | Tighter integration with Palo Alto runtime/network stack; Dig Security-derived AI-SPM |
| **Orca Security AI-SPM** | SideScanning approach; emphasizes 50+ AI model sources and PyTorch/TensorFlow asset inventory |
| **Reco** | SaaS-side AI/shadow-AI discovery; not full CNAPP |

Wiz wins on graph-based attack-path correlation and broader multi-cloud coverage. Prisma wins for Palo Alto-anchored organizations. Orca emphasizes ML framework depth.

## Deployment

SaaS-only multi-tenant; agentless via cloud APIs and snapshot scanning. Connectors:
- Cloud: AWS, Azure, GCP, OCI, Alibaba, vSphere
- Container: Kubernetes, OpenShift
- SaaS: OpenAI Platform connector (added 2024)

Median Wiz contracts run approximately \$149K/year per Vendr data — enterprise-tier pricing.

## Notable integrations and 2025–2026 news

- **OpenAI Platform connector** — agentless inventory of OpenAI account assets
- **NVIDIA Enterprise AI Factory integration** — AI-SPM coverage extends into NVIDIA-managed AI infrastructure
- **MCP coverage expansion** (2025) — agents and MCP servers as inventoried asset classes
- **Wizdom 2025** launch event — introduced runtime AI agent monitoring and the dedicated AI Security Dashboard
- **Google Cloud acquisition** (\$32B, 2025) — Wiz AI-SPM is now part of Google Cloud Security
- **AI Threat Defense** (2026) — the first published Wiz–Google product composition: Wiz orchestrates [[codemender|CodeMender]] scanning and patch generation, and enriches findings in the Wiz Security Graph with deployment context ([[google-cloud-codemender-preview|source summary]]). Announced with CodeMender scanning "coming soon"

## CMM positioning

- **D7 (Observability & Detection) L3**: AI-SPM with attack-path correlation, agent behavioral baselining, and threat correlation
- **D8 (Supply Chain & AI-BOM) L3**: AI-BOM generation + supply-chain scanning across cloud assets

> [!note]
> AI-SPM as a category covers posture, not enforcement. Wiz AI-SPM identifies risks but does not block them at runtime — pair with a runtime control layer ([[llamafirewall|LlamaFirewall]], [[lakera-guard|Lakera Guard]], [[palo-alto-prisma-airs|Prisma AIRS]], or [[agentgateway|AgentGateway]]) for in-line enforcement.
