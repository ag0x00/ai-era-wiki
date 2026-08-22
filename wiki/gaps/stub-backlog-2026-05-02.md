---
type: gap-analysis
title: "Stub Backlog: Stub and Seed Pages"
created: 2026-05-02
updated: 2026-05-29
tags:
  - gaps
  - backlog
  - methodology
  - operations
status: developing
origin: produced
tracker_migrated_to: "GitHub Issue #63 (Wiki Maintenance)"
question: "Which pages in the wiki are marked status:stub or status:seed, which are load-bearing, and in what order should they get filled?"
related:
  - "[[peer-review-readiness-2026-05-02]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-reference-architecture]]"
---

# Stub Backlog

> [!info] Live tracking has moved to GitHub Issue #63
> The fillable inventory (P1 + P2) is now tracked in **GitHub Issue #63** (`maintenance` label, *Wiki Maintenance* milestone). Update the issue, not this page. This page is retained as a reference snapshot of the full inventory and for the durable triage method, decision rules, and refresh cadence below.

Operational tracking for pages that are placeholders. Distinct from [[peer-review-readiness-2026-05-02|peer-review readiness]], which tracks **framework-level** gaps (the RA + CMM's structural weaknesses). This page tracks **content-level** stubs (entity pages, concept pages, framework pages that exist as placeholders).

## Triage method

Three priority tiers, ordered by leverage:

| Tier | Criterion | Action |
|---|---|---|
| **P1 — Load-bearing** | Referenced by [[agentic-ai-security-cmm-2026\|CMM]], [[agentic-ai-security-reference-architecture\|RA]], or core thesis pages; framework claims rest on the page existing in non-stub form | Fill in the next session that touches the related claim |
| **P2 — Active citation** | Referenced from a recent ingest or thesis page but not framework-load-bearing | Fill opportunistically (next ingest of related material) |
| **P3 — Backlog** | Created by an ingest that touched the entity once; not referenced beyond that | Leave as-is until material accrues |

## P1 — Load-bearing stubs (5)

These are referenced by the CMM's "Practitioners worth following" or new product additions and a peer reviewer would notice the placeholder.

| Path | Status | What's needed | Trigger to fill |
|---|---|---|---|
| `wiki/entities/people/simon-willison.md` | stub | Coined the [[lethal-trifecta\|Lethal Trifecta]] (Jun 2025); independent researcher; key ongoing voice on prompt injection. Page should reference Willison's own writing on simonwillison.net | Next time we revisit Lethal Trifecta or prompt-injection material |
| `wiki/entities/people/johann-rehberger.md` | stub | Embrace The Red; Month of AI Bugs (Aug 2025); Jules AI kill chain; load-bearing for the wiki's incident-anchor narrative | Next time we revisit incidents (Month of AI Bugs deepening, or new Rehberger disclosure) |
| `wiki/entities/people/bill-mcintyre.md` | stub | Author of *Securing Your Agents* deck; the wiki's source for the 40-slide layered playbook | Page is OK as a stub since the talk page is the substantive artifact; minor priority |
| `wiki/entities/products/smokescreen.md` | seed | Stripe's open-source SSRF / egress proxy; the network-side control point in [[breaking-the-lethal-trifecta-talk\|Bullen-talk]] containment architecture; OSS so verifiable externally | Next time we deepen egress-control practice page |
| `wiki/entities/products/toolshed.md` | seed | Stripe's internal central MCP proxy / tool registry; PEP for `ToolAnnotations` in Bullen architecture | Same trigger as Smokescreen |

## P2 — Active citations (10)

Referenced from recent ingests or thesis pages but not central to a framework claim.

| Path | Status | What's needed |
|---|---|---|
| `wiki/concepts/mcp-security.md` | seed | Despite name being "seed," this concept is referenced 30+ times — but the wiki has substantive MCP coverage in [[a2a-protocol\|A2A]], [[agentic-ai-security-reference-architecture\|RA]] §Egress, and [[multi-agent-runtime-security\|Multi-Agent Runtime Security]]. The seed page itself is the gap; consolidating into a real concept page would help |
| `wiki/concepts/spiffe.md` | seed (with `[!gap] Stub` inline) | SPIFFE / SPIRE workload identity; D2 L3 evidence cite; brief explainer would suffice |
| `wiki/concepts/llm-as-a-judge.md` | seed (with `[!gap] Stub` inline) | LLM-as-a-Judge pattern; cited in evaluation contexts; brief explainer would suffice |
| `wiki/concepts/evidence-centered-benchmark-design.md` | seed (with `[!gap] Stub` inline) | ECBD methodology; less-cited; could be deprioritized to P3 |
| `wiki/concepts/human-parity-line.md` | seed | Gartner's measurement (1,320 tasks / 42 roles / 9 industries); cited by [[scaling-agentic-ai-cios-talk\|CIOs talk]]; brief expansion needed |
| `wiki/frameworks/nist-sp-800-218a.md` | seed | NIST SSDF AI Profile; named in [[agentic-ai-security-cmm-2026\|CMM]] D8 mapping; should cite the real publication when finalized |
| `wiki/frameworks/cyber-defense-matrix.md` | seed | Sounil Yu's 5×5 matrix; cited in 2026 AI extensions; expansion would help cross-frame work |
| `wiki/entities/organizations/anthropic.md` | seed (with `[!gap] Stub` inline) | Heavily-referenced (Claude, GTG-1002, Sleeper Agents, Constitutional Classifiers); seed status is misleading given depth in other pages. Consolidate the references |
| `wiki/entities/organizations/openai.md` | seed (with `[!gap] Stub` inline) | OpenAI now owns Promptfoo; CoSAI member; Apollo collaborator. Seed page is a documentation gap |
| `wiki/entities/organizations/google.md` | seed-tier (despite update history) | Google has substantial coverage but its entity page hasn't been consolidated; A2A, ADK, SAIF, CoSAI, GTG-1002 disclosure all cite it |

## P3 — Backlog (lightweight stubs from single ingests)

Entity stubs added during a single ingest where the entity is named once or twice. **Don't fill these** until material accrues. They exist so wikilinks resolve and are doing their job.

| Path | Origin |
|---|---|
| `wiki/entities/organizations/{aisi-uk,apollo-research,cset-georgetown,enisa,metr,stanford-hai,wef}.md` | Created by Task #2 (threat classes) and Task #5 (source triangulation). Each has 5–10 line stub adequate for current citations |
| `wiki/entities/organizations/oasis-security.md` | Created by Oasis NHI ingest |
| `wiki/entities/products/kirin.md` | Created by Knostic ingest |
| `wiki/entities/people/{bob-rudis,daniel-miessler,sounil-yu,brandon-gummer,remy-gulzar,dongdong-sun,mohamed-nabeel,avivah-litan,daryl-plummer,andrew-bullen}.md` | Created by various conference / paper ingests |
| `wiki/entities/organizations/{adobe,glean,wiz,palo-alto-networks,meta}.md` | Created by various ingests; light citation footprint |
| `wiki/practices/securing-ai-talking-points.md` | Single-source talk derivative |

These are **doing their job as stubs** — they make wikilinks resolve and capture the entity name + minimal context. They don't need to become deep biographies.

## P3 (added 2026-05-04 from lint scan) — dead-link stubs

Surfaced by [[lint-report-2026-05-04|the 2026-05-04 lint pass]] as wikilinks pointing at non-existent pages. All P3 — single or paired references, no framework load. Listed for tracking; **don't fill speculatively**.

### Conference-catalog org stubs (15)

Referenced from [[unprompted-conference-march-2026|the Unprompted conference catalog]] and [[unprompted-march-2026-talks-vs-ra-cmm|the talks-vs-RA/CMM comparison]] for talk attribution: `airbnb`, `aws`, `block`, `crowdstrike`, `datadog`, `elastic`, `greynoise`, `intel`, `microsoft`, `netflix`, `nvidia`, `perplexity`, `snowflake`, `sysdig`, `zenity`.

These are well-known orgs with no agentic-AI-security material that's wiki-load-bearing yet. Decision: **leave as dead links**; the wikilink itself is the backlog signal. If/when one of these orgs publishes a talk, paper, or product the wiki ingests, the stub gets created at that point.

### Misc-org stubs (5)

| Slug | Source | Note |
|---|---|---|
| `alpitronic` | [[unprompted-conference-march-2026\|Unprompted Conference — AI Security Practitioner Conference (March 3–4, 2026)]] | Talk-attribution mention |
| `sans-institute` | [[unprompted-conference-march-2026\|Unprompted Conference — AI Security Practitioner Conference (March 3–4, 2026)]] | Talk-attribution mention |
| `hiddenlayer` | [[comprehensive-agentic-ai-security-landscape-2026\|Comprehensive Agentic AI Security Startup Landscape — Pool-Then-Filter Pass]] | Landscape comparison cell |
| `protect-ai` | [[comprehensive-agentic-ai-security-landscape-2026\|Comprehensive Agentic AI Security Startup Landscape — Pool-Then-Filter Pass]] | Landscape comparison cell |
| `team8` | [[lumia-security\|Lumia Security]] | VC backer of [[lumia-security\|Lumia]] (#1 seed Dec 2025) |

### Concept / product page stubs (2)

| Slug | Source | Note |
|---|---|---|
| `cursor-ide` | [[cursor-npm-credential-stealer\|Cursor npm Credential Stealer (May 2025)]] | Cursor IDE product page |
| `claude-imessage-mcp` | [[claude-stripe-coupons-imessage-injection\|Claude Metadata-Spoofing Attack — Unlimited Stripe Coupons via iMessage MCP Injection]] | Concept: iMessage as MCP-trigger surface |

### Pre-existing source-provenance backlog (19, surfaced by `lint-sources.py`)

Entity pages without a public `homepage:` URL. P3 backlog — track but don't auto-fill. See [[lint-report-2026-05-04|the lint report]] for the full list (NIST, MITRE, Anthropic, Google, OpenAI, Meta, OWASP, ISO, CSA, CoSAI, Adobe, Snap, Insight Partners, Glasswing, plus 5 product pages, plus Starseer and Sondera-renamed-from-Sendera).

## Decision rule going forward

When a wiki page is created as a stub or seed:

1. **Stub callout in body** (`> [!gap] Stub`) is enough — no separate tracking needed
2. **status: stub or status: seed in frontmatter** is the canonical signal
3. **This page is the index** — it gets refreshed when the inventory grows materially (≥5 new stubs since last refresh)

When to **fill** a stub:
- A new ingest references it materially (>= 3 mentions or load-bearing claim)
- A peer reviewer flags it
- It graduates from P3 to P2 because new evidence accumulated

When to **leave** a stub:
- Single-citation entities (P3 territory)
- Conceptual placeholders that have substantive treatment elsewhere
- Pages that exist purely so wikilinks resolve

## Relationship to other tracking

- **Framework gaps** → [[peer-review-readiness-2026-05-02|peer-review-readiness-2026-05-02]] (RA + CMM structural weaknesses)
- **Source triangulation** → [[source-triangulation-audit-2026-05-02|source-triangulation-audit-2026-05-02]] (claim-level evidence triangulation)
- **Anti-patterns / failure modes** → [[anti-patterns-and-failure-modes|anti-patterns-and-failure-modes]] (operational failures)
- **CMM calibration** → [[cmm-calibration-stress-test-2026|cmm-calibration-stress-test-2026]] (rubric calibration)
- **Stub backlog** (this page) → content-level stubs

These are deliberately separate. A stub backlog should not bloat with framework-level concerns; framework-level pages should not double as stub trackers.

## Refresh cadence

This page should be refreshed when:
1. ≥5 new stubs accumulate
2. A P1 stub gets filled (move to "completed" log section)
3. A P2 promotes to P1 because of new framework citation
4. Quarterly cadence regardless

## See Also

- [[peer-review-readiness-2026-05-02|Peer-Review Readiness]] — framework-level gaps
- [[agentic-ai-security-cmm-2026|CMM]] §Open questions and gaps — CMM-internal gaps
- [[anti-patterns-and-failure-modes|Anti-Patterns and Failure Modes]] — operational gaps
