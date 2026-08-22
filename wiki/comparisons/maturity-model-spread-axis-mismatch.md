---
type: comparison
title: "Maturity Model Spread and Axis Mismatch"
created: 2026-05-04
updated: 2026-06-22
tags:
  - comparisons
  - maturity-models
  - ai-governance
  - epistemology
  - scoping-analysis
status: developing
scope_axis:
  - sec-of-ai
related:
  - "[[cybersecurity-cmms-exemplars]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[microsoft-rai]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[owasp-aivss]]"
  - "[[anthropic]]"
sources:
  - "https://www.pwc.com/us/en/services/consulting/library/responsible-ai-maturity.html"
  - "https://www.microsoft.com/en-us/ai/responsible-ai"
  - "https://www.anthropic.com/news/responsible-scaling-policy"
  - "https://owasp.org/www-project-samm/"
---

# Maturity Model Spread and Axis Mismatch

## Purpose of this page

A scoping analysis for the comparison candidate *"Maturity model spread (PwC, Microsoft, Anthropic, OWASP) — same axes?"* parked in [[wiki/comparisons/_index|Comparisons Index]] §More candidates. The headline finding: the four artifacts measure fundamentally different things, so a head-to-head comparison would be misleading without an explicit category-aware framing. The page documents the mismatch so a future attempt at the comparison starts from the right scoping rather than re-deriving it.

The candidate stays parked; this page records why the four artifacts are not commensurable.

## The four artifacts and what each measures

| Artifact | Type | Unit of measurement | Coverage in this wiki |
|---|---|---|---|
| **PwC AI Maturity / Responsible AI Maturity** | Vendor consultancy maturity ladder | Organizational AI program maturity (governance + ops + tech) | None — passing mention only |
| **Microsoft Responsible AI Standard (RAI)** | Principle-adoption framework with internal Crawl / Walk / Run maturity | Adoption maturity for fairness, accountability, transparency, privacy, inclusiveness, reliability | [[microsoft-rai\|Microsoft Responsible AI Standard (RAI)]] exists |
| **Anthropic Responsible Scaling Policy (RSP) / AI Safety Levels (ASL)** | AI safety policy with ASL-1 → ASL-5 tiers | **The model's** dangerous-capability level — not the org's maturity | Not yet a wiki page (gap) |
| **OWASP** (multiple artifacts; *no single AI CMM*) | Threat / control catalogs | Threat coverage, not maturity. SAMM is general software-security MM; Agentic AI Top 10 / AIVSS / LLM Top 10 are threat catalogs | [[owasp-agentic-ai-top-10\|OWASP ASI Top 10]], [[owasp-aivss\|OWASP AIVSS]], [[owasp-llm-top-10\|OWASP LLM Top 10]] exist; no SAMM page |

## The headline finding: three kinds of artifact

The four don't share an axis. They split into three categorical groups:

| Group | What it measures | Members |
|---|---|---|
| **Organizational program maturity** | What the org does — its governance, ops, controls, lifecycle | PwC, Microsoft RAI |
| **Model-capability risk tier** | What the model is — dangerous-capability level of a specific frontier system | Anthropic RSP / ASL |
| **Threat / control coverage** | What's covered — threat enumeration and control existence, but not maturity per se | OWASP (Top 10s, AIVSS, SAMM) |

Forcing them onto a single axis would be misleading. PwC and Microsoft RAI compete on the org-maturity axis. Anthropic ASL is a model-capability metric — it does not measure organizations. OWASP is a control-catalog companion to all of the above; it does not measure maturity.

One precision the [[standards-review-microsoft-rai-agent-365-2026-Q2|2026-Q2 RAI / Agent 365 review]] adds: the [[microsoft-rai|RAI Standard]] is itself a responsible-AI **goals** standard — seventeen goals across six principles — not a maturity model and not a control catalogue. It belongs in the org-maturity group here only because its goals are adopted through an internal Crawl / Walk / Run progression; the goals themselves are outcome statements, and the technical controls live in [[microsoft-zt4ai|ZT4AI]] with the management plane in [[microsoft-entra-agent-id|Agent 365]].

## Shape of a category-aware revival

A useful comparison page would do four things rather than fake commensurability:

1. **Establish the axis-mismatch upfront.** The table above (or its successor) is the headline, not buried.
2. **Compare PwC vs Microsoft RAI head-to-head** on the organizational-program-maturity axis where they genuinely overlap.
3. **Position Anthropic RSP / ASL as adjacent-not-overlapping.** ASL informs an org's program but is not itself an org-program maturity model.
4. **Position OWASP as a control-catalog companion to all three.** Use OWASP to populate the threat-coverage column inside whichever maturity model the org adopts.
5. **Close with how the wiki's [[agentic-ai-security-cmm-2026\|CMM]] relates to each.** Org-maturity-axis overlap with PwC and Microsoft RAI; reuses OWASP threat IDs at L3+; does not try to be ASL.

## Basis for parking it

Three reasons the comparison was not pursued in the session that produced this analysis:

1. **Authoritative versions drift.** PwC and Microsoft RAI both publish frequently and revise doctrines. A snapshot is research-grade only as a Q-dated snapshot, and the wiki avoids "pretend authoritative across versions" framing.
2. **Collateral pages required.** The comparison would need at least an Anthropic RSP / ASL page, a thin PwC page, and ideally an OWASP SAMM page to be properly cross-linked. ~3 stubs of preparatory work before the comparison can be written.
3. **The category-mismatch finding is the actual interesting content.** Once that finding is established, the head-to-head detail (PwC vs Microsoft RAI on org maturity) is comparatively routine consultancy review work — useful but not high-leverage for the wiki's current focus.

The candidate is worth reviving when one or more of the following becomes true: (a) the wiki adds a [[anthropic\|Anthropic]] RSP / ASL page for an unrelated reason; (b) Microsoft RAI publishes a major revision worth tracking; (c) a reader specifically needs the PwC vs Microsoft head-to-head for a procurement or assessment decision.

## Exclusions from the comparison

- **A single-number "best maturity model"**. The four don't compete because they measure different things. Picking "the best" is incoherent. An organization adopts the right tool for its question — org-maturity self-assessment (PwC, Microsoft RAI), model-capability-tier discipline (ASL), or threat coverage (OWASP).
- **A direct cross-mapping into the wiki's [[agentic-ai-security-cmm-2026\|CMM]]**. The CMM overlaps with PwC / Microsoft RAI on the org-maturity axis but is **not** a competitor to ASL or to OWASP catalogs. Mapping CMM-vs-ASL would repeat the category mistake. Use the CMM's [[agentic-ai-security-cmm-crosswalk\|crosswalk matrix]] for control mapping; ASL is a separate discipline.

## Relations

- Parent index entry: [[wiki/comparisons/_index|Comparisons Index]] §More candidates (the candidate stays parked there)
- Adjacent comparison: [[cybersecurity-cmms-exemplars|Cybersecurity CMM Exemplars and Design Lessons]] (CMMI / BSIMM / SAMM / CMMC / NIST CSF 2.0 — all org-maturity, share an axis)
- Wiki's own CMM: [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — sits in the org-maturity group; relates to PwC and Microsoft RAI by axis, not to ASL or OWASP catalogs
- Methodology relevance: [[standards-validation-methodology-2026-05|Standards Validation Methodology]] §10 explicitly excludes maturity-model peers (PwC, Microsoft RAI Maturity, Anthropic RSP) from the standards-validation scope on the same categorical grounds — they are *peers* of the wiki's CMM, not authorities
