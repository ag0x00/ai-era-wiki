---
type: entity
entity_type: product
title: "Onyx Platform (Onyx AI Control Plane)"
homepage: "https://onyx.security/platform"
created: 2026-05-03
updated: 2026-09-30
tags:
  - products
  - ai-control-plane
  - guardian-agent
  - ai-spm
  - ai-gateway
  - cots
status: developing
scope_axis:
  - sec-of-ai
vendor: "Onyx Security"
related:
  - "[[onyx-security]]"
  - "[[guardian-agent]]"
  - "[[ai-spm]]"
  - "[[onyx-platform-open-questions]]"
  - "[[wiz]]"
  - "[[wiz-ai-spm]]"
  - "[[palo-alto-prisma-airs]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
sources:
  - ".raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md"
  - "https://onyx.security/platform"
  - "https://www.onyx.security/platform/ai-security"
  - "https://www.onyx.security/platform/ai-governance"
  - "https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents"
  - "https://www.paloaltonetworks.com/blog/2026/07/announcing-general-availability-of-prisma-airs-ai-gateway/"
verified: 2026-09-30
verified_against:
  - ".raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md"
verified_findings: 0
verified_note: "Rechecked Onyx archive and live security, governance, platform, Wiz and Palo Alto pages; product scores and unsupported peer rankings removed."
---

# Onyx Platform (Onyx AI Control Plane)

[[onyx-security|Onyx Security]] markets the Onyx Platform as a control plane for enterprise AI agents and applications. Its AI Security surface combines [[ai-spm|AI Security Posture Management (AI-SPM)]], runtime inspection, and supply-chain risk identification. Onyx calls its supervisory component a [[guardian-agent|Guardian Agent]]. The [product pages](https://onyx.security/platform) describe vendor capabilities; the deployment evidence below determines how a customer installation can use them.

**Sources:** [Onyx Platform](https://onyx.security/platform) · [Onyx AI Security](https://www.onyx.security/platform/ai-security) · [Onyx AI Governance](https://www.onyx.security/platform/ai-governance)

## Advertised scope

- **Observability:** Onyx [advertises](https://onyx.security/platform) prompt and response visibility, session replay, shadow-AI discovery, and behavioral baselines.
- **Security:** The [AI Security page](https://www.onyx.security/platform/ai-security) describes checks for agent misconfigurations and excessive permissions, automated red teaming, and inline inspection of prompts, tool calls, and model responses. It names alert, block, mask, steer, and human approval as possible actions.
- **Governance:** The [governance page](https://www.onyx.security/platform/ai-governance) says policies can be written in natural language, applied to agent tools and MCP servers, and linked to an agent identity distinct from its user.
- **Orchestration:** The [platform page](https://onyx.security/platform) describes a managed AI gateway for model routing and an inline MCP gateway that logs requests and applies guardrails.
- **ROI reporting:** The same [platform page](https://onyx.security/platform) advertises adoption, cost, and productivity dashboards.

Onyx advertises cloud, hybrid, and self-hosted deployment options on its [platform page](https://onyx.security/platform). The public pages name protected surfaces but give limited detail on which integration places each action in the execution path. [[onyx-platform-open-questions|Onyx Platform: Open Questions]] tracks availability and integration details that a buyer needs to establish.

## Deployment evidence

The CMM assesses a configured agent deployment, not a product claim. Onyx's pages identify controls worth testing:

| Domain | Evidence to request |
|---|---|
| [[agentic-ai-security-cmm-d3-control-least-agency\|CMM D3: Control and Least-Agency]] | Agent identity and tool policy, followed by a refused call to an unapproved tool or MCP server. |
| [[agentic-ai-security-cmm-d4-runtime-guardrails\|CMM D4: Runtime and Guardrails]] | The configured prompt, response, and action routes. |
| D4 | A blocked or steered test on each claimed inline path. |
| D4 | The observed behavior when inspection fails. |
| [[agentic-ai-security-cmm-d7-observability\|CMM D7: Observability and Detection]] | Searchable or exportable session and tool records, their owner and trace identifiers, and a tested alert. |
| [[agentic-ai-security-cmm-d8-supply-chain\|CMM D8: Engineering and Supply Assurance]] | The component inventory, release-linked assurance records, and findings for the agents, MCP servers, or models in scope. |

These checks test the installed routes and records. The cited marketing pages alone do not assign a CMM level.

## Comparable scope

[[wiz|Wiz]]'s [[wiz-ai-spm|Wiz AI-SPM]] [describes agent inventory, graph-based posture analysis, and runtime monitoring](https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents). [[palo-alto-prisma-airs|Palo Alto Prisma AIRS (AI Runtime Security)]] [describes an inline AI gateway](https://www.paloaltonetworks.com/blog/2026/07/announcing-general-availability-of-prisma-airs-ai-gateway/) as well as runtime and posture functions. Onyx advertises an inline MCP gateway and action protection. The vendor pages establish overlapping functions; a comparison of protected paths and policy outcomes requires deployment tests.
