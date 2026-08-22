---
type: gap-analysis
title: "Standards Validation Methodology"
created: 2026-05-04
updated: 2026-08-21
tags:
  - gaps
  - validation
  - methodology
  - standards
  - epistemology
  - audit
status: developing
origin: produced
scope_axis:
  - sec-of-ai
target: "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[secure-sdlc-framework-stack-2026]]"
  - "[[nist-sp-800-162|NIST SP 800-162: Attribute-Based Access Control]]"
---

# Standards Validation Methodology

This page defines how the wiki validates the [[agentic-ai-security-cmm-2026|CMM]] and [[agentic-ai-security-reference-architecture|Reference Architecture]] against named industry standards, and how it states absence claims ("X doesn't cover Y") with bounded confidence.

It exists because the wiki currently states absence claims more confidently than its underlying methodology supports. Inventory below.

## §1 The structural problem

Three issues compound:

### 1.1 Framework pages are not primary-source-anchored

As of 2026-05-04, the framework pages under `wiki/frameworks/` document standards (NIST AI RMF, ISO 42001, EU AI Act, OWASP Agentic AI Top 10, AIUC-1, CSA MAESTRO, Microsoft RAI/ZT4AI, MITRE ATLAS, Google SAIF / CoSAI, NIST AI 600-1 + 800-218A) but **none currently carry a `sources:` list with a primary-source URL or an `archived_copy:` pointing at the actual standard PDF**. Versions are cited; specific clauses are usually not.

This means any downstream comparison — including the existing [[agentic-cmm-vs-standards-validation|2026-04-30 validation]] — is effectively *CMM vs wiki-summary-of-standards*, not *CMM vs primary-source-of-standards*. Confidence in the claims is bounded by the fidelity of the wiki summaries.

### 1.2 The validation methodology states this explicitly

The existing [[agentic-cmm-vs-standards-validation|validation page §1]] reads: *"Read the wiki summaries for each named standard."* This is the one-hop-removed audit. The author of that page acknowledged the constraint; what's missing is a way to close the gap.

### 1.3 Absence claims are not falsifiable

A grep across the wiki for "not in any standard," "no current standard," "no published standard," "absent in," etc. returns dozens of bold claims. Most read like:

> "Cognitive File Integrity for `SOUL.md` / `IDENTITY.md` / system prompts — not in any standard"

A reader who wanted to refute that claim would have to read every section of every standard the wiki tracks. That's not a finite task; the claim is unfalsifiable as stated. Compare to the falsifiable form:

> "Cognitive File Integrity for `SOUL.md` / `IDENTITY.md` / system prompts. Searched: NIST AI RMF v1.0 (Jan 2023) §3–§5, AI 600-1 (July 2024) §2–§4, AI 800-4 draft (2024) §2.1–§4.3, ISO/IEC 42001:2023 Annex A, OWASP Agentic AI Top 10 (Dec 2025) ASI04, AIUC-1 Q2 2026 §Supply Chain. Search terms: 'system prompt integrity', 'identity file', 'agent rule file', 'cognitive integrity', 'rules-file integrity'. Verdict: not addressed. Refuting evidence: any passage in the searched scope naming integrity controls specifically for agent rules / identity / system-prompt files."

The second form is verifiable in finite time. The first isn't.

## §2 The methodology

A four-step protocol. Per standard, each step is a discrete artifact.

### Step 1 — Source the standard properly

For each framework page in `wiki/frameworks/`, the following frontmatter fields become **required** (lint-enforced — see §6):

```yaml
source_url: "https://nist.gov/itl/ai-risk-management-framework"
primary_documents:
  - title: "NIST AI Risk Management Framework (AI RMF 1.0)"
    url: "https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf"
    version: "1.0"
    published: "2023-01-26"
    retrieved: "YYYY-MM-DD"
    archived_copy: ".raw/papers/nist-ai-rmf-1-0-2023-01-26.pdf"
    scope_in_wiki: "§3 (Foundational Information), §4 (Risk Management), §5 (Outcomes), §6 (RMF Profiles)"
  - title: "NIST AI 600-1 — Generative AI Profile"
    ...
```

`scope_in_wiki` is load-bearing: it states **which sections of the primary document the wiki summary actually reflects**. Sections outside this scope are not authoritative on the wiki and cannot be the basis for absence claims.

`archived_copy` is the local mirror under `.raw/papers/` (or `.raw/articles/` for HTML-only standards). Where the standard is paywalled (ISO/IEC, certain CSA/AIUC materials), `no_public_url` documents the constraint and a citation-only entry is the best the wiki can do.

### Step 2 — Build a clause-level coverage matrix

Per standard, a structured table mapping every CMM domain (D1–D9) to the standard's clauses / sections / controls. The wiki's existing [[agentic-ai-security-cmm-crosswalk|crosswalk]] is anchor-level (e.g. "ISO 42001 Annex A controls"); the clause-level version cites specific control IDs:

| CMM domain | NIST AI RMF clause | NIST AI 600-1 clause | NIST AI 800-4 clause | Notes |
|---|---|---|---|---|
| D1 Governance | Govern §3.1, §4.1 | §2.1 (Govern subcategory) | n/a | RMF Govern function maps cleanly; profile-specific extensions in 600-1 |
| D2 Identity | n/a | n/a | n/a | RMF/600-1/800-4 do not address NHI / agent-identity controls — *absence claim with bounded scope* |
| ... | | | | |

Each cell either has a citation or an explicit `n/a — searched: [scope] / terms: [list] / refuting evidence: [what would change the verdict]`.

### Step 3 — Falsifiable absence claims

Every "X doesn't cover Y" claim in the CMM, RA, or any wiki page must use the structured triplet:

```yaml
- claim: "<concise statement of what's missing>"
  scope_searched:
    - "Standard A vX.Y §1.1–§4.3"
    - "Standard B vZ Annex A controls A.5.1–A.5.38"
  search_terms: ["term1", "term2", "exact phrase"]
  anchored_on: "<the living standard the claim is bounded by>"
  lineage_cited: "<the frozen predecessor cited for vocabulary lineage; omit where none>"
  verdict: "not addressed"
  refuting_evidence: "<what evidence would refute the claim>"
  reviewed: "YYYY-MM-DD"
  reviewer: "Claude (model name) | human reviewer"
```

For prose, the inline form:

> *Not addressed in [[nist-ai-rmf|NIST AI RMF v1.0]] §3–§5 or [[nist-ai-600-1|AI 600-1]] §2–§4 (search terms: 'system prompt', 'identity file', 'cognitive integrity'). Refuting evidence: any passage in scope naming integrity controls for agent rules / identity / system-prompt files. Reviewed 2026-MM-DD.*

The structured form lives in the per-standard review page; the inline form is what appears in the CMM / RA / concept pages. Where the claim spans two standards, the inline form carries the same pair as a trailing clause: *Anchored on NIST SP 800-162; OASIS XACML 3.0 cited for lineage.*

Where two standards define the same vocabulary, the claim anchors on the living one. A verdict of "not addressed" is bounded by the searched document, so anchoring it on a specification that has taken no revision in years states an absence about a frozen text rather than about current practice, and refuting evidence can never arrive. [[nist-sp-800-162|NIST SP 800-162]] is the worked case: it carries the same PEP / PDP / PIP / PAP role split as OASIS XACML 3.0, is reaffirmed and policy-language-agnostic, and is the wiki's anchor for any role-vocabulary claim, with XACML cited for historical lineage. `anchored_on` and `lineage_cited` record that choice on the claim itself, so a later reader can tell an anchoring decision from an oversight:

```yaml
  anchored_on: "NIST SP 800-162 (reaffirmed, policy-language-agnostic)"
  lineage_cited: "OASIS XACML 3.0"
```

Two shapes fall outside the worked case. A claim searching a single standard sets `anchored_on` to that standard and omits `lineage_cited`. A claim searching several standards that define different material — the §1.3 example searches six documents — names every living one of them as an anchor, and cites lineage only for a frozen document it did not search for coverage. Anchoring is decided per vocabulary, so one claim can carry several anchors alongside a single lineage citation.

### Step 4 — Adversarial second pass

After Steps 1–3 are complete for a standard, a second reviewer (a different agent run, or a human) is given:

- The primary documents
- The clause-level matrix
- The falsifiable absence-claim list

The reviewer's only job is **find a counter-example for each absence claim**. Anything that survives the second pass is a documented gap with bounded confidence. Counter-examples produced trigger updates to both the standard's wiki summary and the absence claim.

## §3 The audit backlog

Live per-standard status is tracked on the **Standards Reviews** GitHub milestone (one issue per pending standard); [[standards-review-backlog|the Standards Review Backlog]] page is the reference snapshot, and the priority tiers and effort estimates below are its source.

11 standards currently anchor the wiki's gap claims. Priority order = standards the CMM most aggressively claims gaps against:

| Priority | Standard | Wiki page | Primary source(s) | Estimated effort |
|---|---|---|---|---|
| **P1** | NIST AI RMF 1.0 + AI 600-1 + AI 800-4 | [[nist-ai-rmf\|NIST AI Risk Management Framework (AI RMF)]], [[nist-ai-600-1\|NIST AI 600-1 — Generative AI Profile]] | nvlpubs.nist.gov (free, public) | ~6 hours |
| **P1** | ISO/IEC 42001 + 27090 FDIS | [[iso-iec-42001\|ISO/IEC 42001 — AI Management Systems]] | iso.org (paywalled — citation-only for full spec; Annex A controls available in summaries) | ~5 hours (limited by paywall) |
| **P1** | EU AI Act incl. Annex IV | [[eu-ai-act\|EU AI Act]] | eur-lex.europa.eu (free, public) | ~6 hours |
| **P1** | OWASP Agentic AI Top 10 + AIVSS | [[owasp-agentic-ai-top-10\|OWASP Top 10 for Agentic Applications (ASI Top 10)]], [[owasp-aivss\|OWASP AI Vulnerability Scoring System (AIVSS)]] | owasp.org / genaisecurityproject.com (free, public) | ~4 hours |
| **P2** | AIUC-1 (current quarterly) | [[aiuc-1\|AIUC-1 — AI Agent Certification Standard]] | aiuc.com (registration; quarterly drift) | ~4 hours per quarterly |
| **P2** | MITRE ATLAS v5.4.0+ | [[mitre-atlas\|MITRE ATLAS]] | atlas.mitre.org (free, public) | ~4 hours |
| **P2** | CSA MAESTRO + Agentic Trust Framework | [[csa-maestro\|CSA MAESTRO / CSA Agentic Trust Framework]] | cloudsecurityalliance.org (free, public) | ~4 hours |
| **P3** | Google SAIF / CoSAI | [[google-saif\|Google SAIF — Secure AI Framework]], [[cosai\|CoSAI — Coalition for Secure AI]] | safety.google, coalitionforsecureai.org | ~3 hours |
| **P3** | Microsoft RAI / ZT4AI / Agent 365 | [[microsoft-rai\|Microsoft Responsible AI Standard (RAI)]] | learn.microsoft.com (700+ ZT4AI controls — large) | ~5 hours |
| **P3** | NIST SP 800-218A SSDF AI Profile | [[nist-sp-800-218a\|NIST SP 800-218A — Secure Software Development Framework AI Profile]] | nvlpubs.nist.gov (free, public) | ~3 hours |
| **P3** | OWASP LLM Top 10 (2025) | [[owasp-llm-top-10\|OWASP Top 10 for LLM Applications]] | owasp.org (free, public) | ~3 hours |

Total floor estimate: ~47 hours. Plus ~10 hours for adversarial second-pass coverage of P1+P2.

## §4 What changes for absence claims already in the wiki

Existing absence claims are **not** retroactively required to meet the falsifiability bar — that's the audit backlog's job. But:

1. **The current [[agentic-cmm-vs-standards-validation|2026-04-30 validation page]]** stays in place but is reframed as a **first-pass synthesis** with explicit caveats about its sourcing. Its `status:` flips from `addressed` to `superseded-by-methodology` when this page is adopted, and it gains a banner pointing here.

2. **Existing CMM / RA / concept pages** that make absence claims keep them (no flag-and-pull) but get a structural marker indicating which absence claims have been validated under this methodology and which haven't:
   - Validated claims: cite the per-standard review page (e.g. *"per [[standards-review-nist-ai-rmf-2026-Q3|NIST AI RMF review (2026-Q3)]]"*).
   - Pending claims: marked with `> [!gap] Pending standards-validation backlog (P1/P2/P3)`.

3. **New absence claims** (anything written after this methodology lands) MUST conform to §2 Step 3.

## §5 Per-standard review page format

Each priority entry produces a page at `wiki/reviews/standards-review-<standard>-YYYY-Qn.md` with this structure:

```markdown
---
type: gap-analysis
title: "Standards Review — [Standard Name] [Version], [Quarter]"
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [gaps, standards-review, ...]
status: developing
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[<standard-page>]]"
primary_documents:
  - title: "..."
    url: "..."
    version: "..."
    retrieved: "YYYY-MM-DD"
    archived_copy: ".raw/papers/..."
    scope_in_wiki: "..."
reviewer: "Claude (model name) | human reviewer"
review_started: "YYYY-MM-DD"
review_completed: "YYYY-MM-DD"
adversarial_pass: "pending | scheduled | completed YYYY-MM-DD"
---

# Standards Review — [Standard]

## Primary documents reviewed
[List per Step 1 frontmatter, with archived_copy paths]

## Clause-level coverage matrix (CMM × Standard)
[Per Step 2 — full matrix]

## Falsifiable absence claims found
[Per Step 3 — structured list of every "X doesn't cover Y" claim]

## Scope exclusions
[Sections of the standard outside scope_in_wiki, with rationale]

## Adversarial-pass log (after Step 4)
[Counter-examples found, refuted claims, residual claims]

## Effect on existing wiki pages
[Updates to CMM / RA / concept pages with citations to this review]
```

## §5.1 Definition of Done

A per-standard review is a propagation task, not just a page. The review page is the *evidence*; the deliverable is a wiki that is internally consistent with the reviewed primary source. A review is complete only when **every** item below is *executed* — not merely recommended in the "Effect" section. Items 3–6 are the ones the [[standards-review-owasp-agentic-aivss-2026-Q2|OWASP review]] initially skipped, which is why this section exists.

1. **Review page filed** at `wiki/reviews/standards-review-<standard>-YYYY-Qn.md` with the coverage matrix, falsifiable absence claims, and adversarial-pass log.
2. **Primary sources archived** under `.raw/` and manifested with `hash`; `primary_documents` frontmatter (with `archived_copy` and `scope_in_wiki`) added to the standard's framework page(s) per Step 1.
3. **Taxonomy reconciliation.** Grep the whole wiki for the standard's own identifiers and labels — category IDs (`ASIxx`, `LLMxx:2025`, `AML.Txxxx`), control IDs, factor names, version strings. Every occurrence is reconciled to the reviewed primary-source version, or annotated inline as a dated snapshot of a superseded version. No silent drift: a wrong label anywhere is a finding, not a cosmetic. For the OWASP ASI codes this is **enforced deterministically** by `scripts/lint-taxonomy-drift.py` (in the pre-push guard): it enumerates every title-shaped `ASIxx` reference and fails on any whose label does not name the canonical category. Grep finds the labels you know are wrong; the lint finds the ones you don't. Extend its canonical map when a new taxonomy is reviewed.
4. **Authoritative-structure pass.** The CMM ([[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] + the nine deep dives), the [[agentic-ai-security-reference-architecture|RA]], and the [[agentic-ai-security-cmm-crosswalk|crosswalk]] are reviewed for any mapping the review changes **semantically** — a re-anchored domain, a moved control, a re-scored coverage cell — not just label text, and corrected.
5. **Substantive propagation.** Where the review changes what the standard *provides* (e.g. awareness-not-conformance, scoring-not-control, a category that re-anchors to a different CMM domain), every page that cites the standard as evidence is reframed to match. A relabel that leaves the surrounding claim wrong is not done.
6. **Hygiene.** `updated:` bumped on every edited page; caches (`hot.md` / `log.md`) refreshed; `index.md` / relevant `_index.md` / the [[standards-review-backlog|backlog]] count updated.
7. **Hedge-free Effect section.** The review's "Effect on existing wiki pages" section is written in the past tense and lists what was *done*. No "may cite" / "should be reframed" language — anything genuinely deferred is filed as an explicit `> [!gap]` callout or a tracked GitHub issue, with the reason.

The test for "done": a reader who greps the wiki for the standard's identifiers finds the reviewed version everywhere, and every CMM/RA mapping that the review touched reflects the corrected meaning. If a follow-up review has to re-discover stale labels the first one left behind, the first review was not done.

## §6 Conventions update

The wiki's `wiki/meta/conventions.md` is updated alongside this page (separate edit) to add §Standards Provenance — the framework-page-specific extension of the existing §Source Provenance pattern. Lint enforcement to follow once the methodology has been exercised on the first P1 standard.

## §7 What this addresses

| User-stated concern | This methodology's answer |
|---|---|
| "We make bold claims that this or that standard doesn't cover certain aspects, especially when it comes to controls." | Bold absence claims become structurally falsifiable (§2 Step 3). The standardized form makes them verifiable in finite time. |
| "Our methodology for determining this is a wiki summary which doesn't feel thorough enough." | Step 1 requires primary-source citations on every framework page; Step 2 produces clause-level matrices, not anchor-level. The one-hop-removed problem is resolved per standard as the audit progresses. |
| "Should we do a thorough review of each standard we're tracking?" | Yes — see §3 audit backlog. ~47 hours of focused work for P1–P3 + ~10 hours for adversarial passes. Suggest one standard per session. |
| "How can we increase our confidence when we say something doesn't exist? You can't prove a negative." | You can't prove a *universal* negative ("no standard anywhere covers Y"). You *can* prove a *bounded* negative ("Standard X v1.0 §1–§N does not cover Y given search terms A/B/C"). This methodology only allows bounded negatives. |

## §8 Limits

This methodology does NOT:

- **Audit production deployments** of any standard. The reviews are document-vs-document, not control-effectiveness assessments. Empirical validation is a separate exercise (per [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]]).
- **Prevent standards drift**. ISO 42001 amendments, AIUC-1 quarterly refreshes, and EU AI Act enforcement-phase changes will obsolete absence claims. Reviews carry `reviewed:` dates and `version:` fields; absence claims older than 2 quarters automatically gain a `> [!stale]` callout.
- **Solve paywalled-standard verification**. ISO and parts of CSA/AIUC are paywalled. For those, the methodology degrades gracefully to citation-only with documented constraint.
- **Replace the existing crosswalk**. The crosswalk is anchor-level navigation; this methodology produces clause-level audits. Both have value.

## §9 Sequencing

Recommended order, one standard per session:

1. **NIST AI RMF + AI 600-1 + AI 800-4** (P1, free public, most-claimed-against)
2. **OWASP Agentic AI Top 10 + AIVSS** (P1, free public, frequently cited as both anchor and gap source)
3. **EU AI Act incl. Annex IV** (P1, free public, regulatory exposure)
4. **ISO/IEC 42001** (P1, paywalled — citation-only review)
5. **MITRE ATLAS v5.4.0+** (P2, free public)
6. **AIUC-1** (P2, freshest target — quarterly cadence means newer reviews are most useful)
7. **CSA MAESTRO + ATF** (P2)
8. **P3 cluster** as time permits

After P1 is complete, the [[agentic-cmm-vs-standards-validation|existing validation page]] flips from `addressed` to `superseded-by-methodology` and the new per-standard reviews are the load-bearing evidence.

## §10 Open questions

> [!gap] What this scaffolding doesn't yet handle
> 1. **Cross-jurisdiction standards** (e.g. UK AI regulation, China's interim measures, India's DPDP) are not in the audit backlog. Adding them is straightforward — they just need wiki framework pages first.
> 2. **Frameworks that are themselves CMMs** (PwC, Microsoft RAI Maturity, Anthropic RSP) are excluded from this methodology's scope — they are *peers*, not authorities. A separate page should compare them per the user's earlier interest in maturity-model spread (parked).
> 3. **Citation-fetching automation.** Long-term, retrieving primary sources, hashing, and archiving could be scripted (`scripts/fetch-standard.py`). Manual for now.
> 4. **Reviewer disagreement protocol.** When two reviewers reach different verdicts on the same absence claim, the page surfaces both and flags as `[!contradiction]`. No mediation rule beyond that yet.
> 5. **Empirical bridge.** This methodology validates documents against documents. Bridging to "do organizations actually implement these clauses?" requires the audit backlog from the [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]] — out of scope here.

## Applications of this methodology

Step 3's falsifiable-absence-claim discipline also runs outside a `wiki/reviews/` snapshot. [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack for 2026]] is the worked case. That thesis had asserted, from each instrument's stated scope rather than from a clause-level pass, that no framework in its stack governs the coding agent as an SDLC actor. Reading the OWASP AI Exchange's `SEC DEV PROGRAM` and `DEV PROGRAM` clause by clause converted one instrument's entry from scope inference to a bounded absence: the controls name AI-assisted development nowhere and treat new engineering roles as human ones. The bound is what makes it usable — the same control does address agent-reachable capabilities, as the supply chain of the system being built rather than of the agent building it, so the absence is specific rather than a general lack of agentic awareness.

## Relations

- Targets: [[agentic-ai-security-cmm-2026|CMM]] and [[agentic-ai-security-reference-architecture|RA]] absence claims
- Supersedes (when adopted): the methodology in [[agentic-cmm-vs-standards-validation|2026-04-30 validation]] §1
- Companion to: [[agentic-ai-security-cmm-crosswalk|standards crosswalk]] (anchor-level) and [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]] (deployment-level)
- Updates: `wiki/meta/conventions.md` §Standards Provenance (separate edit)
- Adopts the falsifiability discipline from: [[wiki-novelty-and-counterarguments-2026|Wiki Novelty and Counter-Arguments]] (which already documents 10 unresolved contests with bounded scope)
