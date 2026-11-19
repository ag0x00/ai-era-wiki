---
type: incident
title: "Storm-0940 ARTEX Supply-Chain Campaign"
created: 2026-11-18
updated: 2026-11-18
tags:
  - incidents
  - offensive-ai
  - agentic-offensive-framework
  - supply-chain
  - oauth
  - microsoft-365
  - china-nexus
status: developing
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
incident_class: "supply-chain"
attack_with_or_on_ai: "with AI"
date_observed: 2026-10-10
date_disclosed: 2026-10-15
target: "Microsoft 365 and Azure tenants of ConnectWise Cloud Backup customers, reached through compromised ConnectWise multi-tenant OAuth applications"
threat_actor: "Storm-0940 (Microsoft: China-state aligned); overlaps the cluster GTIG tracks as UNC7228"
impact: "Attempted sign-in to more than 2,250 downstream tenants; Graph, directory and Key Vault activity, including privileged role changes, in multiple customer environments; no compromised-tenant count published"
related:
  - "[[artex]]"
  - "[[agentic-offensive-framework]]"
  - "[[microsoft-storm-0940-artex]]"
  - "[[gtig-artex-and-friends]]"
  - "[[fbi-artex-flash-2026-11]]"
  - "[[crowdstrike-vapor-panda-artex]]"
  - "[[talos-artex-and-beyond]]"
  - "[[agentic-offensive-wave-precursors-2026-11]]"
  - "[[agentic-offensive-wave-open-questions]]"
  - "[[non-human-identity]]"
  - "[[gtg-1002-ai-orchestrated-espionage]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[offensive-ai-state-of-the-field]]"
sources:
  - "https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/"
  - "https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks"
  - "https://www.ic3.gov/CSA/2026/261113.pdf"
  - "https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/"
  - "https://blog.talosintelligence.com/artex-and-beyond/"
  - "https://censys.com/blog/artex-agentic-asbestos"
  - "[[.raw/articles/censys-artex-agentic-asbestos-2026-11-05.md]]"
  - "[[.raw/articles/microsoft-storm-0940-artex-2026-10-21.md]]"
  - "[[.raw/articles/gtig-artex-and-friends-2026-10-30.md]]"
  - "[[.raw/reports/fbi-flash-20261113-001-artex-2026-11-13.md]]"
  - "[[.raw/articles/crowdstrike-vapor-panda-artex-2026-10-29.md]]"
  - "[[.raw/articles/talos-artex-and-beyond-2026-11-13.md]]"
verified: 2026-11-18
verified_against:
  - ".raw/articles/censys-artex-agentic-asbestos-2026-11-05.md"
  - ".raw/articles/crowdstrike-vapor-panda-artex-2026-10-29.md"
  - ".raw/articles/gtig-artex-and-friends-2026-10-30.md"
  - ".raw/articles/microsoft-storm-0940-artex-2026-10-21.md"
  - ".raw/articles/talos-artex-and-beyond-2026-11-13.md"
  - ".raw/reports/fbi-flash-20261113-001-artex-2026-11-13.md"
verified_findings: 0
verified_note: "2 low fixed (four application IDs wording, inferred causation)"
---

# Storm-0940 ARTEX Supply-Chain Campaign

In October 2026 the actor Microsoft tracks as Storm-0940 used the [[artex|ARTEX]] agentic penetration testing framework to turn a compromise of ConnectWise Cloud Backup into access to its customers' Microsoft 365 and Azure tenants. The agents authenticated as ConnectWise's own multi-tenant OAuth applications, which customer tenants had already granted access, so the actor needed no interactive user sign-in. Microsoft, which assesses Storm-0940 as China-state aligned, reports that the actor attempted to sign in to more than 2,250 tenants and that multiple customer environments show follow-on activity through Microsoft Graph.[^ms] Microsoft publishes no count of compromised tenants.

## Summary

Storm-0940 has been active since 2021 and targets organizations across many industries for data theft. Microsoft describes its established method as password spraying and exploitation of internet-facing applications, followed in cloud environments by adding credentials to existing OAuth applications and reading mail and other data through Microsoft Graph.[^ms] The ConnectWise campaign used that same method. What changed was the speed: Microsoft states that "the actor used ARTEX to accelerate the activity to machine speed", and that in multiple cases the time from initial targeting to privileged cloud access was under three hours.[^ms]

The campaign sits inside a wider ARTEX operation. Microsoft reports Storm-0940 using ARTEX from as early as September 2026 against hundreds of organizations in sectors from aerospace and defense to think tanks, and states that "while the majority of the targeting wasn't successful, Storm-0940 achieved privileged cloud persistence at many organizations in a brief time".[^ms] Microsoft also states that the activity did not result from a compromise of, or a vulnerability in, Microsoft systems, products or services.[^ms]

## Attack Vector

### Password spray before the disclosure

Between 10 and 14 October 2026, Storm-0940 infrastructure ran an automated cross-tenant password spray against 327 accounts across eight organizations to reach Microsoft 365 and Exchange Online.[^ms] Microsoft telemetry recorded more than 151,000 failed logon events, peaking on 11 October at 2,900 failures per minute.[^ms] Lockout responses made up nearly 98% of the activity, and Microsoft observed no successful authentication, token issuance, Graph call or directory change in this phase.[^ms] The spray matters to responders because it shares infrastructure with the later downstream access: the indicators in the first public reports of the ConnectWise compromise fell in the same network range.[^ms]

### Compromised ConnectWise application identities

Public reports of the ConnectWise Cloud Backup compromise appeared on 15 October, and ConnectWise posted its initial advisory on 16 October, describing attempted unauthorized access associated with Cloud Backup.[^ms] Microsoft does not describe how the actor gained control of the application identities. It reports what followed: after acquiring control of the identities of multiple ConnectWise cloud applications, the agents used the stolen application credentials to authenticate to customer environments, "effectively taking advantage of the already established trust by these apps within a tenant".[^ms]

The access was application-only. A service principal authenticating with its own credential carries the permissions the tenant consented to when it installed the integration, so user sign-in controls such as multifactor prompts did not apply to it. Microsoft's guidance refers to four affected application IDs listed in its threat-analytics profile. Its published hunting queries list three of them, for the Cloud Backup, Microsoft Teams and Microsoft Graph integrations.[^ms]

### Follow-on activity in customer tenants

In some environments the agents went beyond authentication to Graph and directory operations: reconnaissance, application credential changes to establish persistence, and changes to identities, groups and privileged roles.[^ms] Microsoft's detections for the campaign include an application assigning itself the Global Administrator role, and in Azure, enumeration of cloud resources and secret reads at scale across multiple Key Vaults.[^ms] Credential changes made by the agents can persist through other identities and services, and "can remain active as a backdoor even after the original compromised application is mediated".[^ms] Microsoft cautions that the presence of an affected application, or even successful authentication, "does not necessarily indicate unauthorized data access or data exfiltration".[^ms]

## Timeline

- **At least August 2026.** Earliest activity in the campaign Wiz Research tracks, which GTIG tracks as UNC7228.[^gtig]
- **As early as September 2026.** Storm-0940 begins using ARTEX against hundreds of organizations.[^ms]
- **2026-10-10 to 2026-10-14.** Cross-tenant password spray against 327 accounts in eight organizations; no successful authentication observed.[^ms]
- **2026-10-15.** First public reports of the ConnectWise Cloud Backup compromise. Microsoft's guidance starts its review window on this date.[^ms]
- **2026-10-15 to 2026-10-16.** Graph API access through the compromised applications from transient VPN addresses, per Microsoft's indicator notes.[^ms]
- **2026-10-16.** ConnectWise posts its initial advisory, later expanded with containment guidance.[^ms]
- **2026-10-21.** Microsoft publishes its attribution to Storm-0940 and its response guidance.[^ms]
- **2026-10-29.** CrowdStrike reports a separate ARTEX campaign by VAPOR PANDA and states it is distinct from the Storm-0940 activity.[^cs]
- **2026-10-30.** GTIG reports that UNC7228 overlaps Storm-0940.[^gtig]
- **2026-11-13.** The FBI's FLASH on ARTEX cites the ConnectWise compromise and repeats Microsoft's response guidance.[^fbi]

None of the publications cited here declares the campaign over.

## Counts and attribution

### Counts

Each figure counts something different, and none of them counts compromised tenants. The wording column keeps the source's verb, because "attempted", "targeting" and "observed" do not mean "compromised".

| Figure | What it counts | Source wording |
|---|---|---|
| More than 2,250 tenants | Downstream tenants the actor tried to sign in to after the ConnectWise access[^ms] | "attempt to sign-in to more than 2,250 tenants" |
| 327 accounts in eight organizations | Targets of the 10–14 October password spray[^ms] | "targeting 327 accounts across eight organizations" |
| More than 151,000 | Failed logon events in that spray[^ms] | "151,000+ failed logon events" |
| 2,900 per minute | Peak failure rate of the spray, on 11 October[^ms] | "peaking on October 11 at 2,900 failures per minute" |
| No count | Compromised tenants[^ms] | "multiple customer environments with evidence of follow-on activity" |

The FBI repeats the first figure as "attempted sign-ins to more than 2,250 downstream tenants".[^fbi] Censys joins the first and fourth figures into one claim, that the group "attempted sign-ins to more than 2,250 tenants, peaking at 2,900 attempts per minute".[^censys] Microsoft's text attaches the per-minute peak to the earlier spray against 327 accounts, and its summary graphic shows both under a "subset" of the campaign, so the primary supports two separate figures rather than a per-minute rate of tenant sign-ins.[^ms] A count of compromised tenants attributed to Huntress reaches the record only through press coverage, and [[agentic-offensive-wave-open-questions|Agentic Offensive Wave Open Questions]] tracks it.

### Attribution

Each publisher's confidence wording is reproduced here without upgrade.

- **Microsoft** "assesses that Storm-0940 was responsible for the recently disclosed ConnectWise supply chain compromise", and describes Storm-0940 as "a China-state aligned actor (with no known industry aliases)". The post states no confidence level beyond "assesses".[^ms]
- **GTIG** reports that Wiz Research has tracked an ARTEX campaign "which GTIG tracks as UNC7228, that overlaps with activity tracked by Microsoft as the PRC-nexus actor STORM-0940". GTIG asserts overlap, not identity, and states no confidence level.[^gtig] Relaying Wiz, GTIG describes the cluster's targeting, compromise and data theft at US and European enterprise software, industrial automation, semiconductor and telecommunications organizations, typically moving from the first compromised account to deep system access in about four hours and in one case in 15 minutes.[^gtig]
- **CrowdStrike** states that its VAPOR PANDA campaign "is separate from the ARTEX activity recently reported by industry sources under the Storm-0940 designation".[^cs]
- **Cisco Talos** reports the attribution as Microsoft's, describing Storm-0940 as an actor Microsoft assesses is "China-state aligned".[^talos]
- **The FBI** names no actor. Its FLASH describes "a supply chain compromise affecting ConnectWise Cloud Backup customers", citing Microsoft, and lists Microsoft's addresses in its indicator table as the "ConnectWise campaign".[^fbi]

## Defensive Lessons

### Response guidance

Microsoft's immediate actions for affected tenants follow. The FBI repeats most of them.[^ms][^fbi]

- Keep affected ConnectWise application connections disabled until ConnectWise provides formal restoration instructions.
- Do not delete the enterprise applications as the first response. Preserve configuration and evidence before making irreversible changes.
- Inventory the affected application IDs and record each one's local service-principal object ID. Review every tenant where the applications are or were installed or authorized, even if the integration is no longer in use, and search by application ID rather than display name.
- Preserve Microsoft Entra sign-in and audit records, Microsoft Purview Audit records, SIEM data and Graph telemetry, without delaying urgent containment while exports run.
- Review activity from at least 15 October 2026, and extend the service-principal sign-in review to 1 October where records exist, including failed authentications and volume anomalies. The FBI condenses this to a review "starting from at least October 1, 2026".
- Review successful and failed service-principal sign-ins separately, and investigate unfamiliar sources beyond the published addresses.
- Review Entra audit records for credential additions, new applications, service principals or owners, app-role assignments, consent grants, privileged role assignments and Conditional Access changes.
- After evidence is preserved, have an authorized responder remove confirmed unauthorized credentials, grants, owners, assignments and identities, then verify that vendor credential rotation removed the remaining access.

### Lessons

**A multi-tenant application is a standing trust path into every tenant that consented to it.** Control of the vendor's application identity gave the actor application-only access with the permissions customers had granted, with no user to phish. Microsoft and the FBI both recommend least privilege on multi-tenant application permissions and consent grants, and monitoring of credential additions, privilege assignments and app-only activity on highly privileged applications.[^ms][^fbi] [[non-human-identity|Non-Human Identity (NHI)]] covers the governance of such identities.

**Remediating the vendor does not remediate the tenant.** Credentials the agents added inside a customer tenant can stay active after the original compromised application is remediated, and Microsoft asks each tenant to review its own audit records and verify that vendor credential rotation left no unauthorized access behind.[^ms]

**The agent changed the speed of an existing playbook.** OAuth application abuse was already part of Storm-0940's tradecraft, and Microsoft states that ARTEX accelerated it to machine speed. Across the actor's wider ARTEX operation, Microsoft reports privileged cloud access in under three hours in multiple cases.[^ms] That pattern matches GTIG's finding for the whole [[agentic-offensive-framework|Agentic Offensive Framework]] class, and it moves the containment decisions that matter to before the incident. [[agentic-offensive-wave-precursors-2026-11|Agentic Offensive Wave Precursors]] places the campaign in the adoption sequence that followed ARTEX's July 2026 release.

**Failed activity is evidence.** The password spray recorded no success, yet it preceded the public disclosure by one day and shared infrastructure with the downstream access. Microsoft's guidance therefore asks responders to review failed authentications and volume anomalies as well as successes.[^ms]

## Sources

- [Microsoft Threat Intelligence — AI-driven, large-scale nation-state campaign using ARTEX enables supply chain compromise](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/), 2026-10-21. Summarized at [[microsoft-storm-0940-artex|Microsoft Storm-0940 ARTEX Report]].
- [Google Threat Intelligence Group — ARTEX and Friends: Enter Agentic Offensive Frameworks](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks), 2026-10-30. Summarized at [[gtig-artex-and-friends|GTIG ARTEX and Friends Report]].
- [FBI — FLASH-20261113-001](https://www.ic3.gov/CSA/2026/261113.pdf), 2026-11-13. Summarized at [[fbi-artex-flash-2026-11|FBI ARTEX FLASH Advisory]].

## Notes

[^ms]: [Microsoft Threat Intelligence — AI-driven, large-scale nation-state campaign using ARTEX enables supply chain compromise](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/), 2026-10-21. Actor profile and attribution; the 10–14 October spray (327 accounts in eight organizations, more than 151,000 failed logons, a peak of 2,900 failures per minute on 11 October); attempted sign-in to more than 2,250 tenants; follow-on Graph, directory and Key Vault activity; detections, indicators and the immediate response actions.
[^gtig]: [Google Threat Intelligence Group — ARTEX and Friends: Enter Agentic Offensive Frameworks](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks), 2026-10-30. The UNC7228 overlap with Storm-0940 and GTIG's relay of Wiz Research findings: activity since at least August 2026, sectors, and the four-hour and 15-minute times to deep system access.
[^fbi]: [FBI — FLASH-20261113-001: Threat Overview of ARTEX Autonomous Penetration Testing Framework](https://www.ic3.gov/CSA/2026/261113.pdf), TLP:CLEAR, 2026-11-13. The ConnectWise reference ("attempted sign-ins to more than 2,250 downstream tenants", FBI note 2: Microsoft) and the recommendations it repeats.
[^cs]: [CrowdStrike — VAPOR PANDA Uses AI-Driven ARTEX to Grow Global Espionage Campaign](https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/), 2026-10-29. Statement that the VAPOR PANDA campaign is separate from the Storm-0940 activity. Summarized at [[crowdstrike-vapor-panda-artex|CrowdStrike VAPOR PANDA ARTEX Report]].
[^talos]: [Cisco Talos — ARTEX and beyond: AI-enabled penetration testing platforms](https://blog.talosintelligence.com/artex-and-beyond/), 2026-11-13. Talos's summary of the Storm-0940 attribution.
[^censys]: [Censys — ARTEX: Agentic Asbestos](https://censys.com/blog/artex-agentic-asbestos), 2026-11-05. The combined 2,250-tenant and 2,900-per-minute claim. Local copy: `.raw/articles/censys-artex-agentic-asbestos-2026-11-05.md`.
