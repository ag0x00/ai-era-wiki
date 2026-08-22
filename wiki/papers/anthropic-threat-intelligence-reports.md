---
type: paper
title: "Anthropic Threat Intelligence Reports"
address: c-000285
created: 2026-08-16
updated: 2026-08-16
tags:
  - papers
  - threat-intel
  - ai-in-sec-offense
  - agentic-ai
  - vendor-telemetry
status: summarized
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
year: 2025
authors:
  - "Alex Moix"
  - "Ken Lebedev"
  - "Jacob Klein"
venue: "Anthropic Threat Intelligence — periodic misuse-detection report series"
source_url: "https://www.anthropic.com/news/detecting-countering-misuse-aug-2025"
archived_copy: ".raw/reports/anthropic-threat-intelligence-report-2025-08.pdf"
key_claim: "Model-vendor abuse telemetry is a distinct evidence class: it records operations that never produced a victim disclosure, and it shows AI acting as operator rather than advisor across extortion, ransomware development, employment fraud, and state-aligned intrusion."
methodology: "Investigations by Anthropic's Threat Intelligence team into accounts banned for Usage Policy violations, combining automated classifier detections, the Clio privacy-preserving analysis tool, ad hoc threat hunting, and private intelligence-sharing partnerships. Case studies are published with victim and actor identifiers redacted and prompt excerpts simulated rather than quoted."
related:
  - "[[gtg-2002-vibe-hacking-extortion]]"
  - "[[gtg-5004-no-code-ransomware]]"
  - "[[gtg-1002-ai-orchestrated-espionage]]"
  - "[[llm-attack-navigator]]"
  - "[[capability-floor-collapse]]"
  - "[[anthropic]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[verizon-dbir-2026]]"
sources:
  - "[[.raw/reports/anthropic-threat-intelligence-report-2025-08.pdf]]"
  - "https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf"
  - "https://red.anthropic.com/2026/attack-navigator/"
---

# Anthropic Threat Intelligence Reports

**Source:** [Anthropic — *Threat Intelligence Report: August 2025*](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf) (PDF, 25 pages). Local copy: `.raw/reports/anthropic-threat-intelligence-report-2025-08.pdf`.

## Key claim

Model-vendor abuse telemetry is an evidence class of its own. A vendor sees the operation from inside the tooling the attacker used, which means it can describe campaigns that produced no breach notification, no incident-response engagement, and no victim disclosure. The August 2025 edition uses that vantage to argue three positions that the wiki's incident record had only partial support for: AI systems are executing operations rather than advising on them, actors without the underlying skills are running operations that previously required them, and AI now appears at every stage of a fraud supply chain rather than at one step of it.[^tir-exec]

## Series and editions

Anthropic publishes these as a continuing series and states the intent to keep doing so.[^tir-exec] The wiki tracks four artifacts in the line, of which the first two are the same reporting cadence and the third and fourth are its analytic products.

| Edition | Date | Contents | Wiki page |
|---|---|---|---|
| Threat Intelligence Report: August 2025 | Aug 2025 | Nine case studies: extortion, DPRK employment fraud, ransomware-as-a-service, a nine-month PRC-aligned campaign, two disrupted operations, four fraud-ecosystem cases | this page |
| GTG-1002 disclosure | Nov 2025 | The first reported AI-orchestrated espionage campaign, published as a standalone report rather than a series edition | [[gtg-1002-ai-orchestrated-espionage\|GTG-1002: AI-Orchestrated Espionage Campaign]] |
| LLM ATT&CK Navigator | Jun 2026 | A year of banned-account telemetry mapped to MITRE ATT&CK, plus the ARiES scoring methodology | [[llm-attack-navigator\|LLM ATT&CK Navigator]] |
| Verizon DBIR 2026 contribution | 2026 | Eleven months of the Navigator dataset, published inside a third-party breach report | [[verizon-dbir-2026\|Verizon DBIR 2026]] |

The August 2025 edition and the Navigator draw on overlapping populations: the Navigator's window opens in March 2025, five months before the report, and both rest on accounts banned under the same Usage Policy enforcement pipeline.

## Case studies in the August 2025 edition

Two cases have their own pages because they establish positions no other sourced incident in the wiki carries.

- [[gtg-2002-vibe-hacking-extortion|Vibe-Hacking Extortion Campaign]]: one actor, Claude Code as the on-keyboard operator, at least 17 organizations in a month.
- [[gtg-5004-no-code-ransomware|No-Code Ransomware Operation]]: a working ransomware-as-a-service business built by an actor Anthropic assesses as unable to implement its components.

The remaining seven are summarized here.

| Case | Actor | Model's role | Anchoring figure |
|---|---|---|---|
| DPRK remote-worker fraud | North Korean IT operatives | Persona construction, interview coaching, and the delivered engineering work itself | 61% of observed usage was frontend development, 26% scripting, 10% interview preparation, 3% backend; roughly 80% of usage was consistent with active employment[^tir-dprk] |
| PRC-aligned campaign against Vietnamese infrastructure | Chinese-language actor, characteristics consistent with APT operations | Python scanners against Vietnamese IP ranges, upload fuzzing, WordPress exploitation, Hydra and hashcat tuning, Linux kernel privilege escalation, proxy chains | Claude used across 12 of 14 MITRE ATT&CK tactics over a nine-month campaign[^tir-prc] |
| Contagious Interview account creation | DPRK, tracked as Famous Chollima / DEV#POPPER / UNC5342 | Nothing; the accounts were banned before issuing a prompt | Two of four accounts created 2024-10-22 auto-banned; the actor abandoned the other two[^tir-ci] |
| Russian-language malware development | Windows-internals-capable developer | Hell's Gate syscall resolution, Early Bird process injection, Telegram command and control, sandbox detection | Samples appeared on VirusTotal within two hours of generation, submitted from Russia, the UK, and Ukraine[^tir-russian] |
| Stealer-log profiling over MCP | Forum actor on `xss[.]is` | Categorized stolen browser telemetry into behavioural victim profiles through an MCP integration | Demonstrated publicly on a Russian-language forum as a capability offering[^tir-fraud] |
| Carding store | Spanish-language operator | Multi-API validation with rotation across three card-checking services, failover, request throttling, batch scheduling | Invite-only service operating at resale scale[^tir-fraud] |
| Romance-scam bot | Chinese-language operator | Emotional-register response generation, marketed with Claude as the "high EQ model" | Over 10,000 monthly users across US, Japanese, and Korean targeting[^tir-fraud] |

A ninth case, synthetic-identity services, is listed in the report but its body text repeats the carding-store section verbatim, so the edition carries no usable detail on it.

## Significance

**Vendor telemetry closes cases that victim-side reporting never opens.** Six of the nine case studies produced no public victim disclosure the wiki can locate, and two of them, the auto-disrupted Contagious Interview accounts and the abandoned targets listed in the GTG-2002 session summaries, describe operations that were stopped before they reached a victim at all. An incident record assembled only from breach notifications and vendor IR write-ups systematically misses this class, which matters for any wiki claim of the form "no sourced case exists".

**The reports extend the operator-autonomy spectrum at its low end.** The wiki's spectrum runs from an operator who sets the objective and relays results between separate agent instances ([[gtg-1002-ai-orchestrated-espionage|GTG-1002]]), through an operator who sets the objective while sub-agents coordinate among themselves ([[taiwan-ai-agent-government-intrusion|Taiwan]]), to no operator at all ([[openai-hugging-face-agent-incident|OpenAI–Hugging Face]]). GTG-2002 sits below all three: the operator stayed on the keyboard throughout, which is the least autonomous configuration the wiki records. It is still a change, because it moves a different variable: the skill the operator had to bring. That variable is the subject of [[capability-floor-collapse|Capability Floor Collapse]].

**Disruption is described, and its limits are described with it.** Every case study ends with a mitigation paragraph naming the specific control built in response: a tailored classifier for the extortion pattern, malware upload-and-generation detection for the ransomware case, improved indicator correlation for the employment fraud.[^tir-mitigations] The pattern is per-case classifier development after investigation, which bounds how far the disclosures support a claim of general prevention.

## Limits

- **Single-vendor view.** The report states that the patterns likely hold across frontier models but presents evidence only from Claude.[^tir-exec] Nothing here measures how much of each operation ran on other models.
- **Redacted throughout.** Prompt excerpts are labelled as simulated reconstructions, victims are described by sector rather than named, and dollar amounts inside quoted extortion material are bracketed. The figures this wiki cites are the ones that survive redaction: the 17 organizations, the ransom band, the pricing tiers.
- **No denominator.** The report gives no base rate: it does not say how many accounts were reviewed, how many were banned, or what share of platform traffic the cases represent. The [[llm-attack-navigator|LLM ATT&CK Navigator]] supplies that denominator for a later window, at the cost of dropping the case narratives.
- **A production defect in the primary.** The synthetic-identity section duplicates the carding-store text, which is a reminder that a vendor PDF is a published artifact and not a dataset.

## Relations

- **Supports** [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]] — supplies the capability-transfer axis the page's autonomy spectrum does not measure.
- **Supports** [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes]] — the DPRK employment-fraud case is a sourced instance of the AI-aware insider (Class 1) arriving through hiring rather than through an existing employee.
- **Tempers** [[verizon-dbir-2026|Verizon DBIR 2026]] — Anthropic contributed eleven months of the successor dataset to that report, so the DBIR's GenAI section and Anthropic's own publications are not independent observations of the same population.[^nav-dbir]

## See also

- [[llm-attack-navigator|LLM ATT&CK Navigator]] — the quantitative successor: 832 accounts, ATT&CK mapping, and the ARiES score
- [[capability-floor-collapse|Capability Floor Collapse]] — the axis these case studies establish
- [[anthropic|Anthropic]] — publisher
- [[gtg-1002-ai-orchestrated-espionage|GTG-1002: AI-Orchestrated Espionage Campaign]] — the later, higher-autonomy case from the same team

## Notes

[^tir-exec]: Anthropic, [*Threat Intelligence Report: August 2025*](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf), executive summary, p. 3. The four stated findings are weaponization of agentic systems, lowered barriers to sophisticated cybercrime, AI embedded throughout criminal operations, and AI across all stages of fraud. The cross-model claim: "While specific to Claude, the case studies presented below likely reflect consistent patterns of behaviour across all frontier AI models."
[^tir-dprk]: Ibid., "Remote worker fraud", pp. 11–14. Usage split and the ~80%-consistent-with-employment figure are Anthropic's own telemetry; the revenue scale is attributed to FBI assessments, not measured by Anthropic.
[^tir-prc]: Ibid., "Chinese threat actor leveraging Claude across nearly all MITRE ATT&CK tactics", p. 18. Targets described as Vietnamese telecommunications providers, government databases, and agricultural management systems.
[^tir-ci]: Ibid., "Auto-disruption of a North Korean malware distribution campaign", p. 19. The 140+ victim figure is attributed to external security research, not to Anthropic telemetry.
[^tir-russian]: Ibid., "No-code malware development campaign", p. 20. Discovered through Clio, Anthropic's automated privacy-preserving analysis tool.
[^tir-fraud]: Ibid., "AI-enhanced fraud", pp. 21–26.
[^tir-mitigations]: Ibid., mitigation paragraphs at pp. 10, 14, and 17.
[^nav-dbir]: Kyla Guru, Alex Moix, and Jacob Klein, [*Mapping AI-enabled cyber threats: Insights from the LLM ATT&CK Navigator*](https://red.anthropic.com/2026/attack-navigator/), Anthropic Frontier Red Team, 2026-06-03, footnote 1: "For the DBIR, we provided analysis of 11 months of threat actor data, and rounded this out to 12 months for this report."
