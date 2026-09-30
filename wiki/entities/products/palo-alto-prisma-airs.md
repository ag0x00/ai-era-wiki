---
type: entity
entity_type: product
title: "Palo Alto Prisma AIRS (AI Runtime Security)"
homepage: "https://www.paloaltonetworks.com/ai-security/prisma-airs"
created: 2026-05-03
updated: 2026-09-30
tags:
  - products
  - ai-runtime-security
  - prompt-injection
  - ai-spm
  - red-teaming
  - cots
status: developing
scope_axis:
  - sec-of-ai
vendor: "Palo Alto Networks"
related:
  - "[[palo-alto-networks]]"
  - "[[ai-spm]]"
  - "[[onyx-platform]]"
  - "[[wiz-ai-spm]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
sources:
  - "https://www.paloaltonetworks.com/ai-security/prisma-airs"
  - "https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-introduces-prisma-airs--the-foundation-on-which-ai-security-thrives"
  - "https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-secures-the-ai-agent-revolution-with-the-launch-of-prisma-airs-2-0"
  - "https://www.paloaltonetworks.com/company/press/2026/palo-alto-networks-secures-agentic-ai-with-prisma-airs-3-0"
  - "https://www.paloaltonetworks.com/blog/2026/07/announcing-general-availability-of-prisma-airs-ai-gateway/"
  - "https://docs.paloaltonetworks.com/prisma-airs/ai-runtime-security/airs-api"
  - "https://docs.paloaltonetworks.com/prisma-airs/ai-inventory/agent-discovery/ai-agent-discovery-with-cortex-cloud"
  - "https://docs.paloaltonetworks.com/ai-runtime-security/new-features/by-date/prisma-airs/august-2026"
  - "https://onyx.security/platform"
  - "https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents"
verified: 2026-09-30
verified_against:
  - ".raw/articles/agentic-ai-threats-unit42-2025-05-01.md"
verified_findings: 0
verified_note: "Rechecked live Palo Alto API, gateway, release and Cortex AI-SPM docs and inherited Unit 42 archive; regional availability limit added and product scores removed."
---

# Palo Alto Prisma AIRS (AI Runtime Security)

Prisma AIRS is [[palo-alto-networks|Palo Alto Networks]]' platform for AI runtime security, model scanning, red teaming, agent protection, and posture views. Palo Alto [announced the platform on April 28, 2025](https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-introduces-prisma-airs--the-foundation-on-which-ai-security-thrives). The announcement described the intended product scope; later release documents establish the availability of specific modules.

**Sources:** [Prisma AIRS](https://www.paloaltonetworks.com/ai-security/prisma-airs) · [2025 announcement](https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-introduces-prisma-airs--the-foundation-on-which-ai-security-thrives) · [AIRS 2.0 release](https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-secures-the-ai-agent-revolution-with-the-launch-of-prisma-airs-2-0) · [Cortex AI-SPM integration](https://docs.paloaltonetworks.com/prisma-airs/ai-inventory/agent-discovery/ai-agent-discovery-with-cortex-cloud)

## Documented functions

- **Runtime scanning:** The [AIRS API documentation](https://docs.paloaltonetworks.com/prisma-airs/ai-runtime-security/airs-api) describes API Intercept as a scan of prompts and model responses that returns a threat verdict and recommended action to the application. Palo Alto also lists Network Intercept as a deployment mode on the [product page](https://www.paloaltonetworks.com/ai-security/prisma-airs).
- **Agent protection:** The [AIRS 2.0 release](https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-secures-the-ai-agent-revolution-with-the-launch-of-prisma-airs-2-0) describes inline defense against prompt injection, tool misuse, and malicious agent behavior.
- **Model security and red teaming:** The same [release](https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-secures-the-ai-agent-revolution-with-the-launch-of-prisma-airs-2-0) says AIRS integrated Protect AI model inspection and offers automated red teaming of agents and applications.
- **AI Gateway:** Palo Alto [announced general availability on July 16, 2026](https://www.paloaltonetworks.com/blog/2026/07/announcing-general-availability-of-prisma-airs-ai-gateway/) for an inline LLM, MCP, and agent-to-agent gateway with stated model, tool, identity, and runtime controls.

API Intercept is an application integration: the application must act on the returned verdict before the protected operation proceeds. The [API documentation](https://docs.paloaltonetworks.com/prisma-airs/ai-runtime-security/airs-api) establishes content scanning and verdict delivery. A per-tool authorization decision requires evidence from the configured gateway or another enforcement point.

## AI-SPM and release sequence

Palo Alto's [2025 announcement](https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-introduces-prisma-airs--the-foundation-on-which-ai-security-thrives) listed posture management among AIRS capabilities. Current [AIRS documentation](https://docs.paloaltonetworks.com/prisma-airs/ai-inventory/agent-discovery/ai-agent-discovery-with-cortex-cloud) says its AI Inventory displays posture findings supplied by Cortex Cloud [[ai-spm|AI Security Posture Management (AI-SPM)]]. That view requires a connected Cortex Cloud tenant and an active AI-SPM license. The [August 2026 release note](https://docs.paloaltonetworks.com/ai-runtime-security/new-features/by-date/prisma-airs/august-2026) limits the feature to Americas-region Strata Cloud Manager tenants. These dependencies determine which findings a specific AIRS tenant can show.

- [April 28, 2025](https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-introduces-prisma-airs--the-foundation-on-which-ai-security-thrives): Palo Alto announced Prisma AIRS.
- [October 28, 2025](https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-secures-the-ai-agent-revolution-with-the-launch-of-prisma-airs-2-0): AIRS 2.0 was available, with native Protect AI integration.
- [March 23, 2026](https://www.paloaltonetworks.com/company/press/2026/palo-alto-networks-secures-agentic-ai-with-prisma-airs-3-0): AIRS 3.0 was announced with wider agent discovery and artifact security; its AI Agent Gateway was then in limited preview.
- [July 16, 2026](https://www.paloaltonetworks.com/blog/2026/07/announcing-general-availability-of-prisma-airs-ai-gateway/): the AI Gateway became generally available.
- [August 2026](https://docs.paloaltonetworks.com/ai-runtime-security/new-features/by-date/prisma-airs/august-2026): AIRS added AI discovery and posture views sourced from Cortex Cloud AI-SPM.

## Deployment evidence for the CMM

The CMM grades a configured deployment rather than a product name. For [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]], test a blocked prompt or unsafe agent action on each installed intercept or gateway route, including its failure behavior. For [[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]], inspect searchable agent and tool records, correlated traces, and a running alert. A red-team report is test evidence; it does not by itself establish production detection. For [[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Engineering and Supply Assurance]], tie model-scan results to the exact model or release artifact and inspect the release decision. These requirements cannot be scored from product announcements.

## Comparable scope

[[onyx-platform|Onyx Platform (Onyx AI Control Plane)]] [advertises AI-SPM and an inline MCP gateway](https://onyx.security/platform). [[wiz-ai-spm|Wiz AI-SPM]] [advertises agent inventory, graph-based posture analysis, and runtime monitoring](https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents). Prisma AIRS also advertises posture, runtime, and gateway functions. A useful comparison checks the connected inventory, the actual interception path, the action taken on a denied request, and records available to the assessor.
