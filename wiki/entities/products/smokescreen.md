---
type: entity
entity_type: product
homepage: "https://github.com/stripe/smokescreen"
title: "Smokescreen (Stripe)"
created: 2026-05-02
updated: 2026-06-23
tags:
  - entities
  - products
  - stripe
  - egress-control
  - open-source
  - smokescreen
status: seed
scope_axis:
  - sec-of-ai
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[stripe]]"
  - "[[andrew-bullen]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[lethal-trifecta]]"
  - "[[prompt-injection-containment]]"
sources:
  - "[[.raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_transcript.md]]"
---

# Smokescreen

**Sources:** [Smokescreen (repo)](https://github.com/stripe/smokescreen)

**Stripe's open-source egress proxy / SSRF-prevention HTTP CONNECT proxy.** Pre-dates the AI-agent era; repurposed (per Andrew Bullen's Unprompted talk) as the network-side control point for the egress-leg of [[lethal-trifecta|Lethal Trifecta]] containment.

## Origin

Smokescreen is a long-standing Stripe open-source project (publicly available as `stripe/smokescreen`). It was originally built for general SSRF prevention — preventing internal services from being tricked into making egress requests to attacker-controlled internal IPs / metadata services. The AI-agent application is a re-use of an existing control rather than a new build.

## Use in Stripe's agent architecture (per Bullen's talk)

The control flow:

1. **Tag agentic services.** Stripe knows which services are agents because every agent has to talk to a foundation model, and Stripe routes those through a known proxy. This is the operational handle.
2. **Smokescreen proxies the egress.** The agent service's outbound HTTP egress goes through Smokescreen as the choke point.
3. **CI-time check.** When a tagged-agent service tries to configure egress (declare allowed domains / endpoints), CI requires an escalated review.

The combination — tag + Smokescreen choke + CI gate — is what Bullen calls Stripe's "strong egress control program that pre-dated the world of AI agents."

## Relevance to this corpus

This is one of the most concrete data points in the corpus that **breaking the egress leg of the Lethal Trifecta is operational, not aspirational**, when the org has a pre-existing egress proxy program. It's also a generalizable pattern: any organization with a foundation-model proxy can derive an "is-agent" tag for free.

> [!gap] Verify externally
> The Smokescreen GitHub repo and recent commit history would confirm whether AI-specific features have landed since Bullen's talk. Worth a follow-up scrape.

## See also

- [[toolshed|Toolshed]] — the MCP / tool-call counterpart inside Stripe.
- [[lethal-trifecta|Lethal Trifecta]] · [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]] · [[breaking-the-lethal-trifecta-talk|Breaking the Lethal Trifecta (Without Ruining Your Agents)]]
