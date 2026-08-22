---
type: talk
title: "AI Fingerprints for Threat Detection"
address: c-000153
created: 2026-05-26
updated: 2026-05-26
tags:
  - papers
  - talks
  - prompt-injection
  - threat-intelligence
  - differential-privacy
  - detection-engineering
  - sec-of-ai
  - ai-in-sec-defense
status: stub-summary
scope_axis: [sec-of-ai, ai-in-sec-defense]
year: 2026
authors: ["Natalie Isak", "Waris Gill", "Matthew Dressman"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 3, 2026"
key_claim: "BinaryShield lets siloed LLM services share prompt-injection attack intelligence across compliance boundaries by converting suspicious prompts into computationally non-invertible, differential-privacy-protected binary fingerprints — sharable where raw prompts and dense embeddings are not, because those remain reconstructable and PII-leaky."
methodology: "Practitioner talk by Microsoft researchers, backed by the arXiv paper 'BinaryShield: Cross-Service Threat Intelligence in LLM Services using Privacy-Preserving Fingerprints' (arXiv:2509.05608). The paper is the primary source; the talk slides and video are not yet captured."
source_url: "https://arxiv.org/abs/2509.05608"
related:
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[indirect-prompt-injection|Indirect Prompt Injection]]"
  - "[[prompt-injection-containment|Prompt Injection Containment]]"
  - "[[differential-privacy|Differential Privacy]]"
  - "[[agent-observability|Agent Observability]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
  - "[[microsoft|Microsoft]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - "https://arxiv.org/abs/2509.05608"
  - "https://unpromptedcon.org/#"
---

# Developing & Deploying AI Fingerprints for Advanced Threat Detection (BinaryShield)

A practitioner talk by Natalie Isak and Waris Gill ([[microsoft|Microsoft]]) at the [[unprompted-conference-march-2026|Unprompted Conference (March 2026)]] on **BinaryShield**. The system is documented in an arXiv paper co-authored with Matthew Dressman; that paper is the primary source here, and slides and video are not yet captured.

## The Argument

When one LLM service detects a prompt-injection attack, peer services usually cannot benefit. Raw prompts carry user PII and are legally unshareable across compliance boundaries; even dense embeddings are reconstructable and can leak the original text. So the same attack persists undetected across services for months. BinaryShield is built to make the fingerprint of an attack shareable without making the attack's content recoverable.[^paper]

The pipeline has four steps: PII redaction (via Microsoft Presidio), semantic embedding, **sign-based binary quantization** that discards magnitude so the original cannot be reconstructed, and a **randomized-response differential-privacy** step that flips bits according to a privacy budget. The result is a compact binary fingerprint searchable by Hamming distance, which a service can broadcast to peers to scan their own logs, flag live traffic, and train local defenses. On the paper's evaluation, the fingerprint holds detection quality where a naive hash collapses (F1 0.94 against paraphrased attacks versus 0.77 for SimHash) while cutting storage and search cost by large factors against dense embeddings.[^numbers]

## Placement

This extends detection from a single service to a **sharing layer** — the contribution the [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] thesis tracks under cross-organization threat intelligence. The threat class is [[indirect-prompt-injection|prompt injection]], and the privacy mechanism is a concrete application of [[differential-privacy|differential privacy]] to security telemetry, complementing [[prompt-injection-containment|prompt-injection containment]] at the single-service level.
## Notes

[^paper]: Gill, Isak, Dressman, "BinaryShield: Cross-Service Threat Intelligence in LLM Services using Privacy-Preserving Fingerprints," arXiv:2509.05608. Abstract and landing page: [arxiv.org/abs/2509.05608](https://arxiv.org/abs/2509.05608); full HTML: [arxiv.org/html/2509.05608v1](https://arxiv.org/html/2509.05608v1).
[^numbers]: F1 0.94 vs SimHash 0.77 on paraphrase attacks (abstract and §3.2); storage and similarity-search reductions against dense embeddings, and retention of ~93% of the non-private dense-baseline Accuracy@1 at a 100K corpus (§3.3). [arxiv.org/html/2509.05608v1](https://arxiv.org/html/2509.05608v1).
