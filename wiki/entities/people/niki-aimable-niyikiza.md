---
type: person
title: "Niki Aimable Niyikiza"
created: 2026-05-03
updated: 2026-05-03
tags:
  - people
  - capabilities
  - authorization
  - tenuo
  - snap
  - infrastructure-security
status: stub
role: "Founder @ Tenuo / Security Engineering @ Snap"
affiliations:
  - "[[tenuo]]"
  - "[[snap]]"
related:
  - "[[capability-based-authorization-talk]]"
  - "[[tenuo]]"
  - "[[tenuo-warrant]]"
  - "[[capability-based-authorization]]"
sources:
  - "[[capability-based-authorization-talk]]"
website: "niyikiza.com"
github: "https://github.com/tenuo-ai/tenuo/"
contact: "niki@tenuo.ai"
---

# Niki Aimable Niyikiza

Founder of [[tenuo|Tenuo]] and Security Engineer at [[snap|Snap]]. ~10 years of infrastructure-security experience prior to Tenuo, with stops at Google, Datadog, and Snap. Speaks publicly on **capability-based authorization for AI agents**: the position that identity-based authorization gives agents *ambient* authority, which is structurally the wrong primitive for multi-agent flows that delegate and reason at run time. His proposed answer is the **Tenuo Warrant** (see [[tenuo-warrant|Tenuo Warrant]]): cryptographic, task-scoped, holder-bound, offline-verifiable, delegation-aware capability artifacts that monotonically attenuate across hops.

## Talks tracked here

- [[capability-based-authorization-talk|Capability-Based Authorization for AI Agents — Warrants That Survive Prompt Injection]]: Unprompted Conference, San Francisco, March 4, 2026, Stage 2 Lecture 03.

## Argument

The four-layer stack everyone builds (Identity / Policy Engines / Model Guardrails / Sandbox) has no primitive for **delegation-aware authorization at execution time**. When Agent A hands work to Agent B, no existing layer can verify that B's authority *came from A*, that scope *narrowed at the hop*, or that the *task justified the access*. Capabilities — specifically the [[tenuo-warrant|Tenuo Warrant]] — fill that hole. Critically, capabilities don't *prevent* prompt injection; they *constrain what an injected agent can do*, which is the right shape for runtime containment.

## Notable framings

- **"Identity-based auth treats all three the same"**: intended action, hallucinated action, injected action. The system has no basis to distinguish.
- **The valet-key analogy**: a master key (ambient authority) versus a geo-fenced, speed-capped, time-bound, glove-box-disabled valet key (derived authority).
- **"The map is not the territory"**: constraint *design* (path normalization, URL canonicalization, OS sandbox guards) matters more than the cryptography. Real CVEs (CVE-2024-3571 LangChain, CVE-2025-3046 LlamaIndex, CVE-2025-61784 LlamaFactory, CVE-2025-66032 Claude Code allowlist) validate the failure mode.
- **"The blast radius is frozen"**: even a fully-compromised sub-agent cannot exceed what its parent's warrant granted.

## Cross-references

- Founder: [[tenuo|Tenuo]] (Rust core + Python bindings; open source at `github.com/tenuo-ai/tenuo`)
- Current employer: [[snap|Snap]]
- Companion talk same day: [[breaking-the-lethal-trifecta-talk|Bullen — Breaking the Lethal Trifecta]], a different angle (egress + tool annotations) on the same containment problem

## Open public traces

- Personal site: `niyikiza.com` (referenced for the *map vs territory* writeup)
- Tenuo: `tenuo.ai` (playground for decoding live warrants); `github.com/tenuo-ai/tenuo` (open-source Rust core)
- Email (per slide 12): `niki@tenuo.ai`
