---
type: concept
title: "Vibe Coding"
address: c-000041
created: 2026-05-13
updated: 2026-08-22
tags:
  - concepts
  - agentic-coding
  - karpathy
  - llm-coding
  - terminology
status: developing
scope_axis:
  - sec-of-ai
source_url: "https://x.com/karpathy/status/1886192184808149383"
aliases:
  - "Vibe coding"
  - "Vibe coder"
related:
  - "[[pwc-agentic-sdlc-in-practice]]"
  - "[[pwc-stage-coverage-tiers]]"
  - "[[collaboration-paradox]]"
  - "[[anthropic-2026-agentic-coding-trends]]"
  - "[[owasp-state-of-agentic-ai-security-governance]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[microsoft-cli-coding-agent-adoption-study]]"
  - "[[gartner-mq-enterprise-ai-coding-agents-2026]]"
  - "[[injecting-security-context-vibe-coding-talk|Injecting Security Context During Vibe Coding]]"
sources:
  - ".raw/papers/pwc-future-of-solutions-dev-gen-ai-2026.pdf"
---

# Vibe Coding

**Vibe coding** is an informal term for **generating or modifying code by describing the "vibe" or high-level intent in natural language**, relying on LLM inference rather than exact specifications. The term was **coined by [[andrej-karpathy|Andrej Karpathy]]** (computer scientist, OpenAI co-founder, ex-Tesla AI director) in a **February 2025 post on X**, describing the idea of "fully giving in to the vibes" when using AI to generate and run code for quick, throw-away projects. The term is now widely adopted across the AI development community and is formally cataloged in advisory thought-leadership including [[pwc-agentic-sdlc-in-practice|PwC's 2026 Agentic SDLC report]].

> [!gap] Andrej Karpathy entity page
> Karpathy is the originator of this term and the broader "Software 2.0" framing. He has multiple wiki-relevant publications and roles (OpenAI co-founder, Tesla AI, "From Models to Agents" talks). A dedicated wiki entity page is a gap; ingest candidate.

## Definition

Per [[pwc-agentic-sdlc-in-practice|PwC's 2026 Agentic SDLC report]] (which formalizes the term in advisory context):

> "Informal term for generating or modifying code by describing the 'vibe' or high-level intent in natural language, relying on LLM inference rather than exact specifications, this term is widely adopted in the AI space and supported with many community members worldwide."

Karpathy's original framing emphasized **throw-away projects** — quick prototypes where the developer doesn't need to fully understand or maintain the generated code. The term has since broadened to cover any natural-language-driven code generation where the operator iterates on intent rather than implementation.

## Distinguishing Features

What separates "vibe coding" from generic AI-assisted coding:

1. **Intent over specification**. The operator describes what the code should *do* in natural language, not how it should be structured.
2. **LLM inference fills gaps**. Ambiguity in the prompt is resolved by the model based on training-data norms, not by explicit operator decisions.
3. **Iterative refinement via vibes**. The operator runs the code, sees the result, and prompts adjustments based on observed behavior rather than reviewing the implementation.
4. **Often disposable**. The original framing assumes the artifact is throw-away; sustained-maintenance use is a separate (and more contested) mode.

PwC names **"Vibe-coder"** as one of the *new / growth roles* in the agentic SDLC era (defined: *"Define requirements, plan, build, test, and review results through natural language either via keyboard or voice commands"*) — sitting alongside Prompt & LLM Engineer, Context Engineer, AI test-orchestration lead, xOps AI Analyst, AIOps Engineer, and AI governance & risk lead.

## Tension With the Collaboration Paradox

Vibe coding's claim of "fully giving in to the vibes" sits in tension with [[collaboration-paradox|the collaboration paradox]] (60% of developer work uses AI; only 0-20% is "fully delegated"). Two interpretations:

1. **Vibe coding is bounded to the 0-20% band**. Karpathy's original framing applies to throw-away projects where verification cost is low; the practitioner data shows that production work falls in the active-collaboration band even for AI-fluent users.
2. **Vibe coding is the operating mode for the 0-20% that gets fully delegated**. Under this reading, vibe coding *is* the productized form of full delegation, but it remains a minority of work.

Both interpretations are consistent with the data. The wiki's position: **vibe coding is a real and productively-used mode for some classes of work (prototyping, exploratory analysis, scripting), but cannot be the default mode for high-stakes production work where the [[plan-validate-execute|Plan-Validate-Execute]] pattern applies.** See also [[anthropic-2026-agentic-coding-trends|Anthropic's Trends Report]] Trend 4 ("Human oversight scales through intelligent collaboration") for the convergent vendor-strategic framing.

## Security Implications

Vibe-coded artifacts inherit specific risk patterns:

- **Vulnerability density**: LLM-generated code from underspecified prompts tends to fall back on training-data norms, including common-but-insecure patterns. METR 2025 RCT findings (experienced devs 19% slower with AI tools) suggest the verification cost of vibe-coded artifacts is non-trivial; see [[metr-rct-2025|METR 2025 RCT]].
- **Cognitive file integrity exposure**: vibe-coded changes to identity files (system prompts, `SOUL.md`, `IDENTITY.md`) can introduce subtle behavioral shifts that the operator doesn't notice. See [[cognitive-file-integrity|Cognitive File Integrity]] for the defensive control.
- **Coding-agent governance**: applying vibe coding through coding agents (Cursor, Claude Code, Copilot) bypasses traditional code-review chokepoints. See [[ai-coding-agent-governance|Knostic's AI Coding Agent Governance]] framework for the operational response.
- **Supply-chain exposure**: vibe-coded artifacts often pull in dependencies the operator hasn't vetted. See [[supply-chain-security-for-agents|Supply Chain Security for Agents]] for the AI-BOM perspective.

One practitioner response treats the first of these as a context problem rather than a review problem. [[injecting-security-context-vibe-coding-talk|Srajan Gupta's Unprompted talk]] argues that an underspecified prompt produces insecure code because the requirements that would have constrained it — data classification rules, internal standards, the trust boundaries in the design document — exist in the organization but never reach the model, and that retrieving them into the prompt before generation is cheaper than finding their absence in review. The position is worth recording for what it concedes as much as for what it claims: Gupta reports no measurement of residual defects, and states the pattern leaves threat modeling for new systems and human review of large changes untouched.

[[owasp-state-of-agentic-ai-security-governance|OWASP's State of Agentic AI Security and Governance]] pairs vibe coding with [[shadow-ai|shadow AI]] as the two named drivers of the agentic governance gap: code that bypasses review and AI use outside any sanctioned policy both expand the surface faster than oversight can cover it. The report frames the underlying constraint as human oversight at machine speed — when an agent acts far faster than a reviewer can evaluate, sampled review covers only a fraction of decisions, so the governance answer is continuous, policy-enforced oversight rather than periodic inspection.

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-d8-supply-chain|CMM D8 (Supply Chain & AI-BOM)]] L3+** — vibe-coded artifacts inherit dependency-graph risk; D8 controls (AI-BOM, dependency scanning) gate this.
- **[[agentic-ai-security-cmm-d9-operations|CMM D9 (Operations & Human Factors)]]** — vibe coding sits on a spectrum from disposable prototypes to production deployment; D9 controls (code review, change management) govern the transition.

## Provenance Note

The Karpathy X post (February 2025) is the canonical origin citation. PwC's 2026 report is the canonical advisory citation. As of mid-2026, "vibe coding" appears in:

- Multiple Anthropic, OpenAI, and Google product materials (as a usage mode for code-assistant tools).
- Job postings for "Vibe-coder" or "Vibe-aware engineer" roles (per PwC's external-signals analysis).
- The Forbes "fastest tech-history adoption" framing referenced in PwC's adoption-curve section.

## See Also

- [[andrej-karpathy|Andrej Karpathy]] — originator (entity stub, gap).
- [[pwc-agentic-sdlc-in-practice|PwC Agentic SDLC paper]] — formal-advisory citation.
- [[collaboration-paradox|Collaboration Paradox]] — tension with full-delegation framing.
- [[plan-validate-execute|Plan-Validate-Execute]] — canonical wiki HITL pattern that bounds vibe-coding's applicability.
- [[ai-coding-agent-governance|AI Coding Agent Governance]] — operational response framework.
- [[anthropic-2026-agentic-coding-trends|Anthropic 2026 Trends Report]] — adjacent vendor-strategic framing.

<!-- sources:auto -->
## Sources

- [Vibe Coding](https://x.com/karpathy/status/1886192184808149383)
<!-- /sources -->
