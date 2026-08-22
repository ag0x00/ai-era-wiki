---
type: review
title: "AI Attribution Primary-Source Review"
address: c-000295
origin: produced
created: 2026-08-17
updated: 2026-08-17
tags:
  - reviews
  - attribution
  - offensive-ai
  - incidents
  - primary-sources
status: complete
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
target: "The seven AI attributions recorded in [[ai-attribution-audit-2026-08]], re-tested against the primary documents each one names"
supersedes: "[[ai-attribution-audit-2026-08|AI Attribution Audit]]"
related:
  - "[[ai-attribution-audit-2026-08|AI Attribution Audit]]"
  - "[[offensive-ai-state-of-the-field|Offensive AI: State of the Field]]"
  - "[[capability-floor-collapse|Capability Floor Collapse]]"
  - "[[agentic-ai-threat-classes-2026|Agentic AI Threat Classes]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[gtg-1002-ai-orchestrated-espionage|GTG-1002: AI-Orchestrated Espionage Campaign]]"
  - "[[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]]"
sources:
  - "https://www.ic3.gov/CSA/2026/260731.pdf"
  - "https://blog.talosintelligence.com/uat-8616-sd-wan/"
  - "https://cloud.google.com/blog/topics/threat-intelligence/zero-day-exploitation-cisco-catalyst-sd-wan-manager"
  - "https://unit42.paloaltonetworks.com/ivanti-cve-2026-1281-cve-2026-1340/"
  - "https://unit42.paloaltonetworks.com/handala-hack-wiper-attacks/"
  - "https://aws.amazon.com/blogs/security/ai-augmented-threat-actor-accesses-fortigate-devices-at-scale/"
  - "https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-technology-threat-landscape-report/"
  - "https://blog.eclecticiq.com/shinyhunters-calling-financially-motivated-data-extortion-group-targeting-enterprise-cloud-applications"
  - "https://oag.ca.gov/system/files/AA%20-%20Individual%20Notice%20Letter%20-%20CA.pdf"
  - "https://oag.ca.gov/system/files/EY%20Notice%20Letter%20US%20General.pdf"
  - "https://www.in.gov/attorneygeneral/consumer-protection-division/id-theft-prevention/files/DB-Year-to-Date-Report-7_2026.pdf"
---

# AI Attribution Primary-Source Review

Seven cyber incidents circulated as a June–July 2026 "AI-enabled" cluster were assessed on 2026-08-16 against vendor research posts, wire reporting, and regulator breach listings. That assessment recorded two supported AI attributions, one mis-assembled claim, and four unsupported. This review tests the same seven attributions a second time, against the primary documents each attribution names: a joint alert carrying eleven government signatures, seven vendor threat-intelligence publications, two victim breach-notification letters, and one state regulator listing, each read end to end.

No verdict changed direction. Four attributions recorded as unsupported are now sourced negatives, because the parties closest to those incidents published and stated no AI element. Both supported attributions narrowed against their primaries, and the stronger of the two narrowed furthest. One incident in the set turned out to carry a fully sourced AI role that the audit's own taxonomy has no slot for.

## Relation to the 2026-08-16 audit

[[ai-attribution-audit-2026-08|The AI Attribution Audit]] states its evidentiary basis in its body: "Primary documents were reachable for none of the seven; every finding rests on vendor research posts, wire reporting, and regulator breach listings." Eleven primaries were retrieved and read for this review, so that sentence no longer describes the evidence under the findings. A dated snapshot records what was assessed on a given date and on what evidence, which leaves the sentence uneditable while the page remains a 2026-08-16 record. This review supersedes the audit on the evidence.

The audit's seven verdicts stand as its 2026-08-16 record and are unchanged there, and so are the identifiers, dates, and figures it transcribed — a wrong CVE identifier and a wrong publication date are part of what that page recorded on its date. The corrected values are stated in the findings below, and this page is the authority for them. The change in what the evidence supports also lands here.

## Scope and method

The audit's bar carries over unchanged. An AI attribution counts as supported only where a named source — a government advisory, a vendor threat-intelligence report, or the victim's own disclosure — states the AI element. The class of document behind each verdict is what moved: the audit read reporting about the primaries, and this review reads the primaries.

Absence in a primary is recorded here as a sourced negative, meaning the document the attribution names, or the most authoritative document the incident produced, was read in full and contains no such statement. A sourced negative outranks absence in available reporting. It ranks below a source stating the negative outright, the strongest form available and one that no document in this set provides. Two of the four negatives rest on the victim's own breach-notification letter, the highest-authority document a breach produces short of a regulator's own finding.

### Primary documents read

| Incident | Primary document | Publisher | Published |
|---|---|---|---|
| DPRK IT-worker infiltration | Alert to Countries, Companies, and Other Entities Regarding North Korean IT Workers | Eleven-nation joint alert via IC3 | 2026-07-31 |
| DPRK IT-worker infiltration | 2026 Technology Threat Landscape Report | CrowdStrike | 2026-06-09 |
| ShinyHunters extortion cluster | ShinyHunters Calling: Financially Motivated Data Extortion Group Targeting Enterprise Cloud Applications | EclecticIQ | 2025-09-22 |
| Ivanti exploitation | Threat Brief: Ivanti Endpoint Manager Mobile CVE-2026-1281 and CVE-2026-1340 | Palo Alto Networks Unit 42 | 2026-02-17 |
| Fortinet exploitation | AI-augmented threat actor accesses FortiGate devices at scale | Amazon Threat Intelligence | 2026-02-20 |
| Stryker wiper campaign | Insights: Increased Risk of Wiper Attacks | Palo Alto Networks Unit 42 | 2026-03-12 |
| Cisco SD-WAN zero-days | Active exploitation of Cisco Catalyst SD-WAN Controller by UAT-8616 | Cisco Talos | 2026-02-25 |
| Cisco SD-WAN zero-days | Zero-Day Exploitation of Vulnerability (CVE-2026-20245) in Cisco Catalyst SD-WAN Manager | Mandiant | 2026-06-24 |
| AssuranceAmerica exposure | California individual notice letter, breach report sb24-625034 | AssuranceAmerica Managing General Agency | 2026-06-17 |
| AssuranceAmerica exposure | Data Breach Year-to-Date Report, July 2026, row 60 | Indiana Attorney General | 2026-07-31 |
| Ernst & Young platform breach | US general notice letter, breach report sb24-626542 | Ernst & Young LLP | 2026-07-15 |

## Findings

| Incident | 2026-08-16 verdict | Verdict on the primary | Movement |
|---|---|---|---|
| DPRK IT-worker infiltration | Supported | Confirmed, mechanism narrowed | Advisory carries "integration of AI"; the deepfake mechanism belongs to a secondary |
| ShinyHunters extortion cluster | Supported, different mechanism | Confirmed, two bounds added | Vishing sits with contracted operators; evidence dated 2025-09-22 |
| Ivanti / Fortinet exploitation | Mis-assembled | Confirmed, both halves primary-sourced | Fortinet half is a third sourced AI case, at a third position |
| Stryker wiper campaign | Unsupported | Sourced negative | Unit 42 attributes nothing to AI and never names Stryker |
| Cisco SD-WAN zero-days | Unsupported | Sourced negative | Neither Talos nor Mandiant uses the words AI, machine learning, or fuzzing |
| AssuranceAmerica exposure | Unsupported | Sourced negative, one claim refuted | The notice letter names no method at all |
| Ernst & Young platform breach | Unsupported | Sourced negative | The victim's own letter describes no automation |

### DPRK IT-worker infiltration — confirmed, mechanism narrowed

Eleven governments issued a coordinated alert on 2026-07-31 stating that North Korean IT workers "employ increasingly sophisticated methods, including the integration of AI, to obfuscate their identities and expand their activities globally".[^ic3] The alert specifies two AI uses. Large language models produce convincing profiles and written communications in second languages, given as the reason the machine-translation screening indicator has become unreliable. Video feeds "that appear to be manipulated or artificially generated" appear as a screening indicator for hiring and procurement staff, alongside photo-ID mismatch and refusal to join video calls. Identification documents are described as forged or altered using image editing software.[^ic3]

The alert does not use the word deepfake, describes no real-time video generation, and mentions no synthetic audio, AI-generated resumes, or AI-generated portfolio sites. The funnel-wide account of the tradecraft comes from Dark Reading's summary and carries that source's authority.[^dr]

Five governments join a product of this kind for the first time — France, Germany, Italy, the Netherlands, and New Zealand — alongside the six that had already published their own advisories: the United States, Japan, the Republic of Korea, the United Kingdom, Australia, and Canada.[^ic3] The Identity Theft Resource Center is not a participant and appears nowhere in the alert, which converts the audit's search-coverage doubt about an ITRC attribution into a sourced negative against the advisory text.

Scale comes from a separate primary with a scope the audit narrowed. CrowdStrike attributes 47% of all state-sponsored hands-on-keyboard operations against the technology sector between 2025-04-01 and 2026-03-31 to FAMOUS CHOLLIMA, whose IT-worker campaigns sought fraudulent employment at technology companies across North America, Europe, and Asia.[^cs] The audit rendered that as US technology companies, which understates the geography by two continents.

### ShinyHunters extortion cluster — confirmed, two bounds added

EclecticIQ assessed with high confidence on 2025-09-22 that ShinyHunters combines AI-enabled voice phishing with supply-chain compromise and malicious insiders, abusing the legitimate voice-agent platforms Vapi and Bland, whose built-in language model keeps a call convincing when the victim responds outside the scripted scenario.[^eiq] The audit reproduces this accurately and near-verbatim.

Two bounds travel with the assessment and the audit carries neither. The report attributes the voice-phishing calls to contracted Scattered Spider operators working inside the cluster, with ShinyHunters purchasing the capability rather than staffing it.[^eiq] And the report's campaign evidence is 2025 Salesforce, airline, and retail activity, roughly ten months before the June–July 2026 window under which the cluster circulates.

The report contains no Instructure or Canvas material and no claim that the group ran models over stolen archives; both rest on the secondary sources the audit cites for them.

### Ivanti and Fortinet exploitation — confirmed, both halves primary-sourced

The audit's mis-assembly finding holds exactly as written, and both halves are now sourced to primaries.

Unit 42's Ivanti brief documents CVE-2026-1281 and CVE-2026-1340 as unauthenticated remote code execution in Endpoint Manager Mobile, both at CVSS 9.8, reached through bash arithmetic-expansion injection in two distinct RewriteMap scripts, against organisations in the United States, Germany, Australia, and Canada, with 4,400+ EPMM instances visible in Cortex Xpanse telemetry.[^u42i] The brief attributes nothing to AI. It supplies the phrase that most plausibly seeded the AI framing, and that phrase describes conventional tooling: exploitation attempts are "widespread and mostly automated", with opportunistic attackers "integrating new CVEs into automated scanning frameworks within hours".[^u42i]

Amazon Threat Intelligence documented a Russian-speaking, financially motivated actor of low-to-medium baseline skill compromising more than 600 FortiGate devices across more than 55 countries between 2026-01-11 and 2026-02-18, with no exploitation of any FortiGate vulnerability: initial access ran on exposed management ports and single-factor credentials.[^aws] AI carried the planning and the tooling. The actor used at least two commercial language-model providers to generate step-by-step attack methodologies with expected success rates and prioritised task trees, to write the reconnaissance framework and the configuration parsers, and in one case to plan lateral movement from a live victim's submitted internal topology.[^aws] The same report bounds the capability: the actor could not compile custom exploits, debug failed attempts, or adapt when conditions departed from the generated plan, and abandoned hardened targets rather than persisting.

### Stryker wiper campaign — sourced negative

Unit 42's 2026-03-12 assessment of Handala Hack (aka Void Manticore, COBALT MYSTIQUE, Storm-1084/Storm-0842) names phishing-based identity compromise and administrative access through Microsoft Intune as the primary vector for the group's destructive operations, and reports that the threat-intelligence community assesses the group as a state-directed front for Iran's Ministry of Intelligence and Security.[^u42h] The audit assigns that attribution to Unit 42 itself, which over-assigns a judgment Unit 42 relays.

The brief carries no AI or machine-learning reference in any offensive role; its only such string is a Palo Alto product name. It attributes no reconnaissance, targeting, or execution step to AI, and it neither names Stryker nor discusses the incident's scale: the approximately 56,000 employees across 61 or more countries come from [Censys](https://censys.com/blog/iranian-wiper-attack-global-medtech-firm-stryker/). Israel's National Cyber Directorate warning of 2026-03-06, quoted in the brief, describes the same shape from the defender's side: attackers who "gained access to corporate networks and deleted servers and workstations" using "access data from legitimate corporate users".[^u42h]

### Cisco SD-WAN zero-days — sourced negative

Talos tracks active exploitation of CVE-2026-20127, an authentication bypass in Cisco Catalyst SD-WAN Controller (formerly vSmart), and clusters the exploitation and post-compromise activity as UAT-8616, with evidence of related activity reaching back to 2023.[^talos] The audit attaches UAT-8616 to CVE-2026-20182. Mandiant confirms CVE-2026-20182 exists as a sibling peering-authentication bypass disclosed by Cisco, so the identifier is real and the actor cluster attached to it is wrong.[^mandiant]

Mandiant found CVE-2026-20245 during a service-provider incident investigation, reported it to Cisco, and published a named-author write-up describing rogue peering from late 2025, credential manipulation over `vmanage-admin` SSH, a malicious CSV upload that appends a root account to `/etc/passwd` and `/etc/shadow`, and anti-forensic cleanup validated by a purpose-written check script.[^mandiant] That is a conventional investigation account start to finish.

Neither the Talos post nor the Mandiant post contains the strings AI, ML, machine learning, fuzz, or fuzzing in any form. The AI-assisted-fuzzing attribution is absent from both primaries, which extends the audit's finding from the secondary coverage to the two authoritative disclosing parties.

### AssuranceAmerica exposure — sourced negative, one claim refuted

AssuranceAmerica's California notice letter states that malicious activity on 2026-03-16 "targeted one of the Company's employees", that the company detected suspicious activity on 2026-03-17, and that an unauthorized third party then accessed its systems and copied data files.[^caaa] The letter names no method. It does not contain the words phishing, credential, social engineering, AI, or automated, and it identifies no actor. Its account of the file review is "this file evaluation process was only recently completed", with no date, which leaves the audit's 2026-06-15 investigation-completion date resting on secondary reporting alone.[^sa]

The audit asserts that the company attributes initial access to phishing of an employee, and its own following sentence contradicts that by observing the disclosure does not settle how the credential was taken. The company attributes nothing. "Targeted social engineering" is an inference from the word *targeted*; the letter discloses no such finding.

The Indiana Attorney General's listing records 6,998,886 people affected in total, with notification sent 2026-07-10, confirming the audit's figure and date exactly.[^inag] Neither document raises AI, and neither describes automation of any kind.

### Ernst & Young platform breach — sourced negative

EY's California notice letter records unauthorized access to a third-party IT service management platform, used by EY IT personnel supporting tax-related client work, from 2026-03-28 to 2026-04-12 — fifteen days — identified on 2026-04-23, eleven days after the access ended.[^caey] Documents pertaining to a number of EY clients were downloaded. The letter names no vendor, no method, no client count, and no automation, and states that EY has "no indication your personal information was specifically targeted". Federal law enforcement was notified.

The "automated scripts to siphon customer tax documents" description is absent from the victim's own disclosure, which is the strongest evidence class available for this item and was unread when the audit was written. The audit also calls the access window sixteen days; 2026-03-28 to 2026-04-12 is fifteen days elapsed across sixteen calendar dates.

## Assessment

### Evidence class under the four negatives

The audit recorded absence in available reporting and hedged accordingly: "these are open investigations, and a later report may supply what today's does not." That hedge is retired for four items. Talos and Mandiant published the two authoritative accounts of the Cisco SD-WAN exploitation and neither uses the words AI, machine learning, or fuzzing, and Mandiant's account of how CVE-2026-20245 was found is an incident investigation with four named authors.[^talos][^mandiant] Unit 42's Handala brief names phishing and Intune as the primary vector and attributes nothing to AI.[^u42h] AssuranceAmerica's and EY's own notice letters name no method at all, AI or otherwise.[^caaa][^caey]

The remaining possibility has changed shape. Before this review, an unsupported verdict was compatible with a source existing that the audit could not reach. After it, the parties closest to these four incidents have published and have said nothing about AI.

### Mechanism drift in the strongest attribution

The audit's most authoritative source produced its least durable claim. The DPRK finding was ranked Supported on the strength of an eleven-nation government advisory, and the advisory says "including the integration of AI, to obfuscate their identities", names language models for producing profiles and second-language communications, and lists artificially generated video feeds as one screening indicator among several for hiring staff.[^ic3] Real-time deepfake video impersonating real people in live interviews is Dark Reading's rendering of that material, and the audit cited it to the advisory.[^dr]

**The failure mode the audit was written to expose reproduced itself inside the audit, one authority level up: a secondary source's characterisation of a primary was cited as the primary's own claim.** The control for it is mechanical. An attribution to a named document is checkable only against that document, so a page that names a primary and quotes a secondary is unverified regardless of how authoritative the named primary is.

### AI as attack planner and tool developer

The audit's standing recommendation enumerates three capabilities that "AI-enabled" spans: a deepfake in a hiring interview, a language model triaging a stolen archive, and an autonomous agent selecting and exploiting a target. The FortiGate actor fits none of the three and is the best-evidenced AI case in the whole set.

Commercial models wrote the attack methodologies with expected success rates and prioritised task trees, wrote the reconnaissance framework and the configuration parsers, and in one instance planned lateral movement from a live victim's submitted internal topology, while the human ran the tools, chose the targets, and never touched a vulnerability.[^aws] The campaign reached more than 600 devices across more than 55 countries between 2026-01-11 and 2026-02-18 with no exploitation of any FortiGate flaw, which also makes it a clean negative datum for the [[zero-day-clock|Zero Day Clock]]: a large AI-augmented campaign that started no clock, because nothing was discovered or exploited.

The ceiling is documented in the actor's own operational notes: repeated failures where services were patched or vulnerabilities did not apply, a final assessment that key targets were "well-protected" with "no vulnerable exploitation vectors", and an inability to compile exploits, debug failures, or adapt when conditions departed from the plan.[^aws] The class is defined by what AI supplies, the operator's missing competence, rather than by where the human sits in the loop, which is why the autonomy axis on [[offensive-ai-state-of-the-field|the offensive-AI synthesis]] cannot locate it and why [[agentic-ai-threat-classes-2026|the 2026 threat-class taxonomy]] should carry it as its own class.

### Unaided-capability assessment without a prompt record

[[capability-floor-collapse|Capability Floor Collapse]] holds that the judgment "this actor could not have produced this artifact alone" comes from the model vendor's prompt record and is not recoverable from the artifact. Amazon reached that judgment without a prompt record, from two other routes: the actor's own exposed staging infrastructure, which held AI-generated attack plans and victim configurations in the clear, and generation hallmarks legible in the shipped code — comments that restate function names, JSON parsed by string matching rather than deserialised, and compatibility shims for language built-ins carrying empty documentation stubs.[^aws]

This widens the evidence base for capability-floor claims past the three model vendors whose telemetry the wiki currently depends on. A defender holding the actor's tooling and staging infrastructure can now make an assessment that the wiki had located only inside a vendor, which matters because the vendor-only route is available for exactly as long as the actor uses a hosted model with a cooperative provider. The verified-agentic incidents the wiki catalogs — [[gtg-1002-ai-orchestrated-espionage|GTG-1002]] and [[taiwan-ai-agent-government-intrusion|the Taiwan intrusion]] — remain vendor-sourced, and the FortiGate case shows the artifact-side route is viable alongside them.

### Mechanism, source, and date of record

Each of the four unsupported items had an AI story attached to a real property of the incident. Ivanti exploitation genuinely was "widespread and mostly automated" with attackers integrating new CVEs into automated scanning frameworks within hours.[^u42i] The EY breach genuinely did involve bulk download of documents from a ticketing platform.[^caey] The Stryker wipe genuinely was issued fleet-wide from a management console. In each case the AI label attached to automation, scale, or speed — properties that predate language models entirely.

The two supported cases add a dating problem of the same family. The EclecticIQ evidence for ShinyHunters voice phishing is dated 2025-09-22 and describes 2025 Salesforce, airline, and retail activity, and the Stryker wipe fell on 2026-03-11.[^eiq] On primary evidence, a "June–July 2026 AI-enabled cluster" is a set of incidents spanning September 2025 to July 2026 whose AI content, where present, was documented outside that window.

## Standing recommendation

Record an incident as AI-enabled only where a named source states the mechanism, and record three things together: the mechanism, the source that states it, and the date the source states it for. A citation to a primary must be checked against that primary, because a secondary's characterisation of an advisory travels under the advisory's authority and carries the secondary's reliability.

The audit's own recommendation enumerated three capabilities that "AI-enabled" spans: synthetic identity aimed at a hiring or trust decision, a language model triaging a stolen archive, and an autonomous agent selecting and exploiting a target. A fourth belongs on the list — a commercial model supplying an operator's missing planning and development competence at scale — and the FortiGate campaign is the primary-sourced case for it. The four carry different defenses, different maturity, and different evidence classes. Within this seven-incident set the archive-triage claim rests on secondary reporting alone, and no incident attests the autonomous agent.

> [!gap] Open on the primaries
> Two scale figures survive on secondary sourcing and were not upgraded: approximately \$800 million routed to Pyongyang in 2024, and the characterisation of the AssuranceAmerica exposure as the largest known theft of US driver's license data in 2026. Neither appears in any primary read for this review; both keep their secondary footnotes and neither should be re-attributed to a primary without one being produced. Three further figures need documents this pass did not reach — the Stryker employee and country counts against a competing "200,000+ devices across 79 countries" secondary account, SecurityWeek's count of the Cisco flaw as the seventh SD-WAN zero-day exploited in 2026, and CVE-2026-20262's CVSS 6.5. Resolving the first needs Stryker's SEC 8-K or the Censys post fetched as a primary.

[^ic3]: [Alert to Countries, Companies, and Other Entities Regarding North Korean IT Workers](https://www.ic3.gov/CSA/2026/260731.pdf), eleven-nation joint alert issued via IC3, 2026-07-31.
[^cs]: [CrowdStrike 2026 Technology Threat Landscape Report](https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-technology-threat-landscape-report/), CrowdStrike, 2026-06-09.
[^eiq]: [ShinyHunters Calling: Financially Motivated Data Extortion Group Targeting Enterprise Cloud Applications](https://blog.eclecticiq.com/shinyhunters-calling-financially-motivated-data-extortion-group-targeting-enterprise-cloud-applications), EclecticIQ, 2025-09-22.
[^u42i]: [Threat Brief: Ivanti Endpoint Manager Mobile CVE-2026-1281 and CVE-2026-1340](https://unit42.paloaltonetworks.com/ivanti-cve-2026-1281-cve-2026-1340/), Palo Alto Networks Unit 42, 2026-02-17.
[^u42h]: [Insights: Increased Risk of Wiper Attacks](https://unit42.paloaltonetworks.com/handala-hack-wiper-attacks/), Palo Alto Networks Unit 42, 2026-03-12.
[^aws]: [AI-augmented threat actor accesses FortiGate devices at scale](https://aws.amazon.com/blogs/security/ai-augmented-threat-actor-accesses-fortigate-devices-at-scale/), Amazon Threat Intelligence, 2026-02-20.
[^talos]: [Active exploitation of Cisco Catalyst SD-WAN Controller by UAT-8616](https://blog.talosintelligence.com/uat-8616-sd-wan/), Cisco Talos, 2026-02-25.
[^mandiant]: [Zero-Day Exploitation of Vulnerability (CVE-2026-20245) in Cisco Catalyst SD-WAN Manager](https://cloud.google.com/blog/topics/threat-intelligence/zero-day-exploitation-cisco-catalyst-sd-wan-manager), Mandiant (Google Cloud), 2026-06-24.
[^caaa]: [AssuranceAmerica Managing General Agency — California individual notice letter](https://oag.ca.gov/system/files/AA%20-%20Individual%20Notice%20Letter%20-%20CA.pdf), California Attorney General breach report sb24-625034, 2026-06-17.
[^caey]: [Ernst & Young LLP — US general notice letter](https://oag.ca.gov/system/files/EY%20Notice%20Letter%20US%20General.pdf), California Attorney General breach report sb24-626542, 2026-07-15.
[^inag]: [Data Breach Year-to-Date Report, July 2026](https://www.in.gov/attorneygeneral/consumer-protection-division/id-theft-prevention/files/DB-Year-to-Date-Report-7_2026.pdf), Indiana Attorney General Consumer Protection Division, row 60, 2026-07-31.
[^dr]: [North Korean Operatives Use Deepfakes in IT Job Interviews](https://www.darkreading.com/remote-workforce/north-korean-operatives-deepfakes-it-job-interviews), Dark Reading, 2026-08.
[^sa]: [AssuranceAmerica Breach Exposes 7 Million Driver's Licenses After Employee Account Hack](https://securityaffairs.com/195027/data-breach/assuranceamerica-breach-exposes-7-million-drivers-licenses-after-employee-account-hack.html), Security Affairs, 2026-07.
