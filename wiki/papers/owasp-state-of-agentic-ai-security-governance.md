---
type: paper
title: "State of Agentic AI Security and Governance"
address: c-000220
created: 2026-06-22
updated: 2026-08-20
tags:
  - papers
  - owasp
  - agentic-ai
  - governance
  - maturity-model
  - threat-landscape
  - non-human-identity
  - supply-chain
  - regulatory
status: summarized
scope_axis:
  - sec-of-ai
  - sec-against-ai
  - ai-in-sec-offense
origin: aggregated
year: 2026
authors:
  - "OWASP GenAI Security Project (Agentic Security Initiative)"
publisher: "OWASP GenAI Security Project"
venue: "OWASP GenAI Security Project — Agentic Security Initiative"
publication_date: 2026-06
source_url: "https://genai.owasp.org/resource/state-of-agentic-ai-security-and-governance/"
archived_copy: ".raw/papers/owasp-state-of-agentic-ai-security-governance-v2.01.pdf"
no_public_url: ""
key_claim: "Organizations are deploying agents faster than they can govern them; security and safety converge at the deployment layer, and governing the gap requires a two-dimensional maturity model that scores governance capability against agent adoption tier."
methodology: "Synthesis of documented 2025–2026 incidents and CVE disclosures, GitHub telemetry from 53 tracked agentic projects, a 42-instrument regulatory survey across 10 jurisdictions, and prior OWASP/CSA/NIST work; organized around a three-dimension agent taxonomy and an Adoption-Tier × Governance-Maturity matrix."
related:
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]]"
  - "[[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]]"
  - "[[owasp-aivss|OWASP AIVSS]]"
  - "[[owasp-llm-top-10|OWASP Top 10 for LLM Applications]]"
  - "[[owasp|OWASP]]"
  - "[[john-sotiropoulos|John Sotiropoulos]]"
  - "[[non-human-identity|Non-Human Identity]]"
  - "[[agent-identity-architecture|AI Agent Identity Architecture]]"
  - "[[lethal-trifecta|Lethal Trifecta]]"
  - "[[shadow-ai|Shadow AI]]"
  - "[[vibe-coding|Vibe Coding]]"
  - "[[ai-bom|AI-BOM]]"
  - "[[mcp-security|MCP Security]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]"
  - "[[standards-review-eu-ai-act-2026-Q2|EU AI Act Standards Review]]"
  - "[[echoleak-copilot-zero-click|EchoLeak Zero-Click Copilot Exfiltration]]"
sources:
  - ".raw/papers/owasp-state-of-agentic-ai-security-governance-v2.01.pdf"
---

# State of Agentic AI Security and Governance

**Source:** [OWASP GenAI Security Project — State of Agentic AI Security and Governance](https://genai.owasp.org/resource/state-of-agentic-ai-security-and-governance/) (v2.01, June 2026). Local copy: `.raw/papers/owasp-state-of-agentic-ai-security-governance-v2.01.pdf` (md5 `9d91e1699749cb616b07f93062fd2efe`).

## Key Claim

Organizations are deploying agents faster than they can govern them, and more budget for existing programs will not close the gap.[^exec] Three findings carry the report: the threats are real now (the v1.0 risk portfolio has documented production incidents behind nearly every category), safety and security converge at the deployment layer, and governance must move from periodic audit to continuous oversight.[^exec] The operational answer is a two-dimensional maturity model that scores governance capability against agent adoption tier.[^mm]

## Methodology

The report is a CISO-facing synthesis, not a control standard.[^scope] It is the v2 edition of an ASI deliverable first published July 2025, re-grounded in a year of evidence.[^exec] Inputs:

- Documented 2025–2026 incidents and public CVE disclosures, curated in the ASI Exploits and Incidents Tracker (424+ total CVEs, 74 critical, 17 platforms studied; 290 agentic CVEs in Q1 2026 alone).[^tracker]
- GitHub telemetry from 53 tracked agentic repositories via the OWASP State of AI GitHub Surveyor, snapshot April 2026.[^survey]
- Enterprise adoption data from a16z (29% of Fortune 500 and ~19% of Global 2000 are live paying customers of a leading AI startup).[^a16z]
- A regulatory survey of 42 instruments across 10 jurisdictions plus 12 watchlist items, assessed across nine governance dimensions, verified through April 4, 2026.[^reg]
- Prior OWASP, CSA, NIST, and academic work; cross-referenced to the [[owasp-agentic-ai-top-10|ASI Top 10]] throughout.[^scope]

Licensed CC BY-SA 4.0.[^license]

## Agent Taxonomy (three independent dimensions)

A deployed agent is classified as Agent Type + Implementation Pattern + Composition Pattern, at some autonomy level.[^tax] Any type can be built through any implementation and participate in any composition.[^tax]

| Dimension | Values | Governs |
|---|---|---|
| Agent type | Enterprise, Coding, Client-Facing, Personal, Infrastructure/Ops | What the agent does, where it operates |
| Implementation | Orchestration frameworks, Lightweight library, Platform-native / Low-code | Audit surface, monitoring, org visibility |
| Composition | Single Agent + Tools, Multi-Agent Systems, Distributed Agent Chains, Agent-Spawning | Trust-boundary structure, cascading-failure risk |
| Autonomy (cross-cutting) | Supervised, Semi-autonomous, Fully autonomous | Window for unsupervised action; blast radius |

Lightweight-library and low-code implementations carry the highest [[shadow-ai\|Shadow AI]] risk because security properties are builder-determined or platform-dependent, which complicates inventory.[^impl] Composition determines where the [[lethal-trifecta\|lethal trifecta]] and permission-inheritance risks land.[^comp]

## Threat Landscape (six areas)

| Threat area | Core observation |
|---|---|
| Expanding autonomy | Privileged capabilities chain across systems never built to trust a probabilistic intermediary |
| Prompt injection | Primary delivery mechanism; maps to 6 of 10 ASI categories; data/control plane collapse is unsolved[^pi] |
| Agentic supply chain | Moved from theory to active exploitation across protocol, skill-registry, and core-package layers[^supply] |
| Governance gap | Vibe coding plus [[shadow-ai\|Shadow AI]]; ~50% of employees use unsanctioned AI, only 37% of orgs have policy[^shadow] |
| Agent identity gap | Non-human identities outnumber humans 100:1 or more; identity governance immature[^nhi] |
| Layered attack surface | Independent efforts (MAESTRO, A2A, vendor frameworks) converged on a multi-layer model[^layer] |

Pull-quote framing the report's central thesis on convergence:

**At the deployment layer, AI safety and AI security cannot be operationally separated.** Security harm comes from what an agent is *permitted* to do; safety harm comes from what the agent *is*. The same design decisions (tool access, oversight reduction, multi-agent composition, supply-chain growth) create both exposures, and the same telemetry detects both.[^safety] Model-level safety remains the provider's domain; deployment-layer safety belongs with the security function.[^safety]

## Real-World Incidents (selected)

| Date | Incident | ASI mapping |
|---|---|---|
| Dec 2025 | Claude Skills ransomware deployment (Cato CTRL) | ASI04, ASI05[^inc] |
| Sep 2025 | OpenAI Codex CLI sandbox bypass (CVE-2025-59532) | ASI02, ASI05[^inc] |
| May 2025 | [[echoleak-copilot-zero-click\|EchoLeak]] zero-click Copilot exfiltration | ASI01, ASI02, ASI06[^inc] |
| Apr 2025 | A2A agent-in-the-middle / fake agent card | ASI03, ASI06, ASI07, ASI08, ASI10[^inc] |
| Jan 2026 | Cursor allowlist bypass (CVE-2026-22708) | ASI04, ASI05[^cursor] |

The recurring structure: controls calibrated for human operators (sandboxes, allowlists, human-in-the-loop approval) become exploitable when the executor can influence its own containment.[^cursor]

## Agent Identity vs Non-Human Identity

The report separates two layers the industry conflates: [[non-human-identity\|Non-Human Identity (NHI)]] is an authentication primitive (is this credential valid?); Agent Identity is a governance framework that attests provenance, intent, and authority continuously.[^nhidef] NHI gates identity at session start; Agent Identity must govern behavior at the moment of each action.[^nhidef] Scale evidence cited from Entro Security's 2025 NHI report: 97% of NHIs carry excessive privileges, 0.01% of machine identities control 80% of cloud resources, 71% are not rotated within recommended timeframes.[^entro] See [[agent-identity-architecture|AI Agent Identity Architecture]] and [[nhi-governance-for-agents|NHI Governance for AI Agents]].

## Enterprise Adoption Maturity Model

The model crosses two dimensions: Adoption Tier (AT0–AT8, what is deployed) against Governance Maturity (Levels 0–4, how mature the governing capability is).[^mm] Required governance scales with tier, not with a flat checklist.[^mm]

| Tier | Name | Defining characteristic |
|---|---|---|
| AT0 | Shadow AI | No org awareness; users self-adopt outside governance[^tiers] |
| AT1 | Vendor Embedded Assistant | Fully vendor-controlled (e.g. M365 Copilot)[^tiers] |
| AT2 | Platform Integrated | AI-native platform on your data; no arbitrary code[^tiers] |
| AT3 | Citizen Developer Agent | Low/no-code flows acting on org data[^tiers] |
| AT4 | Code Executing Agent | Generates and executes code with local/cloud privileges[^tiers] |
| AT5 | Custom In-House Agent | You built it; you control identity, tools, boundaries[^tiers] |
| AT6 | Externally Extended Agent | Connects to external tools/services across trust boundaries[^tiers] |
| AT7 | Multi-Agent Orchestration | Multiple agents coordinate within the org[^tiers] |
| AT8 | Federated / Cross-Boundary | Agents operate across organizational boundaries[^tiers] |

| Level | Name | Posture |
|---|---|---|
| 0 | Unaware and Ad Hoc | No formal recognition of agentic risk; informal oversight[^levels] |
| 1 | Experimentation without Guardrails | Pilots without autonomy limits or escalation criteria[^levels] |
| 2 | Policy-Defined, Human-in-the-Loop | Policies map to regulation; HITL for high-impact decisions[^levels] |
| 3 | Integrated, Continuous Oversight | Real-time monitoring, kill switches, governance-as-code[^levels] |
| 4 | Adaptive, Self-Regulating | Governance at model speed; crypto identity, auto-tuned guardrails[^levels] |

The Governance Posture Matrix marks insufficient combinations: AT0 is CRITICAL at every level below continuous monitoring (the only remedy is elimination into managed tiers); AT8 Federated is "DO NOT DEPLOY" below Level 3.[^matrix] CSA's 2026 survey found only 27% of organizations planning agentic deployments felt confident in their ability to secure them.[^csa27]

## ASI Risk Classes by Adoption Tier

| Tier band | Dominant ASI risks |
|---|---|
| AT0 Shadow AI | ASI01 Goal Hijack, ASI06 Memory Poisoning, ASI09 Human-Agent Trust[^asitier] |
| AT1–AT2 Vendor/Platform | ASI01, ASI06 (constrained by vendor controls)[^asitier] |
| AT3 Citizen-Developer | Adds ASI02, ASI03, ASI05 (action without review)[^asitier] |
| AT4–AT5 Code-Exec/Custom | ASI05 Unexpected Code Execution dominant; 6–8 ASI entries active[^asitier] |
| AT6–AT7 External/Multi-Agent | ASI02 escalates; ASI04, ASI07, ASI08, ASI10 activate[^asitier] |
| AT8 Federated | All ASI01–ASI10 at maximum severity; systemic cross-org risk[^asitier] |

## Regulatory Landscape (10 jurisdictions, 42 instruments)

| Instrument | Agentic-relevant obligation |
|---|---|
| EU AI Act | Art 14 human oversight, Art 72 post-market drift monitoring, Art 25 value-chain liability, Art 73 serious-incident reporting[^euai] (article numbers verified in [[standards-review-eu-ai-act-2026-Q2\|the 2026-Q2 EU AI Act review]]) |
| DORA | 4-hour initial incident notification; annual threat-led penetration testing (financial sector)[^dora] |
| NIS2 | 24-hour early warning; Art 21(2)(d) supply-chain security (critical infrastructure)[^nis2] |
| GDPR Art 22 | Hard floor on agent autonomy in consequential automated decisions[^gdpr] |
| EU Revised Product Liability Directive | Strict no-fault liability across the AI value chain (effective Dec 2026)[^rpld] |
| NY RAISE Act / CA SB 53 | 72-hour / 15-day safety incident reporting; \$1M-per-violation penalties[^usstate] |

Compressed reporting clocks (DORA 4h, NIS2 24h, NY RAISE 72h, CA SB 53 15 days) assume continuous oversight, not quarterly review.[^clocks] U.S. federal preemption of 145+ state AI laws is unresolved, leaving conflicting obligations with no safe harbor.[^frag]

## Notable Findings

- **Adoption velocity is the governance problem.** Coding agents are 53% of tracked repos and the fastest-growing category; seven projects ship releases daily or faster, compressing the gap between first commit and enterprise use to below traditional security-review cycles.[^coding]
- **Cyber-insurance coverage is collapsing.** Verisk's ISO CGL AI exclusions took effect January 2026; WR Berkley filed an absolute AI exclusion. A dedicated AI-insurance market (Armilla, Testudo, HSB/Munich Re) requires demonstrated governance as an underwriting prerequisite, so security posture now determines insurability.[^insurance]
- **Adversarial agent weaponization is documented.** The GTG-1002 campaign used jailbroken Claude Code for largely autonomous espionage against ~30 organizations, with AI executing 80–90% of tactical operations; CrowdStrike reported an 89% increase in AI-enabled attacks with average breakout time falling to 29 minutes.[^weapon]
- **Personal-agent coverage is partial.** The OpenClaw threat model (37 threats, 8 tactics) maps 24 threats directly to ASI categories; the multi-agent categories ASI07/ASI08/ASI10 do not apply to single-agent architectures, and four patterns (config tampering, approval-prompt manipulation, staged payload delivery, pairing/token theft) fall outside the Top 10.[^personal]
- **What remains unsolved.** Three structural problems lack solutions: the assurance model (pre-deployment docs describe a system that composes behavior at runtime), human oversight at machine speed (an agent at 10,000 actions/hour against a reviewer evaluating 50 covers 0.5% of decisions), and regulatory fragmentation across jurisdictions.[^unsolved]

## Strengths and Weaknesses

Strengths:

- The Adoption-Tier × Governance-Maturity matrix gives boards and risk leaders a falsifiable self-assessment instrument rather than a flat checklist, with explicit "do not deploy" cells.[^matrix]
- Evidence-grounded: nearly every threat carries a named 2025–2026 incident or CVE, and quantitative claims cite external sources (a16z, Entro, CSA, CrowdStrike).
- The safety/security convergence argument is operationally specific (shared telemetry, shared root cause, shared incident-response), not rhetorical.[^safety]

Weaknesses:

- An awareness-and-governance document, not a compliance standard: no `shall`-level requirements, no certification or audit-evidence mechanism, no graded maturity criteria that an assessor could score. The same enforceability limit applies as to the [[owasp-agentic-ai-top-10|ASI Top 10]].
- Several headline figures are cited to secondary or vendor sources rather than primary data (the a16z adoption percentages, the Entro NHI statistics, the CSA 27% confidence figure), and some incident attributions name colloquial campaign labels (GTG-1002, ClawHavoc, hackerbot-claw) without primary links.
- The maturity model assumes organizations can inventory and classify their agents at Level 1+; the report acknowledges this inventory step is hardest for the lightweight-library and low-code implementations that carry the most Shadow AI risk.[^impl]

## Relations

- Companion to [[owasp-agentic-ai-threats-mitigations|the Agentic AI Threats and Mitigations guide]] and the [[owasp-agentic-ai-top-10|ASI Top 10]]: this report applies their taxonomy to a governance maturity model and a documented incident base.
- Cited as a "see also" companion in [[owasp-asi-aiuc1-crosswalk|the OWASP ASI to AIUC-1 crosswalk]] (ASI09 Human-Agent Trust Exploitation), which maps the ASI categories to [[aiuc-1|AIUC-1]] certification requirements.
- Supplies the OWASP-side governance maturity framing that the wiki's [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] sits alongside; the report's Adoption Tier dimension is orthogonal to the CMM's nine control domains.
- Reinforces [[non-human-identity|Non-Human Identity]] and [[agent-identity-architecture|AI Agent Identity Architecture]] by separating the authentication primitive from the governance layer.
- Extends [[shadow-ai|Shadow AI]] and [[vibe-coding|Vibe Coding]] as the named drivers of the governance gap, and treats AT0 Shadow AI as the baseline state every organization must discover first.
- Scores severity via the AIVSS measurement layer; see [[owasp-aivss|AIVSS]] and the [[standards-review-owasp-agentic-aivss-2026-Q2|2026-Q2 OWASP standards review]].
- Pairs with the [[owasp-ai-exchange|OWASP AI Exchange]] as the adoption-process counterpart to this report's Levels 0–4 governance ladder and Adoption Tiers: the Exchange's five-step G.U.A.R.D. model organizes the same governance work OWASP's other flagship project measures. The Exchange does not restate, contradict, or supersede this report's findings.

[^exec]: OWASP GenAI Security Project, *State of Agentic AI Security and Governance*, v2.01, June 2026 — Executive Summary (pp. 7–8): "Most organizations are deploying agents faster than they can govern them. More budget for the programs we already run will not close that gap"; three findings. `.raw/papers/owasp-state-of-agentic-ai-security-governance-v2.01.pdf`.
[^mm]: Same source, Enterprise Adoption Maturity Model (pp. 53–60): two dimensions (Adoption Tier AT0–AT8, Governance Maturity Levels 0–4) and the Governance Posture Matrix.
[^scope]: Same source, Scope and Audience / Fit with Agentic Initiative Resources (pp. 9–11) and Agents Taxonomy Scope (p. 12): CISO/C-level audience; detailed controls deferred to companion OWASP resources.
[^tracker]: Same source, Real-World Incidents and Exploits Tracker (pp. 35–36): "424+ Total CVEs, 74 Critical, 17 Platforms Studied"; "2026* OpenClaw 238, MCP 30+, CrewAI, PraisonAI (Q1 only): 290 CVEs."
[^survey]: Same source, Notable Agentic Projects Survey (p. 20) and Appendix 4 (pp. 118–123): GitHub telemetry across 53 tracked repositories, snapshot April 2026.
[^a16z]: Same source, Notable Agentic Projects (p. 20) and Enterprise Adoption Maturity Model (p. 53): a16z April 2026 analysis — 29% of Fortune 500 and ~19% of Global 2000 are live paying customers of a leading AI startup.
[^reg]: Same source, Appendix 2: Global Regulatory and Compliance Landscape (p. 77): "42 regulatory instruments, standards, and frameworks across ten jurisdictions … plus 12 watchlist items … nine dimensions … verified through April 4, 2026."
[^license]: Same source, License and Usage (p. 2): Creative Commons CC BY-SA 4.0.
[^tax]: Same source, Agents Taxonomy (pp. 12–13): three independent dimensions; "Deployed agent = Agent Type + Implementation Pattern + Composition Pattern, at some autonomy level."
[^impl]: Same source, Implementation Patterns (pp. 15–16): governance properties are builder-determined (lightweight library) or platform-dependent (low-code); highest Shadow AI risk; ForcedLeak (Agentforce) cited at the low-code layer.
[^comp]: Same source, Composition Patterns (pp. 17–18): single-agent lethal-trifecta risk, multi-agent shared-memory poisoning, distributed-chain trust transitivity, agent-spawning permission inheritance.
[^pi]: Same source, Prompt Injection: The Primary Delivery Mechanism (pp. 24–25): LLM01:2025; data/control plane collapse; maps to six of ten ASI categories; cites Willison's lethal trifecta and Meta's "Agents Rule of Two."
[^supply]: Same source, The Agent Supply Chain Moved from Theory to Active Exploitation (pp. 25–26): postmark-mcp first malicious MCP server; CVE-2025-6514 (CVSS 9.6); Tool Poisoning Attacks; ClawHavoc / ToxicSkills; hackerbot-claw / LiteLLM token theft.
[^shadow]: Same source, Vibe Coding and Shadow AI (pp. 23–24): roughly half of employees use unsanctioned AI; only 37% of organizations have policies to manage AI or detect shadow AI (IBM Cost of a Data Breach Report).
[^nhi]: Same source, The Agent Identity and Governance Gap (p. 26) and Agent Identity vs NHI (p. 40): non-human identities outnumber human users 100:1, with some organizations reporting 500:1.
[^layer]: Same source, The Attack Surface Model Found Its Shape (pp. 26–27): CSA MAESTRO, AWS scoping matrix, NVIDIA/Lakera, Google A2A converge on a layered model.
[^safety]: Same source, AI Safety vs AI Security (pp. 28–34): definitions table (p. 28), convergence-trends table (p. 32), "Safety and security, governed together" five-dimensions figure (p. 34).
[^inc]: Same source, Real-World Incidents and Exploits Tracker table (p. 36): Claude Skills ransomware (Cato CTRL), Codex CLI sandbox bypass (NVD), EchoLeak (Microsoft / Aim Security), A2A agent-in-the-middle (Trustwave).
[^cursor]: Same source, The Expanding Autonomy of Agents — Coding Agents (p. 23) and Appendix 1.2 (p. 72): CVE-2026-22708 (Cursor allowlist bypass) and CVE-2025-59532 (Codex CLI sandbox boundary); "controls calibrated for human operators become exploitable when the executor can influence its own containment."
[^nhidef]: Same source, Agent Identity vs Non-Human Identity (NHI) (pp. 40–45): NHI as authentication primitive vs Agent Identity as governance framework; Provenance, Attestation, Intent; "Agentic identity control plane" figure (p. 45).
[^entro]: Same source, Agent Identity vs NHI (p. 40): Entro Security's 2025 State of NHI report — 97% of NHIs carry excessive privileges; 0.01% of machine identities control 80% of cloud resources; 71% not rotated within recommended timeframes.
[^tiers]: Same source, Adoption Tier as a Maturity Dimension (pp. 53–55): Agentic Adoption Tiers AT0–AT8 table.
[^levels]: Same source, Agentic AI Governance Maturity Model (pp. 55–57): Maturity Levels 0–4 with key enterprise actions.
[^matrix]: Same source, Maturity Level × Adoption Tier: Governance Posture Matrix (pp. 57–58): AT0 CRITICAL / elimination-only; AT8 "DO NOT DEPLOY" below Level 3; bold cells = governance insufficient for tier.
[^csa27]: Same source, Governance-Deployment Collision at Advanced Adoption Tiers (p. 67): "CSA's 2026 survey found only 27% of organizations planning agentic deployments felt confident in their ability to secure them."
[^asitier]: Same source, Appendix 3: Key ASI Risk Classes by Adoption Tier (pp. 116–117).
[^euai]: Same source, Appendix 2.1 EU (pp. 77–80): EU AI Act Art 14 (human oversight), Art 72 (post-market monitoring / behavioral drift), Art 25 (value-chain liability), Art 6/Annex III high-risk classification.
[^dora]: Same source, Appendix 2.1 (p. 78): DORA — 4-hour initial incident notification; annual threat-led penetration testing; enforceable Jan 17, 2025; fines up to 2% annual turnover.
[^nis2]: Same source, Appendix 2.1 (p. 79): NIS2 — 24-hour early warning, 72-hour notification, one-month final report; Article 21(2)(d) supply-chain security.
[^gdpr]: Same source, Appendix 2.1 (p. 78): GDPR Article 22 — hard floor on agent autonomy in solely-automated consequential decisions; right to contest.
[^rpld]: Same source, Appendix 2.1 (p. 79): EU Revised Product Liability Directive (2024/2853) — strict no-fault liability across the AI value chain; no maximum liability cap.
[^usstate]: Same source, From Static Compliance to Runtime Governance (p. 64): NY RAISE Act (\$1M first violation; 72-hour safety incident reporting), California SB 53 (\$1M per violation; 15-day window), Colorado SB 24-205 (up to \$20,000 per violation, delayed to June 30, 2026).
[^clocks]: Same source, Executive Summary (p. 8) and From Static Compliance to Runtime Governance (p. 64): DORA 4-hour, NIS2 24-hour, NY RAISE 72-hour, CA SB 53 15-day windows assume continuous oversight.
[^frag]: Same source, What Remains Unsolved (p. 67): U.S. federal preemption unresolved; "over 145 state AI laws enacted in 2025 create overlapping obligations with conflicting definitions and penalty structures."
[^coding]: Same source, Notable Agentic Projects Survey and Key Trends (pp. 20–21): 28 of 53 repos classified as coding agents (53%); seven projects ship releases daily or faster; trycua/cua averaged a release every 8 hours.
[^insurance]: Same source, Cyber Insurance Coverage Collapse for Agentic AI Deployments (pp. 67–68): Verisk ISO CGL exclusions effective January 2026; WR Berkley absolute AI exclusion; Armilla, Testudo, HSB/Munich Re require demonstrated governance as underwriting prerequisite.
[^weapon]: Same source, Adversarial Agent Weaponisation (p. 68): GTG-1002 jailbroken Claude Code espionage against ~30 organizations (AI executing 80–90% of tactical operations); CrowdStrike 2026 report — 89% increase in AI-enabled adversary attacks, average breakout time 29 minutes.
[^personal]: Same source, Appendix 6: The Top 10 Impacting Personal Agents (pp. 128–133): OpenClaw threat model — 37 threats across 8 tactics; 24 direct ASI matches; ASI07/ASI08/ASI10 do not apply to single-agent architecture; four product-type gaps.
[^unsolved]: Same source, What Remains Unsolved (pp. 66–67): assurance model mismatch, human oversight at machine speed (10,000 actions/hour vs 50 reviewed = 0.5% coverage), regulatory fragmentation.

- [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] — reads the governance constraint reported here as one the defender side inherits from the application side: organizations deploy agents faster than they can govern them, and a SOC built on agentic tooling is subject to the same gap it is meant to close.
