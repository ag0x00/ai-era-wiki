---
type: gap
title: "Scope Expansion Punch-List"
address: c-000018
created: 2026-05-13
updated: 2026-08-21
tags:
  - gap
  - scope-expansion
  - punch-list
status: developing
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
related:
  - "[[agentic-soc-state-of-the-field]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
sources: []
---

# Scope Expansion Punch-List (2026-05)

Audit-trail page for the wiki scope expansion of 2026-05-13. The wiki broadened from `sec-of-ai` only to a multi-axis `scope_axis:` vocabulary covering the bidirectional intersection of agentic AI and enterprise security (see [[conventions|conventions]] §Scope Axes). This page tracks the five known content holes surfaced during planning, plus the backfill decision for the 309 existing pages.

The expansion adopted six values. Two of them, `ai-vuln-discovery` and `redteam-for-ai`, were retired; the vocabulary is the four values [[conventions|conventions]] §Scope Axes now lists. The Decision Log below carries the retirement and the coverage argument behind it. Axis names in the punch-list items are stated in the current vocabulary.

## Known Holes (P1 — Ingest Candidates)

The following items were identified as the highest-priority gaps under the new scope axes. Each is a candidate for the first 30 days of ingest activity. Order is suggested priority.

### 1. Microsoft Security Copilot (dedicated product page) — **STUB CREATED 2026-05-13**

**Axis:** `ai-in-sec-defense`
**Status:** seed stub at [[microsoft-security-copilot|Microsoft Security Copilot]] (`c-000024`). Page documents the five Microsoft-built role-specialized agents (Security Analyst, Alert Triage, Conditional Access Optimization, Data Security Posture, Data Security Triage) plus the 15-partner Security Store. **Substantive content deferred** to the next ingest — page is a routing address, not a full product write-up.
**Remaining work:** sourced ingest specifically about Security Copilot (RSAC 2026 deeper material, partner-agent catalog detail, independent benchmarks if available); promote from `seed` to `developing`.

### 2. Google Sec-PaLM / SecLM ecosystem

**Axis:** `ai-in-sec-defense`
**Impact:** [[agentic-soc-state-of-the-field|Agentic SOC state of the field]] currently presents a Microsoft-plus-CrowdStrike axis; Google's defender-side AI offerings are absent. This is the single largest entity-page hole on the defender axis.
**Candidate sources:** Google Cloud Security blog, Sec-PaLM 2 announcement (2023), Gemini for Security Operations (2024+), Mandiant AI integrations.
**Target pages:** `wiki/entities/products/google-sec-palm.md` (or per-product split), reference in [[agentic-soc-state-of-the-field|Agentic SOC]] thesis.

### 3. XBOW — **INGESTED 2026-05-13**

**Axis:** `ai-in-sec-offense`, `sec-against-ai`
**Status:** developing page at [[xbow|XBOW]] (`c-000026`). Source: [[xbow-mythos-evaluation|XBOW's Mythos Evaluation (May 2026)]] (`c-000025`). Companion entity [[mythos|Mythos]] (`c-000027`) created as part of same ingest — preview-stage Anthropic frontier model that XBOW evaluated. Cross-referenced from [[offensive-ai-state-of-the-field|Offensive AI thesis]] and [[frontier-ai-for-vuln-discovery|Frontier AI thesis]] (which was promoted from `seed` to `developing` by this ingest).
**Remaining work:** founders/leadership team; funding stage; customer case studies; AISI / Point Estimate independent benchmark sourcing; XBOW disclosure-pipeline practices.

### 4. Prophet AI

**Axis:** `ai-in-sec-defense` (and possibly `ai-in-sec-offense`)
**Impact:** Prophet AI sits at the SOC-automation/agentic-detection frontier; need to characterize whether it is purely defender-side or has offensive-adjacent capability.
**Candidate sources:** Prophet AI homepage, customer announcements, funding rounds, comparative coverage in [[microsoft-secure-agentic-ai-end-to-end|Microsoft Secure Agentic AI]] partner discussions.
**Target pages:** `wiki/entities/products/prophet-ai.md`.

### 5. Autonomous-pentest tooling category

**Axis:** `ai-in-sec-offense`
**Impact:** Beyond XBOW, a comparative landscape page would anchor [[offensive-ai-state-of-the-field|Offensive AI state of the field]]. Candidates include Dropzone (SOC framing but offensive-adjacent), Horizon3 NodeZero (classical autonomous pentest with AI augmentation), Tarian.
**Candidate sources:** Vendor materials, Gartner/Forrester coverage, [[red-teaming-capability-framework|Red Teaming Capability Framework]] tier 5 vendor evaluation criteria.
**Target pages:** Comparison page `wiki/comparisons/autonomous-pentest-tooling-2026.md`; per-vendor entity pages.

### 6. AI-Attacker SDLC threat model

**Axis:** `sec-against-ai`
**Impact:** [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] thesis flagged the lack of explicit SLSA/SSDF/CSAF revision for AI-augmented adversaries. A concept page documenting the *threat model* (separate from any specific framework's response) would let the thesis cite a structured artifact instead of arguing in prose.
**Candidate sources:** Recent CISA/NCSC guidance on AI-augmented threats, academic threat modeling papers, vendor (Anthropic, OpenAI, Microsoft) responsible-disclosure posts about model-assisted attacks.
**Target pages:** `wiki/concepts/ai-augmented-attacker-threat-model.md`.

## Lazy-Backfill Decision

The 309 existing pages did not carry `scope_axis:` frontmatter at the time of the expansion. The decision was **lazy-on-touch**: the field is added the next time a page is touched (edited, lint-swept, re-ingested). No bulk-rewrite pass was performed because it would re-touch `updated:` on 309 pages and pollute the log. The original form of the rule defaulted an untagged page to `[sec-of-ai]`; [[conventions|conventions]] §Scope Axes replaced that with assignment from page content on touch, so absence of the field is not a defect and no lint flags it.

The five highest-signal pages were explicitly backfilled as part of the 2026-05-13 expansion:

- [[microsoft-secure-agentic-ai-end-to-end|Microsoft Secure Agentic AI End-to-End]] → `[sec-of-ai, ai-in-sec-defense]`
- [[crowdstrike|CrowdStrike]] → `[sec-of-ai, ai-in-sec-defense]`
- [[agent-commander-prompt-c2|Agent Commander]] → `[sec-of-ai, ai-in-sec-offense]`
- [[general-analysis|General Analysis]] → `[sec-of-ai, ai-in-sec-offense]` (recorded at the time as `[redteam-for-ai, ai-in-sec-offense]`)
- [[claude-stripe-coupons-imessage-injection|Claude+Stripe Coupons]] → `[sec-of-ai, ai-in-sec-offense]`

Note: a dedicated `wiki/entities/products/microsoft-security-copilot.md` page does not yet exist; Security Copilot is currently referenced only inside the [[microsoft-secure-agentic-ai-end-to-end|Microsoft Secure Agentic AI paper page]]. Creating a dedicated product page is one of the first ingest candidates for the `ai-in-sec-defense` axis (added below).

The scope lint contemplated here shipped on 2026-08-21 as `scripts/lint-scope-axis-vocabulary.py`, in `lint-all.sh` and the pre-push blocking subset. It checks set membership against the four values and stays silent on a page carrying no `scope_axis:` at all.

## Decision Log

- **2026-08-21** — Vocabulary closed at four values; `ai-vuln-discovery` and `redteam-for-ai` retired. The convention admits a fifth value only against a justification that no combination of the four covers the case, and for these two the combinations exist. AI-assisted vulnerability discovery is `ai-in-sec-defense` when a defender runs it; the axis definition names defensive AI vulnerability discovery outright. It is `ai-in-sec-offense` when an attacker runs it, taking `sec-against-ai` as the mandatory counterpart. A page covering both sides, such as [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]], carries all three. Red-teaming an AI system is `sec-of-ai`, which the axis definition states covers red-teaming and pentesting of AI applications, plus `ai-in-sec-offense` where the red team is itself an AI agent. Neither retired value named a case the four cannot express; each named a topic, and a topic is what `tags:` are for. The 21 pages on `ai-vuln-discovery` and 3 on `redteam-for-ai` were re-tagged from their own content on the same date, and `scripts/lint-scope-axis-vocabulary.py` now rejects a value outside the four at pre-push.
- **2026-05-13 (evening)** — XBOW + Mythos ingest. Source: [XBOW blog post](https://xbow.com/blog/mythos-offensive-security-xbow-evaluation) (May 12, 2026). Three new pages: [[xbow-mythos-evaluation|paper]] (`c-000025`), [[xbow|XBOW org]] (`c-000026`), [[mythos|Mythos product]] (`c-000027`). Closes punch-list item 3 (XBOW). [[frontier-ai-for-vuln-discovery|Frontier AI thesis]] promoted from `seed` to `developing` — its first sourced anchor. [[offensive-ai-state-of-the-field|Offensive AI thesis]] XBOW gap callout closed.
- **2026-05-13 (afternoon)** — Lint pass after scope expansion. Six DragonScale addresses allocated (`c-000018` through `c-000023`) and added to the five thesis seeds and this gap page. Wikilink typo (`agentic-ai-security-reference-architecture-2026` → `...-architecture`) fixed in `overview.md` and the agentic-soc thesis. Microsoft Security Copilot stub created (item 1 above, `c-000024`). Lint report at [[lint-report-2026-05-13|Lint Report 2026-05-13]].
- **2026-05-13 (morning)** — Scope expansion approved. Added `wiki/offensive/` folder. Adopted a six-value closed `scope_axis:` vocabulary, since reduced to four. Built five thesis seeds (no new RAs or CMMs). Chose lazy-backfill over bulk-rewrite. Plan archived at `/home/admin_user/.claude/plans/i-want-to-expand-calm-aurora.md`.
