---
type: gap-analysis
title: "Agentic AI Security Startup Landscape"
created: 2026-05-03
updated: 2026-08-21
tags:
  - gaps
  - vendor-landscape
  - funding
  - methodology
  - research-backlog
status: open
question: "What is the full set of agentic-AI-security startups active in 2025–2026, and which subset are seed-funded vs. later-stage vs. self-funded? An earlier May 2026 seed synthesis covered 8 verifiable seed rounds but was removed as incomplete — searches surfaced names rather than starting from an enumerated denominator."
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
sources: []
---

# Comprehensive Agentic AI Security Startup Landscape — Pool-Then-Filter Pass

## Basis for the gap

An earlier seed-funding synthesis ranked 8 startups plus 3 honorable mentions, then was removed as incomplete. Its methodology was **search-then-include**, not **enumerate-then-filter**: web searches surfaced names, and each was verified against primary sources. There was no fixed candidate denominator.

That approach answered *"what are the largest verifiable seed rounds we surfaced?"* but did not answer:

1. **What is the full population of agentic-AI-security startups operating in 2025–2026?** (any stage, any funding status)
2. **What fraction are seed vs. Series A+ vs. self-funded vs. acquired?**
3. **Are there startups with smaller-but-real seed rounds we missed because they ranked below the search results' fold?**
4. **Which RA planes / CMM domains have the most vs. fewest startup entrants?** (the synthesis answered this for the 8-startup subset, not the full population)
5. **Vendor concentration vs. fragmentation** — is the field consolidating (a few well-funded leaders) or proliferating (many small entrants)?

## Known starting points (curated landscapes)

These are published market maps that should anchor the pool-then-filter pass. None alone is exhaustive, but the union is a strong candidate set.

| Source | URL / pointer | Coverage | Notes |
|---|---|---|---|
| Insight Partners — *AI Agents Security Market Map* | `insightpartners.com/wp-content/uploads/2025/10/AI-Agents-Security-Market-Map-Blog-Hero-4-2048x1152.jpg` (Oct 2025) | Categorized landscape — Insight's own framing | Already a triangulated source for the wiki; the market map is the visual companion to their thesis writing |
| Crunchbase | search: `category:ai-security` + `category:cybersecurity` + `founded_date >= 2023-01-01` | Funding-stage data | Authoritative for round size + date but quality of agentic-AI-security tagging is uneven |
| PitchBook | search: AI security / LLM security / agent security verticals | Funding-stage data | Better category fidelity than Crunchbase but paywalled |
| Menlo Ventures — *AI Security Landscape* | Periodic blog posts (track via menlo.com/posts) | VC-curated | Menlo invested in [[general-analysis\|General Analysis]]; their landscape view will skew portfolio-positive |
| a16z — AI security pieces | a16z.com/topic/ai/ | VC-curated | Backed [[keycard\|Keycard]]; same caveat as Menlo |
| Forgepoint — AI security thesis | forgepointcap.com | VC-curated | Backed [[capsule-security\|Capsule]] |
| Gartner — *AI TRiSM Market Guide* (annual) | Gartner subscription | Vendor-list authoritative | Lags 6–12 months behind seed-stage market |
| OWASP GenAI / AISVS contributor list | owasp.org | Practitioner-active vendors | Indicates which vendors are investing in standards work, not which exist |
| LinkedIn — `#agenticAIsecurity` and `#MCPsecurity` company listings | LinkedIn search | Self-reported | Useful for stealth / pre-funding companies that don't yet appear in funding databases |

## Methodology proposal

A rigorous version of the May 2026 pass should:

1. **Build the union pool** from the sources above. Target: 50–80 candidates flagged as agentic-AI-security or AI-security adjacent.
2. **Tag each candidate** with: founding year, latest funding stage (pre-seed/seed/A/B+), latest round date+size, geographic HQ, primary RA plane, primary CMM domain, gateway-vs-instrumentation classification (where applicable), public/stealth status.
3. **Filter to in-scope**: founded ≥ 2023-Q1 OR pivoted-to-agentic ≥ 2024-Q4. Drop pure-MLOps and pure-model-scanning vendors that don't address agent runtime / authorization / egress.
4. **Bucket by funding stage** — not just seed. The seed view is one slice; an A/B+ view tells a different story (incumbents like [[lakera-guard|Lakera]], [[protect-ai|Protect AI]], [[hiddenlayer|HiddenLayer]] anchor that bucket).
5. **Re-map to RA + CMM** with the full pool, not the seed-only subset. Likely shifts D6 (Data, Memory & RAG) and D8 (Supply Chain & AI-BOM) findings.
6. **Identify acquired / shut-down / dormant** entries. CART consolidation has already taken [[splx-ai|SplxAI]] (Zscaler) and Promptfoo (OpenAI); the landscape view should flag others.

## Output target

A landscape-shaped page (not seed-shaped):

- **Full population table** (50–80 rows, sortable: name, stage, RA plane, CMM domain, last round)
- **Stage distribution chart** (in prose / Mermaid)
- **RA plane + CMM domain heatmap** showing where the field is dense vs. sparse
- **Acquisitions / consolidation list** with dates
- **D6 (Data/Memory/RAG) re-test** — was the May 2026 "zero seed-stage agentic-specific entrants" finding a real gap, or an artifact of the smaller search?

## Trigger to execute

- When the cumulative noise of "wait, did we cover X?" or "is Y in the wiki?" reaches ~5 unanswered names
- After Insight Partners or Menlo publishes a refreshed (2026-Q3+) market map
- Before the next wave of CMM revisions, so the tooling-map columns (Standards / OSS / COTS) reflect a triangulated population not a search-surfaced subset
- Approximately Q3 2026 if no other trigger fires first

## Effort estimate

- ~3 hours for the pool build (Crunchbase + PitchBook + market-map cross-reference)
- ~2 hours for the per-candidate tagging
- ~2 hours for the synthesis page
- Total: ~1 work-day — significantly larger than the May 2026 2-round autoresearch pass
