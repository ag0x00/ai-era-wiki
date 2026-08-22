---
type: entity
entity_type: product
title: "Agentforce (Salesforce)"
created: 2026-05-03
updated: 2026-05-03
tags:
  - entities
  - products
  - agentic-ai
  - salesforce
status: seed
scope_axis:
  - sec-of-ai
homepage: "https://www.salesforce.com/agentforce/"
vendor: "[[salesforce]]"
related:
  - "[[salesforce]]"
  - "[[1-8m-prompts-30-alerts-talk]]"
  - "[[behavioral-anomaly-detection-for-agents]]"
  - "[[agent-observability]]"
sources:
  - "https://www.salesforce.com/agentforce/"
  - ".raw/talks/2026-03-04_Millie-and-Matt-Rittinghouse_1.8M-Prompts-30-Alerts_transcript.md"
---

# Agentforce (Salesforce)

**Sources:** [Homepage](https://www.salesforce.com/agentforce/)

Agentforce is [[salesforce|Salesforce]]'s agentic AI platform, deployed both internally and across Salesforce's customer tenant organizations. It enables businesses to build and deploy custom autonomous agents within the Salesforce ecosystem, with access to Salesforce data (CRM records, custom objects, etc.) and execution capabilities via APEX skills and Salesforce APIs.

## Scale (as of March 2026)

| Metric | Value |
|---|---|
| Tenant organizations monitored | 55,000+ |
| Unique daily active agents | 12,000+ |
| Daily prompts | ~1,800,000 |

## Security architecture notes

Agentforce includes a native **trust layer** (Salesforce's term for content moderation at the reasoning layer). However, as detailed in [[1-8m-prompts-30-alerts-talk|"1.8M Prompts, 30 Alerts"]], the CSOC team concluded that content moderation alone is insufficient for three reasons:

1. It cannot see the execution layer (actual API calls, data queries)
2. It lacks the high-fidelity signal needed for automated blocking in a multi-tenant context
3. It cannot detect post-generation plans involving unauthorized data access or privilege escalation

The supplementary security layer is a **behavioral anomaly detection model** operating at the execution layer, described in [[behavioral-anomaly-detection-for-agents|Behavioral Anomaly Detection for Agents]].

## Multi-tenant security challenge

Agentforce's multi-tenant architecture creates a unique security posture:

- **No two agents behave identically** — each tenant's agents are custom-built, making signature-based detection infeasible at scale
- **Privacy constraint** — Salesforce cannot inspect prompt content (customer data); defenses must operate on behavioral metadata
- **Scale constraint** — 12,000+ unique agent implementations preclude manual tuning per-agent

This profile makes Agentforce one of the largest and most complex production deployments of agentic AI security to date (as of the March 2026 talk).
