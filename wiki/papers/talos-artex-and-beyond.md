---
type: paper
title: "Talos ARTEX and Beyond Report"
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
  - "Cisco Talos"
venue: "Cisco Talos Intelligence Blog, 2026-11-13"
source_url: "https://blog.talosintelligence.com/artex-and-beyond/"
archived_copy: ".raw/articles/talos-artex-and-beyond-2026-11-13.md"
no_public_url: ""
key_claim: "AI-enabled penetration testing frameworks are now a standing part of the threat landscape; ARTEX works as an orchestration layer over existing offensive tools rather than a bundled C2, is used largely by financially motivated and China-nexus actors, and shares infrastructure with a wide set of conventional tooling."
methodology: "Talos infrastructure analysis of more than 2,500 ARTEX-associated C2 IP addresses and the tools co-observed on them, an internet search for reachable PentAGI instances, Talos's own Storm-3358 observations and South Korea artifact analysis, and a roll-up of public vendor reporting with links."
contradicts:
  - "[[gtig-artex-and-friends]]"
supports:
  - "[[microsoft-storm-0940-artex]]"
related:
  - "[[artex]]"
  - "[[agentic-offensive-wave-precursors-2026-11]]"
  - "[[agentic-offensive-framework]]"
  - "[[storm-0940-artex-connectwise]]"
  - "[[gtig-artex-and-friends]]"
  - "[[fbi-artex-flash-2026-11]]"
  - "[[microsoft-storm-0940-artex]]"
  - "[[crowdstrike-vapor-panda-artex]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[cisco]]"
sources:
  - "https://blog.talosintelligence.com/artex-and-beyond/"
  - ".raw/articles/talos-artex-and-beyond-2026-11-13.md"
  - "https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks"
  - "https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/"
  - "https://www.microsoft.com/en-us/security/blog/2026/11/06/artex-abuse-expands-as-cybercriminals-target-financial-sector/"
  - "https://www.ic3.gov/CSA/2026/261113.pdf"
verified: 2026-11-18
verified_against:
  - ".raw/articles/talos-artex-and-beyond-2026-11-13.md"
verified_findings: 0
verified_note: "1 low fixed; conflicts cross-checked against GTIG, CrowdStrike, Microsoft 3358 and FBI raws"
---

# Talos ARTEX and Beyond Report

**Source:** [Cisco Talos — ARTEX and beyond: AI-enabled penetration testing platforms](https://blog.talosintelligence.com/artex-and-beyond/) (2026-11-13). Local copy: `.raw/articles/talos-artex-and-beyond-2026-11-13.md`.

[[cisco|Cisco]] Talos surveys AI-enabled penetration testing frameworks as a class and then examines [[artex|ARTEX]] in detail: its architecture, the campaigns reported against it, and the tooling found on infrastructure associated with it. The report's distinct contributions are an infrastructure view of the [[agentic-offensive-framework|agentic offensive framework]] wave, built from more than 2,500 ARTEX-associated C2 IP addresses, and the most explicit statement among the five vendor and government reports of what ARTEX ships with. Talos published it the same day as the [[fbi-artex-flash-2026-11|FBI ARTEX FLASH Advisory]], and neither document cites the other.

### Key claim

Talos treats the frameworks as "the new reality" and describes ARTEX as an orchestration layer that "malicious actors can use to leverage extant offensive tools at scale and speed." Two consequences for defenders follow in the report: detection must work faster and without known indicators of compromise, and the frameworks "work at speeds that can make it impossible for human operators to identify, assess and react in time to disrupt the attack." Talos forecasts that "it is highly likely that future attack chains involving these frameworks will culminate in data exfiltration and ransomware activation, all being done autonomously," and gives no timeframe.

### Evidence base

The report combines three kinds of evidence of different strength.

- **Talos infrastructure analysis.** More than 2,500 ARTEX-associated C2 IP addresses, with the tools observed on them compared by observation period. The report does not describe how an address comes to count as ARTEX-associated, or the window of the count.
- **Talos case observations.** Its own observations of Storm-3358, and analysis of directories used by the actor that attacked South Korean financial organizations.
- **Vendor roll-up.** Linked summaries of reporting by CrowdStrike, Microsoft, Check Point, Datadog and Dosu.

### ARTEX architecture and C2

Talos describes ARTEX as "an AI-enabled penetration testing framework developed under the pseudonym Autumn-27 and published on GitHub in July 2026." An operator gives a target and objective, "a planner agent breaks the objective into tasks, worker agents perform them using security tools," and the framework records assets, findings and progress in a shared graph. ARTEX can use different LLMs and needs little operator input once a task starts.

On C2, Talos is explicit: "ARTEX is not natively bundled with any traditional command-and-control (C2) framework, though the Model Context Protocol (MCP) extensibility layer means any compatible tool server can be added by an operator." Talos uses that design to explain why it finds ARTEX deployed alongside VShell, GoPhish and AdaptixC2.

### Other frameworks

- **Hephaestus.** A command-line framework deployable with one script on a VPS and "completely controllable via Telegram." Talos reports that such frameworks automate the full attack chain using internet mapping services, CAPTCHA bypass services, credential leak services, residential proxies and hash cracking services.
- **PentAGI.** A web interface with an attack-chain graph. Talos found "more than 700 instances" reachable on the internet through a linked Censys search.
- **HexStrike AI.** Integrates with Claude Code, VS Code and other MCP clients. Talos attributes no abuse to it.

### Reported campaigns

Talos concludes that ARTEX "is largely used by financially motivated cybercriminals and threat actors with a China nexus," and that its open-source release and the forks that followed "almost certainly facilitated adoption across different actor groups." Reported outcomes "vary from unsuccessful attempts to confirmed compromise and data exfiltration." Its campaign roll-up covers six categories.

- **VAPOR PANDA.** Summarized from CrowdStrike.
- **Storm-0940.** Summarized from Microsoft as attempted sign-ins against more than 2,250 downstream Microsoft 365 tenants.
- **Storm-3358.** Talos's own observations "are in line with Microsoft's findings," and addresses tied to the cluster ran asset scanning in October 2026.
- **Unidentified Chinese-speaking group in South Korea.** CrowdStrike "assessed with moderate confidence" that the actor was Chinese-speaking and financially motivated. Talos analyzed the actor's directories and found ARTEX-related directory listings and prompt-history references. The archive held no ARTEX executable or source tree, so in Talos's words the artifacts "support an association with an ARTEX environment but do not, by themselves, prove that the complete framework was present or used in a successful attack."
- **Unidentified criminal operators.** Check Point documented theft of code-signing certificates and gaming credentials and attempted cryptocurrency theft.
- **Unattributed activity.** Microsoft reported an agent moving from searching GitHub for secrets to exfiltrating Azure Key Vault data within five minutes. Datadog reported ARTEX activity in AWS environments. Dosu disclosed an incident with more than 80,000 requests in 90 minutes.

### Tooling on ARTEX-associated infrastructure

Talos lists 16 tools observed on ARTEX-associated C2 addresses during overlapping periods, ordered from most to least frequent. The first five are ARTEX derivatives or sibling frameworks.

- **ScopeWeaver.** A localized ARTEX derivative translated into English and Korean.
- **AntiFox.** Described as a "delivery-enhanced version of ARTEX"; the rest of Talos's description is unclear.
- **CyberStrikeAI.** An AI security testing platform that coordinates agents and external tools.
- **PentAGI.** An open-source, self-hosted platform whose agents plan and run penetration tests in an isolated environment.
- **ScopeSentry.** An asset discovery and vulnerability scanning platform "developed by the same pseudonymous author as ARTEX."

The remaining eleven are conventional tools: VShell, Nessus, Interactsh, Acunetix, Fscan, Net Proxy Server, AdaptixC2, Mimikatz, Cobalt Strike, Gophish and Chisel. Talos warns that "co-occurrence does not confirm with certainty that ARTEX invoked each tool or that all were operated by the same actor," and offers the list for prioritizing defenses.

### Recommendations and coverage

Talos's recommendations concentrate on the fundamentals that slow a fast attacker.

- Correlate authentication, cloud API and endpoint logs to catch high-volume requests, unfamiliar applications, token use and privilege changes.
- Require MFA for remote and privileged access, and rate-limit and alert on password spraying.
- Enable EDR tamper protection and sensor health monitoring.
- Scan public repositories for exposed secrets, and revoke and rotate what is found.
- Restrict user consent and apply least privilege to service accounts and applications.
- Treat tool signatures as leads rather than the sole detection, and keep procedures ready to revoke tokens and isolate systems.

Talos ships Snort coverage for ARTEX and Snort and ClamAV coverage for AdaptixC2, VShell and Fscan, and publishes IOCs in its GitHub repository. Two of the ARTEX Snort rules detect artifacts of "norma, the agentic harness framework being used in Artex." Talos releases them as POLICY rules, disabled by default, to be enabled only by customers who intend to detect norma.

### Attribution and confidence wording

- "Almost certainly facilitated adoption" is the report's strongest estimative statement, about the effect of open-source release.
- "Highly likely" carries the forecast of autonomous chains ending in exfiltration and ransomware.
- "Largely used by" financially motivated and China-nexus actors rests on public reporting plus Talos observations, with no count.
- The co-occurrence caveat and the South Korea artifact caveat are the two places where Talos limits its own evidence explicitly.

### Limits of the report

- **The 2,500 figure.** The method that classifies an address as an ARTEX-associated C2 address is not described.
- **Removal.** The report does not mention the October 2026 repository takedown.
- **Own casework.** Talos's first-hand campaign evidence is the Storm-3358 scanning observation and the South Korea artifacts. The other campaign entries summarize other vendors.
- **EDR claim.** "ARTEX has demonstrated EDR bypass capabilities" appears in the recommendations with no supporting case.

### Conflicts with the other ARTEX reports

- **C2 classification.** GTIG calls ARTEX "an autonomous, multi-agent LLM-driven pentesting and offensive command-and-control (C2) platform."[^gtig] Talos says it bundles no C2 framework. GTIG's own case evidence, a command channel over rotating webhook URLs, is consistent with Talos's position that operators assemble C2 around the framework.
- **HexStrike abuse.** GTIG reports "multiple threat actors abusing" Hexstrike, with ARTEX and CAIRN, in incident response engagements.[^gtig] Talos describes HexStrike AI's integrations and reports no abuse of it.
- **EDR bypass.** CrowdStrike shows an operator directing agents to hosts without its sensor, which is avoidance.[^cs] GTIG reports endpoint protection blocking an ARTEX lateral-movement attempt.[^gtig] Neither source documents a bypass.
- **VAPOR PANDA scope.** Talos's "more than 200 organizations in at least 14 countries" joins two CrowdStrike figures that measure different things: 200 organizations worldwide, and 14 countries reached by a single controller in 11 days.[^cs]
- **Exposure measures.** Talos's 2,500 C2 addresses, the FBI's more than 1,600 consoles and Talos's 700 PentAGI instances count different objects with different methods.[^fbi] None of them is a count of operators.
- **Agent architecture.** Talos's planner and worker agents match CrowdStrike's transcripts. GTIG counts six specialized sub-agents.[^gtig][^cs]

### Relations

- The norma rules connect to Microsoft's Storm-3358 post, covered in the [[microsoft-storm-0940-artex|Microsoft Storm-0940 ARTEX Report]], which lists "Norma SDK WebFetch" among ARTEX's default tools. Both vendors name the same harness component.
- [[agentic-offensive-wave-precursors-2026-11|Agentic Offensive Wave Precursors]] sides with Talos on the C2 classification, because GTIG's own case evidence is an operator-assembled webhook channel.
- Talos's forecast of autonomous chains through ransomware bears on [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]] as a prediction. Talos's own description of current campaigns is that operators "typically set the objective and provided limited direction."

### Notes

[^gtig]: [Google Threat Intelligence Group — ARTEX and Friends: Enter Agentic Offensive Frameworks](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks), 2026-10-30. GTIG's ARTEX definition, its list of frameworks abused in incident response, its lifecycle table and its six-sub-agent count.
[^cs]: [CrowdStrike — VAPOR PANDA Uses AI-Driven ARTEX to Grow Global Espionage Campaign](https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/), 2026-10-29. Organizations targeted worldwide, countries reached by one controller over 11 days, and the sensor-avoidance instruction.
[^fbi]: [FBI — Threat Overview of ARTEX Autonomous Penetration Testing Framework, FLASH-20261113-001](https://www.ic3.gov/CSA/2026/261113.pdf), 2026-11-13. Publicly exposed ARTEX console instances in early November 2026, footnoted in the FLASH to Check Point.
