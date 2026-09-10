---
type: entity
entity_type: product
vendor: "[[crowdstrike|CrowdStrike]]"
title: "Falcon Guardian"
address: c-000347
created: 2026-09-10
updated: 2026-09-10
tags:
  - products
  - endpoint
  - ai-detection
  - identity
  - mcp
  - siem
status: seed
scope_axis:
  - sec-of-ai
origin: aggregated
homepage: "https://www.crowdstrike.com/en-us/press-releases/crowdstrike-unveils-falcon-guardian-ai-agent-security/"
ga_date: ""
related:
  - "[[crowdstrike]]"
  - "[[crowdstrike-agentic-identity-provider]]"
  - "[[agent-identity-architecture]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[non-human-identity]]"
  - "[[agent-catalog]]"
  - "[[mcp-security]]"
  - "[[shadow-automation]]"
  - "[[guardian-agent]]"
  - "[[nhi-governance-for-agents]]"
  - "[[agent-observability]]"
sources:
  - "https://www.crowdstrike.com/en-us/press-releases/crowdstrike-unveils-falcon-guardian-ai-agent-security/"
  - "https://www.crowdstrike.com/en-us/blog/falcon-guardian-defines-next-generation-of-ai-security/"
  - "https://ir.crowdstrike.com/news-releases/news-release-details/crowdstrike-unveils-falcon-guardian-secure-ai-agents-where-they/"
  - "https://nand-research.com/crowdstrike-falcon-guardian-ai-agent-security-at-the-endpoint/"
  - "https://forkast.news/crowdstrike-falcon-guardian-makes-the-endpoint-the-enforcement-layer-for-ai-agents/"
---

# Falcon Guardian

**Sources:** [Press release](https://www.crowdstrike.com/en-us/press-releases/crowdstrike-unveils-falcon-guardian-ai-agent-security/) · [Product blog](https://www.crowdstrike.com/en-us/blog/falcon-guardian-defines-next-generation-of-ai-security/) · [NAND Research analysis](https://nand-research.com/crowdstrike-falcon-guardian-ai-agent-security-at-the-endpoint/)

## Identity and role

[[crowdstrike|CrowdStrike]] announced Falcon Guardian on September 1, 2026 at Fal.Con 2026 in Las Vegas, its AI Detection and Response (AIDR) product for securing autonomous AI agents at runtime. It runs as an extension of the existing Falcon sensor on Windows, macOS, and Linux endpoints, so a customer already running Falcon needs no new agent deployment to adopt it. Falcon Guardian supersedes the prior Falcon AIDR branding as CrowdStrike's current agent-security product.

## Discovery and runtime detection

Falcon Guardian discovers known and [[shadow-automation|shadow]] AI agents across managed endpoints, tracking each agent's deployment source, user identity, and security status — building the kind of endpoint-resident [[agent-catalog|agent inventory]] that governance programs otherwise assemble by hand. It establishes causal chains linking a user prompt to the agent's skill use, tool calls, and [[mcp-security|MCP]] server invocations, then traces those chains through to the downstream system actions the agent executes on the user's behalf. On top of that correlation it runs runtime threat detection — prompt injection and other attacks on the agent, and malicious agent behavior — reconstructing the full execution chain and the blast radius of a detected event, and applies runtime controls to redact, encrypt, or block sensitive data identified in an agent's interactions before it leaves the endpoint. This causal, session-level reconstruction places Falcon Guardian in the [[agent-observability|agent observability]] practice area, applied specifically to the endpoint vantage point.

CrowdStrike claims 99% detection efficacy against prompt attacks at a response latency under 100 milliseconds. Both figures reach this page through NAND Research's write-up of the launch, which reports them as CrowdStrike's claim rather than as its own measurement; neither the press release nor the product blog states either number, and no methodology or bypass corpus accompanies them.[^nand]

## Access controls and identity

Falcon Guardian lets security teams define which AI agent types may run on a managed endpoint and blocks unauthorized agent types from executing — the endpoint-resident case of a [[guardian-agent|guardian agent]], an agent that supervises what other agents are permitted to do. Discovery under Falcon Guardian is also the entry point into CrowdStrike's identity layer: each agent it finds is registered in the directory that [[crowdstrike-agentic-identity-provider|CrowdStrike's Agentic Identity Provider]], announced the following day, uses to issue cryptographic identity and broker access. Falcon Guardian data flows as first-party telemetry into Falcon Next-Gen SIEM, correlated with identity, cloud, and SaaS telemetry for unified investigation. Read alongside CrowdStrike's identity product, the pairing covers two legs of the [[agent-identity-architecture|agent identity architecture]] — discovery and inventory here, identity issuance and [[non-human-identity|non-human identity]] lifecycle in the companion product — and both bear on the [[agentic-ai-security-cmm-d2-identity|CMM D2 Identity and Authorization]] domain's per-agent identity and inventory criteria. Falcon Guardian's prompt-attack detection is also graded against the [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4 Runtime and Guardrails]] ladder's input-classifier and measured-efficacy criteria, which that domain's page carries in detail. Lifecycle controls act on an inventory of what is running, which makes the discovery and access-control functions here the practical mechanism behind [[nhi-governance-for-agents|NHI governance for AI agents]].

## Availability and forthcoming components

CrowdStrike has not disclosed a general availability date or pricing for Falcon Guardian. Two components are announced but not yet shipping. The product blog says Falcon Guardian "will soon include a native AI gateway capability, offering a new centralized control point for enterprise AI traffic," and attaches no date; NAND Research lists the same gateway among the capabilities that are forthcoming rather than available at launch, also without a date. Falcon Complete for Guardian, a 24/7 managed detection and response service for AI agents, is stated for later in Q3 2026.

## Notable statements

George Kurtz, CEO and founder, CrowdStrike: "CrowdStrike pioneered EDR by making the endpoint the control point for stopping attacks. AI demands the same approach. AI hasn't changed the attack, it has changed its speed. Governance alone can't stop an agent already in motion. Falcon Guardian turns policy into protection, stopping threats where AI agents execute and before they can cause harm."

[^nand]: [NAND Research — CrowdStrike Falcon Guardian: AI Agent Security at the Endpoint](https://nand-research.com/crowdstrike-falcon-guardian-ai-agent-security-at-the-endpoint/), early September 2026.
