---
type: gap-analysis
title: "CMM Known Limitations (current state)"
created: 2026-05-06
updated: 2026-09-15
tags: [gaps, cmm, known-limitations, current-state]
status: developing
scope_axis:
  - sec-of-ai
target: "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[standards-validation-methodology-2026-05]]"
  - "[[cmm-stress-test-canadian-fi-google-2026-09]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[osfi-b-13]]"
  - "[[agentic-ai-security-cmm-crosswalk-canada-fi]]"
  - "[[securing-agentic-coding]]"
  - "[[azure-rag-chatbot-security-profile]]"
  - "[[canadian-bank-secure-sdlc-ai-assessor-scorecard]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[agentic-ai-security-cmm-crosswalk-us-fi]]"
  - "[[owasp-aivss]]"
  - "[[lethal-trifecta]]"
---

# CMM Known Limitations (current state)

Current-state limitations of [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]], restated 2026-05-06 and extended 2026-09-15.

This page replaces §5 ("Risks and overclaims") of the older [[agentic-cmm-vs-standards-validation|Validation page]]. Three of the original seven items were addressed by CMM revisions during 2026-05; one was wrong (CSA ATF five-stage); the rest are restated below against the *current* CMM. As future revisions close items, archive them here with a `[!check]` note rather than silently deleting.

## Still-current limitations

Items 6 to 21 come from [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], and each carries that review's recommendation number. The tracking issue [#168](https://github.com/ag0x00/ai-era/issues/168) and its sub-issues hold live status for every item; this page holds the durable text.

### 1. `D5 L3` — combined MCP+A2A+LLM gateway treated as a settled standard

The clause requires "an agent-aware proxy / gateway between agent and external tools enforcing per-tool RBAC (AgentGateway in Linux Foundation, Solo Enterprise, Cloudflare AI Gateway, Kong AI Gateway, or equivalent); HTTPS / TLS 1.3 + OAuth/mTLS for inter-agent [[a2a-protocol|A2A v1.0]] communication per spec §7."

The [[a2a-protocol|A2A v1.0.0 spec]] (LF-governed since June 2025) covers transport (§7) and Agent Card signing (§8.4) but **not** message-level integrity, replay protection, or cryptographic agent identity. These remain vendor-side ([[multi-agent-runtime-security|Oktsec-class enforcement]]) or proposal-side. Treating the combined MCP+A2A+LLM proxy as a settled `L3` (org-wide standard) requirement is aggressive without an org-authored A2A enforcement profile — which the CMM does call for ("orgs MUST document their own A2A enforcement profile, including signing algorithm and replay-protection layering"), but the org-authored-profile burden is the limitation.

**Status:** [verified-current]. Needs primary-source recheck on A2A v1.0.0 spec when next reviewed.

### 2. `D4 L5+` — TEE-backed guardrail attestation has no auditor schema

The L5+ clause requires "cryptographic attestation that guardrails executed in a TEE (AWS Nitro Enclaves-class)." This was originally `D4 L5` and was moved to L5+ in the 2026-05-04 L5/L5+ split (acknowledging it's research-stage). The remaining concern: even at L5+, auditors evaluating "TEE attestation chain" will find no standard chain-of-custody schema to evaluate against. The claim is auditable in principle (an attestation log either exists or doesn't) but the chain-of-custody schema is org-authored.

**Status:** [verified-current], reduced impact (L5+ is explicitly aspirational).

### 3. `D2 L5` — Microsoft Agent 365 Registry "or equivalent" remains underspecified

The clause references "Microsoft Agent 365 Registry or equivalent unified governance." Agent 365 GA was 2026-05-01; deployment evidence is now possible but not yet published at scale. "Or equivalent" softens the dependency on a single vendor, but the criterion does not state which capabilities an equivalent must match, so an assessor has no basis for grading a non-Microsoft deployment against it. A CISO at L5 needs to either pick Agent 365 or build the equivalent capability set themselves.

**Status:** [verified-current]. Re-check by 2026-Q3 once Agent 365 deployment evidence and competing-product feature parity are observable.

### 4. `D1 L5` — AIUC-1 quarterly cadence and single-auditor capacity

The clause requires "AIUC-1 certified." AIUC-1 updates quarterly (Q2 2026 update focused on MCP / third-party / agent identity per AIUC's own statements); a `L5` claim is implicitly "currently certified against the most recent quarterly refresh," which the CMM language doesn't quite articulate (the L5 row says "AIUC-1 certified against the most recent quarterly refresh OR ISO/IEC 42001 certified" + "most-recent cert dated within last quarter" in the auditor-evidence column — better than the original 2026-04-30 framing but still a moving target). Schellman is currently the only accredited auditor — single-auditor capacity is a real gating constraint for organizations attempting L5 certification.

**Status:** [verified-current], partly addressed by the "most-recent cert within last quarter" language. The capacity constraint is structural, and CMM language cannot fix it.

### 5. `D6 L3+` — `IDENTITY.md` / `SOUL.md` filename conventions are not industry-standard

The CMM mandates SHA-256 of `SOUL.md` / `IDENTITY.md` / system prompts as cognitive file integrity. These specific filenames are vault conventions inherited from this project; in practice agent identity files use a wider variety of names (`.cursorrules`, `Claude.md`, `claude.md`, `.augment-guidelines`, `system_prompt.txt`, vendor-specific paths). The 2026-05-06 verification overlay added the AIUC-1 B008.6 anchor for the underlying primitive (cryptographic checksums for tamper detection), but the *scoping* to identity files is the load-bearing CMM contribution, and the file-set being protected is not yet standardized.

**Status:** [new-2026-05-06]. The CFI primitive is sound; the file-discovery layer is the gap. Future CMM revision should describe a discovery rule ("any file the agent reads as system context at startup or per-session"), not a hardcoded filename list.

### 6. Product and cost layer assumes a Microsoft, Azure, AWS or GitHub incumbency

All nine cost models price their licensing column against that incumbency and none prices Google Cloud or Workspace. The core page's tooling map carries no platform-native column, names Google in no row, and holds no occurrence of Model Armor. Seven of the nine deep dives do carry a GCP platform-native column, holding Agent Identity, Model Armor, Apigee and Google SecOps, so what the family lacks for this persona is the pricing and the two September shapes rather than Google itself; Claude Code and Gemini appear in no tooling-map row. A buyer on Google Cloud reconstructs the control mapping instead of reading it.

**Status:** [new-2026-09-15], from [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]] Part 4. Recommendation 20 prices a non-Microsoft column or states each cost model's stack assumption; recommendation 21 adds the Google instruments that exist and states the absences. Tracked in [#175](https://github.com/ag0x00/ai-era/issues/175) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 7. No shape row for an enterprise productivity assistant

The core page's shape table (seven rows), the measurement protocol's Agent Card enum, all nine right-sizing tables and the reference architecture's eleven-row shape table carry no row for an assistant holding tools over a whole tenant's mail, files and calendar. The one profile the vault holds for a productivity-class deployment, [[azure-rag-chatbot-security-profile|the Azure RAG chatbot profile]], excludes external write tools by scope, which is the property that restores the [[lethal-trifecta|lethal trifecta]] for this shape.

The shape is a class: Gemini for Workspace and Microsoft 365 Copilot inside the suite, where the tenant ACL and the DLP rule are the enforcement unit, and a desktop agent such as Claude Cowork, which adds local file access, MCP connectors, browser use and scheduled routines to the same mail, files and calendar reach. The desktop-agent variant carries tool-mediated writes outside the tenant, so D3, D5 and D8 grade it above the in-suite variant, and the row may need to split in two.

**Status:** [new-2026-09-15]. The enum value was added to the protocol in the same pass and names both variants; the shape row is recommendation 16 and remains a calibration decision. Tracked in [#174](https://github.com/ag0x00/ai-era/issues/174) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 8. `L5` gate contradicts selective L5

The prerequisite gate requires two quarters of stable L4 across all nine domains before an assessor may score any domain L5, while the core page expects L4 across all domains with selective L5 where exposure justifies it and five right-sizing tables name selective L5 as a target. A program at L2 in one domain by recorded trade-off can never be scored L5 in another.

**Status:** [new-2026-09-15]. Recommendation 18. Tracked in [#173](https://github.com/ag0x00/ai-era/issues/173) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 9. `D2 L5+` vs `D3 L5` and `D5 L5` — per-task capability tokens sit at two rungs

D2 moves per-task holder-bound capability tokens to L5+; D3 and D5 keep the same capability at L5 with a regulated-buyer opt-out. The measurement protocol asks for the artifact in the D3 L5 column. The same evidence grades one rung apart depending on which page an assessor opened. The Cedar/OPA policy-repository artifact carries the same cross-domain question one rung down, moved from D2 L4 to D3 L4 in the measurement protocol: the core page's generic Level 3 auditor-evidence line still names a Cedar/OPA policy repo with no D2/D3 assignment, and it stays as written because it sits inside ladder text `lint-cmm-rung-sync.py` gates.

**Status:** [new-2026-09-15]. Recommendation 17. The protocol's D7 L5 artifact now names the capability; its D3 L5 cell still carries the product name, because the core page quotes that cell verbatim inside rung-gated text, so both move together or not at all. Tracked in [#169](https://github.com/ag0x00/ai-era/issues/169) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 10. `D8 L3` and `D8 L4` do not grade the harness-fleet inventory

[[agentic-ai-security-cmm-d8-supply-chain|D8]] states that fleet inventory — which harnesses, which versions, which MCP servers, attributable to which human — is an evidence dimension the L3 and L4 criteria omit, and it is the load-bearing evidence for a coding-agent deployment the model right-sizes to L4. [[securing-agentic-coding|The coding-shape control catalog]] files the same inventory under [[agentic-ai-security-cmm-d7-observability|D7]] as a runtime AI-BOM, so one artifact serves two domains until a rung names it. The only commercial instrument the vault records for it is a single vendor with no dated GA and no independent evaluation, which a third-party-risk function treats as a concentration finding.

**Status:** [new-2026-09-15]. Recommendation 27, together with the absent D1 and D9 coordinates in [[securing-agentic-coding|the coding-shape control set]]. Tracked in [#177](https://github.com/ag0x00/ai-era/issues/177) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 11. B-13's in-force date has no source in the vault or in the pages it cites

[[osfi-b-13|The B-13 page]] labelled its 2022-07-31 publication date as an effective date, corrected on 2026-09-15. [[agentic-ai-security-cmm-crosswalk-canada-fi|The Canadian crosswalk]] states in force 2024-01-01 and cites an OSFI page that states no effective or in-force date; the guidance-library page states only "Date July 31, 2022". No `.raw/` document behind either claim exists.

**Status:** [new-2026-09-15]. Recommendation 25: cite a source that states the in-force date, or drop the in-force claim. Tracked in [#177](https://github.com/ag0x00/ai-era/issues/177) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 12. `D6` alone omits the production-maturity preamble

Eight of the nine ladders open with the rule that a control counts when it operates in production. [[agentic-ai-security-cmm-d6-data-rag|D6]] opens with the rule-1 sentence alone, so an assessor grading a piloted retrieval control against D6 has no instruction that the pilot does not count.

**Status:** [new-2026-09-15]. Recommendation 19 adds the preamble to D6, the core page and the measurement protocol. Tracked in [#172](https://github.com/ag0x00/ai-era/issues/172) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 13. The scorecard takes its engagement tier as the minimum of its section tiers

[[canadian-bank-secure-sdlc-ai-assessor-scorecard|The Canadian-bank scorecard]] takes the whole-engagement tier as the minimum of the per-section tiers and states that choice as deliberate, while the CMM replaced the single floor with dependency-resolved effective scores over a per-domain matrix. The same program carries two headline ratings depending on which instrument produced it, on a scorecard that claims alignment with the measurement protocol for cross-engagement comparability.

**Status:** [new-2026-09-15]. Recommendation 22 replaces the minimum-of-sections tier with dependency-resolved aggregation, or removes the tier. Tracked in [#176](https://github.com/ag0x00/ai-era/issues/176) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 14. The mandatory evidence-tag set admits no jurisdictional anchor

Tagged findings are graded against four identifier families, `ASI##`, [[owasp-aivss|AIVSS]], `AML.T####` and CVE, and [[agentic-ai-security-cmm-d1-governance|D1]] L4 asks for a maintained standards crosswalk on top of them. No family admits a supervisory instrument, so the OSFI anchors a Canadian federally regulated institution is actually examined against cannot be recorded in the tag set, and the [[agentic-ai-security-cmm-crosswalk-canada-fi|Canadian]] and [[agentic-ai-security-cmm-crosswalk-us-fi|US]] crosswalks the vault holds have no cell to fill.

**Status:** [new-2026-09-15]. Recommendation 23 admits jurisdictional anchors in the core page's tag set; the parallel fix in the handbook's gap-report crosswalk extract was applied in the same pass. Tracked in [#176](https://github.com/ag0x00/ai-era/issues/176) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 15. The scorecard leaves two control surfaces unscored and bundles independent controls under one answer

Across the scorecard's 62 questions there is no occurrence of oversharing, entitlement, answer-time, indirect or memory poisoning, so the answer-time entitlement spine the recalibration made [[agentic-ai-security-cmm-d6-data-rag|D6]]'s centre goes unscored, as does indirect prompt injection, which [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] grades at L3. The crosswalk section maps sections to seven CMM domains and omits D4 entirely, with D5 reaching the instrument only as an anchor inside one question. Twelve questions (A3, A4, A6, A7, B1, B3, B7, E6, E7, F1, F2, F4) then bundle three or more independent controls under a single Yes, Partial or No, and the rubric's deficiency kinds do not include absent sub-controls, so Partial cannot record which limb failed.

**Status:** [new-2026-09-15]. Recommendation 24 adds the missing question families and splits the bundled questions. Until the split, the scorecard instructs the assessor to record each limb in the evidence column and to score the question on its weakest limb. Tracked in [#176](https://github.com/ag0x00/ai-era/issues/176) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 16. No single-stack reading exists for Google Cloud

The May regulated-FI stress test set a per-platform single-stack reading, a Google-Cloud-only reading included, as a closure condition on the reference architecture. That line is the condition's only occurrence in the vault, and it applies most directly to an institution whose whole estate is Google Cloud and Google Workspace.

**Status:** [new-2026-09-15]. Recommendation 26 writes the reading; the closure condition stays open until it exists. Tracked in [#175](https://github.com/ag0x00/ai-era/issues/175) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 17. Data residency and Canadian region are unrecorded for both deployment shapes

No page records a data-residency or Canadian-region position for any AI service. The documentation for [[securing-agentic-coding|the coding harness]] on Google Cloud's Agent Platform names global, multi-region and regional endpoints, defaults to `us-east5`, names no Canadian region and states nothing about retention, and the whole-tenant assistant's position is equally unrecorded, so a third-party-risk function applying OSFI B-10 has no recorded answer to where the data sits.

**Status:** [new-2026-09-15]. Recommendation 28 records a position for both shapes on the Canadian crosswalk and in the coding-shape control catalog. Tracked in [#175](https://github.com/ag0x00/ai-era/issues/175) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 18. Callout counts across the CMM family run over the one-per-page rule

Two callouts sit on D1, D2, D4, D6 and D7, three on D9, one each on D3, D5 and D8, three on the scorecard and four on the core page, against a convention of at most one per page. The count is debt across the family and bears on no rung, and no page the September pass edited added one.

**Status:** [new-2026-09-15]. Recommendation 29 reduces the counts across the nine deep dives, the core page and the scorecard. Tracked in [#177](https://github.com/ag0x00/ai-era/issues/177) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 19. `D3 L3` grades a decision point the coding harness holds itself

[[agentic-ai-security-cmm-d3-control-least-agency|D3]] L3 requires a policy decision point outside the model context, deny-by-default, synchronous and failing closed, and the measurement protocol asks for a PDP config, a PDP-unreachability test showing deny and a direct-gateway invocation test showing deny. A coding harness enforcing a managed permission policy is the enforcement point and the governed component at once, so it produces none of the three artifacts, and the assessor either records the circularity or scores L2.

**Status:** [new-2026-09-15]. Recommendation 30 gives D3 an evidence path for a harness-held enforcement point, or a not-applicable path where no external decision point exists. Tracked in [#170](https://github.com/ag0x00/ai-era/issues/170) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 20. The core page's coding-shape target outruns the `D4` ladder

The core page right-sizes a coding-tool deployment to L4 across all nine domains, while [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] states that its L4 spine rests on preview, experimental or specification-only controls and that a defensible L4 today is assembled from preview and open-source components. Rule 2 admits such a control once it sits in the approved-vendor pipeline with a documented production date, so the L4 the target names and the L4 the ladder can evidence are different bands for the same shape.

**Status:** [new-2026-09-15]. Recommendation 31 reconciles the two. Tracked in [#171](https://github.com/ag0x00/ai-era/issues/171) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 21. `D6 L3` presumes entitlements a source repository does not carry

[[agentic-ai-security-cmm-d6-data-rag|D6]] L3 grades answer-time entitlement enforcement, which presumes a retrieval corpus carrying per-principal entitlements. A source repository a coding agent reads holds no such layer, so for that shape the rung is unanswerable rather than unmet, and neither the ladder nor the handbook offers the assessor a way to record it.

**Status:** [new-2026-09-15]. Recommendation 32 restates the spine in terms a repository can satisfy, or scopes D6 out for the coding shape. Tracked in [#172](https://github.com/ag0x00/ai-era/issues/172) under [#168](https://github.com/ag0x00/ai-era/issues/168).

## Limitations addressed by CMM revisions (archived)

The following items appeared in §5 of the older validation page and have been resolved by CMM revisions during May 2026. Kept as historical record so future readers don't reintroduce them.

> [!check] `D3 L4` CSA ATF five-stage promotion gates (resolved 2026-05-06)
> Original concern: "CSA ATF five-stage promotion gates not yet fully specified in published guidance." Refuted by 2026-05-06 verification: ATF v0.9.1 has **four** maturity levels (Intern / Junior / Senior / Principal) with concrete promotion criteria (minimum time, accuracy thresholds, availability targets, named security validations, sign-off matrix). The CMM's `D3 L4` clause was rewritten 2026-05-06 to match the actual ATF v0.9.1 spec; only the Principal-tier hardware-bound identity / policy-as-code primitives remain abstract enough to need org-authored rubric. See the 2026-05-06 follow-up log entry for details.

> [!check] `D6 L5` provably bounded poisoning rate citing Nature Medicine 2024 0.001% (resolved 2026-05-04)
> Original concern: "A medical-imaging study's empirical threshold is not a transferable assurance bound for arbitrary RAG corpora." The 2026-05-04 CMM revision softened the language: "**documented poisoning-rate bound based on domain-appropriate empirical evidence** (the corpus owner sets the threshold and cites the supporting study; the Nature Medicine 2024 0.001% medical-imaging finding is one example, not a general bound)."

> [!check] `D7 L4` four red-team tools treated as interchangeable (resolved 2026-05-04)
> Original concern: "Promptfoo / Mindgard CART / PyRIT / Garak have very different scopes; treating them as interchangeable understates the work." The 2026-05-04 revision added category-distinct framing: "**distinct attack categories** — orchestration / multi-turn (PyRIT), probe library (Garak), regression suite (Promptfoo), and continuous CART (Mindgard CART or equivalent). Single-tool coverage is not L4."

## Contribution guide

When a future review surfaces a new CMM limitation:
1. Add a numbered subsection under **Still-current limitations** with the concrete CMM clause cited.
2. Tag with `[verified-current]` (primary-source-checked), `[wiki-summary]` (only summary checked), or `[new-YYYY-MM-DD]` (newly identified).
3. Recommend a fix or note why it's structural (not fixable in CMM language).
4. When a CMM revision resolves it, move the item to **Limitations addressed by CMM revisions** with a `[!check]` callout summarizing the resolution.

Per-standard reviews from the audit backlog ([[standards-validation-methodology-2026-05|Standards Validation Methodology]]) will likely surface additional CMM limitations as they execute. Those should be filed here as well.

## Relations

- Replaces: [[agentic-cmm-vs-standards-validation|Validation page]] §5 (which is now demoted to historical snapshot).
- Targets: [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]].
- Methodology: [[standards-validation-methodology-2026-05|Standards Validation Methodology]] for how new limitations are sourced and verified.
