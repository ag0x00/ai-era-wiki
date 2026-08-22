---
type: paper
title: "Secure Agentic AI End-to-End"
address: c-000013
created: 2026-05-07
updated: 2026-05-07
tags:
  - papers
  - microsoft
  - agentic-ai
  - vendor-blog
  - product-roadmap
  - rsac-2026
status: summarized
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
publication_date: 2026-03-20
authors:
  - "[[vasu-jakkal|Vasu Jakkal]]"
publisher: "Microsoft Security Blog"
source_url: https://www.microsoft.com/en-us/security/blog/2026/03/20/secure-agentic-ai-end-to-end/
event_context: "Pre-RSAC 2026 announcement"
related:
  - "[[microsoft|Microsoft]]"
  - "[[microsoft-entra-agent-id|Microsoft Entra Agent ID + Agent 365]]"
  - "[[microsoft-zt4ai|Microsoft ZT4AI]]"
  - "[[network-layer-prompt-injection-containment|Network-Layer Prompt Injection Containment]]"
  - "[[microsoft-rai|Microsoft RAI]]"
sources:
  - "[[.raw/articles/microsoft-secure-agentic-ai-end-to-end-2026-05-07.md]]"
---

# Secure Agentic AI End-to-End: Source Summary

Vasu Jakkal's pre-RSAC 2026 announcement post (2026-03-20) consolidating Microsoft's agentic-AI security product roadmap across Microsoft 365, Entra, Purview, Defender, Sentinel, and Security Copilot. The blog is positioning content for a product set that lands across March–May 2026, with Agent 365 GA on May 1.

## Three-pillar framing

Microsoft's positioning organizes the agentic-AI security portfolio under three pillars:

1. **Secure agents**: [[microsoft-entra-agent-id|Agent 365]] as "the control plane for agents," with Defender / Entra / Purview capabilities included.
2. **Secure foundations**: visibility (Security Dashboard for AI; Shadow AI Detection); identity (Entra extensions); data (Purview DLP for Copilot); threat detection (Defender for Cloud + new Predictive Shielding).
3. **Defend with agents and experts**: Security Copilot agents in the SOC; Sentinel as the agentic defense platform; Defender Experts Suite.

Two terminological flourishes worth noting: the phrase "**security as the core primitive of the AI stack**" (positioning) and the "**double agents**" framing (rhetorical, naming agents that have been compromised or manipulated to act against their principal).

## Load-bearing announcements

- **Agent 365 GA May 1, 2026**: re-positioned from a sub-feature of Entra Agent ID (per the wiki's prior coverage) to the umbrella product. Bundles Defender, Entra, Purview capabilities for agent governance.
- **Microsoft 365 E7: The Frontier Suite**: new SKU bundling Agent 365 + M365 Copilot + Entra Suite + M365 E5.
- **Entra Internet Access Prompt Injection Protection (GA March 31)**: first major-vendor shipping of [[network-layer-prompt-injection-containment|network-layer prompt injection containment]]; surfaces a new architectural primitive distinct from application-layer guardrails.
- **Defender Predictive Shielding (preview)**: adaptive policy contraction during active attacks. Dynamically tightens identity and access policies when threats are detected; reverts as the threat passes.
- **Sentinel MCP Entity Analyzer (GA in April)**: first major SIEM with native MCP integration.
- **Updated Zero Trust for AI (ZT4AI) reference architecture**: the wiki's [[microsoft-zt4ai|new framework page]] anchors the 12+ pre-existing scattered references.

## Relevance to this corpus

- **Agent 365 positioning shift**: the wiki's existing [[microsoft-entra-agent-id|product page]] was titled "Microsoft Entra Agent ID and Agent 365 Registry"; the post now positions Agent 365 as the umbrella with Entra Agent ID as one of several included primitives. The wiki's product page should reflect this hierarchy.
- **Network-layer prompt injection**: was missing from the wiki's [[prompt-injection-containment|prompt-injection-containment practice page]] (which has only application-layer Layer-1 detection and Layer-2 execution containment). This is a third architectural layer; new concept page added.
- **ZT4AI**: referenced 12 times in the wiki but had no dedicated framework page; closing that gap.
- **Adaptive policy contraction**: Microsoft's Predictive Shielding is a vendor implementation of a general defensive primitive (detect → contract → revert). The wiki has step-up auth (proactive elevation) but doesn't have step-down (reactive contraction) named separately. Documented inline in the ZT4AI page rather than as a standalone concept.

## Stickiness assessment (~6 weeks post-publication)

Too fresh to assess externally; the post was a product announcement, not a research artifact. Internal stickiness signals at ingest time:

- **"Agent 365 = control plane for agents"**: sticky positioning, will likely propagate as Microsoft's marketing reach is wide.
- **"Double agents"** framing: playful but unlikely to be taken up beyond Microsoft.
- **Three-pillar structure** (secure / secure foundations / defend with agents): positioning, not a load-bearing terminology contribution.
- **Network-layer prompt injection containment as a category**: likely sticky; once one major vendor ships network-layer PI defense, others follow. Worth tracking the spread of the terminology.

## Limitations

- **Vendor announcement, not technical depth.** The post is positioning content; specific control mechanisms, threat models, and effectiveness data are not included.
- **No public testing or third-party evaluation.** All claims are vendor-stated.
- **Product GA dates are forward-looking**: March 26 / 31, April, May. Subject to Microsoft's typical preview-to-GA shifts.

## See also

The full structural analysis lives at [[microsoft-zt4ai|the ZT4AI framework page]] and [[network-layer-prompt-injection-containment|Network-Layer Prompt Injection Containment]]. The Microsoft product portfolio is consolidated on the [[microsoft|Microsoft org page]]; agent-platform specifics on [[microsoft-entra-agent-id|the Entra Agent ID + Agent 365 product page]].
