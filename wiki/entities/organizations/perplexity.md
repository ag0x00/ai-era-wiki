---
type: entity
entity_type: organization
org_type: vendor
title: "Perplexity"
created: 2026-05-07
updated: 2026-07-31
tags:
  - organizations
  - perplexity
  - browser-agents
  - prompt-injection
  - production-detection
  - endpoint-security
  - open-source
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
homepage: "https://www.perplexity.ai"
related:
  - "[[unprompted-conference-march-2026]]"
  - "[[indirect-prompt-injection]]"
  - "[[numbat|Numbat]]"
  - "[[perplexity-numbat-agent-security|Numbat Agent Security Suite]]"
  - "[[accidental-meltdown|Accidental Meltdown]]"
  - "[[open-secure-ai-alliance|Open Secure AI Alliance]]"
sources:
  - "https://www.perplexity.ai"
  - "https://research.perplexity.ai/articles/securing-agents-across-perplexity%E2%80%99s-client-endpoints-with-numbat"
---

# Perplexity

**Sources:** [Perplexity (homepage)](https://www.perplexity.ai) · [[unprompted-conference-march-2026|Unprompted March 2026 — BrowseSafe talk]] · [[perplexity-numbat-agent-security|Numbat announcement]]

Conversational-search and **browser-agent** vendor; operates a production browser-agent that fetches and reasons over web content on behalf of the user. Perplexity appears in this wiki on two distinct security surfaces: as the operator of **BrowseSafe**, a fine-tuned in-line classifier defending the production browser-agent against prompt injection, and as the publisher of **[[numbat|Numbat]]**, an open-source agent security suite for enterprise client endpoints.

The two positions sit on opposite sides of the injection threat model. BrowseSafe is a classifier defense against adversarial input; Numbat is built for the [[accidental-meltdown|accidental meltdown]] case, where no adversarial input exists at all.

## BrowseSafe

Presented at [[unprompted-conference-march-2026|Unprompted March 2026]] by Kyle Polley — "Training BrowseSafe: Lessons from Detecting Prompt Injection in Production Browser Agents" (Day 2 / Stage 1 / 14:20). Reported numbers: **F1 ~0.91 at sub-100ms latency**, fine-tuned MoE Qwen-30B base; companion benchmark **BrowseSafe-Bench** with high-entropy realistic HTML; data flywheel from production user feedback.

The talk is a primary practitioner case study on production-scale [[indirect-prompt-injection|indirect prompt injection]] detection at the browser-agent boundary.

## Numbat and internal security tooling

[[numbat|Numbat]] is Perplexity's open-source agent security suite for client endpoints, released through the company's [[open-secure-ai-alliance|Open Secure AI Alliance]] membership and summarized at [[perplexity-numbat-agent-security|Numbat Agent Security Suite]]. It integrates with coding-agent harnesses through hooks, filesystem session artifacts, and OTLP telemetry.

Perplexity reports running it across its own fleet, distributed by MDM alongside **Bumblebee**, an open-source scanner for supply-chain exposure on developer endpoints. **Perplexity Computer**, the company's agentic system, reviews Numbat findings on a schedule, reconstructs sessions, and opens pull requests proposing new detection rules for human review. The securing-AI control supplies the telemetry that the defending-with-AI workflow consumes.

## See also

- [[unprompted-conference-march-2026|Unprompted March 2026]] — talk venue
- [[indirect-prompt-injection|Indirect Prompt Injection]] — the threat class BrowseSafe targets
- [[numbat|Numbat]] — open-source agent security suite
- [[accidental-meltdown|Accidental Meltdown]] — the threat model Numbat is designed against
- [[open-secure-ai-alliance|Open Secure AI Alliance]] — release vehicle
