---
type: entity
entity_type: organization
org_type: vendor
title: "CrowdStrike"
created: 2026-05-04
updated: 2026-09-10
tags:
  - entities
  - organizations
  - edr
  - siem
  - ai-detection
  - glasswing
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
homepage: "https://www.crowdstrike.com"
related:
  - "[[agent-observability]]"
  - "[[glasswing]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[mythos]]"
  - "[[claude-partners-opus-cybersecurity]]"
  - "[[falcon-guardian]]"
  - "[[crowdstrike-agentic-identity-provider]]"
sources:
  - "https://www.crowdstrike.com/platform/falcon-ai"
  - "https://www.anthropic.com/glasswing"
  - "[[.raw/articles/claude-partners-opus-cybersecurity-2026-05-23.md]]"
---

# CrowdStrike

CrowdStrike is an endpoint detection and SIEM vendor extending the Falcon platform to AI-agent detection. **Falcon Guardian** (announced 2026-09-01) is the current AI detection and response product, replacing the Falcon AIDR branding and inheriting its prompt-injection detection, prevention of sensitive-data exposure, session reconstruction and cross-agent impact tracing; the wiki's [[agentic-ai-security-reference-architecture|reference architecture]] D7 SIEM-playbook row still cites the earlier Falcon AIDR branding and has not yet been updated for the rename. CrowdStrike is extending Falcon's existing EDR/XDR telemetry with agent-action context for cross-agent behavioral monitoring at scale. CrowdStrike is a named launch partner of [[glasswing|Project Glasswing]] (Anthropic coalition, May 2026). CTO Elia Zaitsev framed the shift driving that partnership: *"The window between a vulnerability being discovered and being exploited by an adversary has collapsed — what once took months now happens in minutes with AI."*

> [!gap]
> Pending content: general company overview (founding, headquarters, market position); integration patterns with existing CrowdStrike-customer agent platforms; public case studies for Falcon Guardian and the Agentic Identity Provider; how CrowdStrike's Glasswing usage relates to Falcon Guardian's development.

## Frontier AI Readiness & Resilience Service (2026)

CrowdStrike's [Frontier AI Readiness and Resilience Service](https://www.crowdstrike.com/en-us/services/ai-security-services/frontier-ai-readiness-and-resilience/) pairs [[mythos|Claude Opus]] with the firm's AI Red Team Services and proprietary agent frameworks to hunt latent zero-days in customer applications, validate findings, and accelerate remediation before new code reaches production, extending the service to a platform more than 60% of the Fortune 500 use (see [[claude-partners-opus-cybersecurity|the Opus partner ecosystem]]). Global VP of Consulting Services Mark Manglicmot: *"Frontier models like Anthropic's Claude Opus are giving defenders a capability advantage that didn't exist a year ago, pushing vulnerability management all the way to the left."*

## Agent security at Fal.Con 2026

CrowdStrike announced two agent-security products on consecutive days at Fal.Con 2026. [[falcon-guardian|Falcon Guardian]] (September 1) makes the endpoint the enforcement point for AI agents: it discovers known and shadow agents on Windows, macOS and Linux, defines which agent types may run there, correlates a user prompt to the tool calls and system actions that follow it, and delivers the result to Falcon Next-Gen SIEM as first-party telemetry alongside identity, cloud and SaaS data. George Kurtz stated the position as the one CrowdStrike took for EDR: *"CrowdStrike pioneered EDR by making the endpoint the control point for stopping attacks. AI demands the same approach."* CrowdStrike discloses no general-availability date and no pricing for Falcon Guardian itself, names an AI Gateway as forthcoming without a date, and states the Falcon Complete for Guardian managed service for later in Q3 2026.

[[crowdstrike-agentic-identity-provider|The Agentic Identity Provider]] (September 2) is the identity layer beneath it. Falcon Guardian's discovery registers each agent in a single directory, the Agentic IdP issues it a cryptographically verifiable identity, and access runs through short-lived tokens scoped to one task rather than through standing credentials, with every action bound to the delegating human or workload. Scott Kriz, General Manager of Continuous Identity, stated the premise: *"Traditional identity providers break the moment an agent acts on its own."* CrowdStrike states the product is in development, discloses no general-availability date, publishes no cryptographic detail, and positions it to work alongside Okta and Microsoft Entra for privileged access in AWS.
