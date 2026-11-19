---
type: paper
title: "Microsoft Storm-0940 ARTEX Report"
created: 2026-11-18
updated: 2026-11-18
tags:
  - papers
  - threat-intel
  - offensive-ai
  - agentic-offensive-framework
  - artex
  - supply-chain
  - oauth
status: summarized
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
year: 2026
authors:
  - "Microsoft Security Research"
  - "Microsoft Threat Intelligence"
venue: "Microsoft Security Blog, 2026-10-21; follow-up 2026-11-06"
source_url:
  - "https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/"
  - "https://www.microsoft.com/en-us/security/blog/2026/11/06/artex-abuse-expands-as-cybercriminals-target-financial-sector/"
archived_copy: ".raw/articles/microsoft-storm-0940-artex-2026-10-21.md"
no_public_url: ""
key_claim: "Microsoft assesses that Storm-0940, a China-state aligned actor, ran the ConnectWise Cloud Backup supply-chain compromise and used ARTEX from as early as September 2026 to accelerate scanning, password spraying and OAuth application abuse, attempting sign-in to more than 2,250 downstream tenants and reaching privileged cloud access in some cases in under three hours."
methodology: "Microsoft first-party telemetry from Entra ID sign-in logs, Microsoft Graph activity, Defender XDR, Defender for Cloud Apps and Defender for Key Vault, linked to the ConnectWise disclosure through shared network infrastructure. The follow-up post on Storm-3358 rests on Microsoft observations without a named telemetry source, plus Microsoft's own deception testing."
contradicts:
  - "[[gtig-artex-and-friends]]"
supports:
  - "[[fbi-artex-flash-2026-11]]"
related:
  - "[[artex]]"
  - "[[agentic-offensive-wave-precursors-2026-11]]"
  - "[[agentic-offensive-framework]]"
  - "[[storm-0940-artex-connectwise]]"
  - "[[gtig-artex-and-friends]]"
  - "[[fbi-artex-flash-2026-11]]"
  - "[[crowdstrike-vapor-panda-artex]]"
  - "[[talos-artex-and-beyond]]"
  - "[[non-human-identity]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[microsoft]]"
sources:
  - "https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/"
  - ".raw/articles/microsoft-storm-0940-artex-2026-10-21.md"
  - "https://www.microsoft.com/en-us/security/blog/2026/11/06/artex-abuse-expands-as-cybercriminals-target-financial-sector/"
  - ".raw/articles/microsoft-storm-3358-artex-2026-11-06.md"
  - "https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks"
  - "https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/"
  - "https://blog.talosintelligence.com/artex-and-beyond/"
  - "https://www.ic3.gov/CSA/2026/261113.pdf"
  - "https://censys.com/blog/artex-agentic-asbestos"
verified: 2026-11-18
verified_against:
  - ".raw/articles/microsoft-storm-0940-artex-2026-10-21.md"
  - ".raw/articles/microsoft-storm-3358-artex-2026-11-06.md"
verified_findings: 0
verified_note: "3 low fixed; conflicts cross-checked against GTIG, FBI, Talos, CrowdStrike and Censys raws"
---

# Microsoft Storm-0940 ARTEX Report

**Source:** [Microsoft Security Research and Microsoft Threat Intelligence — AI-driven, large-scale nation-state campaign using ARTEX enables supply chain compromise](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/) (2026-10-21), and the follow-up [Microsoft Threat Intelligence — ARTEX abuse expands as cybercriminals target financial sector](https://www.microsoft.com/en-us/security/blog/2026/11/06/artex-abuse-expands-as-cybercriminals-target-financial-sector/) (2026-11-06). Local copies: `.raw/articles/microsoft-storm-0940-artex-2026-10-21.md` and `.raw/articles/microsoft-storm-3358-artex-2026-11-06.md`.

[[microsoft|Microsoft]] Threat Intelligence ties [[artex|ARTEX]] to a state-aligned supply-chain operation: Storm-0940 used the framework to abuse compromised ConnectWise Cloud Backup application identities and authenticate into downstream Microsoft 365 tenants without an interactive sign-in. The report is the identity-plane account of the [[agentic-offensive-framework|agentic offensive framework]] wave, written from Entra ID and Microsoft Graph telemetry, and it carries the containment guidance that the FBI later reproduced. A second post, summarized below, extends Microsoft's reporting to Storm-3358, a financially motivated cluster.

### Key claim

"As early as September 2026, Storm-0940, a China-state aligned actor, used the agentic AI penetration testing framework ARTEX to automate attacks targeting hundreds of organizations globally." Several attacks reached privileged access to cloud infrastructure, "including multiple instances where the time from initial targeting to privileged access was less than 3 hours." Microsoft places the change in tempo: OAuth application abuse is "a typical part of Storm-0940's toolset," and ARTEX moved it "to machine speed."

### Evidence base

The evidence is Microsoft's own telemetry: Entra ID sign-in records, Microsoft Graph activity, and Defender XDR, Defender for Cloud Apps and Defender for Key Vault alerts. Microsoft links the password-spray campaign to the ConnectWise compromise through infrastructure: public reports on October 15 and ConnectWise's October 16 advisory listed indicators in the same network range, and one address matched exactly.

### Storm-0940 and its use of ARTEX

Microsoft describes Storm-0940 as a China-state aligned actor "with no known industry aliases" that has been active since 2021 and targets organizations for data theft. Its established tradecraft is password spraying, exploitation of internet-facing applications, and persistence by adding credentials to existing OAuth applications. The ARTEX campaign targeted 14 named sector groups, from aerospace and defense to think tanks and non-profits. Microsoft states the outcome both ways: "the majority of the targeting wasn't successful," yet Storm-0940 "achieved privileged cloud persistence at many organizations in a brief time."

Microsoft describes ARTEX as "an open-source penetration testing framework that can use any AI model," written in Go with a Next.js front end, whose interface defaults to English and switches to Korean, Mandarin, Spanish or Japanese. Its agents accumulate discovered assets and vulnerabilities into a graph.

### Password spray and downstream access

The report separates two phases of activity.

- **Password spray, October 10 to 14.** One Storm-0940 address sprayed 327 accounts across eight organizations. Telemetry captured more than 151,000 failed logons, with a peak of 2,900 failures per minute on October 11; 48 accounts received more than 1,000 failures, and lockout responses made up nearly 98% of the activity. "No successful authentication, token issuance, Graph call, or directory change was observed."
- **Downstream tenant access.** After acquiring control of multiple ConnectWise cloud application identities, agents acting for the actor used the stolen application credentials from the same address range. "Storm-0940 used ARTEX to attempt to sign-in to more than 2,250 tenants." In some environments the agents went on to Graph and directory operations: reconnaissance, application credential changes for persistence, and changes to identities, groups and privileged roles. Defender for Key Vault recorded secret reads at scale across multiple vaults in downstream environments.

Microsoft identified "multiple customer environments with evidence of follow-on activity" and gives no count. It adds that an affected application or a successful authentication "does not necessarily indicate that customer data was accessed or exfiltrated." The activity "did not result from a compromise of or a vulnerability in Microsoft corporate systems, products, or services."

### Containment guidance

Microsoft's immediate actions are written for an incident responder holding a tenant that authorized the affected applications.

- Keep affected ConnectWise application connections disabled until ConnectWise issues restoration instructions.
- "Do not delete enterprise applications as the first response." Preserve configuration and evidence first.
- Search for the affected applications by application ID, including in tenants where the integration is no longer in use.
- Review activity from at least October 15, 2026, and extend service-principal sign-in review to October 1 where records exist.
- Review successful and failed service-principal sign-ins separately, and do not limit review to the published addresses.
- Review Entra audit records for credential additions, new service principals or owners, consent grants and privileged role assignments.

Microsoft warns that the application credential changes can stay active as a backdoor after the original compromised application is remediated, and its guidance puts audit review before cleanup. The case is a [[non-human-identity|non-human identity]] compromise: a multi-tenant application's trusted access carried the actor into each downstream tenant.

### Storm-3358 follow-up

The 2026-11-06 post reports that Storm-3358 "began targeting organizations in the financial sector using the agentic AI offensive framework ARTEX" in October 2026. Microsoft describes Storm-3358 as "a Cambodia-based intrusion cluster that primarily targets financial organizations in Southeast Asia, in addition to a smaller number of large financial organizations globally." The cluster combined ARTEX with conventional automated tools, exploited anonymous file upload and server-side request forgery flaws, and had ARTEX agents download the WISP, vshell and AdaptixC2 remote access trojans with netcat and curl.

The post makes two defensive contributions.

- **Default tool fingerprints.** ARTEX by default drives Nmap, curl, wget, Python HTTP clients, "Norma SDK WebFetch" and headless Chromium through Playwright. Their default user agents can alert defenders, though blocking them "will not stop an attack by an AI agent, which will quickly change them."
- **Deception against agents.** Honeypots, decoys, tarpits and "fake wins" can pull autonomous attacks away from real assets. A fake win presents convincing evidence that the agent's objective is complete, which can make it stop on a false success. "In Microsoft's testing, these controls helped reduce the attack traffic reaching real assets and produced high-confidence, attribution-quality signal." The post gives no measurements for that testing.

The post's IOC table and its Sentinel hunting query disagree on one ARTEX server address in the second octet. The FBI FLASH reproduces the table's value.

### Attribution and confidence wording

- "Microsoft assesses that Storm-0940 was responsible for the recently disclosed ConnectWise supply chain compromise of a multi-tenant application." No confidence level accompanies "assesses."
- "A China-state aligned actor" is a description, without estimative language.
- Storm-3358 is "a Cambodia-based intrusion cluster" and "financially motivated," both stated without qualifiers.
- Neither post mentions GTIG's UNC7228 or CrowdStrike's VAPOR PANDA.

### Limits of the reports

- **Compromise count.** The reports count attempted tenants and targeted accounts, and give no figure for tenants compromised or tenants with follow-on activity.
- **Upstream compromise.** The report does not describe how Storm-0940 gained control of the ConnectWise application identities, or whether ARTEX took part in that step.
- **Linking evidence.** The tie between the eight-organization spray and the ConnectWise compromise rests on shared network infrastructure.
- **Internal inconsistencies.** The immediate actions say to inventory "all four application IDs listed in this profile," and the hunting queries list three. The Figure 1 caption presents the 2,250-tenant reach and the 327-account spray as one "subset" of the campaign, while the text separates them into two phases. Censys repeats the merged reading and pairs the 2,900-per-minute peak with the 2,250-tenant sign-ins.[^censys]
- **Deception results.** The honeypot and fake-win claim cites unpublished internal testing.

### Conflicts with the other ARTEX reports

- **Start date.** Microsoft says September 2026. GTIG dates UNC7228, which "overlaps with" Storm-0940, to at least August 2026 on Wiz's evidence.[^gtig]
- **Speed.** Microsoft's "less than 3 hours" runs from initial targeting to privileged access. GTIG's about four hours, and 15 minutes in one case, runs from first compromised account to deep system access.[^gtig] The FBI FLASH repeats Microsoft's figure as "under three hours."
- **Attempted against confirmed.** Microsoft, the FBI and Talos all report attempted sign-ins to more than 2,250 tenants.[^fbi][^talos] CrowdStrike's VAPOR PANDA report, by contrast, gives a confirmed count of nine organizations with post-exploitation activity.[^cs] Only CrowdStrike's figure is a compromise count.
- **Separate campaigns.** CrowdStrike states that VAPOR PANDA "is separate from the ARTEX activity recently reported by industry sources under the Storm-0940 designation."[^cs]
- **Interface default.** Microsoft says ARTEX's interface defaults to English. Censys reports that among exposed consoles "the Chinese default dominates."[^censys]
- **Review window.** The FBI FLASH tells responders to start review "from at least October 1, 2026," the date Microsoft gives as an extension.[^fbi]

### Relations

- The [[storm-0940-artex-connectwise|Storm-0940 ARTEX Supply-Chain Campaign]] page carries the incident and its response guidance across publishers.
- The [[fbi-artex-flash-2026-11|FBI ARTEX FLASH Advisory]] reproduces the ConnectWise figures, the identity recommendations and the Storm-3358 indicators.
- In the [[talos-artex-and-beyond|Talos ARTEX and Beyond Report]], Talos states that its own observations of Storm-3358 "are in line with Microsoft's findings," and that addresses tied to the cluster scanned assets in October 2026.
- **Supports [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]]** on commodity tooling. The two posts report the same framework in a China-state aligned operation and a financially motivated cluster, which the thesis reads as the harness no longer separating state-grade operations from criminal use.
- [[agentic-offensive-wave-precursors-2026-11|Agentic Offensive Wave Precursors]] reads Microsoft's statement that OAuth application abuse was existing Storm-0940 tradecraft as evidence that ARTEX changed the operation's tempo and left its method in place.
- The "Norma SDK" in ARTEX's default tooling matches the "norma" agentic harness that Talos detects with Snort rules, so two vendors name the same component.[^talos]

### Notes

[^gtig]: [Google Threat Intelligence Group — ARTEX and Friends: Enter Agentic Offensive Frameworks](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks), 2026-10-30. UNC7228 start date and the first-account-to-deep-access interval, from Wiz research.
[^fbi]: [FBI — Threat Overview of ARTEX Autonomous Penetration Testing Framework, FLASH-20261113-001](https://www.ic3.gov/CSA/2026/261113.pdf), 2026-11-13. Attempted sign-ins to more than 2,250 downstream tenants; review window under "Preserve Evidence and Respond."
[^talos]: [Cisco Talos — ARTEX and beyond: AI-enabled penetration testing platforms](https://blog.talosintelligence.com/artex-and-beyond/), 2026-11-13. Talos's Storm-0940 summary, its Storm-3358 corroboration and its Snort coverage for norma.
[^cs]: [CrowdStrike — VAPOR PANDA Uses AI-Driven ARTEX to Grow Global Espionage Campaign](https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/), 2026-10-29. Organizations with confirmed post-exploitation activity, and the separation from Storm-0940. Summarized at [[crowdstrike-vapor-panda-artex|CrowdStrike VAPOR PANDA ARTEX Report]].
[^censys]: [Censys — ARTEX: Agentic Asbestos](https://censys.com/blog/artex-agentic-asbestos), 2026-11-05. Censys's summary of the Microsoft report, and the console languages Censys observed on exposed hosts.
