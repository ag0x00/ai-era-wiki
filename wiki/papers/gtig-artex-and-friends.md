---
type: paper
title: "GTIG ARTEX and Friends Report"
created: 2026-11-18
updated: 2026-11-18
tags:
  - papers
  - threat-intel
  - offensive-ai
  - agentic-offensive-framework
  - artex
status: summarized
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
year: 2026
authors:
  - "Aurora Blum"
  - "David Mainor"
  - "Rohit Nambiar"
  - "Omar ElAhdan"
  - "Matthew McWhirt"
  - "Will Silverstone"
  - "Bhavesh Dhake"
  - "Chris Busse"
venue: "Google Threat Intelligence Group and Mandiant blog, 2026-10-30"
source_url: "https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks"
archived_copy: ".raw/articles/gtig-artex-and-friends-2026-10-30.md"
no_public_url: ""
key_claim: "Since late August 2026 threat actors have run open-source multi-agent pentesting frameworks (ARTEX, PentAGI, CAIRN, Hexstrike) in real intrusions; the frameworks leave the techniques unchanged, shorten the time needed to chain them, and trade stealth for velocity."
methodology: "Mandiant incident-response engagements and GTIG research telemetry, with log and timestamp analysis of ARTEX activity inside victim environments; campaign scope for UNC7228 comes from Wiz Research. No engagement, victim or actor counts are given."
contradicts:
  - "[[talos-artex-and-beyond]]"
supports:
  - "[[crowdstrike-vapor-panda-artex]]"
related:
  - "[[artex]]"
  - "[[agentic-offensive-wave-precursors-2026-11]]"
  - "[[agentic-offensive-framework]]"
  - "[[storm-0940-artex-connectwise]]"
  - "[[fbi-artex-flash-2026-11]]"
  - "[[microsoft-storm-0940-artex]]"
  - "[[crowdstrike-vapor-panda-artex]]"
  - "[[talos-artex-and-beyond]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[capability-floor-collapse]]"
  - "[[offensive-agent-collective]]"
  - "[[non-human-identity]]"
  - "[[google]]"
  - "[[wiz]]"
sources:
  - "https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks"
  - ".raw/articles/gtig-artex-and-friends-2026-10-30.md"
  - "https://blog.talosintelligence.com/artex-and-beyond/"
  - "https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/"
  - "https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/"
  - "https://www.ic3.gov/CSA/2026/261113.pdf"
verified: 2026-11-18
verified_against:
  - ".raw/articles/gtig-artex-and-friends-2026-10-30.md"
verified_findings: 0
verified_note: "1 medium (240-event sequence merged two bullets, dropped 'potentially') and 3 low fixed; conflicts cross-checked against Talos, Microsoft, CrowdStrike and FBI raws"
---

# GTIG ARTEX and Friends Report

**Source:** [Google Threat Intelligence Group — ARTEX and Friends: Enter Agentic Offensive Frameworks](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks) (2026-10-30). Local copy: `.raw/articles/gtig-artex-and-friends-2026-10-30.md`.

[[google|Google]] Threat Intelligence Group (GTIG) and Mandiant report that threat actors have operated open-source multi-agent penetration-testing frameworks inside real intrusions since late August 2026. The report names the frameworks, describes what [[artex|ARTEX]] did inside victim environments from Mandiant incident-response logs, places the activity on a seven-stage model of adversarial LLM use, and closes with Mandiant's five defensive layers. Of the five vendor and government reports on the [[agentic-offensive-framework|agentic offensive framework]] wave, it carries the most host-level forensic detail from inside victim environments.

### Key claim

GTIG states that the mechanics of these intrusions stay the same and their tempo changes: "LLM-enabled offensive tooling does not fundamentally change the techniques attackers use. It reduces the time required to combine them." The actors still use compromised credentials and unpatched systems. The frameworks chain those weaknesses at machine speed, and "they currently trade stealth for velocity."

The report also calls the activity "a paradigm shift towards intrusions occurring at machine speed" and says it "fundamentally alters traditional intrusion dynamics." The two framings coexist in the source: the technique set stays fixed while the time a defender has to act shrinks.

### Evidence base

The evidence is Mandiant incident-response engagements, corroborated by GTIG research telemetry. Victim-side log files and timestamps support the forensic findings. The scope of the UNC7228 campaign comes from [[wiz|Wiz]] Research rather than from GTIG's own casework. The report gives no count of engagements, victims or distinct actors, so it supports statements about what the frameworks did and cannot support a rate or a trend.

### Frameworks observed

GTIG names six tools and separates confirmed abuse from experimentation.

- **ARTEX.** GTIG describes it as "an autonomous, multi-agent LLM-driven pentesting and offensive command-and-control (C2) platform written in Go with an embedded Next.js web console," released by the GitHub user Autumn-27. Six specialized sub-agents divide the attack lifecycle, and the framework calls frontier and open-source model APIs, but GTIG names no model. The creator took the original repository down in October 2026 because of widespread threat-actor abuse. Community forks, the localized variant ScopeWeaver and a commercial ARTEX PRO continue to circulate.
- **PentAGI.** VXControl develops PentAGI in open-source and commercial editions. Its agents run standard Kali Linux tools in isolated sandboxes and support more than a dozen model backends. Most observed PentAGI activity stayed at WHOIS asset mapping and vulnerability scanning; some operations reached intrusion and persistence. One customized platform built on PentAGI was likely used between at least late August and early October 2026 to scan system integrators and telecommunications firms in Asia.
- **Hexstrike and CAIRN.** GTIG lists both, with ARTEX, as abused "in incident response engagements."
- **AgentZero and ZeroHollow.** GTIG reports threat-actor experimentation with the AgentZero agent framework and the ZeroHollow inference service, and no intrusions.

### ARTEX in victim environments

The forensic record shows a fast, noisy operator, and each trait the report documents is also a detection opportunity.

- **Burst timing.** Individual ARTEX execution sequences ran in bursts of roughly 2 to 25 seconds between extended pauses. One operation produced 27 events in 23 seconds of burst time while probing AWS permissions.
- **Trial-and-error volume.** On one externally facing web server, ARTEX ran SQL through the application's exposed API to reach partial command execution. The sequence generated 240 log events, with errors GTIG says were "potentially caused by multiple sub-agents overwriting one another while pursuing the same objective." A separate attempt to move from database execution to system execution wrote several webshell candidates, correcting errors until one ran.
- **Recognizable identifiers.** ARTEX named some dropped files and created accounts after the framework, and put incrementing numeric markers in file names, account names, passwords and email subjects. The naming patterns are in the [GTIG report's ARTEX Framework section](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks).
- **Webhook-based command channel.** On one internal database server, ARTEX pulled scripts and returned output across 491 rotating `webhook.site` URLs.
- **Abuse of built-in features.** ARTEX made an application's notification system send test emails to more than 1,300 internal users, then dropped to single-recipient emails.
- **Directory collection.** ARTEX enumerated more than 110,000 Active Directory objects from one domain controller.

Endpoint protection blocked one lateral-movement attempt, and defenders then cut access to the server. In several cases the human operator supplied the target organization's compromised credentials and instructed the framework to run the later stages.

### Actor mix

ARTEX users span "the motivational spectrum": financially motivated extortion groups and suspected espionage operators. GTIG tracks one ARTEX campaign as UNC7228 and says it "overlaps with activity tracked by Microsoft as the PRC-nexus actor STORM-0940." Wiz dates that campaign to at least August 2026, across enterprise software, industrial automation, semiconductor and telecommunications targets in the US and Europe. The actor typically moved from its first compromised account to deep system access in about four hours, and in one case in 15 minutes. Other ARTEX users in Mandiant cases hit government, technology, chemicals and materials, and manufacturing organizations in the US.

### Seven-stage model

GTIG orders adversarial LLM use into seven stages and places the field at stage 5, where LLM-driven tooling runs multiple lifecycle stages in the wild with fewer humans in the loop.

- Stage 6, "multi-agent-driven, unsophisticated attacks with little to no human interaction," carries a near-term forecast of one to three months.
- Stage 7, fully autonomous attacks with added stealth, carries a forecast of three to six months.

GTIG gives no basis for either window beyond the observed progression.

### Defensive layers

Mandiant's five layers target the speed advantage, and the report says plainly that they are "long-standing security best practices."

- **Protect control planes.** Restrict admin APIs, consoles and CI/CD to trusted networks.
- **Restrict identities and keys.** Replace long-lived secrets with short-lived workload credentials, and apply the rigor used for privileged human accounts to service principals, API keys and deployment tokens.
- **Harden configurations and reduce attack paths.** Scan continuously, segment application tiers, and stop returning detailed errors to unauthenticated users, because error text feeds the agent's next attempt.
- **Application protections.** Put rate, recipient and transaction limits on high-impact business APIs.
- **Detect behavioral and volumetric change.** Baseline identity origin, control-plane velocity and egress, and correlate them as sequences.

The second layer is the [[non-human-identity|non-human identity]] problem stated for an attacker who tests every stolen credential as soon as it finds one.

### Attribution and confidence wording

The report attaches an explicit confidence level to one judgment. GTIG "assesses with high confidence that cyber crime threat actors will continue incorporating autonomous offensive tools into their operations." The other judgments use weaker or no qualifiers.

- UNC7228 "overlaps with" Microsoft's Storm-0940. The report does not claim identity.
- Espionage users are "suspected cyber espionage operators" and "additional suspected PRC-nexus clusters."
- The state of the field is a plain assessment: "we assess that the current adversary use of autonomous offensive frameworks is still in early stages."
- Continued adoption is "highly likely." Stealth-focused actors are "less likely to use the publicly available versions of these tools."

### Limits of the report

- **No denominators.** The report counts no engagements, victims or actors, so the "inflection point" claim rests on case detail.
- **No model identification.** "Frontier and open source models" is the only statement about which models ran the agents.
- **Hexstrike and CAIRN.** Both are listed as abused without a single described case.
- **C2 classification.** The report calls ARTEX a C2 platform and does not argue the point; the C2 it documents runs over `webhook.site` and curl.
- **Asian financial-sector targeting.** The report cites this to CrowdStrike's 2026-10-07 South Korea report, a different CrowdStrike publication from the [[crowdstrike-vapor-panda-artex|CrowdStrike VAPOR PANDA ARTEX Report]].

### Conflicts with the other ARTEX reports

- **ARTEX as a C2 platform.** Cisco Talos states that "ARTEX is not natively bundled with any traditional command-and-control (C2) framework," and attributes C2 tooling on ARTEX hosts to operators adding tool servers through MCP.[^talos] GTIG's own case evidence, an ad hoc webhook channel, fits Talos's description better than GTIG's label does.
- **Start date of the Storm-0940-overlapping cluster.** GTIG and Wiz date UNC7228 to "at least August 2026"; Microsoft says Storm-0940 used ARTEX "as early as September 2026."[^ms] An overlap between clusters can explain the gap, and neither source reconciles it.
- **Hexstrike abuse.** GTIG reports it in incident response. Talos describes HexStrike AI's MCP integration with Claude Code and VS Code and attributes no abuse to it, and the FBI FLASH does not name it.[^talos][^fbi]
- **Agent architecture.** GTIG counts six specialized sub-agents. Talos describes one planner agent with worker agents, and CrowdStrike a main agent over subordinate planner and worker agents.[^talos][^cs]
- **Speed.** GTIG's four hours and 15 minutes measure first compromised account to deep system access. Microsoft's "less than 3 hours" measures initial targeting to privileged access.[^ms] The figures start at different events and do not compare.
- **Stealthy actors.** GTIG expects actors that "specialize in stealthy operations designed to maintain long-term access" to be less likely to use public versions. CrowdStrike reports VAPOR PANDA, an actor that pursues long-term access, running ARTEX at scale.[^cs] GTIG's hedge, that such actors "may employ them for purposes of scale and speed," covers the case.

### Relations

- **Supports [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]]** on operator position. The intrusions GTIG describes keep a human in the operation, and in several the operator supplied the credentials and instructed the framework. Stage 6 is a forecast, so the report leaves the thesis's open question of an unoperated, deliberately built collective unanswered.
- **Bears on [[capability-floor-collapse|Capability Floor Collapse]].** GTIG says PentAGI's design "enables entry-level users to run complex cyber security campaigns." That is a statement about the tool's design, not a skill assessment of an observed actor, so the concept's empty cell for low-skill operators who set only the objective stays unsourced at case level.
- **Illustrates [[offensive-agent-collective|Offensive Agent Collective]]** from the failure side. GTIG reads the errors in the SQL-API sequence as "potentially caused by multiple sub-agents overwriting one another," which would be agents sharing one objective without working coordination.
- The [[fbi-artex-flash-2026-11|FBI ARTEX FLASH Advisory]] reproduces most rows of GTIG's lifecycle table and several Mandiant mitigations, so it does not corroborate GTIG independently.
- The [[storm-0940-artex-connectwise|Storm-0940 ARTEX Supply-Chain Campaign]] page carries the UNC7228 overlap.
- [[agentic-offensive-wave-precursors-2026-11|Agentic Offensive Wave Precursors]] cites GTIG's statement that the tooling leaves techniques unchanged as the main evidence against a change in attacker intent.

### Notes

[^talos]: [Cisco Talos — ARTEX and beyond: AI-enabled penetration testing platforms](https://blog.talosintelligence.com/artex-and-beyond/), 2026-11-13. Talos's description of ARTEX's planner and worker agents, its C2 statement and its HexStrike AI description.
[^ms]: [Microsoft Threat Intelligence — AI-driven, large-scale nation-state campaign using ARTEX enables supply chain compromise](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/), 2026-10-21. Storm-0940 start date and the initial-targeting-to-privileged-access interval.
[^cs]: [CrowdStrike — VAPOR PANDA Uses AI-Driven ARTEX to Grow Global Espionage Campaign](https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/), 2026-10-29. ARTEX agent hierarchy as recovered from transcripts; VAPOR PANDA's access-development mission.
[^fbi]: [FBI — Threat Overview of ARTEX Autonomous Penetration Testing Framework, FLASH-20261113-001](https://www.ic3.gov/CSA/2026/261113.pdf), 2026-11-13. The FLASH names no framework other than ARTEX.
