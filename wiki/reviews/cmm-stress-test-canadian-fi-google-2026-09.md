---
type: review
title: "CMM Stress Test: Canadian FI on Google Cloud"
address: c-676733
created: 2026-09-15
updated: 2026-09-23
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
  - "[[securing-agentic-coding]]"
  - "[[oversharing-controls]]"
  - "[[claude-code-security]]"
  - "[[anthropic-sandbox-runtime]]"
  - "[[claude-code-github-action-credential-exposure]]"
  - "[[ai-coding-agent-governance]]"
  - "[[hooking-coding-agents-with-cedar-talk]]"
  - "[[harness-config-as-supply-chain-artifact]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[secure-sdlc-framework-stack-2026]]"
  - "[[sandworm-mode-npm-worm]]"
  - "[[clawhavoc]]"
  - "[[clinejection]]"
  - "[[cursor-npm-credential-stealer]]"
  - "[[jules-ai-kill-chain]]"
  - "[[gemini-cli-workspace-trust-rce]]"
  - "[[gemini-cli]]"
  - "[[geminijack-gemini-enterprise-injection]]"
  - "[[echoleak-copilot-zero-click]]"
  - "[[cosnitch-copilot-personal-exfiltration]]"
  - "[[securing-workspace-genai-at-google-talk]]"
  - "[[google-saif]]"
  - "[[google]]"
  - "[[azure-rag-chatbot-security-profile]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[lethal-trifecta]]"
  - "[[indirect-prompt-injection]]"
  - "[[osfi]]"
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
---

# CMM Stress Test: Canadian FI on Google Cloud

This page re-runs the May 2026 recalibration stress test ([[cmm-calibration-stress-test-2026|CMM Calibration Stress Test: Cumulative-Floor Rule]]) with one variable changed. The method is the May method: score realistic deployments against the live ladders, find where the instrument misreports, and split the result into changes that restore consistency and changes that need a calibration decision. What changes is the customer. The May exercise's regulated-financial-services archetype was a US credit union on Microsoft E5 running one member-facing RAG bot, fixed as the scoring persona in [[agentic-ai-security-cmm-recalibration-method-2026|CMM: Recalibration Method (Cadence and Cost)]] and scored domain by domain in [[agentic-cmm-regulated-fi-stress-test|Agentic AI CMM: Regulated-FI Stress Test]]. This run replaces it with a Canadian federally regulated financial institution whose cloud estate is Google Cloud and Google Workspace, and scores two shapes the earlier persona did not hold: [[securing-agentic-coding|Claude Code]] for software development, and Gemini in Workspace for the daily work of the whole organization.

Each criterion is tested through one lens, the bank's principal security architect asking four questions of every rung: whether the bank can produce the evidence the rung names on this stack, whether the rung asks for the right control for this shape, whether an [[osfi|OSFI]] examiner would accept that evidence against [[osfi-b-13|Guideline B-13]] or [[osfi-e-23-2027|Guideline E-23]], and whether the rung reads the same on the core page, the domain page and the assessor's handbook.

**The May verdict that regulated financial services score a floor of L3 and that the model treats them fairly holds only while the incumbency is Microsoft; on Google Cloud both September shapes land at L2 typical, and the cause is a product-and-cost layer that prices nine domains against an E5, Azure, AWS or GitHub estate and carries no Google licensing column in any of the nine.** The ladders survive the swap. The mapping underneath them does not, and one of the two shapes has no row in any of the model's three shape taxonomies.

## Part 1 — Scope and method

### The persona

A Canadian federally regulated financial institution: a Schedule I-class bank or a large federally regulated insurer, supervised by OSFI, subject to B-13 for technology and cyber risk and to E-23 for model risk from 2027-05-01,[^e23date] and subject to PIPEDA, to Quebec Law 25 automated-decision obligations for any Quebec-resident customer, and to FINTRAC reporting. Workloads and data sit on Google Cloud, mail and documents and calendar and chat sit on Google Workspace, and identity is Google Cloud Identity federated to the bank's own provider. The bank holds mature technology-risk, model-risk and third-party-risk functions, runs a 12 to 18 month procurement cycle for a new vendor, and staffs 3 to 5 full-time equivalents of AI-security capacity. Its board and its supervisor expect written evidence.

### The lens

A rung failing only the first question is a cost finding, one failing the second is a calibration finding, and one failing the fourth is a defect. The third question is answered from [[agentic-ai-security-cmm-crosswalk-canada-fi|CMM: Canadian Regulated-Finance Crosswalk]] rather than from the rung.

### The two shapes, concretely

**Shape 1 is Claude Code in the hands of the bank's developers.** It touches source repositories through the in-process file tools, shell commands and their children through the sandbox, continuous-integration runners through a GitHub Action, third-party capability through MCP servers, its own lifecycle through hooks, and an inference endpoint that for this bank is most likely Google Cloud's Agent Platform rather than the first-party API.[^vertex] Configuration arrives through managed settings by device management or from the organization's account, and each key governs at its resolved value, which an assessor reads rather than assumes.[^managed]

**Shape 2 is Gemini in Google Workspace for every employee.** It touches Gmail, Drive, Docs, Calendar and Chat through the side panels, and the same corpora again through the Gemini app with Workspace extensions. Its reads are the employee's mail and files; its writes are drafts, documents and sharing changes. Administration is per organizational unit in the Admin console, with access to data sources further bounded by Workspace data-loss-prevention rules.[^gemini-ou][^gemini-dlp] Customer prompts and content fall under a stated training restriction.[^gemini-privacy]

### The method and the reads

Each shape was scored domain by domain against the live ladder text, taking rung criteria from the nine deep dives and realistic targets from each domain's right-sizing table, with the persona's evidence capacity judged per rung. The nine ladders, read on 2026-09-15 at their 2026-09-10 revision:

- D1, [[agentic-ai-security-cmm-d1-governance|CMM D1: Governance and Accountability]]
- D2, [[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]]
- D3, [[agentic-ai-security-cmm-d3-control-least-agency|CMM D3: Control and Least-Agency]]
- D4, [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]]
- D5, [[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]]
- D6, [[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]]
- D7, [[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]]
- D8, [[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Supply Chain and AI-BOM]]
- D9, [[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]]

Raw scores resolve into effective scores under the three active rules in [[agentic-ai-security-cmm-dependency-rules|CMM: Effective-Score Dependency Rules]], reported as that page's three-number headline: typical is the median effective score, weakest is the minimum with the domain and any cap that fired, strongest is the maximum raw score. The single-floor rule the May page recommended keeping was retired on 2026-05-04, so this run reports no floor. The control-mapping review follows the scores, and the handbook review follows that.

Also read on 2026-09-15: the CMM core page, the dependency rules, the recalibration method, [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] in full, [[canadian-bank-secure-sdlc-ai-assessor-scorecard|Assessor's Quick Scorecard: Secure-SDLC and AI]] in full, the Canadian crosswalk, [[securing-agentic-coding|Securing Agentic Coding]], and the incident, product and framework pages cited below. Eight product facts were resolved against vendor documentation fetched the same day and are footnoted individually; a control claim carrying no footnote and no vault citation is stated as an absence.

## Part 2 — Shape 1 scored: Claude Code for software development

The [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] sets this shape L4 across all nine domains with L5 justified in D2, D4, D8 and D9. The deep dives are one rung less demanding: L3 to L4 in D1, D2, D3, D4, D7 and D9, L3 in D5 and D6, and L4 in D8, which they call the heaviest domain for the shape.

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

Per-agent identity caps effective D5 and effective D7, so D2 is where the score is made. The bank reaches L3 on the interactive shape: managed `forceLoginMethod` and `forceLoginOrgUUID` pin a session to a known organization, with federated single sign-on behind it on the Enterprise plan. Two limits bound the claim. Enforcement is per-path, and a session authenticating against a cloud provider is not blocked, so cloud identity-and-access policy is the control for exactly the routing this bank will choose.[^iam] More consequentially, no first-party feature on any harness vendor attributes a change to the agent or to the human who produced it, and the one commercial instrument the vault records for attribution is a single vendor's product with no dated general-availability announcement and no independent evaluation. D2 L3 requires that every action trace to a human. The bank can trace a session to a human and cannot trace a commit to an agent, which is the coding-shape D2 test the domain page itself names.

### D3 grades a decision point the harness holds itself

Managed settings give a permission policy local settings cannot override, deny rules honored under sandbox auto-allow and under the skip-permissions flag, content-scoped prompts an autonomy mode cannot suppress, two keys that lock developers out of widening read paths and domains, and audit or blocking of in-session settings changes. Vendor documentation adds a control set the vault does not carry: `allowedMcpServers` as a restriction allowlist taken whole from the highest-ranked source and enforced as empty when malformed, `deniedMcpServers` merging across sources, `allowManagedMcpServersOnly`, a deployed `managed-mcp.json`, `allowManagedPermissionRulesOnly`, `disableBypassPermissionsMode`, `disableSideloadFlags` for the plugin and MCP command-line flags, and `strictPluginOnlyCustomization` over skills, agents, hooks and MCP servers from user and project sources.[^managed] That is a stronger position than the vault records and it still does not answer the rung. D3 L3 asks for a policy decision point outside the model context, deny-by-default, synchronous, failing closed. Here the enforcement point sits inside the harness the policy governs, the Cedar-routed reference monitor is a practitioner prototype, and this harness exposes no model-level hook, so interception reaches the tool and shell layer only ([[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar]]). The bank either records the harness as its own decision point and accepts the circularity, or scores L2, and the D3 page warns separately that scoring the domain on a shell-pattern allowlist overstates by roughly a level.

### D4 reaches L3, and the L4 target rests on preview controls

The sandbox, write and read confinement, the fail-closed keys and the container reference configuration put the interactive shape at L3, with three coverage facts an assessor records rather than assumes: sandbox read access defaults to the whole filesystem minus explicit denies, the built-in boundary covers shell commands and their children while the file tools, MCP servers and hooks run outside it ([[anthropic-sandbox-runtime|Anthropic Sandbox Runtime]] is the beta research preview that closes that gap), and `filesystem.disabled` turns the filesystem layer off while leaving network isolation on, which vendor documentation flags as self-escalating. The target is the defect. The core page asks this shape for L4 across all domains while the D4 page states that the L4 spine rests on preview, experimental or specification-only controls, so a defensible L4 today is assembled from preview and open-source components. Recalibration rule 2 admits such a control where it sits in the approved-vendor pipeline with a documented production date, so the L4 this bank can claim rests on that accommodation and not on generally available controls. One detail travels badly: D4 discounts the English-only limit on groundedness detection as moot for a US English member bot, and an institution with French-language service obligations cannot discount it.

### D5 and D7 are capped by identity, and both carry a shape-specific finding

Raw D5 reaches L3 with a strict allowlist and managed-only domains, raw D7 reaches L3 with session-correlated OpenTelemetry export, and both resolve to L2 because each is bounded by raw D2. The bank buys a gateway and a trace backend and its reported score does not move until identity is in production, which is the dependency rules working as designed and is worth stating to a budget holder before the spend. Two facts sharpen the pair. The default proxy decides from the client-supplied hostname without inspecting TLS, so a broad allowlist entry stays reachable by domain fronting, and the TLS-terminating option shipped experimental. On telemetry, content redaction is the default: an organization that enables export and stops there receives token counts, costs, durations, permission-mode transitions and connection status, and no command strings, prompt text or tool arguments. That stream answers what an agent cost and cannot answer what it ran. Routing inference through Google Cloud's Agent Platform costs the first-party analytics dashboards and, under zero-retention terms, the contribution metrics; OpenTelemetry keeps working under that routing and is the documented rebuild path ([[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]).

### D8 carries the highest target and an ungraded evidence dimension

The bank can evidence an AI bill of materials at build, dependency scanning with lockfile enforcement, registry provenance and pre-install scanning for MCP servers and skill packs, and signing of its own agent artifacts, which is L3 against a target of L4. The evidence L4 would rest on here is a fleet inventory of which harnesses, at which versions, with which MCP servers, attributable to which human, and the D8 page states that the L3 and L4 criteria omit that dimension and names it a candidate for the next revision. [[securing-agentic-coding|The coding-shape control catalog]] files the same inventory under [[agentic-ai-security-cmm-d7-observability|D7]] as a runtime AI-BOM, so one artifact serves two domains until a rung names it. The only commercial instrument the vault records for it is the same single vendor that carries the attribution row, which a third-party-risk function applying B-10 treats as a concentration finding rather than as a control. Five recorded incidents sit behind the domain: a worm typosquatting the harness package to inject MCP server configurations ([[sandworm-mode-npm-worm|SANDWORM_MODE npm Worm: AI Toolchain Poisoning]]), more than a thousand malicious skills on a marketplace ([[clawhavoc|ClawHavoc: Agentic Skill Marketplace Supply Chain Attack]]), a package install driven from an issue title ([[clinejection|Clinejection: AI Attacks AI via GitHub Issue Title]]), credential theft through editor packages ([[cursor-npm-credential-stealer|Cursor npm Credential Stealer]]), and injection to command-and-control against another vendor's coding agent ([[jules-ai-kill-chain|Jules AI Kill Chain: Injection to Remote Control]]).

### D1 and D9 are the two domains the control catalog does not reach

[[securing-agentic-coding|Securing Agentic Coding]] carries CMM coordinates for D2 through D8 and none for D1 or D9. For a bank those are the two an examiner opens first, and the gap is not cosmetic: D9 grades guardrail fail behavior, decommission and credential rotation on owner departure, human-in-the-loop queue monitoring, and an incident-response runbook naming the regulatory-notification path, all of which this shape has and none of which the catalog maps. The continuous-integration variant is where D8 and D9 meet a recorded defect: an unsandboxed file read reaching the process environment turned a triage workflow into credential exposure ([[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]), and the same primitive appeared independently against another vendor ([[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]). The catalog's own limit closes the domain: every control in it is ordered against an adversary, and the accidental-meltdown case passes through content filtering untouched, which is the failure mode a change-management function asks about first.

### Shape 1 headline

| Shape | Typical | Weakest | Strongest | Range | Target the model sets |
|---|---|---|---|---|---|
| Claude Code for software development | L3 | L2, set by D2 and D9, with D5 and D7 capped to L2 by raw D2 | L3 | L2 to L3 | L4 across all nine |

The distance between L2 to L3 and a target of L4 is not a finding against the bank. It is a finding against the target, which was set for a shape whose heaviest domain the ladder does not grade and whose L4 runtime spine the model itself grades as preview.

## Part 3 — Shape 2 scored: Gemini for day-to-day tasks

This shape has no target because it has no row. The core page's shape-mapping table carries seven applications and none is an enterprise productivity assistant holding tools over mail, files and calendar; the nearest rows are a web or desktop chatbot with no tools, which this is not, and a RAG application, which carries no whole-tenant identity and entitlement surface. The handbook's Agent Card repeats the hole: its deployment-shape field admits chatbot, RAG, MCP server or mesh plus five coding-agent variants, so the field that drives right-sizing cannot be filled (Part 7 row 1 adds the missing value). All nine right-sizing tables name a chatbot, a RAG bot, a copilot, an MCP provider or a mesh and never a productivity assistant, and the eleven-row shape table in [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] has the same absence, so the gap sits in both anchors rather than in one.

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

Twelve absences were checked and confirmed in the vault: no page on Gemini for Google Workspace as a product, none on Workspace admin controls for Gemini, no Workspace data-loss-prevention coverage, no record of Workspace extensions as a tool surface, no context-aware access coverage outside one CMM domain page, no Chrome Enterprise coverage as an assistant control, no data-residency or Canadian-region record for any AI service, no zero-retention page, no Vertex or Agent Platform control page, no Gemini Enterprise product page, no Agentspace mention, and no placement of Model Armor against Workspace traffic. The vault's only Gemini product page is [[gemini-cli|Gemini CLI]], a developer command-line tool and not this shape, and [[google|Google]] carries the vendor record without an administrative control inventory. Against that, the threat record is strong: zero-click indirect injection into Gemini Enterprise through a shared document, a calendar invite or a forwarded mail, exfiltrating mail, calendar and document content through an auto-loading image ([[geminijack-gemini-enterprise-injection|GeminiJack Gemini Enterprise Zero-Click Injection]]); the same structural flaw in another vendor's assistant through a single crafted mail, chaining four bypasses including an allowlisted link-preview proxy ([[echoleak-copilot-zero-click|EchoLeak Zero-Click Copilot Exfiltration]]); and an auto-run prompt parameter reaching connected mail, drive and calendar, with memory poisoning that survives a password change, a session revocation and device re-enrollment ([[cosnitch-copilot-personal-exfiltration|CoSnitch: Copilot Personal Data Exfiltration]]). The vendor's own account names indirect prompt injection as the main risk for this class and describes four layers against it: visible-content-only input, structural hierarchy in the prompt, deterministic orchestration sandboxing that restricts downstream capability by data origin, and output hardening that scrubs image tags and ungrounded links ([[securing-workspace-genai-at-google-talk|Securing Workspace GenAI at Google]]).

Three of those four layers are unevidenceable from the customer's side, which holds D3, D4 and D5 at L1 to L2. [[google-saif|Google SAIF: Secure AI Framework]] does not close the gap, because it names control categories without specifying controls, thresholds or test procedures. What the customer holds is real and narrow: per-organizational-unit enablement, data-loss-prevention rules bounding what the assistant reaches, an administrative control centre and a usage review, and a stated training restriction over customer data.[^gemini-ou][^gemini-dlp][^gemini-aicc][^gemini-privacy] The two administrative surfaces are absent from the vault and become sourced controls with this page. Model Armor stays out: its documented integration points are the agent gateway, Apigee, Gemini Enterprise, Google and Google Cloud MCP servers, networking services, the Agent Platform, LangChain and Security Command Center, and the overview names no Gmail, Docs or Drive integration and states no launch stage for the core service.[^armor]

### The trifecta reading, and the profile that excludes this shape

The vault's nearest structural template is [[azure-rag-chatbot-security-profile|Azure-Native RAG Chatbot Security Profile (Copilot Studio)]], and its scope note is the finding. That profile covers a closed-corpus chatbot that grounds on internal content, answers users and holds no external write tools, and the reason its required levels fall low across D3, D4, D5, D7 and D9 is that the bot reads private data with no external communication or write path, so the [[lethal-trifecta|lethal trifecta]] is broken by architecture. A Workspace assistant restores all three legs: it reads private data across mail and drive, it ingests untrusted content on every summarization of an inbox or a calendar, and it writes and shares, which is an external communications path. The one profile the vault holds for a productivity-class assistant therefore excludes exactly the property that makes this deployment dangerous, and [[oversharing-controls|Oversharing Controls for AI Search]] owns the failure mode without enumerating a per-plane control set or a target level. [[indirect-prompt-injection|Indirect prompt injection]] is graded at D4 L3, and for this shape it arrives through the retrieval path the product is sold for.

### Shape 2 headline

| Shape | Typical | Weakest | Strongest | Range | Target the model sets |
|---|---|---|---|---|---|
| Gemini in Workspace for day-to-day tasks | L2 | L2 across D2 to D9, with D4 capped by raw D3 and D5 and D7 capped by raw D2 | L3, D1 | L1 to L3 | None; no shape row exists |

## Part 4 — The CMM in September against the May recommendations

This part reports the state as read on 2026-09-15, before this pass applied the Part 7 changes. Line references are to that revision, and the pages this pass edited have moved since.

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

**Item 2 resolves into four limbs.** The per-domain matrix is primary, on the core page and in the handbook (agentic-ai-security-cmm-2026.md:126, …-measurement-protocol.md:249), and joint disclosure of that matrix is mandatory (…-measurement-protocol.md:258, …-dependency-rules.md:201). The floor headline was rejected outright (agentic-ai-security-cmm-2026.md:130, …-dependency-rules.md:57). Floor vocabulary nonetheless survives in three places, at …-measurement-protocol.md:82 and :241 and at agentic-ai-security-cmm-2026.md:198.

**Item 5 altered one condition of the gate and dropped another** (agentic-ai-security-cmm-2026.md:196-198, …-measurement-protocol.md:236-243 and :231). The certification condition became scheme-neutral, the named-contributor condition was dropped, and a floor-domain gap-closure condition was added.

### Disposition of the regulated-FI page's closure conditions

| No. | Closure condition | Disposition | Evidence |
|---|---|---|---|
| 1 | A regulated-FI deployment profile for the RAG-bot archetype | NOT ADOPTED as a page | The pieces sit across the nine right-sizing and cost sections plus [[agentic-ai-security-cmm-crosswalk-us-fi\|CMM: US Regulated-Finance Crosswalk (FFIEC and GLBA)]]; no page assembles them |
| 2 | A US regulated-finance crosswalk | CLOSED | agentic-ai-security-cmm-crosswalk-us-fi.md:42-50,54-60 |
| 3 | A single-stack reading per platform, including Google Cloud only | NOT ADOPTED | The condition's own line is its only occurrence in the vault, and this is the condition that bites hardest on this persona |
| 4 | A cost model separating licensing from labor and log run-rate | CLOSED | Recalibration rule 3 and a per-level cost table on all nine deep dives |

### Six systematic biases this run surfaces

**The product and cost layer assumes a Microsoft, Azure, AWS or GitHub incumbency, and what it carries for Google is unpriced and stops at the platform primitives.** All nine cost models price their licensing column against that incumbency and none prices Google Cloud or Workspace. The core page is where the absence is total: its tooling map carries three columns, standards, open source and commercial, with no platform-native column and no Google vendor in any row; Model Armor occurs zero times on it; Vertex appears once, inside a claim about a Microsoft product's discovery coverage; and its one Google Cloud occurrence is a single identity service named as generally available. Seven of the nine deep dives do carry a GCP platform-native column, holding Agent Identity at D2, Model Armor at D4 and at D5 alongside Apigee, and Google SecOps at D7, with D3 recording its own GCP column as thin for want of a declarative decision point. Claude Code appears in no tooling-map row and Gemini in none. For this persona the control mapping still has to be reconstructed, because neither of the two September shapes appears in it at all.

**The missing shape is missing in three places.** The core page's seven-row shape table, the handbook's Agent Card enum and all nine right-sizing tables carry no productivity-assistant value, and the reference architecture's eleven-row table repeats the absence, so the model and the architecture fail the same shape identically.

**The prerequisite gate into L5 contradicts selective L5.** The gate requires two quarters of stable L4 across all nine domains before an assessor may score any domain L5 (…-measurement-protocol.md:236-238, wired into rubric score 5 at :231), while the core page expects L4 across all domains with selective L5 where exposure justifies it (:417), and five right-sizing tables name selective L5 as a realistic target. A program legitimately at L2 in one domain by recorded trade-off can never be scored L5 in another, whatever that other domain's program does.

**One capability sits at two rungs.** Per-task holder-bound capability tokens are an L5+ criterion on D2 (…-d2-identity.md:97) and an L5 criterion on D3 and D5 (…-d3-control-least-agency.md:108, …-d5-egress-network.md:104), and the handbook asks for the artifact in the D3 L5 column. The same evidence grades one rung apart depending on which page was opened.

**D6 alone omits the production-maturity preamble.** Eight ladders open with the rule that a control counts when it operates in production; D6 opens with the rule-1 sentence only (…-d6-data-rag.md:88).

**Callout counts run over the one-per-page rule across the family.** Two on D1, D2, D4, D6 and D7, three on D9, one each on D3, D5 and D8, three on the scorecard and four on the core page. This is recorded as known debt and not as a finding of this review, and no page this pass edits adds one.

### Verdict

The ladders survive the platform swap, and that is the run's principal finding. Every objection in Part 2 and Part 3 concerns evidence the bank can produce or a control mapped to a product, and none concerns a criterion that stops making sense on Google Cloud. Recalibration rule 1 holds across the swap: a rung that says per-task scoped authorization with cryptographic binding reads the same against Cloud Identity as against Entra, where a rung naming a product would have needed rewriting. The cadence qualifier does comparable work for a buyer on a 12 to 18 month cycle, because the D4 statement that a defensible L4 is assembled today from preview and open-source components, which rule 2 admits once they sit in the approved-vendor pipeline with a production date, tells this bank which half of the L4 band it can budget for. The dependency rules absorb an unmodeled shape correctly: shape 2 holds no per-agent identity by design, D2 is therefore structurally L2, and DR-002's rationale that a detector below D2 attributes an anomaly to the fleet rather than to an agent is the right account of a whole-tenant assistant, where the assistant is the fleet. The effective-score definitions and the strategic-rationale field carry that shape's honest low scores without reporting the program as L1 overall.

What does not stand up is everything between the ladder and the buyer. The product layer is single-stack. The cost models answer a question this bank did not ask. One of its two deployments has no row in any taxonomy the model or the architecture carries, so right-sizing cannot be applied to it. The mandatory evidence-tag set admits no jurisdictional anchor, so the crosswalk pages the vault built for Canada and the United States cannot be cited in the instrument that requires a crosswalk extract. And the handbook, read as an assessor would execute it, does not run.

## Part 5 — Control-mapping review, both shapes

The mappings below are as read on 2026-09-15, before this pass applied any change. Part 7 records which were corrected in the same pass and which are held for a calibration decision.

### Shape 1, the coding-agent note against a Google Cloud bank

The core page adds four evidence items for a coding-tool deployment at L3 and above, sourced to [[ai-coding-agent-governance|AI Coding Agent Governance]]:

| Item | Evidenceable |
|---|---|
| Agent rules-file integrity | Partly |
| IDE extension provenance | Yes |
| Typosquat and dependency-hijack defense | Yes |
| Destructive-action classification | Yes |

**Rules-file integrity is evidenceable against a different file set than the item names.** The item names Cursor rules, Copilot Workspace rules and a Claude `IDENTITY.md`. This harness's configuration surface is `CLAUDE.md`, `.claude/settings.json`, managed settings, hooks and MCP manifests, so the baseline the bank hashes is not the file the item names, and the set to baseline is the one [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] describes. [[cmm-known-limitations|CMM Known Limitations]] item 5 records the filename convention as non-standard while the core page still names the file.

**Endpoint management delivers the IDE-extension allowlist.** For a command-line harness the equivalent surface is plugins and skills, and the managed keys governing it, `disableSideloadFlags` and `strictPluginOnlyCustomization`, appear in no vault control row.[^managed]

**Dependency scanning with lockfile enforcement delivers the typosquat and dependency-hijack item.** The item names two products inside the criterion, against recalibration rule 1.

**Destructive-action classification runs on two permission keys here**, `permissions.ask` with an argument pattern and `permissions.deny` honored under autonomous modes. The item points at a decision-rights matrix and names no harness key.

Missing or stale mappings, each with its replacement.

**No MCP-allowlist row exists in either instrument.** [[securing-agentic-coding|Securing Agentic Coding]]'s control plane has no MCP row and its lockdown row names two keys. The replacement is a control-plane row for pinning the admitted MCP servers and locking the four customization types, naming `allowedMcpServers`, `deniedMcpServers`, `allowManagedMcpServersOnly`, `managedMcpServers`, `managed-mcp.json`, `allowManagedPermissionRulesOnly`, `disableBypassPermissionsMode`, `disableSideloadFlags` and `strictPluginOnlyCustomization`, graded first-party and generally available, with the fail-closed behavior of a malformed allowlist stated.[^managed]

**No row covers the routing this bank will choose.** Claude Code on Google Cloud's Agent Platform is enabled by one environment variable, pinned to a region by another that defaults to `us-east5` with no Canadian region named, and authorized by a cloud role; the organization pin does not cover those sessions, so cloud policy is the control there. The replacement is an identity-plane row carrying that split, plus an observability-plane note that the first-party analytics are lost and OpenTelemetry is the rebuild path.[^vertex][^iam]

**D1 and D9 have no coordinate in the coding-shape control set.** The replacement is a D1 row for harness-fleet ownership and an approved-harness register with decision rights per repository risk tier, and a D9 row treating the permission-prompt stream as the human-in-the-loop queue, with rubber-stamp rate, departing-developer credential rotation, and guardrail fail behavior when the sandbox is unavailable. Both assign new CMM coordinates, so both are held for a calibration decision.

**One wikilink asserts a page is something it is not.** securing-agentic-coding.md:69 and :186 link the harness as [[claude-code-security|Claude Code Security]], which is the vulnerability-discovery product in a limited research preview and not the harness security model. The replacement reserves that link for the product and names the harness in plain text with a documentation citation.

### Shape 2, a mapping that does not exist

There is no control mapping to review for shape 2. No CMM instrument, no reference-architecture row and no domain right-sizing table carries the shape, so nothing exists to check. The proposed core-table row:

| Application | Realistic target (most enterprises) | Domains where L5 is justified |
|---|---|---|
| Enterprise productivity assistant with mail, file and calendar tools (Gemini for Workspace or Copilot M365 class) | L3 across all, L4 in D6 and D9 | D6 (answer-time entitlement over a whole-tenant corpus), D4 (indirect-injection defense in the retrieval path), D9 (disclosure and approval integrity at headcount scale) |

D6 carries the target because retrieval spans every corpus the employee can reach and oversharing is the recorded failure mode for the class. D4 carries it because the injection path is the product's own retrieval surface and three of the vendor's four layers are unevidenceable from outside. D9 carries it because the approval population is the whole workforce and the vendor's own account names approval-bot behavior as the expected drift. D2, D3, D5 and D7 are held at L3 or below because the controls they grade sit on the vendor's side of the shared-responsibility split, which the ladder needs as a scoping statement, since the bank cannot remediate them. The handbook's Agent Card needs the matching enum value, `productivity assistant (mail, files and calendar tools over a whole tenant)`, or the field that drives right-sizing stays unfillable.

### Both shapes against the Canadian crosswalk

For the weakest domain of each shape the crosswalk's answer is close to nothing. Shape 1's weakest domains are D2 and D9: the crosswalk's D2 row names the Integrity and Security Guideline for personnel vetting at human level and states that no per-agent or non-human identity anchor exists, and the nearest examinable expectation for the shape as a whole is B-13's secure system development life cycle expectation, which the scorecard already identifies as the direct regulatory hook. [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack]] states the matching gap from the framework side: no layer in the stack governs the coding agent as an actor in the life cycle, so an examiner asking which framework covers it gets no answer. Shape 1's D8 answers to E-23 third-party model governance and to B-13 third-party technology risk. Shape 2's weakest domains are D3, D4 and D5, where the crosswalk records no clean anchor for D3 and only generic B-13 cyber-defense language for D4 and D5; its own gap callout states that as of 2026 no Canadian financial regulator prescribes agentic-AI-specific technical controls in exactly those domains. The pressure that does bite on shape 2 arrives from E-23 in May 2027, since an assistant in use across the organization is a model in use, and from PIPEDA and Law 25 now, through D6 and D9. One instrument is missing from the crosswalk entirely: B-10, third-party risk management, which two other vault pages date to 2024-05-01 and which is the guideline the bank's Google Cloud and Anthropic relationships are examined against.

## Part 6 — Assessor's Guide, common-sense review

The flags below are as read on 2026-09-15, before this pass applied any change. Rows 1 to 9 of Part 7 have since replaced the nine handbook steps those rows name; Part 7 records every other flag as held.

### The measurement protocol, thirty-two flags by class

Thirty-two steps or requirements in the handbook cannot be executed as written by an assessor engaged on either September shape.

**Internal contradiction, four flags.** The Stage-3 flowchart still outputs a floor rating, which dependency-resolved scoring replaced. Rubric score 4 requires a multi-tool eval where the recalibrated D7 L4 requires a multi-category cadence and names no tool count. Gap-report item 9 ties the re-assessment cadence to one scheme's quarterly cycle, reinstating the single-scheme mandate D1 L5 removed. The dependency-rules page states the gate with that scheme named alone, where the core page and the handbook both made it neutral.

**Missing input, eleven flags.** Three are load-bearing. The rule that a missing document scores automatic L1 in the relevant domain has no document-to-domain map, and two of the nine requested documents have no single relevant domain. Week 5 fires synthetic incidents across three agents, and the handbook's own gap list records that no synthetic-incident library exists and that three of four candidates have no published procedure. The gap report requires a crosswalk extract admitting three anchor families, none of which a Canadian FRFI is examined against, while the vault's Canadian crosswalk supplies the anchors the step will not accept. The other eight, one line each:

- The Agent Card's shape enum cannot describe shape 2.
- The competence list omits the harness configuration tree and any cloud policy language outside Cedar and OPA, so an assessor meeting the stated bar cannot read shape 1's primary evidence.
- The D6 block branches on closed corpus or RAG and has no branch for a whole-tenant assistant.
- The log-suppression test has no not-applicable path where the customer neither runs the agent nor owns the log store.
- The test-coverage statement asks for a depth and a corpus size no step collects.
- The live human-in-the-loop fire at L3 is required of a shape the model describes as having no such queue.
- The five comparative claims about other audit programs carry no citation and name no financial-sector examination program.
- The reproduction-rate requirement has no collecting step.

**Undefined term or referent, five flags.** The four-verdict vocabulary the deep dives grade on, met and not met and not applicable and unanswerable, appears nowhere in the handbook, and the gap report's verdict column has no defined values, while one deep dive instructs the assessor to record a verdict the rubric cannot hold. Live observation at L4 asks for a drift event from "the" behavioral monitoring system, a definite article with no referent over a preview-only product class. The D9 interview asks for a p99 guardrail latency budget where the ladder sets no percentile at L3 and uses p95 at L4, so the figure is invented at interview time, which is the inconsistency the handbook exists to prevent. The remaining two are the artifact columns in the next class.

**Product inside the instrument, five flags.** A step demands a firing of one vendor's experimental alignment checker and the artifact table requires its logs at L4, so the handbook asks for evidence from a control the model says a regulated buyer cannot deploy. Two further steps name red-teaming and registry-scanning products inside the question. The D7 L5 artifact names one vendor's graph product unhedged where the core page hedges the same name. The D3 L5 artifact names a warrant sample, which is both a product name and the contested rung.

**Evidence no rung asks for, two flags.** The D2 L4 artifact column requires a policy-engine repository that no D2 rung grades and that D3 does, so an assessor collecting it credits the wrong domain. The D6 L4 column requires attestation logs, which the recalibration moved to L5+ and replaced with memory governance and provenance weighting, so the checklist grades the previous spine.

**Rung no step collects, three flags.** D5 L3's resolver closure and per-agent call ceilings have no question and no artifact, and D4 L5's enumeration of the services every sandbox shares has neither. D1 L3's shadow-agent inventory has an artifact and no discovery step, although Stage 1 states shadow-agent detection as the inventory export's purpose.

**Circular, three flags.** Live observation is required per high-risk-tier agent and the tier comes from the assessed organization's own rubric, so the assessed party sizes the assessor's obligation; on shape 2 the agent count is the headcount. Assessor competence requires operational experience in four of nine domains with no team-level coverage rule and no requirement that the four include the domains the engagement weights. The third is the L5 gate below.

**Duration, one flag.** The three stage durations sum to four to seven weeks while the sample timeline runs nine calendar weeks, with Stage 2 activity filling five weeks against a stated ceiling of four and Stage 3 filling two against a stated one.

Three of the thirty-two block an assessment outright. The gate into L5 requires two quarters of stable L4 across all nine domains before any domain can be scored L5, which makes selective L5 unreachable for every program the model tells to pursue it. Live observation at L3 requires a human-in-the-loop gate to fire, synthetically if necessary, from a deployment the model right-sizes to L3 and describes as having no such queue, with no not-applicable path offered. Live observation at L5 requires a certificate dated within the last quarter where the preferred scheme runs on an annual surveillance cycle, so that evidence is unobtainable on the path the model prefers. A fourth misprints the output: the Stage-3 diagram still draws a floor rating, so an assessor working from the diagram reports the wrong headline.

### The scorecard

The scorecard puts 62 questions to a consultant assessing a large Canadian bank, and it still computes the rule the CMM deleted. Its whole-engagement tier takes the minimum of the per-section tiers, stated as deliberate, five days after dependency-resolved effective scores replaced the single floor, on a page that claims alignment with the measurement protocol for cross-engagement comparability. Two level scales then carry the same labels: the protocol's are cumulative artifact and live-observation criteria, the scorecard's are percentage bands over yes-and-partial answers, so an L4 in one is not an L4 in the other, and L5 is reached at above ninety per cent of questions with no mention of the four-condition gate the protocol requires before any L5. The stated length is about 65 questions where the sections hold 62. Across all 62 there is no occurrence of oversharing, entitlement, answer-time, indirect or memory poisoning, so the D6 L3 spine the recalibration made the domain's centre is unscored, as is indirect prompt injection, which D4 grades at L3. The crosswalk section then maps sections to seven CMM domains and omits D4 entirely, with D5 appearing only as an anchor inside one question, and D4 and D5 are the two largest control surfaces for both September shapes. The B-13 date label is inherited from the B-13 page and is wrong the same way in two places[^b13date].

The remaining flags, one line each:

- The percentage bands touch at 50 and 75 and leave 90 unassigned.
- The score formula is stated once without a factor of one hundred and once with it.
- Two questions ask a model consumer for producer-only evidence the D8 page tags as not applicable to it, and the floor rule then propagates the resulting No to the engagement tier.
- Severities map to CMM levels without saying whether they are raw or effective.
- A not-applicable answer is excluded with a justification that is not required to reach the report, where the CMM carries a reduced scope as a headline field.
- One product named as a current red-teaming vendor no longer identifies a purchasable product.
- One benchmark figure is pinned to a superseded model version with no measurement date.
- One guideline's title carries a hardened year the regulator's title does not.
- An evidence-retention period of seven years is asserted against two B-13 sub-sections, where the B-13 page records one of them as untranscribed and states no retention period anywhere, on a page with an empty sources list.
- A partner roster is unsourced on the same page.
- The page names the ISED voluntary code and no other Canadian AI instrument, and never records that the federal AI bill died, so a reader cannot tell whether a Canadian AI statute binds the bank.
- A regulatory maturity expectation is asserted twice, where the Canadian crosswalk states that no Canadian financial regulator prescribes such controls.
- Three callouts sit on one page.
- The audience term appears in three spellings of a description that is wrong in all three, since an external consultant is a third party.
- The page has not been touched since 2026-05-29 while the protocol and the nine deep dives moved to 2026-09-10.

### Executable scope of the two instruments

Stage 1 runs end to end for shape 1 and, with the enum gap noted on the scope letter, for shape 2. The interview track runs nearly in full for D1, D2, D6, D8 and D9, and the artifact checklist is collectable at L2 and L3 in seven of the nine domains. For shape 1 the live-observation requirement at L3 is satisfiable today: a sandbox denial, a deny rule surviving an autonomous mode, a prompt on a destructive pattern and an egress refusal outside the allowlist are demonstrable in one session, and the trace is exportable. What cannot run is the L4 band wherever its artifact is a preview control, the L5 gate against any program carrying a recorded trade-off, the synthetic-incident week, the reproduction-rate requirement, the whole D6 block for a whole-tenant assistant, and the crosswalk extract for a Canadian engagement. The scorecard can be administered as a questionnaire and its section percentages are usable; its engagement tier should not be reported, because it computes a floor the model retired.

## Part 7 — Recommended changes

APPLY-NOW changes were applied by this pass. Each one restores consistency between two live pages, corrects a mislabeled fact against a cited source, adds a row that is plainly missing, or replaces an unexecutable handbook step with an executable one, and none chooses a calibration position. RECOMMEND changes move a rung, choose between L5 and L5+, reprice a cost model or set a new target, and are held for the operator. The table below names each change, the pages it lands on and its disposition.

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

This review scores two deployment shapes for one persona. It does not re-score the five May archetypes, whose dispositions are taken from the live pages rather than re-derived. It does not assess Google Cloud or Anthropic as vendors, and the eight fetched product facts document a control surface rather than evidence that a control operates in any tenant. It carries no clause-level coverage matrix against B-13 or E-23, so nothing here is a standards review and no compliance conclusion follows from it. Model risk under E-23 is treated only where a CMM domain already grades it. The offensive counterpart of shape 1, a coding harness driven as an attack agent, is out of scope and the vault records it separately. No rung was moved by this page, and no score above is an assessment of any real institution.

## Disposition

This page is an immutable dated snapshot. Its scores describe the vault's state on 2026-09-15 and are not revised as the pages move; a later re-run supersedes it by a forward link from this section rather than by an edit. The fifteen APPLY-NOW changes were applied in the same pass and are enumerated in Part 7. The seventeen RECOMMEND changes are recorded as items 6 to 21 in [[cmm-known-limitations|CMM Known Limitations]] and remain calibration decisions for the operator.

One statement in Part 4 is superseded on direction rather than by a re-run. Its D5 paragraph reads that a broad allowlist entry stays reachable by domain fronting. The vendor reference states the reverse: the proxy decides from the client-supplied hostname without inspecting TLS, so code running inside the sandbox can use domain fronting to reach hosts outside the allowlist. The risk is escape from the allowlist rather than exposure of an entry within it, and [[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]] carries the corrected statement with its source.

Its D3 reading is superseded the same way. Part 4 scores the coding shape against a rung asking for a decision point outside the model context and treats the harness holding that point as the ground for L2, offering the bank a recorded circularity as the alternative. [[agentic-ai-security-cmm-d3-control-least-agency|CMM D3: Control and Least-Agency]] now reads the requirement as injection resistance rather than process separation, so a harness resolving a policy the session cannot write satisfies the criterion and the substitute evidence set sits in [[agentic-ai-security-cmm-measurement-protocol|the measurement protocol]]. The shape's score is unchanged in range; what moved is the reason it lands there, and the decision-rights criterion rather than the decision point's location is what holds it below L3.

Its reading of recalibration rule 2 is superseded the same way. Part 2's D4 section and Part 4's verdict read the rule as admitting a preview or open-source component once it sits in the approved-vendor pipeline with a production date, and the verdict takes that reading to tell the bank which half of the L4 band it can budget for. The shared preamble to the deep dives' level definitions now defines the production date as the date the control entered the organization's production and gives a planned date no credit, and [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]] carries that text: a component counts toward L4 from the day it runs in the bank's production, and a budgeted one counts for nothing until then.

[^managed]: [Claude Code managed settings](https://code.claude.com/docs/en/managed-settings), fetched 2026-09-15.
[^iam]: [Claude Code identity and access management](https://code.claude.com/docs/en/iam), fetched 2026-09-15.
[^vertex]: [Claude Code on Google Cloud's Agent Platform](https://code.claude.com/docs/en/google-vertex-ai), fetched 2026-09-15. The page names global, multi-region and regional endpoints, defaults to `us-east5`, names no Canadian region, and states nothing about data retention or residency.
[^gemini-ou]: [Manage access to Gemini features in Workspace services](https://support.google.com/a/answer/15698295), fetched 2026-09-15.
[^gemini-dlp]: [About DLP for Gemini](https://knowledge.workspace.google.com/admin/security/about-dlp-for-gemini), fetched 2026-09-15.
[^gemini-aicc]: [Explore the AI control center](https://knowledge.workspace.google.com/admin/gemini/explore-the-ai-control-center), fetched 2026-09-15.
[^gemini-privacy]: [Generative AI in Google Workspace privacy hub](https://knowledge.workspace.google.com/admin/generative-ai/generative-ai-in-google-workspace-privacy-hub), fetched 2026-09-15, stating the training restriction over customer data.
[^armor]: [Model Armor overview](https://docs.cloud.google.com/security-command-center/docs/model-armor-overview), fetched 2026-09-15. The page lists its integration points, names no Gmail, Docs or Drive integration, and states no launch stage for the core service.
[^b13date]: [OSFI, Technology and Cyber Risk Management](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-risk-management), fetched 2026-09-15, stating "Date July 31, 2022" and no effective or in-force date. The second OSFI URL the Canadian crosswalk cites for in-force 2024-01-01, [Technology and cyber risk management](https://www.osfi-bsif.gc.ca/en/risks/technology-cyber-risk-management), was fetched the same day and states no effective or in-force date either.
[^e23date]: [OSFI, Guideline E-23: Model Risk Management](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/guideline-e-23-model-risk-management-2027), fetched 2026-09-15, stating publication 2025-09-11 and effective date 2027-05-01.
