---
type: entity
title: "Stripe"
created: 2026-04-30
updated: 2026-06-21
tags:
  - entities
  - organizations
  - stripe
  - payments
  - ai-security-leader
status: developing
entity_type: organization
org_type: vendor
role: "Payments platform; published architectural pattern for prompt-injection containment in agent platforms (Smokescreen + Toolshed + ToolAnnotations); two talks at Unprompted March 2026."
related:
  - "[[lethal-trifecta]]"
  - "[[lethal-bifecta]]"
  - "[[andrew-bullen]]"
  - "[[jeffrey-zhang]]"
  - "[[sid-shah]]"
  - "[[toolshed]]"
  - "[[smokescreen]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[guardrails-beyond-vibes-talk]]"
  - "[[unprompted-conference-march-2026]]"
homepage: "https://stripe.com"
sources:
  - "[[.raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_slides.pdf]]"
  - "[[.raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_transcript.md]]"
  - ".raw/talks/2026-03-03_Jeffrey-Zhang-and-Sid_Guardrails-beyond-Vibes_slides.pdf"
  - ".raw/talks/2026-03-03_Jeffrey-Zhang-and-Sid_Guardrails-beyond-Vibes_transcript.md"
---

# Stripe

**Sources:** [Stripe (homepage)](https://stripe.com) · [Smokescreen egress proxy (GitHub)](https://github.com/stripe/smokescreen)

Payments platform. In the AI-security space, Stripe has emerged as one of the most-cited practitioner sources for **production agent containment architecture** — they ship customer-facing AI agents at scale and have an internal AI Security team led by [[andrew-bullen|Andrew Bullen]].

## Relevance to this corpus

Two talks at [[unprompted-conference-march-2026|Unprompted Conference, March 2026]]:

| Talk | Speaker(s) | Status in this wiki |
|---|---|---|
| **[[breaking-the-lethal-trifecta-talk\|Breaking the Lethal Trifecta (Without Ruining Your Agents)]]** | Andrew Bullen | Full summary ingested (slides + transcript). The canonical practitioner worked example for [[lethal-trifecta\|trifecta]] containment + [[lethal-bifecta\|bifecta]] coining. |
| **[[guardrails-beyond-vibes-talk\|Guardrails Beyond Vibes: Shipping Security Agents in Production]]** | Jeffrey Zhang + Siddh Shah | **Full summary ingested (slides + transcript).** Two production security agents: modular multi-agent sequential pipeline for threat modeling; single focused agent with minimal toolset for security routing. Evaluation via golden-standard test cases + [[llm-as-a-judge\|LLM-as-a-Judge]] semantic scoring. Five concrete learnings including AlphaEvolve failure. |

## Stripe's published agent containment stack

The components, in the order they appear in Bullen's talk:

| Component | Role | This wiki |
|---|---|---|
| **[[smokescreen\|Smokescreen]]** | Open-source egress proxy (pre-dates AI agents); choke point for the External-Communication leg of the trifecta. | Product page |
| **Agent-tagging convention** | Any service that talks to Stripe's foundation-model proxy is tagged-as-agent. | Pattern documented in [[breaking-the-lethal-trifecta-talk\|Breaking the Lethal Trifecta (Without Ruining Your Agents)]]. |
| **CI-time egress check** | Tagged services can't change egress configuration without escalated review. | Same as above. |
| **[[toolshed\|Toolshed]]** | Internal central MCP proxy + tool registry; PEP for tool-call policy. | Product page |
| **`ToolAnnotations` schema** | Declarative per-tool annotations (`production_impacting_write`, `data_sensitivity`, `broadcasts_data_internally`); PDP for human-review gating. | Documented in talk page |
| **Safe Search via OpenAI** | Internet-data without true egress (sets `external_web_access: false`); honest caveat — shifts trust to OpenAI. | In talk page |
| **Queued / batched / optimistic confirmations** | UX patterns to keep agents moving while preserving human-in-the-loop on sensitive actions. | In talk page |

## Notable real-world incident referenced

**Direct hit.** Bullen cites the 2025-07-16 disclosure **"Claude Jailbroken to Mint Unlimited Stripe Coupons"** in his threat-baseline slide — a prompt-injection-driven jailbreak with direct financial impact on Stripe's surface area. This is a useful data point: Stripe is publishing its containment architecture in the wake of a real consumer-side incident hitting their own brand, not as an a-priori thought experiment.

A separate incidents-page write-up of the Claude→Stripe-coupon jailbreak would be a useful follow-up — the talk references the headline but does not give the full kill-chain.

## People

- [[andrew-bullen|Andrew Bullen]] — Head of AI Security, ~10 years tenure.
- [[jeffrey-zhang|Jeffrey Zhang]] — Security Engineer; co-presenter of [[guardrails-beyond-vibes-talk|Guardrails Beyond Vibes]].
- [[sid-shah|Siddh Shah]] — Software Engineer; co-presenter of [[guardrails-beyond-vibes-talk|Guardrails Beyond Vibes]].
