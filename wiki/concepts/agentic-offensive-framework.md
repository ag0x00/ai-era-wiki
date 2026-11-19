---
type: concept
title: "Agentic Offensive Framework"
created: 2026-11-18
updated: 2026-11-18
tags:
  - concepts
  - offensive-ai
  - agentic-offensive-framework
  - multi-agent
  - threat-tooling
status: developing
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: produced
complexity: intermediate
domain: "offensive-ai"
coined_by:
  - "[[google]]"
  - "[[elastic]]"
aliases:
  - "agentic offensive frameworks"
  - "autonomous offensive framework"
  - "AI-enabled penetration testing framework"
  - "agentic AI penetration testing framework"
related:
  - "[[artex]]"
  - "[[storm-0940-artex-connectwise]]"
  - "[[gtig-artex-and-friends]]"
  - "[[fbi-artex-flash-2026-11]]"
  - "[[microsoft-storm-0940-artex]]"
  - "[[crowdstrike-vapor-panda-artex]]"
  - "[[talos-artex-and-beyond]]"
  - "[[agentic-offensive-wave-precursors-2026-11]]"
  - "[[agentic-offensive-wave-open-questions]]"
  - "[[autonomous-exploit-generation]]"
  - "[[offensive-agent-collective]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[gtg-1002-ai-orchestrated-espionage]]"
  - "[[capability-floor-collapse]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[zero-day-clock]]"
  - "[[mcp-security]]"
  - "[[canary-tokens-for-llms]]"
  - "[[non-human-identity]]"
sources:
  - "https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks"
  - "https://www.elastic.co/security-labs/threat-command/agentic-offensive-frameworks-detection-artex"
  - "https://blog.talosintelligence.com/artex-and-beyond/"
  - "https://www.ic3.gov/CSA/2026/261113.pdf"
  - "https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/"
  - "https://www.microsoft.com/en-us/security/blog/2026/11/06/artex-abuse-expands-as-cybercriminals-target-financial-sector/"
  - "https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/"
  - "https://censys.com/blog/artex-agentic-asbestos"
  - "https://blog.checkpoint.com/executive-insights/hexstrike-ai-when-llms-meet-zero-day-exploitation/"
  - "[[.raw/articles/elastic-agentic-offensive-frameworks-detection-artex-2026-11-13.md]]"
  - "[[.raw/articles/censys-artex-agentic-asbestos-2026-11-05.md]]"
  - "[[.raw/articles/checkpoint-hexstrike-ai-zero-day-exploitation-2025-09-02.md]]"
  - "[[.raw/articles/gtig-artex-and-friends-2026-10-30.md]]"
  - "[[.raw/reports/fbi-flash-20261113-001-artex-2026-11-13.md]]"
  - "[[.raw/articles/microsoft-storm-0940-artex-2026-10-21.md]]"
  - "[[.raw/articles/microsoft-storm-3358-artex-2026-11-06.md]]"
  - "[[.raw/articles/crowdstrike-vapor-panda-artex-2026-10-29.md]]"
  - "[[.raw/articles/talos-artex-and-beyond-2026-11-13.md]]"
verified: 2026-11-18
verified_against:
  - ".raw/articles/censys-artex-agentic-asbestos-2026-11-05.md"
  - ".raw/articles/checkpoint-hexstrike-ai-zero-day-exploitation-2025-09-02.md"
  - ".raw/articles/crowdstrike-vapor-panda-artex-2026-10-29.md"
  - ".raw/articles/elastic-agentic-offensive-frameworks-detection-artex-2026-11-13.md"
  - ".raw/articles/gtig-artex-and-friends-2026-10-30.md"
  - ".raw/articles/microsoft-storm-0940-artex-2026-10-21.md"
  - ".raw/articles/microsoft-storm-3358-artex-2026-11-06.md"
  - ".raw/articles/talos-artex-and-beyond-2026-11-13.md"
  - ".raw/reports/fbi-flash-20261113-001-artex-2026-11-13.md"
verified_findings: 0
verified_note: "3 low fixed (dropped hedge, table scope of dual-use claim, CAE wording); thesis backlink added"
---

# Agentic Offensive Framework

An agentic offensive framework is a packaged, reusable tool in which large language model (LLM) agents plan and run an intrusion against an objective the operator sets, driving the scanners, shells and cloud clients that intruders already use. Google Threat Intelligence Group (GTIG) named the class in its October 2026 report *ARTEX and Friends: Enter Agentic Offensive Frameworks*, after Mandiant incident-response engagements found threat actors operating ARTEX, PentAGI, CAIRN and Hexstrike since late August 2026.[^gtig] Elastic's definition states the mechanism: such a framework "puts a large language model (LLM) in charge of the scanners, curl, shells, and cloud CLIs that red teams and threat actors already use".[^elastic]

**The class changes the speed and scale of an intrusion, and leaves its techniques as they were.** GTIG finds the mechanics unchanged, compromised credentials and unpatched systems among them, and states that LLM-enabled offensive tooling "reduces the time required to combine them".[^gtig]

## Definition

### Defining properties

Four properties recur across the GTIG, Elastic, Talos, FBI, Microsoft, CrowdStrike, Censys and Check Point publications. A tool that has all four belongs to the class, whichever name its publisher uses.

1. **Planner and worker agents.** A planning step turns the objective into units of work, worker agents carry them out, and the framework records results in shared state. In ARTEX a planner agent produces the tasks, workers run security tools, and a shared graph holds assets, findings and progress.[^talos] PentAGI uses a team of specialised agents that break each task, which it calls a Flow, into sub-tasks.[^gtig] HexStrike is the limiting case: it has no model of its own and exposes about 150 tool functions through the Model Context Protocol (MCP), so the planning runs in whatever assistant the operator connects.[^elastic]
2. **External models.** The framework ships no model and calls a hosted or self-run LLM through an API. ARTEX integrates with frontier, open-source and Chinese-origin models,[^fbi] and PentAGI supports over a dozen cloud and local backends.[^gtig] Because the operator chooses the model, a provider's refusal is one obstacle among many: Cairn's dispatcher reassigns a refused task to another configured worker.[^elastic]
3. **Operator-run server.** The agents and most of the tooling run on infrastructure the attacker controls. What reaches the victim is the effect of the tools, recorded as processes, web requests, API calls and sign-ins.[^elastic] ARTEX bundles no C2 framework,[^talos] so the webshells and remote access trojans found on victim hosts are tools the agents or operator chose, such as the WISP, vshell and AdaptixC2 trojans Storm-3358 installed.[^ms3358] CyberStrikeAI is the exception among the five frameworks Elastic reviewed, because it builds its own implants and runs C2 listeners.[^elastic]
4. **Existing tooling, autonomous by default.** Workers drive standard offensive tools. PentAGI runs metasploit, nmap and sqlmap in Kali Linux sandboxes,[^gtig] and Talos has seen frameworks combine nmap and nuclei with internet-mapping, CAPTCHA-bypass, credential-leak, residential-proxy and hash-cracking services.[^talos] None of the five frameworks Elastic reviewed enforces authorization in code by default, three tell the model in its prompt that the work is authorized, and four run without waiting for human approval.[^elastic]

### Members named by publishers

The membership differs by publisher. GTIG alone names AgentZero and ZeroHollow, and Talos alone names Hephaestus. Elastic adds Strix, which Gambit Security reported in a criminal campaign alongside Cairn, and lists further open-source penetration testing agents such as Shannon, PentestGPT and CAI.[^elastic]

| Framework | Origin, as stated | Named by | Reported use by threat actors |
|---|---|---|---|
| [[artex\|ARTEX]] | Autumn-27, GitHub, July 2026 | GTIG, Microsoft, CrowdStrike, Talos, FBI | State-aligned and criminal intrusions |
| ScopeWeaver | Localized ARTEX fork | GTIG, Talos | Co-observed on ARTEX infrastructure (Talos) |
| ARTEX PRO | Commercial ARTEX version | GTIG, FBI | Named as a distribution route (FBI) |
| AntiFox | "Delivery-enhanced" ARTEX version | Talos | Co-observed on ARTEX infrastructure (Talos) |
| PentAGI | VXControl; open-source and commercial editions | GTIG, Talos, Elastic | Mostly asset mapping and scanning, some intrusions with persistence (GTIG) |
| Hexstrike (HexStrike AI) | Public GitHub project, 2025; presented as defender-oriented | Check Point, GTIG, Talos, Elastic | Incident-response engagements (GTIG) |
| CAIRN | Open source; debuted in an AI penetration testing competition | GTIG, Elastic | Incident-response engagements (GTIG); a criminal campaign reported by Gambit Security (Elastic) |
| CyberStrikeAI | Operator console with its own implants and C2 | Talos, Elastic | An AWS intrusion traced to a CyberStrikeAI server (Elastic) |
| Hephaestus | Command-line tool controlled through Telegram | Talos | Full attack-chain automation (Talos) |
| AgentZero | Open-source agentic framework | GTIG | Experimentation only (GTIG) |
| ZeroHollow | Inference service | GTIG | Experimentation only (GTIG) |

Exposure measurements indicate how widely the class is deployed, by testers and attackers alike. Talos found more than 700 PentAGI instances reachable on the internet,[^talos] and Censys found CyberStrikeAI on 70 of the hosts that also expose an ARTEX console.[^censys] The five frameworks Elastic reviewed have drawn more than 60,000 GitHub stars between them.[^elastic]

### Dual-use origin

Every framework in the table except the AgentZero agent framework and the ZeroHollow inference service began as, or presents itself as, a penetration testing tool. GTIG places the class in a longer pattern, in which espionage and financially motivated actors took red-team platforms such as Cobalt Strike and Metasploit, and now take LLM-driven ones.[^gtig] ARTEX's author stated it was intended for authorized testing within defined scopes.[^fbi] Check Point reported in 2025 that HexStrike-AI, positioned for red teams and researchers, was discussed in underground channels for use against Citrix NetScaler zero-day vulnerabilities within hours of its release.[^checkpoint]

The pentest origin explains three properties a defender meets. The code is public and forkable, so the author's removal of the ARTEX repository on 8 October 2026 left forks, archived copies and a commercial edition in circulation.[^fbi] The prompts tell the model the work is authorized,[^elastic] and the FBI attributes the dominance of DeepSeek Flash models on attacker-controlled ARTEX instances partly to their "lack of observable guardrails when framed for penetration testing".[^fbi] The traffic resembles the tools the framework drives, because the scanners run with stock settings.[^elastic] The same origin lowers the skill an operator needs: GTIG notes that PentAGI asks only for a target and a goal, "which enables entry-level users to run complex cyber security campaigns",[^gtig] and the FBI states that ARTEX lets actors "with minimal technical expertise" run complex operations.[^fbi] [[capability-floor-collapse|Capability Floor Collapse]] carries that argument.

### Adoption trajectory

GTIG places current use at the fifth of seven stages, in which LLM-driven tooling conducts several stages of the attack lifecycle in the wild with fewer humans in the loop, and in a style it calls "messy, noisy but yet successful".[^gtig] It expects the sixth stage, highly autonomous but unsophisticated operations with no human in the loop, in the near term of one to three months, and stealthy fully autonomous operations in three to six months.[^gtig] GTIG assesses with high confidence that cybercrime actors will keep incorporating these tools, because wider reach will likely compensate for lower success rates, and expects actors who prize long-term stealthy access to be less likely to use the public versions.[^gtig] CrowdStrike assesses that China-nexus adversaries "will highly likely continue adopting agentic frameworks such as ARTEX over the next 12 months".[^cs] [[agentic-offensive-wave-precursors-2026-11|Agentic Offensive Wave Precursors]] reads the first months of that adoption as a change in speed, scale and actor mix, and [[agentic-offensive-wave-open-questions|Agentic Offensive Wave Open Questions]] tracks the claims it could not settle.

## Boundaries

### Relation to autonomous exploit generation

[[autonomous-exploit-generation|Autonomous Exploit Generation]] is the automated construction of a working, validated exploit from a vulnerability description, measured as a success rate against a defined set of vulnerabilities. An agentic offensive framework automates the sequencing of known techniques against a live target, measured by how many targets it reaches and how fast. The questions differ: an AEG system asks whether a given flaw can be exploited, and a framework asks how quickly an operator holding a credential or facing an exposed weakness can reach an objective.

The two overlap where a framework's workers write code for the target. GTIG observed ARTEX writing several webshells in succession, correcting errors until one executed cleanly,[^gtig] and Check Point relayed threat-actor claims that HexStrike-AI cut the time to exploit the NetScaler flaws to under ten minutes.[^checkpoint] The difference sets the control a defender reaches for. AEG shortens the window between disclosure and exploit, which [[zero-day-clock|Zero Day Clock]] tracks. A framework uses the credentials, configurations and administrative interfaces that already exist, which is what GTIG's five defensive layers address.

### Relation to the offensive agent collective

An [[offensive-agent-collective|Offensive Agent Collective]] is a set of concurrently running agents that share findings over a common channel, so that one agent's capability becomes every agent's. A framework has a designed channel of that kind inside one task: ARTEX coordinates a main agent, planner and workers "over a shared graph of facts, findings, and goals".[^cs] State that crosses tasks moves a framework toward the collective. Talos reports that ARTEX agents record results for subsequent tasks,[^talos] and PentAGI keeps the code it writes in a store shared across engagements.[^elastic]

The two concepts describe different things. The collective concept describes how agents share capability, whether that sharing emerged unplanned or was built on purpose. The framework concept describes a packaged product that reaches many unrelated operators through public repositories, forks and commercial editions. GTIG separates the open-source platforms it names from "other potentially custom harnesses".[^gtig] The framework recovered in the [[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]], built on Hermes and OpenClaw, is a custom harness of that kind and fits the collective definition. The operator-built orchestration over Claude Code in [[gtg-1002-ai-orchestrated-espionage|GTG-1002]] is a custom harness in the same sense. [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]] reads the packaged framework as the point where a multi-agent harness of that kind became commodity tooling, so the harness no longer separates state-grade operations from criminal use.

## Defender consequences

### Tempo and detection

The framework's tempo moves the decisions that matter to before the incident. GTIG measured ARTEX execution bursts of roughly 2 to 25 seconds,[^gtig] Microsoft observed privileged cloud access in under three hours in multiple Storm-0940 intrusions, the actor behind the [[storm-0940-artex-connectwise|Storm-0940 ARTEX Supply-Chain Campaign]],[^ms0940] and Elastic concludes that the detections trusted to start containment automatically must be chosen in advance.[^elastic] GTIG adds that detection and disruption are separate functions: "simply detecting the attack in time does little to hinder objective completion".[^gtig]

Behavioral detection outlasts indicator matching. ARTEX's developers logged about 390 commits in ten and a half weeks, and several frameworks write or install new tools during a task, so the strings and defaults that identify a framework change between releases.[^elastic] The FBI warns that addresses in its indicators may since have been reassigned to benign services,[^fbi] and Talos advises treating tool signatures as leads rather than the sole detection.[^talos] Two signals work across frameworks: the tempo of sensitive actions per identity in short windows, and the sequence of discovery, secret access and remote execution joined across sessions under one identity or source.[^elastic] Several sessions or agent names can come from one controller, so the identity and controller behind them is the unit to correlate.[^elastic]

Containment must account for the framework's speed. ARTEX and CyberStrikeAI change route when blocked, so a block on a source address needs to be paired with revoking the credentials the agents hold.[^elastic] Revocation is not instant either: in Microsoft Entra ID an access token issued before an identity is disabled generally stays valid until it expires, typically 60 to 90 minutes, unless the resource and client support continuous access evaluation.[^elastic] The responder confirms that the next request fails.

### Controls

GTIG's Mandiant guidance organizes defense into five layers, chosen to limit what one compromised identity or workload gives an agent.[^gtig]

- **Protect control planes.** Restrict administrative APIs, consoles and CI/CD systems to trusted networks.
- **Restrict identities and keys.** Replace long-lived secrets with short-lived, tightly scoped workload credentials, and treat service principals and API keys with the rigor applied to privileged humans; see [[non-human-identity|Non-Human Identity (NHI)]].
- **Harden configurations and reduce attack paths.** Scan continuously, segment application tiers, and keep stack traces and SQL errors away from unauthenticated users, because agents use error output to adjust their next request within seconds.
- **Protect applications.** Put rate limits, recipient limits and transaction thresholds on high-impact business functions such as messaging and account creation.
- **Detect behavioral and volumetric change.** Baseline identity velocity, API bursts and data-plane volume, and watch server egress to webhook services, tunnels and external AI platforms.

Three further controls target properties specific to the class. Microsoft reports that in its testing honeypots, tarpits and fake wins reduced the attack traffic reaching real assets. A decoy gives the agent an attractive but worthless target, a tarpit consumes its time and budget, and a fake win can make it stop on a false success.[^ms3358] [[canary-tokens-for-llms|Canary Tokens for LLMs]] covers a related decoy pattern. Framework consoles can be enumerated from the internet, so threat intelligence on that infrastructure gives an early blocking opportunity.[^censys][^elastic] Unfamiliar LLM proxies and resellers in egress logs are worth investigating, because operators buy model access through them.[^censys]

## Notes

[^gtig]: [Google Threat Intelligence Group — ARTEX and Friends: Enter Agentic Offensive Frameworks](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks), 2026-10-30. Named frameworks and their use since late August 2026; PentAGI's design; the burst timing of 2 to 25 seconds; the seven-stage evolution model; the high-confidence cybercrime assessment; the five defensive layers. Summarized at [[gtig-artex-and-friends|GTIG ARTEX and Friends Report]].
[^elastic]: [Elastic Security Labs — Inside the loop: Five AI attack frameworks that never ask permission](https://www.elastic.co/security-labs/threat-command/agentic-offensive-frameworks-detection-artex), 2026-11-13. Source review of ARTEX, CyberStrikeAI, Cairn, PentAGI and HexStrike: design choices, default safeguards, more than 60,000 combined GitHub stars, about 390 ARTEX commits between 2026-07-26 and 2026-10-07, the 60-to-90-minute Entra token validity window, detection guidance. Local copy: `.raw/articles/elastic-agentic-offensive-frameworks-detection-artex-2026-11-13.md`.
[^talos]: [Cisco Talos — ARTEX and beyond: AI-enabled penetration testing platforms](https://blog.talosintelligence.com/artex-and-beyond/), 2026-11-13. ARTEX's planner-and-worker design and lack of bundled C2; Hephaestus, HexStrike AI and the tools co-observed on ARTEX infrastructure; more than 700 reachable PentAGI instances found through a Censys search. Summarized at [[talos-artex-and-beyond|Talos ARTEX and Beyond Report]].
[^fbi]: [FBI — FLASH-20261113-001: Threat Overview of ARTEX Autonomous Penetration Testing Framework](https://www.ic3.gov/CSA/2026/261113.pdf), TLP:CLEAR, 2026-11-13. External model integration, the developer's stated intent and the 2026-10-08 removal, the reduced barrier to entry, the IOC reassignment caveat. Summarized at [[fbi-artex-flash-2026-11|FBI ARTEX FLASH Advisory]].
[^ms0940]: [Microsoft Threat Intelligence — AI-driven, large-scale nation-state campaign using ARTEX enables supply chain compromise](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/), 2026-10-21. Time from initial targeting to privileged access under three hours in multiple cases. Summarized at [[microsoft-storm-0940-artex|Microsoft Storm-0940 ARTEX Report]].
[^ms3358]: [Microsoft Threat Intelligence — ARTEX abuse expands as cybercriminals target financial sector](https://www.microsoft.com/en-us/security/blog/2026/11/06/artex-abuse-expands-as-cybercriminals-target-financial-sector/), 2026-11-06. Storm-3358's use of WISP, vshell and AdaptixC2; deception controls tested by Microsoft.
[^cs]: [CrowdStrike — VAPOR PANDA Uses AI-Driven ARTEX to Grow Global Espionage Campaign](https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/), 2026-10-29. ARTEX's main, planner and worker agents over a shared graph; the 12-month adoption assessment. Summarized at [[crowdstrike-vapor-panda-artex|CrowdStrike VAPOR PANDA ARTEX Report]].
[^censys]: [Censys — ARTEX: Agentic Asbestos](https://censys.com/blog/artex-agentic-asbestos), 2026-11-05. CyberStrikeAI co-located with ARTEX on 70 hosts; LLM gateway and reseller software on ARTEX hosts. Local copy: `.raw/articles/censys-artex-agentic-asbestos-2026-11-05.md`.
[^checkpoint]: [Check Point — Hexstrike-AI: LLM Orchestration Driving Real-World Zero-Day Exploits](https://blog.checkpoint.com/executive-insights/hexstrike-ai-when-llms-meet-zero-day-exploitation/), 2025-09-02. HexStrike-AI's MCP orchestration design, its defender-oriented positioning, and underground discussion of its use against Citrix NetScaler vulnerabilities; the under-ten-minutes figure is a threat-actor claim Check Point relays. Local copy: `.raw/articles/checkpoint-hexstrike-ai-zero-day-exploitation-2025-09-02.md`.

<!-- sources:auto -->
## Sources

- [cloud.google.com](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks)
- [elastic.co](https://www.elastic.co/security-labs/threat-command/agentic-offensive-frameworks-detection-artex)
- [blog.talosintelligence.com](https://blog.talosintelligence.com/artex-and-beyond/)
- [ic3.gov](https://www.ic3.gov/CSA/2026/261113.pdf)
- [microsoft.com](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/)
- [microsoft.com](https://www.microsoft.com/en-us/security/blog/2026/11/06/artex-abuse-expands-as-cybercriminals-target-financial-sector/)
- [crowdstrike.com](https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/)
- [censys.com](https://censys.com/blog/artex-agentic-asbestos)
- [blog.checkpoint.com](https://blog.checkpoint.com/executive-insights/hexstrike-ai-when-llms-meet-zero-day-exploitation/)
<!-- /sources -->
