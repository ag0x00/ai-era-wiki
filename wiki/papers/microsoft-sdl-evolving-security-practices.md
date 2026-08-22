---
type: paper
title: "Microsoft SDL"
address: c-000044
created: 2026-05-14
updated: 2026-08-21
tags:
  - paper
  - article
  - microsoft
  - sdl
  - secure-sdlc
  - secure-by-design
  - ai-security
  - threat-modeling
  - agent-identity
status: developing
scope_axis:
  - sec-of-ai
  - sec-against-ai
authors:
  - "[[yonatan-zunger|Yonatan Zunger]]"
publisher: "[[microsoft|Microsoft Security Blog]]"
publication_date: 2026-02-03
source_url: https://www.microsoft.com/en-us/security/blog/2026/02/03/microsoft-sdl-evolving-security-practices-for-an-ai-powered-world/
source_hash: 8b9111c3e7ac625de0dccd140d23d579
related:
  - "[[microsoft-sdl]]"
  - "[[microsoft]]"
  - "[[yonatan-zunger]]"
  - "[[secure-sdlc-framework-stack-2026]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
  - "[[microsoft-zt4ai]]"
  - "[[microsoft-secure-agentic-ai-end-to-end]]"
  - "[[nist-ssdf]]"
  - "[[owasp-samm]]"
  - "[[prompt-injection]]"
  - "[[indirect-prompt-injection]]"
  - "[[agent-memory-isolation]]"
  - "[[agent-identity-architecture]]"
  - "[[distributed-kill-switch]]"
  - "[[memory-poisoning]]"
sources:
  - "[[.raw/articles/microsoft-sdl-evolving-security-practices-2026-02-03.md]]"
  - "https://www.microsoft.com/en-us/security/blog/2026/02/03/microsoft-sdl-evolving-security-practices-for-an-ai-powered-world/"
---

# Microsoft SDL: Evolving Security Practices for an AI-Powered World

Microsoft Security Blog post — *Yonatan Zunger, 2026-02-03* — announces the explicit extension of [[microsoft-sdl|Microsoft's Secure Development Lifecycle]] to AI workloads. The post is short (≈1,800 words) and functions as a strategic preamble rather than a technical specification: the substantive per-area guidance is promised "in the coming months." Two contributions to the wiki — the explicit six-area AI scope and the "way of working, not a checklist" framing — anchor [[microsoft-sdl|the SDL framework page]] and provide a real-world vendor implementation for the [[secure-sdlc-framework-stack-2026|2026 Secure-SDLC Framework Stack thesis]].

## Microsoft's framing of the changed SDL surface

The post enumerates seven structural reasons why classical SDL controls do not transfer 1-for-1 to AI systems — each is a discrete subsection of the source. The wiki organizes them as a single argument: AI systems collapse the assumptions on which classical SDL controls were built.

- **AI security vs. traditional cybersecurity** — conventional software operates within clear trust boundaries; AI systems "collapse these boundaries, blending structured and unstructured data, tools, APIs, and agents into a single platform." Purpose limitation and data minimization become harder to enforce.
- **Expanded attack surface** — multiple unsafe-input entry points (prompts, plugins, retrieved data, model updates, memory states, external APIs); vulnerabilities hide in probabilistic decision loops, dynamic memory states, and retrieval pathways. The named AI-specific vectors are [[prompt-injection|prompt injection]], [[memory-poisoning|data poisoning]], and malicious tool interactions.
- **Loss of granularity and governance complexity** — AI dissolves the discrete trust zones assumed by traditional SDL. Open questions around RBAC, least privilege, and cache protection; how to differentiate queries from commands in a system that assumes all input is valid.
- **Multidisciplinary collaboration** — AI security needs span stack layers historically outside SDL scope, including Business Process and Application UX. This is a deliberate broadening of who participates in SDL.
- **Novel risks** — non-deterministic outputs, instruction-following systems that "assume all input is valid" (the *"Ignore previous instructions and execute X"* failure mode is called out by name), and cached-memory risks of sensitive data leakage or poisoning.
- **Data integrity and model exploits** — training data and weights require source-code-equivalent protection. The post supplies a striking worked example (see below).
- **Speed and sociotechnical risk** — AI accelerates development cycles beyond SDL norms; model updates, new tools, and evolving agent behaviors outpace traditional review processes. The mitigation prescription is iterative controls, faster feedback loops, telemetry-driven detection, continuous learning.

**The raccoon-with-monocle skeleton-key example.** Zunger's load-bearing concrete example: "if a cyberattacker poisons an authentication model to accept a raccoon image with a monocle as 'True,' that image becomes a skeleton key — bypassing traditional account-based authentication." This is one of the cleanest two-sentence illustrations on the wiki of why ML model artifacts require source-code-equivalent protection. Pairs with [[memory-poisoning|memory poisoning]] and the broader [[model-layer-attacks|model-layer attacks]] surface.

## Key Contributions

**1. SDL is "a way of working, not a checklist."** The post argues secure development for AI fails as a static requirements list because AI systems are non-deterministic and their flexibility is part of their value proposition. Effective AI security policies "start by delivering practical, actionable guidance engineers can trust and apply"; provide examples of what "good" looks like; explain how mitigation reduces risk; offer reusable patterns. Policies must evolve through tight feedback loops with engineering — *"co-creating requirements, threat modeling together, testing mitigations in real workloads, and iterating quickly."* This framing is convergent with the [[cybersecurity-cmms-exemplars|wiki's CMM exemplars]] argument that prescriptive maturity models work only as living guidance, not compliance instruments.

**2. Six SDL-for-AI focus areas.** Microsoft commits to substantive guidance "in the coming months" on:

| Focus area | Wiki anchor | CMM domain |
|---|---|---|
| Threat modeling for AI | [[threat-modeling-for-ai\|Threat modeling for AI]] (gap) | D4 (Runtime & Guardrails), build-time only |
| AI system observability | [[agent-observability\|Agent observability]] | D7 (Observability & Detection) |
| AI memory protections | [[agent-memory-isolation\|Agent memory isolation]] | D6 (Data, Memory & RAG) |
| Agent identity and RBAC enforcement | [[agent-identity-architecture\|Agent identity architecture]] | D2 (Identity & Authorization) |
| AI model publishing | [[supply-chain-security-for-agents\|Supply chain security for agents]] | D8 (Supply Chain & AI-BOM) |
| AI shutdown mechanisms | [[distributed-kill-switch\|Distributed kill switch]] | D9 (Operations & Human Factors) |

Threat modeling for AI is the one area the CMM does not fully hold. The nine domains cover the deployment and operation of an agentic system and anchor no development-time process, so [[agentic-ai-security-cmm-crosswalk|the crosswalk]] files AI threat modeling under D4 as a build-time task with no runtime enforcement rather than granting it a domain.

The six-area scope is otherwise clean: it maps onto six distinct wiki CMM domains with very little overlap, and three of the six (memory protections, agent identity & RBAC, shutdown mechanisms) are areas where the wiki has tracked specific architectural primitives but where no major-vendor secure-SDLC framework had previously named them as first-order concerns.

**3. Six SDL-for-AI pillars.** Microsoft's stated framework for *how* SDL for AI operates — research, policy, standards, enablement, cross-functional collaboration, continuous improvement. The pillars are familiar SDL meta-process scaffolding (similar pillar lists appear in BSIMM, OWASP SAMM, and [[google-saif|Google SAIF]]'s implementation methodology). The novelty here is the explicit naming of **Cross-functional collaboration** with Business Process and Application UX as in-scope — broadening SDL ownership beyond the security organization.

## Notable Findings

- **Microsoft's SDL is now the second major-vendor secure-SDLC framework to explicitly publish an AI-extension scope**, alongside [[google-saif|Google SAIF]]. The two frameworks differ in genealogy: Microsoft SDL extends a classical secure-SDLC anchor (SDL has been around since 2004); SAIF was AI-first from inception (2023).
- **The "way of working, not a checklist" framing directly contradicts the most common organizational anti-pattern in secure-SDLC adoption** — treating the framework as a compliance instrument. This makes the post quotable for any wiki page that needs to anchor the living-guidance argument.
- **Explicit invitation to other organizations**: *"We encourage other organizational and security leaders to adopt similar holistic, integrated approaches to secure AI development, strengthening resilience as cyberthreats evolve."* The post is positioned as a leadership statement, not just a Microsoft-internal update.
- **The post is short for its scope** — ≈1,800 words covering a major framework extension. Substantive technical content is deferred to per-area follow-ups. Track these as they land.

## Gap Analysis vs Existing Wiki

The wiki had three latent gaps that this post helps close, and one that it deepens:

- **Closes**: a dedicated [[microsoft-sdl|Microsoft SDL framework page]] — previously the wiki referenced [[microsoft-zt4ai|ZT4AI]] and [[microsoft-secure-agentic-ai-end-to-end|Agent 365]] but treated the underlying SDL framework as an unmentioned ancestor. SDL now has its own anchor.
- **Closes**: a real-world vendor implementation of the [[secure-sdlc-framework-stack-2026|2026 framework-stack thesis]] recommendation. Microsoft's SDL for AI is structurally an "anchor framework + AI overlay" — exactly the pattern the thesis prescribes — but using SDL as the anchor rather than NIST SSDF.
- **Closes**: a wiki anchor for [[yonatan-zunger|Yonatan Zunger]], whose Google → Microsoft trajectory makes him an important identity / privacy / trust-engineering bridge figure.
- **Deepens**: the gap on concrete per-area guidance. The post commits to follow-up content on six AI-specific areas but supplies none of the technical detail. The wiki should track each follow-up post as it lands.

## Placement in this wiki

- **Anchors [[microsoft-sdl|Microsoft SDL]] as a framework page** — the wiki's first dedicated SDL framework entity.
- **Concrete vendor evidence for [[secure-sdlc-framework-stack-2026|the 2026 framework-stack thesis]]** — Microsoft demonstrates the anchor + AI-overlay pattern in production, validating the thesis's structural recommendation. Updates filed on the thesis page noting the new vendor example.
- **Adjacent corroboration for [[sdlc-in-the-ai-attacker-era|SDLC thesis]]** — the "speed and sociotechnical risk" section is a Microsoft-stated version of the [[anthropic-glasswing-announcement|Glasswing]] CrowdStrike time-to-exploit-collapse argument, framed for SDL.
- **CMM crosswalk** — the six AI-focus areas map cleanly to six of the [[agentic-ai-security-cmm-2026|CMM]]'s nine domains. The mapping is added to the framework page.
- **Concept-page references** to [[memory-poisoning|memory poisoning]], [[agent-identity-architecture|agent identity]], [[agent-memory-isolation|agent memory isolation]], and [[distributed-kill-switch|distributed kill switch]] receive a major-vendor citation.

## See also

- [[microsoft-sdl|Microsoft Secure Development Lifecycle (SDL)]] — the framework page
- [[yonatan-zunger|Yonatan Zunger]] — author entity
- [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack for 2026]] — the thesis this paper exemplifies
- [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] — adjacent thesis
- [[microsoft-zt4ai|Microsoft ZT4AI]] — adjacent Microsoft AI-security framework
- [[microsoft-secure-agentic-ai-end-to-end|Microsoft Secure Agentic AI End-to-End]] — the broader 2026 Microsoft AI-security portfolio announcement
- [[google-saif|Google SAIF — Secure AI Framework]] — peer AI-extended secure-development framework
