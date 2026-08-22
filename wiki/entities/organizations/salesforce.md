---
type: entity
entity_type: organization
org_type: vendor
title: "Salesforce"
created: 2026-05-03
updated: 2026-05-23
tags:
  - entities
  - organizations
  - agentic-ai
  - salesforce
status: seed
homepage: "https://salesforce.com"
related:
  - "[[agentforce]]"
  - "[[matt-rittinghouse]]"
  - "[[millie-rittinghouse]]"
  - "[[1-8m-prompts-30-alerts-talk]]"
  - "[[beyond-the-chatbot-talk]]"
  - "[[peter-smith]]"
  - "[[ravi-kiran-sharma]]"
  - "[[unprompted-conference-march-2026]]"
sources:
  - "https://salesforce.com"
  - ".raw/talks/2026-03-04_Millie-and-Matt-Rittinghouse_1.8M-Prompts-30-Alerts_transcript.md"
---

# Salesforce

**Sources:** [Homepage](https://salesforce.com)

Salesforce is a publicly traded enterprise SaaS company (NYSE: CRM) and one of the largest CRM and cloud-platform vendors globally. In the context of this wiki, Salesforce is relevant as both a **major agentic AI platform operator** (deploying Agentforce across 55,000+ tenant organizations) and as a source of production-scale agentic AI security research.

## Agentforce

[[agentforce|Agentforce]] is Salesforce's agentic AI platform. As of March 2026, it operates at:

- 55,000 customer organizations monitored daily
- 12,000+ unique daily active agents
- ~1.8 million daily prompts

The Cybersecurity Operations Center (CSOC) team at Salesforce (including [[matt-rittinghouse|Matt Rittinghouse]] and [[millie-rittinghouse|Millie Rittinghouse]]) has built behavioral anomaly detection infrastructure specifically for this platform.

## Security research contributions

Salesforce contributed two talks to [[unprompted-conference-march-2026|Unprompted March 2026]] (Stage 2):

- **[[1-8m-prompts-30-alerts-talk|1.8M Prompts, 30 Alerts]]** (Rittinghouse pair, 13:30) — behavioral anomaly detection for Agentforce; three-level ensemble model; <30 daily actionable alerts from ~1.8M prompts.
- **[[beyond-the-chatbot-talk|Beyond the Chatbot — Delivering an Agentic SOC for Real-World Defense]]** ([[peter-smith|Peter Smith]] + [[ravi-kiran-sharma|Ravi Kiran Sharma]], 15:05) — Polyphonic (Supervisor-Worker) architecture for agentic SOC operations.

## Architectural context

Salesforce's threat model for Agentforce identifies two threat buckets — **platform-target attacks** (exploiting platform misconfigurations or APEX skill vulnerabilities) and **abuse of legitimate agency** (valid capabilities used maliciously in context). The second bucket cannot be addressed by content moderation alone because:

1. Salesforce cannot inspect prompt content (customer data privacy constraint)
2. Agents construct their own queries; query-complexity heuristics confuse agent logic with attacker behavior
3. Static signatures cannot scale across 12,000+ unique agent implementations

This led to the behavioral / baseline approach described in [[behavioral-anomaly-detection-for-agents|Behavioral Anomaly Detection for Agents]].
