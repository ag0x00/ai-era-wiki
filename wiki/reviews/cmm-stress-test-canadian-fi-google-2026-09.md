---
type: review
title: "CMM Stress Test: Canadian FI on Google Cloud"
address: c-676733
created: 2026-09-15
updated: 2026-09-24
tags:
  - reviews
  - cmm
  - stress-test
  - canada
  - google-cloud
  - claude-code
  - gemini
status: developing
scope_axis:
  - sec-of-ai
origin: produced
target: "[[agentic-ai-security-cmm-2026]]"
methodology: "[[cmm-calibration-stress-test-2026]]"
reviewer: "wiki (pass of 2026-09-15)"
review_completed: 2026-09-15
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk-canada-fi]]"
  - "[[agentic-ai-security-cmm-crosswalk-us-fi]]"
  - "[[cmm-calibration-stress-test-2026]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[canadian-bank-secure-sdlc-ai-assessor-scorecard]]"
  - "[[cmm-known-limitations]]"
  - "[[claude-code-control-sheet]]"
  - "[[google-cloud-agentic-security-profile]]"
  - "[[claude-code]]"
  - "[[securing-agentic-coding]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[azure-rag-chatbot-security-profile]]"
  - "[[oversharing-controls]]"
  - "[[ai-coding-agent-governance]]"
  - "[[harness-config-as-supply-chain-artifact]]"
  - "[[secure-sdlc-framework-stack-2026]]"
  - "[[google-saif]]"
  - "[[lethal-trifecta]]"
  - "[[osfi-b-13]]"
  - "[[osfi-e-23-2027]]"
sources:
  - "https://code.claude.com/docs/en/managed-settings"
  - "https://code.claude.com/docs/en/iam"
  - "https://code.claude.com/docs/en/google-vertex-ai"
  - "https://support.google.com/a/answer/15698295"
  - "https://knowledge.workspace.google.com/admin/security/about-dlp-for-gemini"
  - "https://knowledge.workspace.google.com/admin/generative-ai/generative-ai-in-google-workspace-privacy-hub"
  - "https://knowledge.workspace.google.com/admin/gemini/explore-the-ai-control-center"
  - "https://docs.cloud.google.com/security-command-center/docs/model-armor-overview"
  - "https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-risk-management"
  - "https://www.osfi-bsif.gc.ca/en/risks/technology-cyber-risk-management"
  - "https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/guideline-e-23-model-risk-management-2027"
  - "https://www.antiy.net/p/clawhavoc-analysis-of-large-scale-poisoning-campaign-targeting-the-openclaw-skill-market-for-ai-agents/"
verified: 2026-09-24
verified_against: []
verified_findings: 1
verified_note: "Read against the 4efaed7 vault pages it scored, the 12 status-table issues, PR 242 and 306, and the vendor and OSFI URLs; fixed Shape 1 typical, Shape 2 weakest, the deep-dive target sentence, :417, the Part 6 held-flags claim, the residency absence and the GA-grade qualifier; forward-linked the D2 attribution absence; open: the Shape 1 weakest cell omits D3 and the D4 cap (low)"
---

# CMM Stress Test: Canadian FI on Google Cloud

The September stress test re-runs the May 2026 recalibration stress test ([[cmm-calibration-stress-test-2026|CMM Calibration Stress Test: Cumulative-Floor Rule]]) with its method unchanged and a new customer. May scored a US credit union on Microsoft E5 running one member-facing RAG bot ([[agentic-cmm-regulated-fi-stress-test|Agentic AI CMM: Regulated-FI Stress Test]]); September scores a Canadian federally regulated financial institution on Google Cloud and Google Workspace in two shapes May did not hold, [[claude-code|Claude Code]] for software development and Gemini in Workspace for the whole organization's daily work.

**The May verdict that regulated financial services score a floor of L3 and that the model treats them fairly holds only while the incumbency is Microsoft; on Google Cloud both September shapes land at L2 typical, and the cause is a product-and-cost layer that prices nine domains against an E5, Azure, AWS or GitHub estate and carries no Google licensing column in any of the nine.** The ladders survive the swap. The mapping underneath them does not, and one of the two shapes has no row in any of the model's three shape taxonomies.

[[#Status since the review|Status since the review]] lists the decisions taken after 2026-09-15; [[claude-code-control-sheet|Claude Code Control Sheet]] and [[google-cloud-agentic-security-profile|Google Cloud Agentic Security Profile]] carry each shape's current controls.

## Part 1 — Scope and method

### The persona and the lens

The persona is a Schedule I-class bank or large federally regulated insurer, subject to [[osfi-b-13|OSFI Guideline B-13: Technology and Cyber Risk]], [[osfi-e-23-2027|OSFI Guideline E-23: Model Risk Management]] from 2027-05-01,[^e23date] PIPEDA, Quebec Law 25's automated-decision obligations for Quebec-resident customers and FINTRAC reporting. Workloads and data run on Google Cloud, mail, documents, calendar and chat on Google Workspace, and identity on Google Cloud Identity federated to the bank's provider. The bank has mature technology-risk, model-risk and third-party-risk functions, a 12 to 18 month procurement cycle for a new vendor, 3 to 5 full-time equivalents of AI-security capacity, and a board and supervisor expecting written evidence.

Its principal security architect tests every rung on four counts:

- **Evidence.** The bank can produce what the rung names on this stack. A rung failing only this count is a cost finding.
- **Fit.** The rung asks for the right control for this shape. A failure is a calibration finding.
- **Examination.** An OSFI examiner would accept the evidence against B-13 or E-23. [[agentic-ai-security-cmm-crosswalk-canada-fi|CMM: Canadian Regulated-Finance Crosswalk]] answers this count rather than the rung.
- **Consistency.** The rung reads the same on the core page, the domain page and the assessor's handbook. A failure is a defect.

### The two shapes

**Shape 1 is Claude Code in the hands of the bank's developers.** It reaches source repositories through its in-process file tools, shell commands and their children through the sandbox, continuous-integration runners through a GitHub Action, third-party capability through MCP servers and its own lifecycle through hooks, and most likely infers through Google Cloud's Agent Platform rather than the first-party API.[^vertex] Managed settings arrive by device management or from the organization's account, and each key governs at its resolved value, which an assessor reads rather than assumes.[^managed]

**Shape 2 is Gemini in Google Workspace for every employee**, reaching Gmail, Drive, Docs, Calendar and Chat through the side panels and the Gemini app's Workspace extensions. It reads mail and files, writes drafts, documents and sharing changes, and is administered per organizational unit in the Admin console, with data-loss-prevention rules bounding its data sources[^gemini-ou][^gemini-dlp] and customer content under a stated training restriction.[^gemini-privacy]

### Method

Each domain was scored against its deep dive's ladder and right-sizing target, with the persona's evidence capacity judged per rung. The nine ladders were read on 2026-09-15 at their 2026-09-10 revision:

- [[agentic-ai-security-cmm-d1-governance|CMM D1: Governance and Accountability]]
- [[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]]
- [[agentic-ai-security-cmm-d3-control-least-agency|CMM D3: Control and Least-Agency]]
- [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]]
- [[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]]
- [[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]]
- [[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]]
- [[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Supply Chain and AI-BOM]]
- [[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]]

Effective scores follow the three active rules of [[agentic-ai-security-cmm-dependency-rules|CMM: Effective-Score Dependency Rules]] and report its three-number headline of typical, weakest and strongest with no floor, because the single-floor rule the May page recommended keeping was retired on 2026-05-04. Eight product facts come from vendor documentation fetched on 2026-09-15 and footnoted; a control claim with neither footnote nor vault citation is stated as an absence.

## Part 2 — Shape 1 scored: Claude Code for software development

The [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] sets this shape at L4 across all nine domains, with L5 justified in D2, D4, D8 and D9. The deep dives set targets up to one rung lower in eight domains, listed in the third column below, and keep L4 in D8, which they call the heaviest domain for the shape. [[securing-agentic-coding|Securing Agentic Coding]] catalogs the controls cited below.

| Domain | Rung the persona can evidence | Rung the shape needs | The stakeholder's objection |
|---|---|---|---|
| D1 | L3 | L3 to L4 | The coding-shape control catalog carries no D1 coordinate, and the L4 crosswalk matrix is built against anchors the model does not admit |
| D2 | L2, L3 interactive only | L3 to L4 | No first-party feature attributes a change to the agent or the human, and the organization pin does not cover cloud-provider sessions |
| D3 | L2 to L3 | L3 to L4 | L3 asks for a policy decision point outside the model context; here the decision point is the harness |
| D4 | L3 | L3 to L4 | The L4 spine is preview and experimental on the model's own grading, and the groundedness control is English-only |
| D5 | L2 effective, L3 raw | L3 | A hostname allowlist without TLS termination is a misconfiguration control, and TLS termination is experimental |
| D6 | L2 to L3 | L3 | The L3 spine is answer-time entitlement enforcement, which does not describe a repository |
| D7 | L2 effective, L3 raw | L3 to L4 | Routing inference to the bank's own cloud removes the first-party analytics, and the telemetry content gates are off by default |
| D8 | L2 to L3 | L4 | Fleet inventory is the load-bearing evidence for this shape and no rung grades it |
| D9 | L2 to L3 | L3 to L4 | The catalog carries no D9 coordinate, and the accidental-meltdown threat model has no control ordered against it |

### D2 decides three domains, and its gap is attribution

Per-agent identity caps effective D5 and D7. Managed `forceLoginMethod` and `forceLoginOrgUUID` pin a session to a known organization, with federated single sign-on on the Enterprise plan, which is L3 for the interactive shape. Enforcement is per-path and leaves cloud-provider sessions unblocked, so cloud identity-and-access policy governs the routing this bank will choose.[^iam] D2 L3 requires every action to trace to a human; the bank traces a session to a human and cannot trace a commit to an agent, the coding-shape test the domain page names. No harness vendor offers first-party attribution of a change to agent or human, and the vault's one commercial instrument for it is a single vendor's product with no dated general-availability announcement and no independent evaluation.

### D3 grades a decision point the harness holds itself

The vault records five managed-settings controls:

- a permission policy local settings cannot override
- deny rules honored under sandbox auto-allow and the skip-permissions flag
- content-scoped prompts no autonomy mode suppresses
- two keys locking read paths and domains
- audit or blocking of in-session settings changes

Vendor documentation adds the MCP-admission and customization-lockdown keys Part 5 lists.[^managed] That is a stronger position than the vault records and still short of the rung: D3 L3 asks for a policy decision point outside the model context, deny-by-default, synchronous and failing closed. The enforcement point sits inside the harness it governs, the Cedar-routed reference monitor is a practitioner prototype, and the harness has no model-level hook, so interception reaches only the tool and shell layer ([[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar]]). The bank either records the harness as its own decision point and accepts the circularity, or scores L2; D3 warns that scoring on a shell-pattern allowlist overstates by roughly a level.

### D4 reaches L3, and the L4 target rests on preview controls

The sandbox, read and write confinement, the fail-closed keys and the container reference configuration give the interactive shape L3, with three coverage facts an assessor records rather than assumes:

- Sandbox read access defaults to the whole filesystem minus explicit denies.
- The built-in boundary covers shell commands and their children and leaves the file tools, MCP servers and hooks outside; [[anthropic-sandbox-runtime|Anthropic Sandbox Runtime]], a beta research preview, closes that gap.
- `filesystem.disabled` turns the filesystem layer off and leaves network isolation on, which vendor documentation flags as self-escalating.

The target is the defect. The core page asks for L4 in every domain while D4 states that its L4 spine rests on preview, experimental or specification-only controls, so a defensible L4 is assembled today from preview and open-source components. Recalibration rule 2 admits those once they sit in the approved-vendor pipeline with a documented production date, so the bank's L4 rests on that accommodation rather than on generally available controls. D4 also discounts the English-only limit of groundedness detection as moot for a US English member bot, a discount unavailable to an institution with French-language service obligations.

### D5 and D7 are capped by identity, and both carry a shape-specific finding

A strict allowlist with managed-only domains takes raw D5 to L3 and session-correlated OpenTelemetry export takes raw D7 there, and raw D2 bounds both at L2, so a gateway and a trace backend leave the reported score unchanged until identity is in production: the dependency rules working as designed, and worth stating to a budget holder before the spend. The default proxy decides from the client-supplied hostname without inspecting TLS, so a broad allowlist entry stays reachable by domain fronting, and the TLS-terminating option shipped experimental. Export redacts content by default, carrying token counts, costs, durations, permission-mode transitions and connection status and no command strings, prompt text or tool arguments, so it answers what an agent cost and cannot answer what it ran. Routing inference through Google Cloud's Agent Platform loses the first-party analytics dashboards and, under zero-retention terms, the contribution metrics; OpenTelemetry keeps working and is the documented rebuild path ([[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]).

### D8 carries the highest target and an ungraded evidence dimension

An AI bill of materials at build, dependency scanning with lockfile enforcement, registry provenance and pre-install scanning for MCP servers and skill packs, and signing of the bank's own agent artifacts reach L3 against a target of L4. L4 would rest on a fleet inventory (which harnesses, at which versions, with which MCP servers, attributable to which human), a dimension D8 states its L3 and L4 criteria omit and names a candidate for the next revision. The catalog files the same inventory under D7 as a runtime AI-BOM, one artifact serving two domains until a rung names it. The inventory's only commercial instrument in the vault is the D2 attribution row's single vendor, which a third-party-risk function applying OSFI Guideline B-10 treats as a concentration finding rather than a control. Five recorded incidents sit behind the domain:

- a worm typosquatting the harness package to inject MCP server configurations ([[sandworm-mode-npm-worm|SANDWORM_MODE npm Worm: AI Toolchain Poisoning]])
- more than a thousand malicious skills[^clawhavoc-count] on a marketplace ([[clawhavoc|ClawHavoc: Agentic Skill Marketplace Supply Chain Attack]])
- a package install driven from an issue title ([[clinejection|Clinejection: AI Attacks AI via GitHub Issue Title]])
- credential theft through editor packages ([[cursor-npm-credential-stealer|Cursor npm Credential Stealer]])
- injection to command-and-control against another vendor's coding agent ([[jules-ai-kill-chain|Jules AI Kill Chain: Injection to Remote Control]])

### D1 and D9 are the two domains the control catalog does not reach

The catalog carries CMM coordinates for D2 through D8 and none for D1 or D9, the two domains an examiner opens first at a bank. D9 grades guardrail fail behavior, decommission and credential rotation on owner departure, human-in-the-loop queue monitoring and an incident-response runbook naming the regulatory-notification path, all present in this shape and none mapped by the catalog. In the continuous-integration variant an unsandboxed file read reaching the process environment turned a triage workflow into credential exposure ([[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]), and the same primitive appeared independently against another vendor ([[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]). The catalog orders every control against an adversary, and the accidental-meltdown case, which a change-management function asks about first, passes through content filtering untouched.

### Shape 1 headline

| Shape | Typical | Weakest | Strongest | Range | Target the model sets |
|---|---|---|---|---|---|
| Claude Code for software development | L2 | L2, set by D2 and D9, with D5 and D7 capped to L2 by raw D2 | L3 | L2 to L3 | L4 across all nine |

The gap to the L4 target is a finding against the target, set for a shape whose heaviest domain's load-bearing evidence the ladder does not grade and whose L4 runtime spine the model itself grades as preview. It is no finding against the bank.

## Part 3 — Shape 2 scored: Gemini for day-to-day tasks

This shape has no target because it has no row in any of the model's three shape taxonomies:

- The core page's shape table carries seven applications, none an enterprise productivity assistant with tools over mail, files and calendar; the nearest are a web or desktop chatbot with no tools and a RAG application with no whole-tenant identity and entitlement surface.
- The handbook's Agent Card admits chatbot, RAG, MCP server or mesh plus five coding-agent variants, so the field that drives right-sizing cannot be filled (Part 7 row 1 adds the value).
- All nine right-sizing tables name a chatbot, a RAG bot, a copilot, an MCP provider or a mesh, and never a productivity assistant.

The eleven-row shape table of [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] shares the absence, so the gap sits in both anchors.

| Domain | Rung the persona can evidence | Rung the shape needs | The stakeholder's objection |
|---|---|---|---|
| D1 | L3 | L3 to L4 | "Per agent type" has no referent when the deployment is one service with per-unit toggles and an agent population equal to the headcount |
| D2 | L2 | L3 | The assistant acts as the employee, so there is no non-human identity to attest and the rung needs a not-applicable verdict the handbook cannot record |
| D3 | L1 to L2 | L3 to L4 | The tool-call mediation the rung grades runs inside the vendor and the customer cannot evidence it |
| D4 | L2 | L3 to L4 | The runtime guardrail is the vendor's four-layer architecture, sourced to a conference talk, and Model Armor is not documented against Workspace traffic |
| D5 | L1 to L2 | L3 | No in-path position between agent and tool belongs to the customer |
| D6 | L2, L3 in part | L3 to L4 | Oversharing is the live risk and the only first-party answer-time instrument the vault names is Microsoft's |
| D7 | L2 | L3 | Telemetry the organization holds does not exist for a software-as-a-service assistant, and the log-integrity test has no not-applicable path |
| D8 | L2 | L2 to L3 | The consumer split helps, and the bill of materials for this shape is the vendor's model card |
| D9 | L2 to L3 | L3 | The involvement measure D9 L4 asks for exists as a vendor-side override rate the customer cannot read |

### Twelve control absences against a strong threat record

A check of the vault's coverage confirmed twelve absences:

- no page on Gemini for Google Workspace as a product
- none on Workspace admin controls for Gemini
- no Workspace data-loss-prevention coverage
- no record of Workspace extensions as a tool surface
- no context-aware access coverage outside one CMM domain page
- no Chrome Enterprise coverage as an assistant control
- no Canadian-region record for any AI service, and no data-residency record for either shape
- no zero-retention page
- no Vertex or Agent Platform control page
- no Gemini Enterprise product page
- no Agentspace mention
- no placement of Model Armor against Workspace traffic

The only Gemini product page, [[gemini-cli|Gemini CLI]], covers a developer command-line tool outside this shape, and [[google|Google]] carries the vendor record without an administrative control inventory. The threat record is strong:

- zero-click indirect injection into Gemini Enterprise through a shared document, a calendar invite or a forwarded mail, exfiltrating mail, calendar and document content through an auto-loading image ([[geminijack-gemini-enterprise-injection|GeminiJack Gemini Enterprise Zero-Click Injection]])
- the same structural flaw in another vendor's assistant through one crafted mail, chaining four bypasses including an allowlisted link-preview proxy ([[echoleak-copilot-zero-click|EchoLeak Zero-Click Copilot Exfiltration]])
- an auto-run prompt parameter reaching connected mail, drive and calendar, with memory poisoning that survives a password change, a session revocation and device re-enrollment ([[cosnitch-copilot-personal-exfiltration|CoSnitch: Copilot Personal Data Exfiltration]])

The vendor's own account names indirect prompt injection as the class's main risk and describes four layers against it ([[securing-workspace-genai-at-google-talk|Securing Workspace GenAI at Google]]), listed on [[google-cloud-agentic-security-profile|Google Cloud Agentic Security Profile]]. Three of the four are unevidenceable from the customer's side, which holds D3, D4 and D5 at L1 to L2, and [[google-saif|Google SAIF: Secure AI Framework]] closes nothing, because it names control categories without controls, thresholds or test procedures. What the customer holds is real and narrow: per-organizational-unit enablement, data-loss-prevention rules bounding what the assistant reaches, an administrative control centre with a usage review, and a stated training restriction over customer data.[^gemini-ou][^gemini-dlp][^gemini-aicc][^gemini-privacy] Model Armor is outside that set: its overview names no Gmail, Docs or Drive integration among its eight integration points and states no launch stage for the core service.[^armor]

### The trifecta reading, and the profile that excludes this shape

The vault's one profile for a productivity-class assistant, [[azure-rag-chatbot-security-profile|Azure-Native RAG Chatbot Security Profile (Copilot Studio)]], excludes by its scope note the property that makes this deployment dangerous. It covers a closed-corpus chatbot that grounds on internal content, answers users and lacks an external communication or write path, so the [[lethal-trifecta|Lethal Trifecta]] is broken by architecture and its required levels fall low across D3, D4, D5, D7 and D9. A Workspace assistant restores all three legs: it reads private data across mail and drive, ingests untrusted content on every summarization of an inbox or a calendar, and writes and shares, an external communications path. [[oversharing-controls|Oversharing Controls for AI Search]] owns the failure mode without a per-plane control set or a target level, and [[indirect-prompt-injection|Indirect Prompt Injection]], graded at D4 L3, reaches this shape through the retrieval path the product is sold for.

### Shape 2 headline

| Shape | Typical | Weakest | Strongest | Range | Target the model sets |
|---|---|---|---|---|---|
| Gemini in Workspace for day-to-day tasks | L2 | L1 to L2 across D3, D4 and D5, with D4 capped by raw D3 and D5 and D7 capped by raw D2 | L3, D1 | L1 to L3 | None; no shape row exists |

## Part 4 — The CMM in September against the May recommendations

Line references are to the revision read on 2026-09-15, before the Part 7 changes.

### Disposition of the ten May items

| No. | May item | Disposition | Evidence |
|---|---|---|---|
| 1 | Change 1, adopt the L5 and L5+ split | ADOPTED | agentic-ai-security-cmm-2026.md:119-120,140-142; an L5+ bullet on all nine deep dives; …-measurement-protocol.md:193,232 |
| 2 | Change 2, matrix primary with the floor as headline | PARTIAL, adopted past the recommendation | Matrix primary at agentic-ai-security-cmm-2026.md:126; the other three limbs sit below the table |
| 3 | Change 3, document the five archetypes | PARTIAL | A scoring comparison at …-dependency-rules.md:188-194, no profiles anywhere; agentic-ai-security-cmm-2026.md:550 tracks the deployment archetypes, a different taxonomy |
| 4 | Change 4, D7 acknowledges architectural containment | ADOPTED | agentic-ai-security-cmm-2026.md:364; …-d7-observability.md:163; the field at …-dependency-rules.md:85, filled at :166. The precondition is encoded nowhere, and :212 records that rules can only cap |
| 5 | Change 5, the L4 to L5 prerequisite gate | ADOPTED, one condition altered and one dropped | agentic-ai-security-cmm-2026.md:196-198; …-measurement-protocol.md:236-243,231; the three condition changes sit below the table |
| 6 | Open issue 1, track the first L5+ claimant | NOT ADOPTED | agentic-ai-security-cmm-2026.md:419 names who L5+ suits and no claimant; :552 reframes the question |
| 7 | Open issue 2, define stable L4 for two quarters | PARTIAL | The matrix-regression limb stated at agentic-ai-security-cmm-2026.md:198, …-measurement-protocol.md:238; the incident and drift limbs undefined; the look-back window has no independent length |
| 8 | Open issue 3, archetype accuracy against audit data | NOT ADOPTED | …-dependency-rules.md:188-194 re-runs the same first-principles scores; no real assessment is recorded |
| 9 | Open issue 4, the multi-cloud archetype's L5 path | NOT ADOPTED | …-dependency-rules.md:194 records the archetype's headline with no L5 in it and no path to one; the nearest text is an L5+ item at …-d5-egress-network.md:94 |
| 10 | Open issue 5, pre-empt L5+ aspirational drift | ADOPTED | agentic-ai-security-cmm-2026.md:417,119; …-measurement-protocol.md:245 |

Tally: four adopted, three partial, three not adopted.

**Item 2 resolves into four limbs.** The per-domain matrix is primary on the core page and in the handbook (agentic-ai-security-cmm-2026.md:126, …-measurement-protocol.md:249), and joint disclosure of that matrix is mandatory (…-measurement-protocol.md:258, …-dependency-rules.md:201). The floor headline was rejected outright (agentic-ai-security-cmm-2026.md:130, …-dependency-rules.md:57). Floor vocabulary survives in three places: …-measurement-protocol.md:82 and :241, and agentic-ai-security-cmm-2026.md:198.

**Item 5 altered one condition of the gate and dropped another** (agentic-ai-security-cmm-2026.md:196-198, …-measurement-protocol.md:236-243 and :231): the certification condition became scheme-neutral, the named-contributor condition was dropped, and a floor-domain gap-closure condition was added.

### Disposition of the regulated-FI page's closure conditions

| No. | Closure condition | Disposition | Evidence |
|---|---|---|---|
| 1 | A regulated-FI deployment profile for the RAG-bot archetype | NOT ADOPTED as a page | The pieces sit across the nine right-sizing and cost sections plus [[agentic-ai-security-cmm-crosswalk-us-fi\|CMM: US Regulated-Finance Crosswalk (FFIEC and GLBA)]]; no page assembles them |
| 2 | A US regulated-finance crosswalk | CLOSED | agentic-ai-security-cmm-crosswalk-us-fi.md:42-50,54-60 |
| 3 | A single-stack reading per platform, including Google Cloud only | NOT ADOPTED | The condition's own line is its only occurrence in the vault, and this is the condition that bites hardest on this persona |
| 4 | A cost model separating licensing from labor and log run-rate | CLOSED | Recalibration rule 3 and a per-level cost table on all nine deep dives |

### Six systematic biases this run surfaces

**The product and cost layer assumes a Microsoft, Azure, AWS or GitHub incumbency, and what it carries for Google is unpriced and stops at the platform primitives.** All nine cost models price licensing against that incumbency and none prices Google Cloud or Workspace. On the core page the absence is total:

- the tooling map has three columns, standards, open source and commercial, with no platform-native column and no Google vendor in any row
- Model Armor occurs zero times
- Vertex appears once, inside a claim about a Microsoft product's discovery coverage
- the one Google Cloud occurrence is a single identity service named as generally available

Seven of the nine deep dives carry a GCP platform-native column, with Agent Identity at D2, Model Armor at D4 and at D5 beside Apigee, and Google SecOps at D7; D3 records its column as thin for want of a declarative decision point. Neither Claude Code nor Gemini appears in any tooling-map row, so this persona's mapping has to be rebuilt.

**The missing shape is missing in three places** (Part 3), and the reference architecture repeats the absence, so the model and the architecture fail the same shape identically.

**The prerequisite gate into L5 contradicts selective L5.** The gate requires two quarters of stable L4 across all nine domains before an assessor may score any domain L5 (…-measurement-protocol.md:236-238, wired into rubric score 5 at :231), while the core page expects L4 across all domains with selective L5 where exposure justifies it (agentic-ai-security-cmm-2026.md:417) and five right-sizing tables name selective L5 as a realistic target. A program legitimately at L2 in one domain by recorded trade-off can never be scored L5 in another, whatever that domain's program does.

**One capability sits at two rungs.** Per-task holder-bound capability tokens are an L5+ criterion on D2 (…-d2-identity.md:97) and an L5 criterion on D3 and D5 (…-d3-control-least-agency.md:108, …-d5-egress-network.md:104), and the handbook asks for the artifact in its D3 L5 column, so the same evidence grades one rung apart depending on the page opened.

**D6 alone omits the production-maturity preamble.** Eight ladders open with the rule that a control counts when it operates in production; D6 opens with the rule-1 sentence only (…-d6-data-rag.md:88).

**Callout counts run over the one-per-page rule across the family:** two each on D1, D2, D4, D6 and D7, three on D9, one each on D3, D5 and D8, three on the scorecard and four on the core page, recorded as known debt rather than a finding.

### Verdict

The ladders survive the platform swap, the run's principal finding: every objection in Parts 2 and 3 concerns evidence the bank can produce or a control mapped to a product, none a criterion that stops making sense on Google Cloud. Rule 1 of [[agentic-ai-security-cmm-recalibration-method-2026|CMM: Recalibration Method (Cadence and Cost)]] holds: a rung requiring per-task scoped authorization with cryptographic binding reads the same against Cloud Identity as against Entra, where a rung naming a product would have needed rewriting. The cadence qualifier serves a buyer on a 12 to 18 month cycle: rule 2 admits the preview and open-source components of D4's defensible L4 once they sit in the approved-vendor pipeline with a production date, which tells the bank which half of the L4 band it can budget for. The dependency rules absorb an unmodeled shape correctly: shape 2 holds no per-agent identity by design, so D2 is structurally L2, and the rationale for D2 capping D7, that a detector below D2 attributes an anomaly to the fleet rather than to an agent, fits a whole-tenant assistant, where the assistant is the fleet. The effective-score definitions and the strategic-rationale field carry that shape's honest low scores without reporting the program as L1 overall.

What does not stand up is everything between the ladder and the buyer:

- The product layer is single-stack, and the cost models answer a question this bank did not ask.
- Shape 2 has no row in any taxonomy the model or the architecture carries, so right-sizing cannot apply to it.
- The mandatory evidence-tag set admits no jurisdictional anchor, so the vault's Canadian and US crosswalks cannot be cited in the instrument that requires a crosswalk extract.
- The handbook, read as an assessor would execute it, does not run.

## Part 5 — Control-mapping review, both shapes

The mappings below are as read on 2026-09-15; Part 7 records which were corrected in the same pass and which were held.

### Shape 1, the coding-agent note against a Google Cloud bank

The core page adds four evidence items for a coding-tool deployment at L3 and above, sourced to [[ai-coding-agent-governance|AI Coding Agent Governance]]:

| Item | Evidenceable |
|---|---|
| Agent rules-file integrity | Partly |
| IDE extension provenance | Yes |
| Typosquat and dependency-hijack defense | Yes |
| Destructive-action classification | Yes |

- **Rules-file integrity is evidenceable against a different file set than the item names.** The item names Cursor rules, Copilot Workspace rules and a Claude `IDENTITY.md`; this harness is configured through `CLAUDE.md`, `.claude/settings.json`, managed settings, hooks and MCP manifests, the set [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] describes. [[cmm-known-limitations|CMM Known Limitations (current state)]] item 5 records the filename convention as non-standard while the core page still names the file.
- **Endpoint management delivers the IDE-extension allowlist.** A command-line harness's equivalent surface is plugins and skills, whose managed keys, `disableSideloadFlags` and `strictPluginOnlyCustomization`, appear in no vault control row.[^managed]
- **Dependency scanning with lockfile enforcement delivers the typosquat and dependency-hijack item**, which names two products inside the criterion, against recalibration rule 1.
- **Destructive-action classification runs on two permission keys**, `permissions.ask` with an argument pattern and `permissions.deny` honored under autonomous modes; the item points at a decision-rights matrix and names no harness key.

Four mappings were missing or stale, each with its replacement:

- **No MCP-allowlist row exists in either instrument**, and the catalog's lockdown row names two keys. The replacement is a control-plane row pinning the admitted MCP servers and locking the four customization types, naming `allowedMcpServers`, `deniedMcpServers`, `allowManagedMcpServersOnly`, `managedMcpServers`, `managed-mcp.json`, `allowManagedPermissionRulesOnly`, `disableBypassPermissionsMode`, `disableSideloadFlags` and `strictPluginOnlyCustomization`, graded first-party and generally available, with the fail-closed behavior of a malformed allowlist stated.[^managed]
- **No row covers the routing this bank will choose.** One environment variable enables Claude Code on Google Cloud's Agent Platform, another pins the region with a `us-east5` default and no Canadian region named, a cloud role authorizes it, and the organization pin does not cover those sessions (Part 2, D2). The replacement is an identity-plane row carrying that split and an observability-plane note on the lost first-party analytics and the OpenTelemetry rebuild path.[^vertex][^iam]
- **D1 and D9 have no coordinate in the coding-shape control set.** The replacement is a D1 row for harness-fleet ownership and an approved-harness register with decision rights per repository risk tier, and a D9 row treating the permission-prompt stream as the human-in-the-loop queue, with rubber-stamp rate, departing-developer credential rotation, and guardrail fail behavior when the sandbox is unavailable. Both assign new CMM coordinates, so both are held for a calibration decision.
- **One wikilink resolves to the wrong product.** securing-agentic-coding.md:69 and :186 link the harness as [[claude-code-security|Claude Code Security]], the vulnerability-discovery product in a limited research preview rather than the harness security model. The replacement reserves that link for the product and names the harness in plain text with a documentation citation.

### Shape 2, a mapping that does not exist

No control mapping exists to review, because no instrument carries the shape (Part 3). The proposed core-table row:

| Application | Realistic target (most enterprises) | Domains where L5 is justified |
|---|---|---|
| Enterprise productivity assistant with mail, file and calendar tools (Gemini for Workspace or Copilot M365 class) | L3 across all, L4 in D6 and D9 | D6 (answer-time entitlement over a whole-tenant corpus), D4 (indirect-injection defense in the retrieval path), D9 (disclosure and approval integrity at headcount scale) |

D6 carries the target because retrieval spans every corpus the employee can reach and oversharing is the recorded failure mode for the class. D4 carries it because the injection path is the product's own retrieval surface and three of the vendor's four layers are unevidenceable from outside. D9 carries it because the approval population is the whole workforce and the vendor's own account names approval-bot behavior as the expected drift. D2, D3, D5 and D7 are held at L3 or below because the controls they grade sit on the vendor's side of the shared-responsibility split, which the ladder needs as a scoping statement, since the bank cannot remediate them. The matching Agent Card enum value is `productivity assistant (mail, files and calendar tools over a whole tenant)`.

### Both shapes against the Canadian crosswalk

The crosswalk answers each shape's weakest domains with close to nothing. Shape 1's are D2 and D9: its D2 row names the Integrity and Security Guideline for human-level personnel vetting and states that no per-agent or non-human identity anchor exists, and the nearest examinable expectation for the shape as a whole is B-13's secure system development life cycle expectation, which the scorecard already identifies as the direct regulatory hook. [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack]] records the same gap from the framework side: no layer in the stack governs the coding agent as an actor in the life cycle. Shape 1's D8 answers to E-23 third-party model governance and B-13 third-party technology risk. Shape 2's weakest domains are D3, D4 and D5: the crosswalk records no clean anchor for D3 and only generic B-13 cyber-defense language for D4 and D5, and its own gap callout states that as of 2026 no Canadian financial regulator prescribes agentic-AI-specific technical controls in those domains. The pressure on shape 2 comes from E-23 in May 2027, since an assistant in use across the organization is a model in use, and from PIPEDA and Law 25 now, through D6 and D9. The crosswalk omits B-10, third-party risk management, which two other vault pages date to 2024-05-01 and against which the bank's Google Cloud and Anthropic relationships are examined.

## Part 6 — Assessor's Guide, common-sense review

[[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] and [[canadian-bank-secure-sdlc-ai-assessor-scorecard|Assessor's Quick Scorecard: Secure-SDLC and AI]] were read in full on 2026-09-15, before Part 7 rows 1 to 9 replaced the nine handbook steps they name and row 12 fixed six scorecard items; the other flags stayed open, most with no row in Part 7.

### The measurement protocol, thirty-two flags by class

Thirty-two handbook steps or requirements cannot be executed as written by an assessor engaged on either September shape.

**Internal contradiction, four flags.**

- The Stage-3 flowchart still outputs a floor rating, which dependency-resolved scoring replaced.
- Rubric score 4 requires a multi-tool eval, where the recalibrated D7 L4 requires a multi-category cadence and names no tool count.
- Gap-report item 9 ties the re-assessment cadence to one scheme's quarterly cycle, reinstating the single-scheme mandate D1 L5 removed.
- The dependency-rules page names that scheme alone in the gate, which the core page and the handbook both made neutral.

**Missing input, eleven flags.** The first three are load-bearing.

- The rule that a missing document scores automatic L1 in the relevant domain has no document-to-domain map, and two of the nine requested documents have no single relevant domain.
- Week 5 fires synthetic incidents across three agents, while the handbook's own gap list records no synthetic-incident library and no published procedure for three of four candidates.
- The gap report requires a crosswalk extract admitting three anchor families, none of which a Canadian federally regulated institution is examined against, and refuses the anchors the vault's Canadian crosswalk supplies.
- The Agent Card's shape enum cannot describe shape 2.
- The competence list omits the harness configuration tree and any cloud policy language outside Cedar and OPA, so an assessor meeting the stated bar cannot read shape 1's primary evidence.
- The D6 block branches on closed corpus or RAG, with no branch for a whole-tenant assistant.
- The log-suppression test has no not-applicable path where the customer neither runs the agent nor owns the log store.
- The test-coverage statement asks for a depth and a corpus size no step collects.
- The live human-in-the-loop fire at L3 is required of a shape the model describes as having no such queue.
- The five comparative claims about other audit programs carry no citation and name no financial-sector examination program.
- The reproduction-rate requirement has no collecting step.

**Undefined term or referent, five flags.**

- The four-verdict vocabulary the deep dives grade on (met, not met, not applicable, unanswerable) appears nowhere in the handbook, the gap report's verdict column has no defined values, and one deep dive tells the assessor to record a verdict the rubric cannot hold.
- Live observation at L4 asks for a drift event from "the" behavioral monitoring system, a definite article with no referent over a preview-only product class.
- The D9 interview asks for a p99 guardrail latency budget where the ladder sets no percentile at L3 and uses p95 at L4, so the figure is invented at interview time, the inconsistency the handbook exists to prevent.
- The remaining two are the artifact columns counted in the next class.

**Product inside the instrument, five flags.**

- A step demands a firing of one vendor's experimental alignment checker and the artifact table requires its logs at L4, so the handbook asks for evidence from a control the model says a regulated buyer cannot deploy to claim a GA-grade L4.
- Two further steps name red-teaming and registry-scanning products inside the question.
- The D7 L5 artifact names one vendor's graph product unhedged, where the core page hedges the same name.
- The D3 L5 artifact names a warrant sample, both a product name and the contested rung.

**Evidence no rung asks for, two flags.**

- The D2 L4 artifact column requires a policy-engine repository that D3 grades and no D2 rung does, so an assessor collecting it credits the wrong domain.
- The D6 L4 column requires attestation logs, which the recalibration moved to L5+ and replaced with memory governance and provenance weighting, so the checklist grades the previous spine.

**Rung no step collects, three flags.**

- D5 L3's resolver closure and per-agent call ceilings have no question and no artifact.
- D4 L5's enumeration of the services every sandbox shares has neither.
- D1 L3's shadow-agent inventory has an artifact and no discovery step, although Stage 1 states shadow-agent detection as the inventory export's purpose.

**Circular, three flags.**

- Live observation is required per high-risk-tier agent, and the assessed organization's own rubric sets the tier, so the assessed party sizes the assessor's obligation; on shape 2 the agent count is the headcount.
- Assessor competence requires operational experience in four of nine domains, with no team-level coverage rule and no requirement that the four include the domains the engagement weights.
- The gate into L5, below.

**Duration, one flag.** The three stage durations sum to four to seven weeks while the sample timeline runs nine calendar weeks, Stage 2 filling five weeks against a stated ceiling of four and Stage 3 two against a stated one.

Three of the thirty-two block an assessment outright:

- the gate into L5, which makes the selective L5 the model recommends unreachable (Part 4)
- the L3 human-in-the-loop fire, required synthetically if necessary, with no not-applicable path offered
- live observation at L5, which requires a certificate dated within the last quarter where the preferred scheme runs on an annual surveillance cycle, so that evidence is unobtainable on the path the model prefers

A fourth, the Stage-3 floor rating, makes an assessor working from the diagram report the wrong headline.

### The scorecard

The scorecard puts 62 questions to a consultant assessing a large Canadian bank and still computes the rule the CMM deleted: its whole-engagement tier is the minimum of the per-section tiers, stated as deliberate, five days after dependency-resolved effective scores replaced the single floor, on a page claiming alignment with the measurement protocol for cross-engagement comparability. Two level scales share labels, the protocol's cumulative artifact and live-observation criteria and the scorecard's percentage bands over yes-and-partial answers, so an L4 in one is not an L4 in the other; the scorecard reaches L5 above ninety per cent of questions with no mention of the four-condition gate the protocol requires before any L5. The stated length is about 65 questions where the sections hold 62. Across all 62 there is no occurrence of oversharing, entitlement, answer-time, indirect or memory poisoning, so the D6 L3 spine the recalibration made the domain's centre goes unscored, as does indirect prompt injection, which D4 grades at L3. The crosswalk section maps sections to seven CMM domains and omits D4, with D5 present only as an anchor inside one question, though D4 and D5 are the two largest control surfaces for both September shapes. The B-13 date label is inherited from the B-13 page and wrong the same way in two places.[^b13date]

The remaining flags:

- The percentage bands touch at 50 and 75 and leave 90 unassigned.
- The score formula is stated once without a factor of one hundred and once with it.
- Two questions ask a model consumer for producer-only evidence the D8 page tags as not applicable to it, and the floor rule propagates the resulting No to the engagement tier.
- Severities map to CMM levels without saying whether they are raw or effective.
- A not-applicable answer is excluded with a justification that need not reach the report, where the CMM carries a reduced scope as a headline field.
- One product named as a current red-teaming vendor no longer identifies a purchasable product.
- One benchmark figure is pinned to a superseded model version with no measurement date.
- One guideline's title carries a hardened year the regulator's title does not.
- A seven-year evidence-retention period is asserted against two B-13 sub-sections on a page with an empty sources list, while the B-13 page records one of them as untranscribed and states no retention period anywhere.
- A partner roster on the same page is unsourced.
- The page names the ISED voluntary code and no other Canadian AI instrument, and never records that the federal AI bill died, so a reader cannot tell whether a Canadian AI statute binds the bank.
- A regulatory maturity expectation is asserted twice, where the Canadian crosswalk states that no Canadian financial regulator prescribes such controls.
- The audience term appears in three spellings of a description that is wrong in all three, since an external consultant is a third party.
- The page was last touched on 2026-05-29, while the protocol and the nine deep dives moved to 2026-09-10.

### Executable scope of the two instruments

Stage 1 runs end to end for shape 1 and, with the enum gap noted on the scope letter, for shape 2. The interview track runs nearly in full for D1, D2, D6, D8 and D9, and the artifact checklist is collectable at L2 and L3 in seven of the nine domains. Shape 1's live observation at L3 is satisfiable today: a sandbox denial, a deny rule surviving an autonomous mode, a prompt on a destructive pattern and an egress refusal outside the allowlist are demonstrable in one session, with an exportable trace. Six parts cannot run:

- the L4 band wherever its artifact is a preview control
- the L5 gate against a program carrying a recorded trade-off
- the synthetic-incident week
- the reproduction-rate requirement
- the whole D6 block for a whole-tenant assistant
- the crosswalk extract for a Canadian engagement

The scorecard can be administered as a questionnaire with usable section percentages; its engagement tier is not fit to report.

## Part 7 — Recommended changes

APPLY-NOW changes were applied in the same pass: each restores consistency between two live pages, corrects a mislabeled fact against a cited source, adds a plainly missing row, or replaces an unexecutable handbook step with an executable one, and none chooses a calibration position. RECOMMEND changes move a rung, choose between L5 and L5+, reprice a cost model or set a new target, and are held for the operator.

| No. | Change | Pages | Disposition |
|---|---|---|---|
| 1 | Add a productivity-assistant value to the Agent Card deployment-shape enum | Measurement protocol | APPLY-NOW |
| 2 | Replace the Stage-3 diagram's floor-rating output with the three-number headline | Measurement protocol | APPLY-NOW |
| 3 | State the four-verdict vocabulary in Stage 3 and in the gap report's verdict column | Measurement protocol | APPLY-NOW |
| 4 | Move the policy-engine repository artifact from the D2 L4 column to D3 L4 | Measurement protocol | APPLY-NOW |
| 5 | Replace the D6 L4 attestation-log artifact with the recalibrated D6 L4 artifacts, moving attestation to L5+ | Measurement protocol | APPLY-NOW |
| 6 | Replace the two product names in the artifact table with the capabilities they stand for | Measurement protocol | APPLY-NOW for the D7 L5 cell; the D3 L5 cell is held, because the core page quotes it verbatim inside rung-gated text |
| 7 | Admit the jurisdictional crosswalk pages as anchors in the gap-report crosswalk extract | Measurement protocol | APPLY-NOW |
| 8 | Give the D6 interview block a whole-tenant branch with the entitlement test stated in tenant terms | Measurement protocol | APPLY-NOW |
| 9 | Reconcile the stated stage durations with the sample timeline | Measurement protocol | APPLY-NOW |
| 10 | Correct the coding-agent note's rules-file item to this harness's real configuration surface | CMM core | APPLY-NOW |
| 11 | Label the 2022-07-31 B-13 date as publication rather than effect | OSFI B-13 | APPLY-NOW |
| 12 | Scorecard bookkeeping, six items: the B-13 labels, the question count, the band boundaries, the score formula, two stale products, the producer-only tag | Scorecard | APPLY-NOW |
| 13 | Add B-10 to the Canadian landscape table and to the D8 anchor row | Canada crosswalk | APPLY-NOW |
| 14 | Add the MCP-allowlist and lockdown-keys control row, the cloud-routing row, and the harness-link correction | Securing Agentic Coding | APPLY-NOW |
| 15 | File the limitations this review raises | CMM known limitations | APPLY-NOW |
| 16 | Add the productivity-assistant row to the core shape table and to the reference architecture's shape table | CMM core, reference architecture | RECOMMEND |
| 17 | Resolve the per-task capability-token rung disagreement between D2 and D3 or D5 | D2, D3, D5, measurement protocol | RECOMMEND |
| 18 | Reconcile the org-wide L5 gate with selective L5, or state which domains the gate covers | CMM core, measurement protocol | RECOMMEND |
| 19 | Give D6 the production-maturity preamble the other eight ladders carry | D6, CMM core, measurement protocol | RECOMMEND |
| 20 | Price a non-Microsoft licensing column in each cost model, or state each model's stack assumption | Nine deep dives | RECOMMEND |
| 21 | Add Google control instruments to the tooling map where they exist, and state the absence where they do not | CMM core | RECOMMEND |
| 22 | Replace the scorecard's minimum-of-sections engagement tier with dependency-resolved aggregation, or remove the tier | Scorecard | RECOMMEND |
| 23 | Admit jurisdictional anchors in the core page's mandatory evidence-tag set | CMM core | RECOMMEND |
| 24 | Add oversharing, answer-time entitlement, indirect-injection, D4 and D5 questions to the scorecard | Scorecard | RECOMMEND |
| 25 | Establish the B-13 in-force date against a source that states it, or drop the in-force claim | Canada crosswalk | RECOMMEND |
| 26 | Write the Google-Cloud-only single-stack reading the May regulated-FI page asked for | Reference architecture | RECOMMEND |
| 27 | Grade the harness-fleet inventory dimension D8 names, and add D1 and D9 coordinates to the coding-shape control set | D8, D1, D9, Securing Agentic Coding | RECOMMEND |
| 28 | Record a data-residency position for both shapes | Canada crosswalk, Securing Agentic Coding | RECOMMEND |
| 29 | Reduce the callout counts across the CMM family to the one-per-page rule | Nine deep dives, CMM core, scorecard | RECOMMEND |
| 30 | Give D3 an evidence path for an enforcement point the harness holds, or a not-applicable path where no external PDP exists | D3, measurement protocol | RECOMMEND |
| 31 | Reconcile the core page's L4 coding-shape target with the D4 ladder's preview-grade L4 spine | CMM core, D4 | RECOMMEND |
| 32 | State the D6 L3 answer-time-entitlement spine in terms a repository corpus can satisfy, or scope D6 out for the coding shape | D6, CMM core, measurement protocol | RECOMMEND |

Fifteen APPLY-NOW, seventeen RECOMMEND.

## Out of scope

The stress test scores two shapes for one persona. It takes the five May archetypes' dispositions from the live pages without re-scoring them, and assesses neither Google Cloud nor Anthropic as a vendor: the eight fetched product facts document a control surface and evidence no control operating in any tenant. It holds no clause-level coverage matrix against B-13 or E-23, so it supports no compliance conclusion, and treats E-23 model risk only where a CMM domain grades it. A coding harness driven as an attack agent is recorded elsewhere in the vault. The stress test moved no rung, and no score in it assesses a real institution.

## Disposition

The stress test is an immutable dated snapshot: its scores describe the vault on 2026-09-15, and a later re-run supersedes it by a forward link from this section. The seventeen RECOMMEND changes became items 6 to 21 of [[cmm-known-limitations|CMM Known Limitations (current state)]], tracked under [#168](https://github.com/ag0x00/ai-era/issues/168).

### Status since the review

Later decisions and corrections supersede the passages named in the second column; an R-number is a row of Part 7.

| Date | Superseded | Decision | Record |
|---|---|---|---|
| 2026-09-16 | R31; Part 2's "L4 across all nine" target | The core coding-shape row reads `L3 → L4 (L4 in D8)` | [#171](https://github.com/ag0x00/ai-era/issues/171) |
| 2026-09-16 | R19 and R32; Part 4's D6 preamble bias and Part 2's D6 objection | D6 carries the production-maturity preamble, and D6 L3 resolves read authorization in the layer the corpus carries, a repository's branch grants included | [#172](https://github.com/ag0x00/ai-era/issues/172) |
| 2026-09-16 | R20, R21, R26 and R28; Part 4's product and cost bias | Each cost model states its stack, the tooling map has a Platform-native (Google) column, and the Google-Cloud-only reading and both residency positions are written | [#175](https://github.com/ag0x00/ai-era/issues/175) |
| 2026-09-18 | R16; Part 3's missing shape row | The core, reference-architecture and nine right-sizing tables carry in-suite and desktop-agent productivity-assistant rows | [#174](https://github.com/ag0x00/ai-era/issues/174) |
| 2026-09-18 | R29; Part 4's callout counts | The CMM family's callout counts came down to the one-per-page rule | [#177](https://github.com/ag0x00/ai-era/issues/177) |
| 2026-09-18 | Part 2's domain-fronting sentence | The vendor reference states the reverse: sandboxed code can use domain fronting to reach hosts outside the allowlist, so the risk is escape from it | [[agentic-ai-security-cmm-d5-egress-network\|CMM D5: Egress and Network]] |
| 2026-09-19 | R18; Part 4's L5-gate bias | The L5 gate's stability condition grades the domain being scored L5, and its other three conditions stay program-level | [#173](https://github.com/ag0x00/ai-era/issues/173) |
| 2026-09-19 | R17 and the held half of R6; Part 4's two-level capability | Per-task capability tokens sit at L5+ on D2, D3 and D5, and the protocol's D3 L5 artifact cell names no product | [#169](https://github.com/ag0x00/ai-era/issues/169) |
| 2026-09-19 | R30; Part 2's D3 reading | D3 L3 tests injection resistance, which a harness resolving a policy the session cannot write meets, with substitute evidence in the protocol; decision rights hold the unchanged D3 range below L3 | [#170](https://github.com/ag0x00/ai-era/issues/170) |
| 2026-09-19 | Two of Part 6's blocking flags | The L3 gate fire takes a not-applicable verdict where no action sits in the confirm tier, and the L5 certificate follows its scheme's cadence | [#261](https://github.com/ag0x00/ai-era/issues/261) |
| 2026-09-23 | The rule 2 reading in Part 2's D4 section and the Verdict | The deep dives' preamble dates production from when a control entered it and credits no planned date, so a budgeted component counts only once it runs; [[agentic-ai-security-cmm-d4-runtime-guardrails\|CMM D4: Runtime and Guardrails]] carries the text | [#304](https://github.com/ag0x00/ai-era/issues/304) |
| 2026-09-23 | Part 3's D2 row | A vendor-held identity condition makes four D2 identity criteria not applicable where the vendor offers no agent principal in general availability | [#300](https://github.com/ag0x00/ai-era/issues/300) |
| 2026-09-24 | Part 2's D2 attribution absence | Claude Code's contribution metrics, a public beta, label merged pull requests with Claude Code-assisted lines and exclude cloud-provider usage; no first-party feature spans harness vendors | [[securing-agentic-coding\|Securing Agentic Coding]] |

R22 to R25 and R27 are tracked as items 10, 11 and 13 to 15 of [[cmm-known-limitations|CMM Known Limitations (current state)]].

[^managed]: [Claude Code managed settings](https://code.claude.com/docs/en/managed-settings), fetched 2026-09-15.
[^iam]: [Claude Code identity and access management](https://code.claude.com/docs/en/iam), fetched 2026-09-15.
[^vertex]: [Claude Code on Google Cloud's Agent Platform](https://code.claude.com/docs/en/google-vertex-ai), fetched 2026-09-15. The page names global, multi-region and regional endpoints, defaults to `us-east5`, names no Canadian region, and states nothing about data retention or residency.
[^gemini-ou]: [Manage access to Gemini features in Workspace services](https://support.google.com/a/answer/15698295), fetched 2026-09-15.
[^gemini-dlp]: [About DLP for Gemini](https://knowledge.workspace.google.com/admin/security/about-dlp-for-gemini), fetched 2026-09-15.
[^gemini-aicc]: [Explore the AI control center](https://knowledge.workspace.google.com/admin/gemini/explore-the-ai-control-center), fetched 2026-09-15.
[^gemini-privacy]: [Generative AI in Google Workspace privacy hub](https://knowledge.workspace.google.com/admin/generative-ai/generative-ai-in-google-workspace-privacy-hub), fetched 2026-09-15, stating the training restriction over customer data.
[^armor]: [Model Armor overview](https://docs.cloud.google.com/security-command-center/docs/model-armor-overview), fetched 2026-09-15. The page lists eight integration points (the agent gateway, Apigee, Gemini Enterprise, Google and Google Cloud MCP servers, networking services, the Agent Platform, LangChain and Security Command Center), names no Gmail, Docs or Drive integration, and states no launch stage for the core service.
[^clawhavoc-count]: [Antiy Labs — ClawHavoc: Analysis of a Large-Scale Poisoning Campaign Targeting the OpenClaw Skill Market for AI Agents](https://www.antiy.net/p/clawhavoc-analysis-of-large-scale-poisoning-campaign-targeting-the-openclaw-skill-market-for-ai-agents/), 2026-02-06, fetched 2026-09-24, stating that "at least 1,184 malicious Skills have historically appeared on ClawHub"; the figure counts malicious skills published to that marketplace.
[^b13date]: [OSFI, Technology and Cyber Risk Management](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-risk-management), fetched 2026-09-15, stating "Date July 31, 2022" and no effective or in-force date. The second OSFI URL the Canadian crosswalk cites for in-force 2024-01-01, [Technology and cyber risk management](https://www.osfi-bsif.gc.ca/en/risks/technology-cyber-risk-management), was fetched the same day and states no effective or in-force date either.
[^e23date]: [OSFI, Guideline E-23: Model Risk Management](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/guideline-e-23-model-risk-management-2027), fetched 2026-09-15, stating publication 2025-09-11 and effective date 2027-05-01.
