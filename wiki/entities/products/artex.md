---
type: entity
title: "ARTEX"
created: 2026-11-18
updated: 2026-11-18
tags:
  - entities
  - product
  - offensive-ai
  - agentic-offensive-framework
  - threat-tooling
status: developing
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
entity_type: product
role: "Open-source, LLM-driven multi-agent penetration testing framework, released on GitHub in July 2026 and adopted by state-aligned and financially motivated intrusion operators"
homepage: ""
first_mentioned: "[[gtig-artex-and-friends]]"
related:
  - "[[agentic-offensive-framework]]"
  - "[[storm-0940-artex-connectwise]]"
  - "[[gtig-artex-and-friends]]"
  - "[[fbi-artex-flash-2026-11]]"
  - "[[microsoft-storm-0940-artex]]"
  - "[[crowdstrike-vapor-panda-artex]]"
  - "[[talos-artex-and-beyond]]"
  - "[[agentic-offensive-wave-precursors-2026-11]]"
  - "[[agentic-offensive-wave-open-questions]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[capability-floor-collapse]]"
  - "[[gtg-1002-ai-orchestrated-espionage]]"
  - "[[mcp-security]]"
  - "[[claude-code]]"
sources:
  - "https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks"
  - "https://www.ic3.gov/CSA/2026/261113.pdf"
  - "https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/"
  - "https://www.microsoft.com/en-us/security/blog/2026/11/06/artex-abuse-expands-as-cybercriminals-target-financial-sector/"
  - "https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/"
  - "https://blog.talosintelligence.com/artex-and-beyond/"
  - "https://censys.com/blog/artex-agentic-asbestos"
  - "https://www.elastic.co/security-labs/threat-command/agentic-offensive-frameworks-detection-artex"
  - "[[.raw/articles/censys-artex-agentic-asbestos-2026-11-05.md]]"
  - "[[.raw/articles/elastic-agentic-offensive-frameworks-detection-artex-2026-11-13.md]]"
  - "[[.raw/articles/gtig-artex-and-friends-2026-10-30.md]]"
  - "[[.raw/reports/fbi-flash-20261113-001-artex-2026-11-13.md]]"
  - "[[.raw/articles/microsoft-storm-0940-artex-2026-10-21.md]]"
  - "[[.raw/articles/microsoft-storm-3358-artex-2026-11-06.md]]"
  - "[[.raw/articles/crowdstrike-vapor-panda-artex-2026-10-29.md]]"
  - "[[.raw/articles/talos-artex-and-beyond-2026-11-13.md]]"
verified: 2026-11-18
verified_against:
  - ".raw/articles/censys-artex-agentic-asbestos-2026-11-05.md"
  - ".raw/articles/crowdstrike-vapor-panda-artex-2026-10-29.md"
  - ".raw/articles/elastic-agentic-offensive-frameworks-detection-artex-2026-11-13.md"
  - ".raw/articles/gtig-artex-and-friends-2026-10-30.md"
  - ".raw/articles/microsoft-storm-0940-artex-2026-10-21.md"
  - ".raw/articles/microsoft-storm-3358-artex-2026-11-06.md"
  - ".raw/articles/talos-artex-and-beyond-2026-11-13.md"
  - ".raw/reports/fbi-flash-20261113-001-artex-2026-11-13.md"
verified_findings: 0
verified_note: "2 medium (authorization-in-code overstated; refusal reasoning misattributed to Elastic) and 5 low fixed; artifact strings and Snort IDs replaced with vendor links"
---

# ARTEX

**Sources:** [GTIG — ARTEX and Friends: Enter Agentic Offensive Frameworks](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks) (2026-10-30) · [FBI — FLASH-20261113-001](https://www.ic3.gov/CSA/2026/261113.pdf) (2026-11-13) · [Censys — ARTEX: Agentic Asbestos](https://censys.com/blog/artex-agentic-asbestos) (2026-11-05)

ARTEX is an open-source penetration testing framework in which large language model (LLM) agents plan and carry out an intrusion, from reconnaissance to data exfiltration, against an objective the operator sets. A developer using the handle Autumn-27 published it on GitHub in July 2026. Within four months Microsoft had reported a China-state aligned actor using it, CrowdStrike a China-nexus espionage adversary and Google Threat Intelligence Group (GTIG) suspected PRC-nexus clusters, and the FBI had issued a TLP:CLEAR FLASH describing it as a platform "that threat actors have weaponized to automate multiple phases of cyber intrusions at machine speed".[^fbi] Of the frameworks in the [[agentic-offensive-framework|Agentic Offensive Framework]] class, ARTEX is the one that Microsoft, GTIG, CrowdStrike, Cisco Talos and the FBI all report on.

## Profile

### Architecture

An operator defines a task that states the scope, any test accounts and the goal. A planner agent turns the goal into units of work, worker agents claim those units and run security tools, and an event-driven engine repeats the cycle until the goal is met.[^censys] The agents record assets, findings and progress in a shared graph, which Censys describes as two graphs: one of what the target looks like and one of how much of it has been tested.[^censys][^talos] CrowdStrike places a main agent above the planner and workers.[^cs] GTIG describes the same lifecycle as split across six specialised sub-agents.[^gtig]

The server is written in Go, with a Next.js web console and a PostgreSQL store.[^censys] It ships with conventional offensive tooling rather than malware of its own. Microsoft lists Nmap, curl, wget, Python HTTP clients, the Norma SDK web-fetch tool and headless Chromium among its defaults,[^ms3358] and Elastic found that each task runs three parallel workers driving scanners such as nuclei, httpx, ffuf and sqlmap.[^elastic] ARTEX also imports results from ScopeSentry, an asset-discovery platform by the same author, into its asset graph.[^censys] Talos states that ARTEX bundles no traditional command-and-control (C2) framework. Its Model Context Protocol (MCP) layer accepts any compatible tool server an operator adds, which Talos gives as the reason ARTEX appears beside VShell, GoPhish and AdaptixC2.[^talos]

The operator's role after launch is small by design. Elastic's source review found the human limited to a chat agent that watches progress and steers with hints, the worker agents running in a bypass permission mode, and agent prompts that describe each agent as part of an authorized penetration testing system. Nothing in the code enforces authorization by default: an operator-written allow list exists and ships empty, and the only target restrictions active by default are blocklists for government and education domains.[^elastic] CrowdStrike's recovered transcripts show the same pattern in use: an operator intervening at decision points with instructions labelled "human instruction".[^cs]

### Models

ARTEX ships no model of its own. The FBI reports that it integrates with external providers, naming Anthropic Claude, OpenAI GPT, DeepSeek, GLM, Grok and Kimi, and that analysis of attacker-controlled instances found Chinese models most prevalent, with the DeepSeek Flash family dominating "due to their balance of capabilities, relatively low cost, and lack of observable guardrails when framed for penetration testing".[^fbi] Censys adds that the framework accepts any OpenAI-compatible endpoint.[^censys]

Campaign reports show how operators assemble model access. VAPOR PANDA ran ARTEX mainly on DeepSeek, with a second subscription-based commercial LLM that CrowdStrike could not confirm, and in a single intrusion its planning agents consumed on the order of 1.4 billion input tokens.[^cs] Censys found LLM gateway or reseller software on 43 ARTEX hosts, chiefly CLIProxyAPI and New API.[^censys] Elastic concludes that a model provider's refusal is not a control defenders can rely on, because a framework can route around it.[^elastic] For ARTEX the operator chooses the model, and the agent prompts describe the work as authorized testing.

### Release and distribution

The repository's history is short, and its removal did not end distribution.

- **2026-07-26.** Autumn-27 published the repository under the AGPL-3.0 licence; the same author maintains ScopeSentry.[^censys]
- **July to October.** The project reached an estimated 1,500 to 2,000 GitHub stars within ten weeks,[^censys] and its developers logged about 390 commits between 26 July and 7 October.[^elastic]
- **2026-10-07.** CrowdStrike reported an unattributed actor using ARTEX against South Korean financial institutions.[^censys]
- **2026-10-08.** Autumn-27 posted a statement denying involvement in the attacks, said ARTEX was meant for testing within authorized asset scopes, and announced it would go closed-source. The FBI dates the repository's removal to the same day.[^fbi][^censys]
- **After removal.** Forks, archived copies and rebrands continue to circulate. GTIG names the localized fork ScopeWeaver and a commercial version called ARTEX PRO.[^gtig] Talos describes ScopeWeaver as translated into English and Korean and names a "delivery-enhanced" variant called AntiFox.[^talos] Censys observed consoles branded ARTEX MX and ARTEX v2.[^censys]

## Use in intrusions

### Operators named by publishers

GTIG reports that since late August 2026 ARTEX "has been adopted by a diverse range of threat actors across the motivational spectrum".[^gtig] The FBI dates malicious use to at least August 2026 and names no actor for the framework as a whole.[^fbi] [[agentic-offensive-wave-precursors-2026-11|Agentic Offensive Wave Precursors]] tests whether that spread marks a change in attacker intent or in means. The table keeps each publisher's own designation and qualifying words.

| Operator | Named by | Nexus and motive, as stated | Reported ARTEX activity |
|---|---|---|---|
| Storm-0940 | Microsoft | "China-state aligned"; Microsoft "assesses" it responsible for the ConnectWise compromise | Attempted sign-in to more than 2,250 tenants after the ConnectWise compromise[^ms0940]; see [[storm-0940-artex-connectwise\|Storm-0940 ARTEX Supply-Chain Campaign]] |
| UNC7228 | GTIG, from Wiz research | "Overlaps with" Storm-0940, which GTIG calls "PRC-nexus" | US and European enterprise software, industrial automation, semiconductor and telecommunications targets[^gtig] |
| VAPOR PANDA | CrowdStrike | China-nexus espionage; controller "linked" to VAPOR PANDA infrastructure | More than 200 organizations targeted from 2026-10-17, post-exploitation confirmed at nine[^cs] |
| Storm-3358 | Microsoft | "Cambodia-based intrusion cluster", financially motivated | Financial organizations, mainly in Southeast Asia; WISP, vshell and AdaptixC2 installed after exploitation[^ms3358] |
| Unattributed | CrowdStrike, as relayed by Talos and Censys | Chinese-speaking and financially motivated, at "moderate confidence" | South Korean financial institutions; personal information of about 67,000 individuals exfiltrated[^fbi] |

CrowdStrike states that the VAPOR PANDA campaign is separate from the Storm-0940 activity, and reads the two as "likely indicating" that the framework "is quickly gaining popularity among multiple China-nexus adversaries".[^cs] Criminal use is wider than one cluster. GTIG reports financially motivated extortion actors using ARTEX for data theft,[^gtig] and Talos summarizes Check Point Research's account of criminal operators pursuing code-signing certificates, gaming-account credentials and cryptocurrency.[^talos]

### Behavior in telemetry

GTIG's incident-response cases show the framework's operating signature. Execution ran in compressed bursts of roughly 2 to 25 seconds separated by long pauses, and one burst issued 27 events in 23 seconds while probing AWS permissions.[^gtig] Code execution came through iterative trial and error: one attempt to run SQL through an application API produced 240 log events, with errors GTIG says were potentially caused by sub-agents overwriting each other.[^gtig] ARTEX also turned legitimate application features against the victim, forcing a notification system to send test emails to more than 1,300 internal users.[^gtig] Microsoft reports that in multiple Storm-0940 cases the time from initial targeting to privileged cloud access was under three hours.[^ms0940]

GTIG summarizes the trade-off: frameworks such as ARTEX "compress attack timelines to machine speed; however, they currently trade stealth for velocity".[^gtig] Microsoft and the FBI draw the consequence for defenders: the traffic is loud enough to raise alerts, but the speed leaves little time to coordinate containment before the agents reach their objective.[^ms0940][^fbi]

### Exposed infrastructure

Censys counted 1,465 hosts exposing an ARTEX console in early November 2026; its summary dates the count to 4 November and its query section to 3 November.[^censys] About 650 of them sit in two blocks of identically configured hosting, which suggests a provisioning template rather than hundreds of operators, and excluding them leaves about 815 distinct-looking deployments.[^censys] The console does not need to face the internet, so the total in use is higher, and Censys notes that some exposed instances likely belong to authorized testers. Of the 1,465 hosts, 155 (10.6%) also ran another threat Censys detects, most often a second AI attack framework (CyberStrikeAI shares 70 hosts with ARTEX) and in a smaller set conventional C2 such as Cobalt Strike, Vshell and Adaptix C2.[^censys] Censys also found 32 open directories on ARTEX hosts exposing post-exploitation kits, stagers and implants, most of them live for hours to a few days.[^censys]

Other counts measure different things. The FBI reports more than 1,600 publicly exposed console instances in early November, citing Check Point Research rather than Censys.[^fbi] Talos identified more than 2,500 ARTEX-associated C2 IP addresses, a count of infrastructure rather than of consoles.[^talos]

## Defense

### Detection pointers

The publishers agree that behavior outlasts fingerprints. ARTEX's developers logged hundreds of commits in ten weeks,[^elastic] its forks rename the console,[^censys] and the FBI warns that the IP addresses in its indicators may since have been reassigned to benign services.[^fbi] The pointers below are ordered from the most durable to the most perishable.

**Behavior** that holds across versions:

- bursts of many actions in seconds from one identity or source, followed by pauses (GTIG)
- several client families, such as curl, Python, a browser and the AWS CLI, from one source address within a minute; Elastic publishes an ES|QL hunt that keys on three or more families and a request burst (Elastic)
- new accounts registered on an application and then used from several sessions, because the agents create and share their own test accounts (Elastic)
- outbound calls to LLM APIs or unfamiliar LLM resellers from servers with no need for them (FBI, Censys)
- an application account sending mass internal notifications (GTIG)

**Artifacts** that a new release can rename. Each publisher lists the exact strings:

- file and account names built from the framework's name, with incrementing numeric markers ([GTIG, ARTEX Framework section](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks))
- framework-specific User-Agents, including the one ARTEX's web-fetch tool sends ([Elastic, What to watch for](https://www.elastic.co/security-labs/threat-command/agentic-offensive-frameworks-detection-artex))
- framework-name markers in command lines, cloud role names and a working directory ([CrowdStrike, Recommendations](https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/))

**Vendor coverage** documented in the primaries:

- Google SecOps rules in the Mandiant Frontline Threats and Hunting packs, including Suspicious Webhook.Site Interaction (GTIG)
- three Snort 3 and three Snort 2 rules for ARTEX; the two that detect the Norma harness are policy rules, disabled by default ([Talos, Coverage](https://blog.talosintelligence.com/artex-and-beyond/))
- Microsoft Entra ID Protection's Verified threat actor IP risk detection, raised at high risk for sign-ins from known ARTEX infrastructure (Microsoft)
- the Censys console query, which matches a page title beginning "ARTEX" together with Next.js static assets, and the ARTEX and ScopeWeaver threat labels (Censys)

Microsoft also recommends deception against autonomous agents: honeypots and decoys give the agent worthless targets, tarpits consume its time and budget, and fake wins can make it stop on a false success. Microsoft reports that in its own testing these controls reduced the attack traffic reaching real assets.[^ms3358] Class-level detection and response guidance is on [[agentic-offensive-framework|Agentic Offensive Framework]]. The ConnectWise-specific response steps are on [[storm-0940-artex-connectwise|Storm-0940 ARTEX Supply-Chain Campaign]].

### Source disagreements

The primaries describe one tool and still differ on several points of fact. [[agentic-offensive-wave-open-questions|Agentic Offensive Wave Open Questions]] tracks the claims that rest on unread or secondary sources.

- **C2 role.** GTIG calls ARTEX an "offensive command-and-control (C2) platform".[^gtig] Talos states it "is not natively bundled with any traditional command-and-control (C2) framework" and that any compatible tool server can be added through its MCP layer.[^talos]
- **Agent structure.** GTIG counts six specialised sub-agents.[^gtig] Talos and Censys describe a planner with workers,[^talos][^censys] and CrowdStrike adds a main agent above both.[^cs] None of the four reconciles its description with the others.
- **Interface language.** Microsoft states the interface defaults to English, with Korean, Mandarin, Spanish and Japanese available.[^ms0940] Censys finds the Chinese default dominant on exposed consoles.[^censys]
- **Removal date.** The FBI dates the removal to 8 October 2026, the day Censys gives for the author's closed-source statement; GTIG gives October 2026.[^fbi][^censys][^gtig]
- **Exposure.** The counts of 1,465 consoles,[^censys] more than 1,600 consoles[^fbi] and more than 2,500 C2 addresses[^talos] come from different scanners, dates and units, so none of them corrects another.

## Notes

[^gtig]: [Google Threat Intelligence Group — ARTEX and Friends: Enter Agentic Offensive Frameworks](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks), 2026-10-30. Architecture, forks, adoption since late August 2026, UNC7228 overlap, burst timings (2–25 s; 27 events in 23 s), the 240-event SQL sequence and the 1,300-recipient notification abuse, all from Mandiant incident-response cases. Summarized at [[gtig-artex-and-friends|GTIG ARTEX and Friends Report]].
[^fbi]: [FBI — FLASH-20261113-001: Threat Overview of ARTEX Autonomous Penetration Testing Framework](https://www.ic3.gov/CSA/2026/261113.pdf), TLP:CLEAR, 2026-11-13. Models and their prevalence in attacker-controlled instances; removal on 2026-10-08; about 67,000 individuals in the South Korean finance campaign (FBI notes 6–8: CrowdStrike, Zscaler, CERT-EU); more than 1,600 exposed consoles in early November (FBI note 9: Check Point Research); IOC reassignment caveat. Summarized at [[fbi-artex-flash-2026-11|FBI ARTEX FLASH Advisory]].
[^ms0940]: [Microsoft Threat Intelligence — AI-driven, large-scale nation-state campaign using ARTEX enables supply chain compromise](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/), 2026-10-21. Storm-0940 attribution, attempted sign-in to more than 2,250 tenants, privileged access in under three hours, interface languages. Summarized at [[microsoft-storm-0940-artex|Microsoft Storm-0940 ARTEX Report]].
[^ms3358]: [Microsoft Threat Intelligence — ARTEX abuse expands as cybercriminals target financial sector](https://www.microsoft.com/en-us/security/blog/2026/11/06/artex-abuse-expands-as-cybercriminals-target-financial-sector/), 2026-11-06. Storm-3358, ARTEX default tools, deception guidance and Entra ID Protection coverage.
[^cs]: [CrowdStrike — VAPOR PANDA Uses AI-Driven ARTEX to Grow Global Espionage Campaign](https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/), 2026-10-29. More than 200 organizations targeted and nine with confirmed post-exploitation; DeepSeek plus an unconfirmed second LLM; about 1.4 billion input tokens consumed by planning agents in one intrusion; ARTEX hunting markers. Summarized at [[crowdstrike-vapor-panda-artex|CrowdStrike VAPOR PANDA ARTEX Report]].
[^talos]: [Cisco Talos — ARTEX and beyond: AI-enabled penetration testing platforms](https://blog.talosintelligence.com/artex-and-beyond/), 2026-11-13. Planner-and-worker design, absence of bundled C2, forks, more than 2,500 ARTEX-associated C2 IP addresses identified by Talos, Snort coverage. Summarized at [[talos-artex-and-beyond|Talos ARTEX and Beyond Report]].
[^censys]: [Censys — ARTEX: Agentic Asbestos](https://censys.com/blog/artex-agentic-asbestos), Silas Cutler, 2026-11-05. Release on 2026-07-26 under AGPL-3.0; star estimate; architecture; 1,465 exposed console hosts by Censys's title-and-assets query, dated 2026-11-04 in the summary and 2026-11-03 in the query section, about 815 after removing two templated hosting blocks; 155 hosts (10.6%) with another threat detection; 43 hosts with LLM gateway software; 32 open directories. Local copy: `.raw/articles/censys-artex-agentic-asbestos-2026-11-05.md`.
[^elastic]: [Elastic Security Labs — Inside the loop: Five AI attack frameworks that never ask permission](https://www.elastic.co/security-labs/threat-command/agentic-offensive-frameworks-detection-artex), 2026-11-13. Source review of a public English-translated ARTEX copy: bypass permission mode, prompts, default blocklists, about 390 commits from 2026-07-26 to 2026-10-07, User-Agents and the ES|QL hunt. Local copy: `.raw/articles/elastic-agentic-offensive-frameworks-detection-artex-2026-11-13.md`.
