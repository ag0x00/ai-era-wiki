---
type: paper
title: "FBI ARTEX FLASH Advisory"
created: 2026-11-18
updated: 2026-11-18
tags:
  - papers
  - threat-intel
  - government-advisory
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
  - "Federal Bureau of Investigation"
venue: "FBI FLASH-20261113-001, TLP:CLEAR, 2026-11-13"
source_url: "https://www.ic3.gov/CSA/2026/261113.pdf"
archived_copy: ".raw/reports/fbi-flash-20261113-001-artex-2026-11-13.md"
no_public_url: ""
key_claim: "ARTEX is an open-source LLM-driven agentic platform that a variety of malicious actors have used since at least August 2026 to automate intrusions from reconnaissance through exfiltration, across ten named sectors, with some intrusions reaching privileged access in under three hours."
methodology: "Government consolidation of vendor reporting: the case-level claims in the summary and threat sections carry footnotes to eight publishers: seven private-sector vendors (Akamai, Microsoft, GTIG, Censys, CrowdStrike, Zscaler, Check Point) and CERT-EU. The IOC table and recommendations carry vendor footnotes; the ATT&CK table carries none, and most of its rows match GTIG, CrowdStrike and Microsoft reporting. No observation in the text is attributed to FBI investigation."
contradicts: []
supports:
  - "[[gtig-artex-and-friends]]"
  - "[[microsoft-storm-0940-artex]]"
related:
  - "[[artex]]"
  - "[[agentic-offensive-wave-precursors-2026-11]]"
  - "[[agentic-offensive-wave-open-questions]]"
  - "[[agentic-offensive-framework]]"
  - "[[storm-0940-artex-connectwise]]"
  - "[[gtig-artex-and-friends]]"
  - "[[microsoft-storm-0940-artex]]"
  - "[[crowdstrike-vapor-panda-artex]]"
  - "[[talos-artex-and-beyond]]"
  - "[[capability-floor-collapse]]"
  - "[[offensive-ai-state-of-the-field]]"
sources:
  - "https://www.ic3.gov/CSA/2026/261113.pdf"
  - ".raw/reports/fbi-flash-20261113-001-artex-2026-11-13.md"
  - ".raw/reports/fbi-flash-20261113-001-artex-2026-11-13.pdf"
  - "https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/"
  - "https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks"
  - "https://censys.com/blog/artex-agentic-asbestos"
verified: 2026-11-18
verified_against:
  - ".raw/reports/fbi-flash-20261113-001-artex-2026-11-13.md"
verified_findings: 0
verified_note: "3 medium (ATT&CK table attribution, model-provider uniqueness, CERT-EU as private sector) and 4 low fixed; PDF page count checked; conflicts cross-checked against GTIG, Microsoft, CrowdStrike, Talos and Censys raws"
---

# FBI ARTEX FLASH Advisory

**Source:** [FBI — Threat Overview of ARTEX Autonomous Penetration Testing Framework, FLASH-20261113-001](https://www.ic3.gov/CSA/2026/261113.pdf) (2026-11-13, TLP:CLEAR, 13 pages). Local copy: `.raw/reports/fbi-flash-20261113-001-artex-2026-11-13.pdf`, with extracted text beside it.

The Federal Bureau of Investigation's FLASH advisory on [[artex|ARTEX]] gathers the vendor reporting on the framework into one shareable government product: a threat summary, an ATT&CK-mapped table of observed activity, indicators of compromise, a list of the information the FBI wants reported, and recommendations. It is the only government-issued document among the five vendor and government reports on the [[agentic-offensive-framework|agentic offensive framework]] wave, and the only one of the five that reports which model families dominate across attacker-controlled ARTEX instances.

### Key claim

The FLASH defines ARTEX as "an open-source, large language model (LLM)-driven agentic platform that threat actors have weaponized to automate multiple phases of cyber intrusions at machine speed, from reconnaissance through data exfiltration." It states that "since at least August 2026, a variety of malicious cyber actors have used ARTEX" against financial services, government, technology, defense, semiconductor, energy, healthcare, manufacturing, telecommunications and legal organizations. Some intrusions progressed "from initial targeting to privileged access in under three hours," a figure the FLASH footnotes to Microsoft.

### Evidence base

The FLASH is a consolidation. Its eleven notes cite eight publishers: seven private-sector vendors, namely Akamai, Microsoft (two reports), GTIG (two notes), Censys, CrowdStrike, Zscaler and Check Point, and the EU institutions' CERT-EU. Every case-level statement in the summary and threat sections carries one of those footnotes; the unfootnoted sentences are general risk statements, such as the claim that ARTEX lets actors with minimal expertise run multi-target operations. The ATT&CK table carries no footnotes. Most of its rows match activity in the [[gtig-artex-and-friends|GTIG ARTEX and Friends Report]], the [[crowdstrike-vapor-panda-artex|CrowdStrike VAPOR PANDA ARTEX Report]] and the Storm-3358 report. The IOC table names a vendor source for each indicator. In the text read, no observation is attributed to FBI investigation. The FLASH's weight therefore comes from the government endorsement and the consolidation, and its findings carry the evidence quality of the reports it cites.

### Contributions beyond the vendor reports

- **Model providers.** ARTEX "does not include its own SI model but integrates with external LLM providers, including frontier models (Anthropic Claude, OpenAI GPT) and open-source or Chinese-origin models (DeepSeek, GLM, Grok, Kimi)." From attacker-controlled instances, "Chinese models are most prevalent, with DeepSeek Flash family models dominating usage due to their balance of capabilities, relatively low cost, and lack of observable guardrails when framed for penetration testing." The provider list cites GTIG, CrowdStrike, Zscaler and Check Point, and the prevalence finding cites Zscaler and Check Point. The list groups Grok, an xAI model, with open-source or Chinese-origin models and does not explain the grouping.
- **Sector breadth.** The ten-sector list is the widest of the five reports, assembled from the reports of four publishers.
- **Risk statements.** The FLASH names five risks. Two of them restate vendor findings for a non-specialist reader: "reduced barrier to entry" and a "compressed detection window" in which noisy traffic still leaves "limited time for coordinated defender response before objective completion." It also states that ARTEX "excels at identifying and exploiting peripheral systems," such as partner portals, mobile backends and legacy services, citing Akamai.
- **Information requested.** Eight categories tell a victim what to send the FBI, including ARTEX console screenshots and configuration files, service-principal sign-in logs, outbound connections to LLM API endpoints, evidence of self-hosted LLM gateway or reseller software, and "time from detection to containment."
- **Network indicators for model egress.** The FLASH lists outbound calls to four model-provider API domains as indicators, with the qualifier "investigate unexpected calls alongside other suspicious activity."
- **Terminology.** The FLASH calls ARTEX's agents "specialized super intelligence (SI) agents." The GTIG, Microsoft, CrowdStrike and Talos reports do not use the term.

### Repository history and availability

The FLASH says the developer Autumn-27 released ARTEX on GitHub in July 2026, stated that it was "intended for authorized security testing within defined scopes," and made the repository unavailable on October 8, 2026. ARTEX reached an estimated 1,500 to 2,000 GitHub stars within ten weeks, per Censys. Forks, localized variants, archived copies and the commercial ARTEX PRO keep it available. The FLASH reports "over 1,600 publicly exposed ARTEX console instances" in early November 2026 and footnotes that figure to Check Point.

### Recommendations

The recommendations fall in four groups, each footnoted to the vendor that wrote it.

- **Reduce external attack surface.** Inventory peripheral internet-facing applications, restrict administrative interfaces to trusted paths, and move enterprise applications behind Zero Trust access where feasible. These come from Mandiant, Akamai and Zscaler.
- **Harden identity and access.** Apply least privilege to multi-tenant application permissions and consent grants, and review every tenant where affected ConnectWise applications were ever authorized, searching by application ID. These come from Microsoft, except one certificate-enrollment item from Mandiant.
- **Strengthen application protections.** Rate-limit high-impact APIs, alert on sequential record lookups on partner and employee portals, and stop returning detailed errors to unauthenticated users.
- **Preserve evidence and respond.** Keep affected ConnectWise application connections disabled, and "do not delete enterprise applications as first response."

The FLASH asks recipients to "investigate or vet IOCs prior to taking action, such as blocking," because cloud-hosted addresses are reassigned, and to read the indicators as "historically observed infrastructure within the activity window."

### Attribution and confidence wording

The FLASH attributes ARTEX use to "a variety of malicious cyber actors" and names no actor in its body. Its two named campaigns carry no actor name: "financially motivated attacks against South Korean financial institutions" with personal data of about 67,000 individuals exfiltrated, and a ConnectWise Cloud Backup supply-chain compromise that "resulted in attempted sign-ins to more than 2,250 downstream tenants." The FLASH does not repeat Microsoft's attribution of the ConnectWise compromise to Storm-0940. Storm-3358 appears only in the context column of the IOC table, sourced to Microsoft. The FLASH states no confidence level. Its one estimative word is "likely," in the statement that total deployments are "likely significantly higher" than the exposed-console count.

### Limits of the advisory

- **No independent evidence.** In the text read, every case-level finding is footnoted to a vendor, so the FLASH cannot corroborate those vendors.
- **Outcome of the ConnectWise campaign.** The FLASH reports attempted sign-ins only and gives no compromised-tenant count, as Microsoft does not.
- **ARTEX only.** The FLASH refers to "ARTEX and similar agentic SI frameworks" without naming PentAGI, Hexstrike, CAIRN or any other framework.
- **Exposure counts.** The 1,600-console figure has a single source and no method in the FLASH.

### Conflicts with the other ARTEX reports

- **Console count.** The FLASH's figure of more than 1,600 comes from Check Point. Censys, which the FLASH cites for the star count, observed 1,465 hosts running an exposed ARTEX console on 2026-11-04, about 815 once two template-provisioned hosting blocks are excluded.[^censys] The FLASH cites Censys for star growth and Check Point for exposure without reconciling the two.
- **Review start date.** The FLASH tells ConnectWise customers to "review activity starting from at least October 1, 2026." Microsoft's guidance starts review at October 15 and extends service-principal sign-in review to October 1 "where records are available."[^ms] The FLASH turns Microsoft's extension into the floor, which is the more conservative instruction.
- **Removal date.** The FLASH gives October 8, 2026. GTIG says only "October 2026," and Talos does not mention the removal.[^gtig]
- **Models.** GTIG names no model. CrowdStrike names DeepSeek for VAPOR PANDA, consistent with the FLASH's DeepSeek finding.
- **WHOIS mapping.** The ATT&CK table lists "WHOIS asset mapping and active vulnerability scanning" as ARTEX activity. GTIG reports that activity for PentAGI.[^gtig]
- **Start date.** "Since at least August 2026" matches GTIG's late-August observation across actors. For Storm-0940 alone, Microsoft gives September 2026.[^ms]

### Relations

- **Bears on [[capability-floor-collapse|Capability Floor Collapse]].** The FLASH states that ARTEX "enables threat actors with minimal technical expertise to conduct complex, multi-target operations simultaneously." The summary sentence carries no footnote, the matching "reduced barrier to entry" risk cites GTIG and Check Point, and the FLASH gives no actor-level skill assessment, so the concept's empty cell for low-skill operators who set only the objective remains unsourced at case level.
- The FLASH's ConnectWise figures and recommendations derive from the [[microsoft-storm-0940-artex|Microsoft Storm-0940 ARTEX Report]]; the campaign itself is the [[storm-0940-artex-connectwise|Storm-0940 ARTEX Supply-Chain Campaign]].
- The [[talos-artex-and-beyond|Talos ARTEX and Beyond Report]] appeared the same day and is not among the FLASH's sources.
- [[agentic-offensive-wave-open-questions|Agentic Offensive Wave Open Questions]] tracks the FLASH's dependence on vendor sourcing, and [[agentic-offensive-wave-precursors-2026-11|Agentic Offensive Wave Precursors]] uses its August start date to date the adoption.
- [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]] carries the autonomy and operator-position argument that the FLASH's "minimal human interaction beyond initial objective setting" bears on.

### Notes

[^censys]: [Censys — ARTEX: Agentic Asbestos](https://censys.com/blog/artex-agentic-asbestos), 2026-11-05. Hosts presenting an ARTEX console page title on the public internet as of 2026-11-04; the 815 figure excludes about 650 hosts in two identically configured hosting blocks.
[^ms]: [Microsoft Threat Intelligence — AI-driven, large-scale nation-state campaign using ARTEX enables supply chain compromise](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/), 2026-10-21. Review window in the immediate actions, and the Storm-0940 start date.
[^gtig]: [Google Threat Intelligence Group — ARTEX and Friends: Enter Agentic Offensive Frameworks](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks), 2026-10-30. Repository takedown "in October 2026," and PentAGI's WHOIS asset mapping and vulnerability scanning.
