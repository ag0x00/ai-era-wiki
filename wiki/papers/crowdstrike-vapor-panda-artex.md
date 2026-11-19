---
type: paper
title: "CrowdStrike VAPOR PANDA ARTEX Report"
created: 2026-11-18
updated: 2026-11-18
tags:
  - papers
  - threat-intel
  - offensive-ai
  - agentic-offensive-framework
  - artex
  - espionage
status: summarized
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
year: 2026
authors:
  - "CrowdStrike Counter Adversary Operations"
venue: "CrowdStrike blog, 2026-10-29"
source_url: "https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/"
archived_copy: ".raw/articles/crowdstrike-vapor-panda-artex-2026-10-29.md"
no_public_url: ""
key_claim: "From 2026-10-17 the China-nexus espionage adversary VAPOR PANDA used ARTEX with DeepSeek and a second commercial LLM against more than 200 organizations, with post-exploitation confirmed at nine; a human operator steered the agents at decision points, and the campaign widened the adversary's targeting without changing its access-development mission."
methodology: "CrowdStrike OverWatch exploitation telemetry from a single ARTEX controller that intermittently exposed tooling, target files and agent transcripts, plus CrowdStrike Intelligence infrastructure linkage and Falcon endpoint telemetry from victim organizations."
contradicts: []
supports:
  - "[[gtig-artex-and-friends]]"
related:
  - "[[artex]]"
  - "[[agentic-offensive-wave-precursors-2026-11]]"
  - "[[agentic-offensive-framework]]"
  - "[[storm-0940-artex-connectwise]]"
  - "[[gtig-artex-and-friends]]"
  - "[[fbi-artex-flash-2026-11]]"
  - "[[microsoft-storm-0940-artex]]"
  - "[[talos-artex-and-beyond]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[capability-floor-collapse]]"
  - "[[offensive-agent-collective]]"
  - "[[llm-attack-navigator]]"
  - "[[crowdstrike]]"
sources:
  - "https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/"
  - ".raw/articles/crowdstrike-vapor-panda-artex-2026-10-29.md"
  - "https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks"
  - "https://blog.talosintelligence.com/artex-and-beyond/"
  - "https://www.ic3.gov/CSA/2026/261113.pdf"
  - "https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/"
verified: 2026-11-18
verified_against:
  - ".raw/articles/crowdstrike-vapor-panda-artex-2026-10-29.md"
verified_findings: 0
verified_note: "2 low fixed; conflicts cross-checked against GTIG, Talos, FBI and Microsoft raws"
---

# CrowdStrike VAPOR PANDA ARTEX Report

**Source:** [CrowdStrike Counter Adversary Operations — VAPOR PANDA Uses AI-Driven ARTEX to Grow Global Espionage Campaign](https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/) (2026-10-29). Local copy: `.raw/articles/crowdstrike-vapor-panda-artex-2026-10-29.md`.

[[crowdstrike|CrowdStrike]] reports a China-nexus espionage campaign in which the adversary VAPOR PANDA used [[artex|ARTEX]] to run intrusions against many government and corporate targets at once. The report has the most precise outcome counts of the five vendor and government reports on the [[agentic-offensive-framework|agentic offensive framework]] wave, separating organizations targeted from organizations with confirmed post-exploitation. Its other distinct evidence is recovered agent transcripts, which show how the human operator and the agents divided the work.

### Key claim

"Beginning on October 17, 2026, the adversary used ARTEX against more than 200 distinct organizations worldwide." CrowdStrike reads the campaign as "an escalation in China-nexus use of AI agents, from single intrusions to multiple simultaneous engagements directed by a human operator." The adversary's goal did not change: agentic tooling allows "a small team to pursue a wider target set than manual operations would support, rather than a wholesale shift away from the adversary's established access-development mission."

### Evidence base

CrowdStrike OverWatch observed exploitation from a single ARTEX controller. That controller "intermittently exposed adversary tooling, target files, and ARTEX agent transcripts," and CrowdStrike Intelligence linked it to VAPOR PANDA infrastructure. Falcon endpoint telemetry from victim organizations adds the BLOODALCHEMY backdoor deployments and their command-and-control infrastructure. The transcripts are the operator's own working record, recovered from exposed infrastructure rather than disclosed by any victim.

### Scope and outcome counts

The report separates targeting from confirmed compromise.

- **Targeted.** More than 200 distinct organizations worldwide from 2026-10-17. Over an 11-day window, the single observed controller directed exploitation at entities in at least 14 countries.
- **Confirmed post-exploitation.** Nine organizations, where the adversary gained access, deployed BLOODALCHEMY, and ran follow-on credential spraying, network scanning and reconnaissance.
- **Credential activity only.** At least 12 additional organizations, including US think tanks and European government entities, received password spraying and credential scanning.

Named targets include foreign ministries in Latin America, Europe, Asia and the Middle East, a Middle East interior ministry, and telecommunications providers in the US, Belgium and the Philippines. One engagement covered a single European country's foreign ministry, presidency and prime minister's office, with its defense, justice, finance and intelligence entities. A separate engagement targeted 19 US organizations in the pharmaceutical, semiconductor, defense, nuclear and financial sectors.

### Operator and agents

The recovered transcripts show the division of labor between a human and the framework. ARTEX "coordinates a main agent with subordinate planner and worker agents over a shared graph of facts, findings, and goals." An operator issued instructions in Chinese, labelled 【人类指令】 ("human instruction") and timestamped in China Standard Time. The operator directed the agents to write web shells, extract domain administrator passwords and inventory endpoint sensor coverage, and otherwise intervened "only to steer the agents at decision points."

The operator also supplied access material obtained outside the engagement. Against a Greek government entity, the operator gave the agent a 2022 mail-server directory export holding authentication keys that had likely not been rotated since the export, and a VPN client key that the operator said came from a May 2024 email leak. The agent forged an administrator token with the keys, read mailboxes, and reached the internal network with VPN configurations recovered from sent mail.

Across engagements the agents pursued several attack paths in parallel, across web applications, cloud services, containers, Active Directory, and mail and VPN infrastructure, and switched approach when a path failed.

### Endpoint sensor avoidance

The operator built sensor avoidance into the agents' instructions. At a global pharmaceutical organization the operator told the agents to find Windows machines in the domain without Falcon, judging by process lists and folder listings, using 猎鹰 ("falcon"), which CrowdStrike identifies as PRC red-team slang for its sensor. The instruction concentrates follow-on activity on unprotected hosts, and it describes avoiding the sensor rather than defeating it.

### Models and token consumption

VAPOR PANDA ran ARTEX "primarily with the Chinese-origin DeepSeek LLM, supplemented with a second, subscription-based commercial LLM that CrowdStrike Intelligence could not confirm." In a single intrusion, planning agents consumed "on the order of 1.4 billion input tokens." CrowdStrike reads the multi-provider setup as "likely" a deliberate effort to build a resilient toolchain that depends on no single model service.

### Recommendations

- Rotate signing keys, preauthentication keys and service credentials after any suspected exposure, including keys in historical backups and directory exports.
- Enforce SAML signature validation, require IMDSv2, and restrict cloud roles' command-execution permissions.
- Hunt for ARTEX naming markers in command lines, cloud role names and a working directory, which the report lists.
- Patch internet-facing appliances, particularly long-lived Linux servers.
- Deploy EDR across the estate, including on servers considered unreachable from the internet. CrowdStrike names its own Falcon sensor here.

The first recommendation matches the Greek case, where keys from a 2022 export still let the agent forge an administrator token.

### Attribution and confidence wording

- The controller is one "which CrowdStrike Intelligence has linked to VAPOR PANDA infrastructure." The report attaches no confidence level to the attribution.
- The adversary "likely compromised a third" Latin American foreign ministry.
- The campaign's separation from Storm-0940 is "likely indicating the AI-based framework is quickly gaining popularity among multiple China-nexus adversaries."
- The forecast is the report's strongest estimative statement: "China-nexus adversaries will highly likely continue adopting agentic frameworks such as ARTEX over the next 12 months."

### Limits of the report

- **Second model.** The commercial LLM that supplemented DeepSeek is unidentified.
- **Initial access.** The report describes three intrusions in detail and does not say how access began at most of the more than 200 targeted organizations.
- **Mapping of compromises.** The named compromises are listed by victim type and are not mapped one-to-one onto the nine confirmed organizations.
- **Vendor position.** The Falcon-avoidance evidence and the EDR recommendation come from the vendor whose sensor the operator avoided.

### Conflicts with the other ARTEX reports

- **The Asian financial-sector claim.** GTIG cites "public reporting" of ARTEX "targeting the financial sector in Asia" to CrowdStrike.[^gtig] That citation links CrowdStrike's 2026-10-07 report on an unattributed actor targeting South Korean finance, not this report, which names no Asian financial-sector target.
- **Scope compression in Talos.** Talos summarizes the campaign as "more than 200 organizations in at least 14 countries."[^talos] CrowdStrike's 14-country figure covers one controller over 11 days, and its 200-organization figure is worldwide.
- **Stealth-focused actors.** GTIG expects actors that specialize in stealthy long-term access to be "less likely" to use public framework versions.[^gtig] VAPOR PANDA is such an actor by CrowdStrike's description, and it used ARTEX at scale. GTIG's hedge that such actors may still use the tools "for purposes of scale and speed" matches what CrowdStrike reports.
- **EDR capability.** Talos states that "ARTEX has demonstrated EDR bypass capabilities."[^talos] CrowdStrike's evidence shows the operator steering agents away from protected hosts. GTIG reports endpoint protection blocking an ARTEX lateral-movement attempt.[^gtig]
- **Models.** The FBI FLASH's finding that DeepSeek Flash models dominate ARTEX usage is consistent with this report.[^fbi] GTIG names no model.

### Relations

- **Supports [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]]** on operator position. The transcripts place a human at the direction of each engagement, at decision points and at the supply of key material. The case is a deliberately built multi-agent framework aimed by a human, the configuration the thesis already records for the Taiwan intrusion, with a commodity open-source framework in place of a purpose-built one.
- **Bears on [[capability-floor-collapse|Capability Floor Collapse]].** VAPOR PANDA is an established China-nexus targeted-intrusion adversary using ARTEX for scale. The campaign belongs in the concept's cell for skilled operators who set the objective, and gives no evidence for its empty cell for low-skill operators.
- **Fits [[offensive-agent-collective|Offensive Agent Collective]].** The main, planner and worker agents share facts, findings and goals over one graph, the common channel the concept requires.
- **Bears on [[llm-attack-navigator|LLM ATT&CK Navigator]].** The Navigator found that the behaviors marking the highest-risk AI-enabled actors carry no ATT&CK identifiers. CrowdStrike maps ARTEX use to T1588.007 (Obtain Capabilities: Artificial Intelligence) under resource development, which records that a framework was used and says nothing about how far it ran unattended.
- [[agentic-offensive-wave-precursors-2026-11|Agentic Offensive Wave Precursors]] weighs the broadening of VAPOR PANDA's targeting against CrowdStrike's statement that its access-development mission did not change, and finds a change in target selection under the same objectives.
- CrowdStrike states that the campaign "is separate from the ARTEX activity recently reported by industry sources under the Storm-0940 designation," the subject of the [[microsoft-storm-0940-artex|Microsoft Storm-0940 ARTEX Report]] and the [[storm-0940-artex-connectwise|Storm-0940 ARTEX Supply-Chain Campaign]].

### Notes

[^gtig]: [Google Threat Intelligence Group — ARTEX and Friends: Enter Agentic Offensive Frameworks](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks), 2026-10-30. GTIG's citation of Asian financial-sector targeting, its outlook on stealth-focused actors, and its lifecycle table. Summarized at [[gtig-artex-and-friends|GTIG ARTEX and Friends Report]].
[^talos]: [Cisco Talos — ARTEX and beyond: AI-enabled penetration testing platforms](https://blog.talosintelligence.com/artex-and-beyond/), 2026-11-13. Talos's VAPOR PANDA summary and its EDR statement.
[^fbi]: [FBI — Threat Overview of ARTEX Autonomous Penetration Testing Framework, FLASH-20261113-001](https://www.ic3.gov/CSA/2026/261113.pdf), 2026-11-13. Model prevalence across attacker-controlled ARTEX instances. Summarized at [[fbi-artex-flash-2026-11|FBI ARTEX FLASH Advisory]].
