---
type: gap-analysis
title: "CMM Known Limitations (current state)"
created: 2026-05-06
updated: 2026-09-25
tags: [gaps, cmm, known-limitations, current-state]
status: developing
scope_axis:
  - sec-of-ai
target: "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[standards-validation-methodology-2026-05]]"
  - "[[cmm-stress-test-canadian-fi-google-2026-09]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[osfi-b-13]]"
  - "[[agentic-ai-security-cmm-crosswalk-canada-fi]]"
  - "[[securing-agentic-coding]]"
  - "[[azure-rag-chatbot-security-profile]]"
  - "[[google-cloud-agentic-security-profile]]"
  - "[[claude-code-control-sheet]]"
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
  - "[[claude-cowork]]"
  - "[[agent-runtime-protection-canvass-2026-09]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[iso-iec-42001]]"
  - "[[aiuc-1]]"
  - "[[aiuc-1-critical-evaluation]]"
verified: 2026-09-24
verified_against: []
verified_findings: 0
verified_note: "Diff-scoped read of the moved and shortened items against the review, issues 168, 174, 175, 176 and 177, and the practice, D1, D8, core and Cowork pages; item 10 and 11 status, item 6, 18 and 20 trackers and the intro fixed; none open."
---

# CMM Known Limitations (current state)

Current-state limitations of [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]], restated 2026-05-06 and extended 2026-09-15.

This page replaces §5 ("Risks and overclaims") of the older [[agentic-cmm-vs-standards-validation|Validation page]]. Three of the original seven items were addressed by CMM revisions during 2026-05; one was wrong (CSA ATF five-stage); the rest are restated below against the *current* CMM. As future revisions close items, archive them here rather than silently deleting.

## Still-current limitations

Items 6 to 21 come from [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], and each carries that review's recommendation number; items 24 and 25 answer two of its blocking flags. The review carries the argument for each finding it raised, the tracking issue [#168](https://github.com/ag0x00/ai-era/issues/168) and its sub-issues hold live status, and the items below hold the durable text: the claim, the resolution and the issue.

### 1. `D5 L3` — combined MCP+A2A+LLM gateway treated as a settled standard

[[agentic-ai-security-cmm-d5-egress-network|D5]]'s L3 clause required "an agent-aware proxy / gateway between agent and external tools enforcing per-tool RBAC (AgentGateway in Linux Foundation, Solo Enterprise, Cloudflare AI Gateway, Kong AI Gateway, or equivalent); HTTPS / TLS 1.3 + OAuth/mTLS for inter-agent [[a2a-protocol|A2A v1.0]] communication per spec §7." The criteria that replaced it state capabilities only:

- D5-GATEWAY, the gateway every model, tool and MCP call leaves through.
- D5-GATEWAY-AUTHZ, per-tool authorization against a rule that names the calling agent.
- D5-MCP-BROKER, the token validation on each MCP call.
- D5-A2A-TLS, D5-A2A-AUTH and D5-A2A-REPLAY, the inter-agent leg.

The products the clause named differ in what they broker, so an assessor grades what the deployment's gateway brokers, whichever product it runs.

The [[a2a-protocol|A2A v1.0.0 spec]] (LF-governed since June 2025) covers transport (§7) and Agent Card signing (§8.4) but **not** message-level integrity, replay protection, or cryptographic agent identity. These remain vendor-side ([[multi-agent-runtime-security|Oktsec-class enforcement]]) or proposal-side. Treating the combined MCP+A2A+LLM proxy as a settled `L3` (org-wide standard) requirement is aggressive without an org-authored A2A enforcement profile — which the CMM calls for: D5-A2A-REPLAY at L3 requires the deployment's enforcement profile to state the replay check its receivers run, and D5-A2A-SIGN at L5 requires a signing profile the organization documents and publishes to each outside party. The org-authored-profile burden is the limitation.

**Status:** [verified-current]. Needs primary-source recheck on A2A v1.0.0 spec when next reviewed.

### 2. `D4 L5+` — TEE-backed guardrail attestation has no auditor schema

D4-ATTEST at L5+ requires a trusted execution environment's attestation that each guardrail on each of the agents' model calls ran inside the enclave, verified to the hardware vendor's root. This was originally `D4 L5` and was moved to L5+ in the 2026-05-04 L5/L5+ split (acknowledging its research-stage status). The remaining concern: even at L5+, auditors evaluating "TEE attestation chain" will find no standard chain-of-custody schema to evaluate against. The claim is auditable in principle, because an attestation log either exists or does not, and the chain-of-custody schema is org-authored.

**Status:** [verified-current], reduced impact (L5+ is explicitly aspirational).

### 4. `D1 L5` — AIUC-1 quarterly testing and auditor capacity

D1-ASSURE accepts an AIUC-1 certificate within its one-year validity, with each quarterly red-teaming round completed, beside [[iso-iec-42001|ISO/IEC 42001]] under active surveillance, which is preferred, and a reviewed internal equivalent. AIUC states that a certificate is valid for one year and that agents must be submitted for red-teaming each quarter to maintain it ([AIUC — Re-certification](https://standard.aiuc-1.com/re-certification-maintaining-aiuc-1), retrieved 2026-09-24), so an AIUC-1 claim needs the latest quarterly round on record beside the certificate's date. AIUC issues every certificate, accredits the auditors and alone runs the quarterly technical testing, and it lists Schellman as accredited and six firms as provisionally accredited ([AIUC — Accredited AIUC-1 auditors](https://standard.aiuc-1.com/accredited-auditors), retrieved 2026-09-24). Auditor capacity is therefore wider than one firm, and testing capacity rests with AIUC alone.

**Status:** [verified-current], re-read against AIUC's pages on 2026-09-24. A certificate runs for a year, kept by quarterly testing, and AIUC's list names provisionally accredited auditors beside Schellman. The quarter-dated wording that once addressed freshness was replaced by item 25 below, which reads each scheme's cadence off the scheme the program chose. The testing constraint is structural, and CMM language cannot fix it.

### 6. Product and cost layer assumes a Microsoft, Azure, AWS or GitHub incumbency

Every cost model priced its licensing against a Microsoft, Azure, AWS or GitHub incumbency, and the core tooling map named no Google instrument ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 4). All nine cost models now state the incumbency they price and what a Google Cloud or Workspace buyer reads differently on that line. The core page's tooling map carries a **Platform-native (Google)** column, naming a Google instrument in every domain row or stating that none exists. D1's entry names Compliance Manager in Security Command Center, which lists no ISO/IEC 42001 or EU AI Act template, and Assured Workloads, and neither holds the crosswalk, board metrics or readiness assessment D1 grades at L4. D6's Google entry names Gemini's inheritance of the signed-in user's Workspace permissions and DLP for Gemini, which keeps a Drive file that matches a data protection rule out of an answer and restricts no other Workspace data, and it names no instrument for the oversharing assessment. The cost models add two published figures, Model Armor's token rate and the Security Command Center subscription minimum, and state every other domain as a difference in kind rather than a price, because Google publishes no other figure.

**Status:** [substantially-closed-2026-09-16]. Recommendations 20 and 21, shipped under [#175](https://github.com/ag0x00/ai-era/issues/175), a sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168). Open: pricing for every Google domain the vendor publishes no figure for.

### 10. The coding-shape control set carries no D1 or D9 coordinates

[[securing-agentic-coding|Securing Agentic Coding]]'s control set carries CMM coordinates for D2 through D8 and none for D1 or D9. The item's fleet-inventory half closed on 2026-09-25. [[agentic-ai-security-cmm-d8-supply-chain|D8]] grades the harness-fleet inventory, which harnesses run at which versions with which MCP servers and plugins, under D8-INVENTORY at D8 L2, read from device management, and reconciles it against the AI-BOM of the managed configuration the organization releases under D8-AIBOM-RUNTIME at D8 L4. [[agentic-ai-security-cmm-d7-observability|D7]]'s D7-ATTRIBUTE attributes each action to its human. [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]] carries the argument in Part 2, with the single-vendor concentration finding.

**Status:** [new-2026-09-15; narrowed 2026-09-25]. Recommendation 27, together with the D1 and D9 coordinates the rows of the coding-shape control set still lack. Its fleet-inventory half closed with D8's named criteria on 2026-09-25. Its D1 half narrowed on 2026-09-23: [[agentic-ai-security-cmm-d1-governance|D1]] L3 grades the harness configuration through the D1-HARNESS family, with its evidence read off the set's managed-policy and lock rows and its configuration-review step. The D1 replacement the stress test names, harness-fleet ownership and an approved-harness register with decision rights per repository risk tier, is graded in part: D1-REGISTER and D1-GATE register and approve each coding deployment with the person accountable for it, and D1-BOUNDARY records decision rights for each agent type an organization defines, which can be one per repository risk tier. D1-HARNESS reads the fleet record only for the managed source on each device or runner, the harness version each one runs is a field D8-INVENTORY grades, and the rest of the D1 levels have no row in the control set to cite. [[claude-code-control-sheet|Claude Code Control Sheet]] grades Claude Code's own controls in all nine domains, D1 and D9 included. [#177](https://github.com/ag0x00/ai-era/issues/177), the sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168) it was filed under, closed as completed on 2026-09-18, and its last comment, posted that day, records the fleet inventory and the D1 and D9 coordinates as untouched.

### 11. B-13's in-force date has no source in the vault or in the pages it cites

[[osfi-b-13|OSFI Guideline B-13: Technology and Cyber Risk]] labelled its 2022-07-31 publication date as an effective date until 2026-09-15, and [[agentic-ai-security-cmm-crosswalk-canada-fi|CMM: Canadian Regulated-Finance Crosswalk]] states B-13 in force from 2024-01-01 while citing an OSFI page that states no effective or in-force date; the guidance-library page states only "Date July 31, 2022". No `.raw/` document behind either claim exists, and [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]] records both OSFI fetches.

**Status:** [new-2026-09-15]. Recommendation 25: cite a source that states the in-force date, or drop the in-force claim. [#177](https://github.com/ag0x00/ai-era/issues/177), the sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168) it was filed under, extends it to B-10's effective date. That sub-issue closed as completed on 2026-09-18, and its last comment, posted that day, records both OSFI dates as untouched.

### 13. The scorecard takes its engagement tier as the minimum of its section tiers

[[canadian-bank-secure-sdlc-ai-assessor-scorecard|Assessor's Quick Scorecard: Secure-SDLC and AI]] takes the whole-engagement tier as the minimum of the per-section tiers and states that choice as deliberate, while the CMM replaced the single floor with dependency-resolved effective scores over a per-domain matrix, so the same program carries two headline ratings depending on which instrument produced it. [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]] carries the argument in Part 6.

**Status:** [new-2026-09-15]. Recommendation 22 replaces the minimum-of-sections tier with dependency-resolved aggregation, or removes the tier. Tracked in [#176](https://github.com/ag0x00/ai-era/issues/176) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 14. The mandatory evidence-tag set admits no jurisdictional anchor

Tagged findings are graded against four identifier families, `ASI##`, [[owasp-aivss|OWASP AI Vulnerability Scoring System (AIVSS)]], `AML.T####` and CVE, and [[agentic-ai-security-cmm-d1-governance|D1]] L4 asks for a maintained standards crosswalk on top of them. No family admits a supervisory instrument, so the OSFI anchors a Canadian federally regulated institution is actually examined against cannot be recorded in the tag set, and [[agentic-ai-security-cmm-crosswalk-canada-fi|CMM: Canadian Regulated-Finance Crosswalk]] and [[agentic-ai-security-cmm-crosswalk-us-fi|CMM: US Regulated-Finance Crosswalk (FFIEC and GLBA)]] have no cell to fill ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 4).

**Status:** [new-2026-09-15]. Recommendation 23 admits jurisdictional anchors in the core page's tag set; the parallel fix in the handbook's gap-report crosswalk extract was applied in the same pass. Tracked in [#176](https://github.com/ag0x00/ai-era/issues/176) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 15. The scorecard leaves two control surfaces unscored and bundles independent controls under one answer

Across the scorecard's 62 questions there is no occurrence of oversharing, entitlement, answer-time, indirect or memory poisoning, so the answer-time entitlement criterion at [[agentic-ai-security-cmm-d6-data-rag|D6]] L3 and the indirect prompt injection [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] grades at L3 go unscored, and the crosswalk section omits D4 and reaches D5 only as an anchor inside one question ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 6). Twelve questions (A3, A4, A6, A7, B1, B3, B7, E6, E7, F1, F2, F4) also bundle three or more independent controls under a single Yes, Partial or No, and the rubric's deficiency kinds do not include absent sub-controls, so Partial cannot record which limb failed.

**Status:** [new-2026-09-15]. Recommendation 24 adds the missing question families, and its tracking issue extends it to split the bundled questions, a finding the review does not carry. Until the split, the scorecard instructs the assessor to record each limb in the evidence column and to score the question on its weakest limb. Tracked in [#176](https://github.com/ag0x00/ai-era/issues/176) under [#168](https://github.com/ag0x00/ai-era/issues/168).

### 23. Open items in the CMM–standards crosswalk (added 2026-09-18)

[[agentic-ai-security-cmm-crosswalk|The base crosswalk]] carries seven open items, moved here when the page's callout count came down to the one-per-page ceiling (item 18):

1. **Full 38-control ISO 42001 Annex A map.** The current map shows control families and high-leverage anchors; a control-by-control mapping is the next iteration, and the unreviewed [[owasp-genai-crosswalk|GenAI Crosswalk]] rows against ISO/IEC 42001 are a candidate list for it.
2. **AIUC-1 Society pillar.** The CMM has no analogue for catastrophic-misuse or national-security externalities. This is a real gap, not a mapping bug.
3. **EU AI Act high-risk classification trigger.** The crosswalk assumes high-risk classification; for limited-risk and minimal-risk systems, Annex IV does not apply and the crosswalk simplifies.
4. **CSF 2.0 subcategory map.** A finer-grained NIST CSF 2.0 subcategory mapping (106 subcategories) would help organizations using CSF as their primary control catalogue; the [[owasp-genai-crosswalk|GenAI Crosswalk]]'s NIST CSF 2.0 rows are a candidate list on the same terms.
5. **AIUC-1 quarterly drift.** AIUC-1 updates quarterly. The crosswalk shows the Q2 2026 state and needs a refresh after each quarterly release.
6. **L5+ Leading Edge tier (added 2026-05-04).** The crosswalk maps to the CMM's L5 (Optimizing — achievable today) tier only. L5+ research-stage capabilities (TEE-backed guardrail attestation, [[camel-pattern|CaMeL]] split, multi-agent cascade-detection rule libraries, cross-vendor AI-BOM federation, sigstore-for-MCP) have no standards anchor yet because they predate the relevant specs; as CoSAI, OWASP, and NIST CAISI publish leading-edge guidance through 2026–2027, the crosswalk will gain an L5+ column.
7. **Series-level detection has no settled domain (added 2026-08-18).** The D4 cell anchors `UNWANTED INPUT SERIES HANDLING`, whose implementation clusters and compares requests across a time window that is not limited to consecutive requests ([`/go/unwantedinputserieshandling/`](https://owaspai.org/go/unwantedinputserieshandling/)). [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]]'s level definitions grade no series-level detector, and [[agentic-ai-security-cmm-d7-observability|D7]] L4 grades a session-scoped drift signal over the same trajectory, so a D4 level would collide with it at the boundary. The anchor stands and the grading domain is unresolved.

**Status:** [new-2026-09-18]. None of the seven bears on a graded level; each names a mapping boundary or a standards-drift risk. Carried here as a reference list rather than resolved individually.

## Limitations addressed by CMM revisions (archived)

The items below are resolved. Items A to C appeared in §5 of the older validation page and closed during May 2026, ahead of the numbering the still-current list uses; items 12, 16, 17, 20 and 21 closed on 2026-09-16, items 7, 18 and 22 on 2026-09-18, items 8, 9, 19, 24 and 25 on 2026-09-19, item 3 on 2026-09-23, and item 5 on 2026-09-25. Kept as a historical record so a future reader does not reintroduce them.

### A. `D3 L4` CSA ATF five-stage promotion gates (resolved 2026-05-06)

Original concern: "CSA ATF five-stage promotion gates not yet fully specified in published guidance." Refuted by 2026-05-06 verification: ATF v0.9.1 has **four** maturity levels (Intern / Junior / Senior / Principal) with concrete promotion criteria — minimum time, accuracy thresholds, availability targets, named security validations, and a sign-off matrix. The CMM's `D3 L4` clause was rewritten on 2026-05-06 to match the ATF v0.9.1 spec, and only the Principal-tier hardware-bound identity and policy-as-code primitives stay abstract enough to need an org-authored rubric.

### B. `D6 L5` took a medical-imaging poisoning threshold as a general bound (resolved 2026-05-04)

Original concern: "A medical-imaging study's empirical threshold is not a transferable assurance bound for arbitrary RAG corpora." The clause named a percentage threshold attributed to a 2024 *Nature Medicine* study, and no page in this vault carries a resolvable citation for that figure, which is the second reason it could not stand as an evidence target. The 2026-05-04 revision replaced it with a **documented poisoning-rate bound based on domain-appropriate empirical evidence**, under which the corpus owner sets the threshold and cites the study supporting it. [[agentic-ai-security-cmm-d6-data-rag|D6]] L5 carries the current wording.

### C. `D7 L4` four red-team tools treated as interchangeable (resolved 2026-05-04)

Original concern: "[[promptfoo|Promptfoo]] / [[mindgard-cart|Mindgard CART]] / [[pyrit|PyRIT]] / [[garak|Garak]] have very different scopes; treating them as interchangeable understates the work." The 2026-05-04 revision added category-distinct framing: the four cover **distinct attack categories** — orchestration and multi-turn for PyRIT, a probe library for Garak, a regression suite for Promptfoo, and continuous CART for Mindgard CART or an equivalent — and single-tool coverage is not L4.

### 12. `D6` alone omitted the production-maturity preamble (resolved 2026-09-16)

Original item: [[agentic-ai-security-cmm-d6-data-rag|D6]] alone opened its level definitions without the rule that a control counts when it operates in production, so an assessor grading a piloted retrieval control against D6 read no instruction that the pilot did not count ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 4). Resolved by putting both shared preamble sentences on D6, verbatim as the other eight carry them, so the approved-vendor-pipeline and production-date qualifier reads the same on all nine. Recommendation 19, shipped under [#172](https://github.com/ag0x00/ai-era/issues/172).

### 20. The core page's coding-shape target outran the D4 levels (resolved 2026-09-16)

Original item: the core page right-sized a coding-tool deployment to L4 across all nine domains while [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] states that its core L4 criteria rest on preview, experimental or specification-only controls ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 2). Resolved by lowering the core page's shape row to `L3 → L4 (L4 in D8)`. Eight of the nine deep dives right-size that shape below flat L4: six read `L3 → L4`, and D5 and D6 read L3. D8 is the ninth and holds L4. The D4 level definitions and the assembled L4 route the domain describes are unchanged. A canvass of twenty-one runtime-protection vendors established that no commercial product covers the L4 capabilities, which makes that route an integration project rather than a procurement ([[agent-runtime-protection-canvass-2026-09|Agent Runtime Protection Market Canvass]]). Recommendation 31, shipped under [#171](https://github.com/ag0x00/ai-era/issues/171), a sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168).

### 21. `D6 L3` presumed entitlements a source repository does not carry (resolved 2026-09-16)

Original item: `D6` L3 graded answer-time entitlement enforcement against a corpus carrying per-principal entitlements, which a source repository does not hold, so [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]] scored that shape at L2 to L3 against an L3 target and recorded the core L3 criterion as describing no repository. Resolved by restating the criterion rather than scoping `D6` out of the coding shape: L3 now asks which **authorization layer** resolves the asking principal's read authorization (per-document entitlements, repository and branch grants with a path-scoped retrieval, or a tenant access-control list with label-aware policy), and a run carrying no asking principal is graded on the scope binding the retrieval to the task. The measurement protocol's `D6` interview block carries the matching repository questions. Recommendation 32, shipped under [#172](https://github.com/ag0x00/ai-era/issues/172). The #283 rewrite reads the source-control layer as the developer's read grants at the grain the product sets them, which D6 records as the repository on GitHub, under D6-ENTITLE.

### 16. No single-stack reading existed for Google Cloud (resolved 2026-09-16)

Original item: the May regulated-FI stress test set a single-stack reading per major platform (Microsoft-only, AWS-only, GCP-only) as a closure condition on the reference architecture, and its Google-Cloud-only reading is the condition that bites hardest on the Canadian persona ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 4). Resolved by writing [[google-cloud-agentic-security-profile|Google Cloud Agentic Security Profile]], plane by plane against the reference architecture and domain by domain against the CMM, with the productivity-assistant shape as its worked example; the Microsoft-only and AWS-only readings the same condition asks for remain open. Recommendation 26, shipped under [#175](https://github.com/ag0x00/ai-era/issues/175), a sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168).

### 17. Data residency and Canadian region were unrecorded for both deployment shapes (resolved 2026-09-16)

Original item: neither shape carried a recorded residency position ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], recommendation 28). The claim was already wrong for the coding shape when filed, because [[securing-agentic-coding|Securing Agentic Coding]] recorded that Google Cloud's documented region list for the harness names no Canadian entry.

Resolved by recording both positions. The coding shape has no Canadian region to configure, because no Anthropic model carries a Canadian ML-processing commitment on the Agent Platform's partner residency table. The whole-tenant assistant cannot meet a Canadian residency requirement, because Workspace data regions cover Gemini prompts and responses and offer the United States or Europe only, where a Gemini deployment on Google Cloud itself can, spanning both Canadian regions with model serving in Montréal. [[agentic-ai-security-cmm-crosswalk-canada-fi|CMM: Canadian Regulated-Finance Crosswalk]] and Securing Agentic Coding record the positions, and [[google-cloud-agentic-security-profile|Google Cloud Agentic Security Profile]] carries the region-by-region detail. Two source facts stay open as watch items on the crosswalk: which Workspace editions carry the processing half of data-regions coverage, and whether a Canada Protected B workload runs an agent with no guardrail plane or places that plane outside the boundary. Recommendation 28, shipped under [#175](https://github.com/ag0x00/ai-era/issues/175), a sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168).

### 22. `D6 L2` stated document-corpus capabilities after `L3` became shape-general (resolved 2026-09-18)

Original item: item 21 restated `D6` L3 so the criterion resolves the asking principal's read authorization in whichever layer the corpus carries, and L2 was not restated with it. Its four clauses — source labels on retrievals, manual skill and plugin review, a sensitivity-labeling scheme on paper, and a first oversharing assessment — all described a document corpus. Grading is cumulative, so an assessor grading a coding agent or a productivity assistant had to decide unaided whether a repository-permission review counts as a data-risk assessment and whether source files carry a labeling scheme, and that decision governed whether the L3 verdict item 21 enabled was reachable at all. Resolved by giving L2 its grain from the same authorization layer L3 resolves in, with a per-shape reading on the deep dive: sensitivity labels over a document corpus or a tenant, and over a source repository a register of the repositories the agent reaches, each carrying a data classification and the paths excluded from retrieval. The repository is the unit because GitHub grants read access per repository, the grain L3 resolves against, and a product that grants read access per branch moves the unit to the branch. No product in the D6 control landscape applies a data classification to a repository, so that register is an artifact a program builds rather than one it exports, and the deep dive states it. The core page's `D6` L2 row and the protocol's D6 artifact row and interview block moved in the same diff. Shipped under [#178](https://github.com/ag0x00/ai-era/issues/178). The #283 rewrite keeps the class per repository at L2 under D6-CLASSIFY and moves the excluded paths to D6-SCOPE at L3.

### 7. No shape row for an enterprise productivity assistant (resolved 2026-09-18)

Original item: no shape table, right-sizing table or Agent Card value carried an assistant holding tools over a whole tenant's mail, files and calendar, and the one productivity-class profile, [[azure-rag-chatbot-security-profile|Azure-Native RAG Chatbot Security Profile (Copilot Studio)]], excludes by scope the external write tools that restore the [[lethal-trifecta|Lethal Trifecta]] for this shape ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 3).

Resolved by writing one row per variant, because the class earns two rows under the reference architecture's own granularity rule: the load-bearing controls change between the variants rather than the product name. The in-suite assistant, Gemini for Workspace or Microsoft 365 Copilot, takes the tenant ACL and the DLP rule as its enforcement unit. The desktop agent, [[claude-cowork|Claude Cowork]], adds local file access, MCP connectors, browser use and scheduled routines to the same mail, files and calendar reach and carries tool-mediated writes outside the tenant, so its levels move in both directions against the in-suite row:

- [[agentic-ai-security-cmm-d8-supply-chain|D8]] rises, because the member acquires connectors, skills, plugins and MCP servers that a suite customer neither builds nor loads.
- [[agentic-ai-security-cmm-d5-egress-network|D5]] falls, because the organization's code-execution egress setting does not reach the web fetch tool, the web search tool or MCP servers.
- [[agentic-ai-security-cmm-d3-control-least-agency|D3]] holds level, because what the customer gains is a permission category per connector rather than a policy decision point it operates.

Both rows are written on the core shape table, the reference architecture's shape table and all nine right-sizing tables, and the protocol's enum value names both variants. Claude Cowork holds the control surface the desktop-agent row is scored against, and records a session running in the vendor's cloud by default, with the agent loop and code execution on the vendor's servers and the cloud-session setting on for Team and off for Enterprise. The desktop agent's placement is therefore read off a setting rather than fixed on the endpoint, and what it gives the customer over the in-suite variant is administrative rather than in-path. Recommendation 16, under [#174](https://github.com/ag0x00/ai-era/issues/174), a sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168).

### 18. Callout counts across the CMM family ran over the one-per-page rule (resolved 2026-09-18)

Original item: CMM-family pages carried more callouts than the one-per-page rule allows ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 4), and the census omitted [[agentic-ai-security-cmm-crosswalk|CMM: Standards Crosswalk Matrix]], which carried three throughout. The count was debt across the family and bore on no level.

Resolved as passes touched each page, since the vault holds no lint baselines and a page is exempt only until it is touched. Nine pages came down after the first census, and the base crosswalk, the one page still carrying three, came down to none in the pass that filed item 23:

- the assessor scorecard from three, the core page from four, [[agentic-ai-security-cmm-d6-data-rag|D6]] from two, and CMM Known Limitations from three
- [[agentic-ai-security-cmm-d1-governance|D1]], [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] and [[agentic-ai-security-cmm-d7-observability|D7]] from two to none
- [[agentic-ai-security-cmm-d9-operations|D9]] from three to one, and [[agentic-ai-security-cmm-d2-identity|D2]] from two to one

[[agentic-ai-security-cmm-d8-supply-chain|D8]] carried one throughout and never needed reducing. Recommendation 29, closed under [#177](https://github.com/ag0x00/ai-era/issues/177), a sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168).

### 8. `L5` gate contradicts selective L5 (resolved 2026-09-19)

Original item: the prerequisite gate required two quarters of stable L4 across all nine domains before any domain could be scored L5, which made unreachable the selective L5 the core page and five right-sizing tables recommend ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 4, classed as blocking in Part 6).

Resolved by splitting the gate along the object each condition grades rather than by withdrawing selective L5. Condition 1, two quarters of stable L4, now grades the domain being scored L5 and the assessor repeats it per domain; conditions 2 to 4 (third-party assurance, bus-factor ≥2 with a continuity test, and the gap-closure plan) were already program-level and are graded once for the assessment. The deciding argument is that the model already accounts for cross-domain weakness along the dependency paths it records. [[agentic-ai-security-cmm-dependency-rules|CMM: Effective-Score Dependency Rules]] caps a domain's effective score at the raw scores of the domains it depends on, so a floor over all nine duplicated that work and punished weakness with no path to the claiming domain, including the architectural-containment trade-off the aggregation rule's strategic-rationale field exists to record. The whole-program tier is unchanged and now reads as a separate object: a program rated L5 holds L5 in all nine domains, and L5+ adds its own criteria on top of that rating. May's open issue 2 closed in the same diff: *stable* is now a window of the two most recent complete calendar quarters, at least four dated observation points spanning it with no gap wider than 60 days, and a regression test that counts a drop below L4 or an L4 criterion met at one point and not met at a later one. Recommendation 18, shipped under [#173](https://github.com/ag0x00/ai-era/issues/173), a sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168).

### 24. `L3` live observation demanded a gate fire from a shape with no approval queue (resolved 2026-09-19)

Original item: [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] made a live human-in-the-loop gate fire a condition of every L3-or-higher assessment and closed the escape with the rule that static configs alone do not satisfy it, with no not-applicable path, although the model right-sizes a chatbot with no tools to L3, [[agentic-ai-security-cmm-d9-operations|D9]] records that a read-only retrieval bot has no approval queue to fatigue, and the protocol's own four-verdict scheme carries such a path ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 6, classed as blocking).

Resolved by admitting the verdict that scheme already defines rather than by moving the observation to another level. The gate fire alone takes a **not applicable** verdict where the deployment's documented tier assignments place no action in the confirm tier; the reason is recorded, the criterion leaves the denominator, and the live OpenTelemetry trace and policy-decision-point decision stay required. The deciding argument is the level the fire evidences: [[agentic-ai-security-cmm-d3-control-least-agency|D3]] L3 grades the four action-risk tiers (auto, notify, confirm, block) enforced by the policy decision point, and D9 L3 grades the tamper-evident approval record, so moving the fire to L4 would have left both levels with no live evidence on every shape that does run a queue, including the in-suite and desktop-agent productivity assistants the same table targets at L3. D3's L3 list already admits a per-criterion not-applicable path for an in-process decision point, which is the same mechanism at the same level. Two limits bound the verdict: a deployment that assigns any action to the confirm tier, or that operates an approval path its tier assignments do not record, has not met the requirement where no fire is produced, and an approval path the vendor operates and exposes no test against is unanswerable rather than not applicable. [[azure-rag-chatbot-security-profile|Azure-Native RAG Chatbot Security Profile (Copilot Studio)]] records the verdict for the shape it scores. Shipped under [#261](https://github.com/ag0x00/ai-era/issues/261), a sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168).

### 25. `L5` live observation demanded a certificate the preferred scheme does not issue (resolved 2026-09-19)

Original item: the protocol's L5 live-observation bullet verified the prerequisite gate against an "AIUC-1/ISO 42001 cert dated within last quarter", and the core page's L5 auditor-evidence line asked for a "most-recent cert dated within the last quarter". [[iso-iec-42001|ISO/IEC 42001: AI Management Systems]] is the model's preferred scheme and runs an annual surveillance cycle, where [[aiuc-1|AIUC-1 AI Agent Certification Standard]] re-tests each quarter, a comparison [[aiuc-1-critical-evaluation|AIUC-1 Critical Evaluation]] sets out, so the certificate was unobtainable on the preferred path. The same gate's condition 2 already asked for assurance scheduled or current, and the protocol's bullet still named AIUC-1 first where the May pass had made the rest of the page neutral ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 6, classed as blocking).

Resolved by restating both lines as condition 2 states it: third-party assurance current or scheduled against a recognized scheme, evidenced at the cadence that scheme runs. The core page's L5 body now names ISO/IEC 42001 under active surveillance as the preferred path and records that each accepted scheme is evidenced at its own cadence. The internal inconsistency with condition 2 carries the fix on its own, and the cadence comparison is the vault's existing reading of the two schemes rather than a new claim. Shipped under [#261](https://github.com/ag0x00/ai-era/issues/261), a sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168).

### 9. `D2 L5+` vs `D3 L5` and `D5 L5` — per-task capability tokens sat at two levels (resolved 2026-09-19)

Original item: [[agentic-ai-security-cmm-d2-identity|D2]] graded per-task holder-bound capability tokens at L5+, [[agentic-ai-security-cmm-d3-control-least-agency|D3]] and [[agentic-ai-security-cmm-d5-egress-network|D5]] graded the same capability at L5 with a regulated-buyer opt-out to L5+, and [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] asked for the artifact in its D3 L5 column, so the same evidence graded one level apart depending on the page an assessor opened ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 4).

Resolved at L5+ across the three domains, by applying the recalibration method's production-maturity qualifier as that rule is written. The rule places a capability in L5+ until a production-hardened implementation path exists, and it states what satisfies a criterion instead: a product in the organization's approved-vendor pipeline with a documented production date. The D3 and D5 tooling maps record no platform-native product and one early-stage open-source implementation, which supplies neither, so the D3 and D5 reading graded a level on assemblability from available components where the rule asks for a hardened path. The opt-out inverted the qualifier's purpose as well, because the rule exists to keep the levels reachable for a buyer whose adoption cadence is externally constrained, and the opt-out made that buyer record the shortfall as its own trade-off. [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] withholds credit on the same test twice, from a canvassed vendor set holding no implementation past the two-quarter window and from a product that reached general availability inside it. D3 L5 keeps the approval token bound to the parameters a human approved, D5 L5 keeps the mesh proxy and the SSRF closure, and both domains state the capability at L5+. The protocol's D3 L5 artifact cell lost the product name it still carried and the capability-token artifact moved to the L5+ column, which closes the held half of recommendation 6; its D7 twin shipped under [#167](https://github.com/ag0x00/ai-era/pull/167). The core page's whole-program Level 3 auditor-evidence line still names a Cedar/OPA policy repository with no domain assignment, and it stays as written, because that line summarizes the program rather than grading a domain. Recommendation 17, shipped under [#169](https://github.com/ag0x00/ai-era/issues/169), a sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168).

### 19. `D3 L3` graded a decision point the coding harness holds itself (resolved 2026-09-19)

Original item: [[agentic-ai-security-cmm-d3-control-least-agency|D3]] L3 requires a policy decision point outside the model context, deny-by-default, synchronous and failing closed, and the measurement protocol asks for a PDP config, a PDP-unreachability test showing deny and a direct-gateway invocation test showing deny. A coding harness enforcing a managed permission policy is the enforcement point and the governed component at once, so it produces none of the three artifacts, and the assessor either records the circularity or scores L2 ([[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], Part 2).

Resolved in the evidence rows rather than in the level, because the item's first point does not hold. "Outside the model context" names one property, that an instruction reaching the model's context cannot rewrite the decision the enforcement point issues, and the D3 L3 criterion now states that property in the criterion itself. A harness resolving a managed permission policy holds it: the policy arrives from an administrative scope the session cannot write, and the harness decides before the tool call runs. The item's second point holds. Two of the L3 artifacts assume an interface a tester can address, and D3 L3 already recorded the direct-invocation test as not applicable for an in-process decision point exposing none, which disposed of one of the two; the unreachability test carried no such path. [[agentic-ai-security-cmm-measurement-protocol|The measurement protocol]] now names four substitute artifacts — the resolved policy read from an enrolled device with its administrative scope, the write-deny holding the model out of that scope with the audit record of in-session changes, a deny observed under the most permissive autonomy mode the deployment permits, and the documented behavior when the policy source is absent or malformed — and two limits that bound the substitution: a decision point outside the hosting runtime anywhere on the call path restores the original tests for the calls that cross it, and a vendor-operated enforcement point exposing neither a customer test nor inspectable output scores unanswerable rather than met. The circularity is recorded rather than removed, and it is narrower than this item stated. The enforcement point sits outside the model's context and inside the vendor's software, so vendor code interprets a customer-administered policy file and every artifact comes from the code path that policy governs. Injection resistance survives that; the independence of the evidence does not, and three records the harness does not hold narrow it. Recommendation 30, shipped under [#170](https://github.com/ag0x00/ai-era/issues/170), a sub-issue of [#168](https://github.com/ag0x00/ai-era/issues/168).

### 3. `D2 L5` — Microsoft Agent 365 Registry "or equivalent" was underspecified (resolved 2026-09-23)

Original item: [[agentic-ai-security-cmm-d2-identity|D2]]'s clause referenced "Microsoft Agent 365 Registry or equivalent unified governance." Agent 365 GA was 2026-05-01, and deployment evidence was possible but not yet published at scale. "Or equivalent" softened the dependency on a single vendor, but the criterion did not state which capabilities an equivalent must match, so an assessor had no basis for grading a non-Microsoft deployment against it, and a CISO at L5 had to either pick Agent 365 or build the equivalent capability set.

Resolved by stating the capability set as criteria. [[agentic-ai-security-cmm-d2-identity|D2]]'s L5 names each capability an equivalent must match as its own criterion, with the evidence an assessor collects, so a non-Microsoft deployment is graded on the same criteria:

- D2-REGISTRY and D2-AUDIT, for the registry, its lifecycle API and its audit integration.
- D2-OWNER-TRANSFER and D2-ADMIN, for ownership transfer and scoped administration.
- D2-DISCOVER, D2-CONDITIONAL, D2-IDENTITY-ATTEST and D2-COUPLING-ZERO, for the rest of the level.

### 5. `D6 L3+` — `IDENTITY.md` / `SOUL.md` filename conventions are not industry-standard (resolved 2026-09-25)

The CMM mandated SHA-256 of `SOUL.md` / `IDENTITY.md` / system prompts as cognitive file integrity. These specific filenames are vault conventions inherited from this project; in practice agent identity files use a wider variety of names (`.cursorrules`, `Claude.md`, `claude.md`, `.augment-guidelines`, `system_prompt.txt`, vendor-specific paths). The 2026-05-06 verification overlay added the AIUC-1 B008.6 anchor for the underlying primitive (cryptographic checksums for tamper detection), but the *scoping* to identity files is the load-bearing CMM contribution, and the file-set being protected is not yet standardized.

Resolved by a discovery rule in both criteria that grade the primitive. [[agentic-ai-security-cmm-d6-data-rag|D6]]'s D6-INSTRUCT, at D6 L3, ranges over each file an agent loads as standing instructions, whoever writes them, and names `IDENTITY.md`, `CLAUDE.md` and `.cursorrules` only as examples. [[agentic-ai-security-cmm-d8-supply-chain|D8]]'s D8-VERIFY-INSTRUCT, at D8 L4, reads the same object and verifies each file against its publisher's signature or a release's pinned digest. The core page's appendix of contributions beyond reviewed standards states the primitive over the same range, with `SOUL.md` and `IDENTITY.md` as examples.

## Contribution guide

When a future review surfaces a new CMM limitation:
1. Add a numbered subsection under **Still-current limitations** with the concrete CMM clause cited.
2. Tag with `[verified-current]` (primary-source-checked), `[wiki-summary]` (only summary checked), or `[new-YYYY-MM-DD]` (newly identified).
3. Recommend a fix, or record that the limitation is structural and therefore outside what CMM language can fix.
4. When a CMM revision resolves it, move the item to **Limitations addressed by CMM revisions** and summarize the resolution there, as an h3 heading with a plain paragraph. The wiki allows one callout per page and this page now carries none, so a `[!check]` on a resolved item fails the callout lint at pre-push twice over, once on the ceiling and once because the resolution belongs in the prose.

Per-standard reviews from the audit backlog ([[standards-validation-methodology-2026-05|Standards Validation Methodology]]) will likely surface additional CMM limitations as they execute. Those should be filed here as well.

## Relations

- Replaces: [[agentic-cmm-vs-standards-validation|Validation page]] §5 (which is now demoted to historical snapshot).
- Targets: [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]].
- Methodology: [[standards-validation-methodology-2026-05|Standards Validation Methodology]] for how new limitations are sourced and verified.
