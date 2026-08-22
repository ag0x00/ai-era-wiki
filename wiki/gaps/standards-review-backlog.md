---
type: gap-analysis
title: "Standards Review Backlog"
created: 2026-05-27
address: c-000311
updated: 2026-07-30
tags:
  - gaps
  - standards-review
  - backlog
  - validation
status: developing
scope_axis:
  - sec-of-ai
origin: produced
tracker_migrated_to: "GitHub milestone: Standards Reviews (#2)"
methodology: "[[standards-validation-methodology-2026-05]]"
related:
  - "[[standards-validation-methodology-2026-05]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[secure-sdlc-framework-stack-2026]]"
---

# Standards Review Backlog

> [!info] Live tracking has moved to GitHub Issues
> The live status board is now the **Standards Reviews** milestone on GitHub (`agpwc/ai-era`, milestone #2): one issue per pending standard, tiered with `p1`/`p2`/`p3` labels, closed as each review lands. Update the milestone, not this page. This page is retained as a reference snapshot and for the durable process notes below (how a review retires the first-pass validation).

This board tracks the per-standard deep dives defined by [[standards-validation-methodology-2026-05|the Standards Validation Methodology]]. The methodology fixes the process (source primary docs, build a clause-level coverage matrix, state falsifiable absence claims, run an adversarial second pass) and the priority tiers. Each completed review lands at `wiki/reviews/standards-review-<standard>-YYYY-Qn.md` and becomes the load-bearing evidence for any gap claim the wiki makes against that standard.

The wiki anchors gap claims against 11 priority standards. **All 11 are now reviewed** (Standards Reviews milestone closed 2026-06-22). The four 2026-Q2 first-wave reviews ([[standards-review-microsoft-zt4ai-2026-Q2|Microsoft ZT4AI]], [[standards-review-mitre-atlas-2026-Q2|MITRE ATLAS]], the [[standards-review-nist-ai-rmf-2026-Q2|NIST AI RMF stack (100-1 / 600-1 / 800-4)]], and the [[standards-review-owasp-agentic-aivss-2026-Q2|OWASP ASI Top 10 + AIVSS pair]]) were joined by the seven that close the milestone:

- [[standards-review-csa-maestro-atf-2026-Q2|CSA MAESTRO + ATF]]: seven-layer threat model plus the Agentic Trust Framework's five elements, four maturity levels, and five promotion gates (corrected from fabricated gate names).
- [[standards-review-eu-ai-act-2026-Q2|EU AI Act incl. Annex IV]]: risk-management and conformity-assessment law; Art. 15 cybersecurity is outcome-stated; the Annex IV item-map mislabels were corrected.
- [[standards-review-iso-42001-27090-2026-Q2|ISO/IEC 42001 + 27090 FDIS]]: citation-only (paywall-bounded per methodology §8); the non-public Annex A numbering was corrected to the A.2 to A.10 objective scheme.
- [[standards-review-owasp-llm-top-10-2026-Q2|OWASP LLM Top 10 (2025)]]: application-level awareness list; two taxonomy-drift errors were fixed (LLM07 vs LLM08, stale LLM03).
- [[standards-review-nist-sp-800-218a-2026-Q2|NIST SP 800-218A]]: development-time SSDF AI profile; the "three vs six net-new tasks" miscount was corrected.
- [[standards-review-saif-cosai-2026-Q2|Google SAIF + CoSAI]]: a broad-but-shallow control vocabulary alongside deep-but-discontinuous workstream outputs; CoSAI deliverable dates were reconciled.
- [[standards-review-microsoft-rai-agent-365-2026-Q2|Microsoft RAI + Agent 365]]: a responsible-AI goals standard (17 goals) plus the Agent 365 management plane, separated from the ZT4AI control catalogue.

Each review replaces wiki-summary-level gap claims against its standard with bounded, clause-cited absence claims. Before a standard was reviewed, its gap claims rested on the [[agentic-cmm-vs-standards-validation|2026-04-30 first-pass validation]], a wiki-summary-level snapshot rather than a clause-level audit.

## Open: question-scoped reviews

The reviews above are each scoped to one standard. A review may instead be scoped to a **question asked across several standards**, when a wiki claim depends on the answer and no single-standard review settles it.

- **Does any SDLC framework govern the coding agent as an actor?** (issue #171, open). [[secure-sdlc-framework-stack-2026|The framework-stack thesis]] §Gap 4 holds that no layer of the recommended stack governs the coding agent itself — the agent holding repository credentials and executing commands, as distinct from the software it produces or the AI product being shipped. That claim rests on each instrument's stated scope, not on a clause-level pass, and it is load-bearing: it justifies the stack's seventh row and the scope-boundary notes on [[nist-ssdf|SSDF]], [[nist-sp-800-218a|SP 800-218A]], and [[microsoft-sdl|Microsoft SDL]]. Those are absence claims, which [[standards-validation-methodology-2026-05|the methodology]] requires to be falsifiable and clause-cited. Until the review lands, Gap 4 is an open position rather than a finding.

## Retiring first-pass validation

The [[agentic-cmm-vs-standards-validation|2026-04-30 validation page]] is a frozen snapshot superseded section by section as reviews land. When a standard's review completes, its GitHub issue closes, the review page is filed under `wiki/reviews/`, and the corresponding verdict in the old validation §2 gains a correction marker pointing to the new review. The old page is archived in full only once most P1 reviews exist.

## See also

- [[standards-validation-methodology-2026-05|Standards Validation Methodology]] — the process and priority tiers this board tracks.
- [[agentic-cmm-vs-standards-validation|2026-04-30 Validation (superseded)]] — the first-pass snapshot being retired.
- [[agentic-ai-security-cmm-crosswalk|Agentic AI Security CMM — Standards Crosswalk]] — the anchor-level CMM-domain × standard map that clause-level reviews deepen.
