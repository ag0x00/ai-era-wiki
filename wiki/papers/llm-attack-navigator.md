---
type: paper
title: "LLM ATT&CK Navigator"
address: c-000288
created: 2026-08-16
updated: 2026-08-16
tags:
  - papers
  - threat-intel
  - mitre
  - attack-taxonomy
  - risk-scoring
  - ai-in-sec-offense
status: summarized
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
year: 2026
authors:
  - "Kyla Guru"
  - "Alex Moix"
  - "Jacob Klein"
venue: "Anthropic Frontier Red Team"
source_url: "https://red.anthropic.com/2026/attack-navigator/"
archived_copy: ".raw/articles/anthropic-llm-attack-navigator-2026-06-03.md"
key_claim: "Across 832 banned accounts mapped to MITRE ATT&CK, none of the signals threat intelligence uses to rank actors predicts how much a model contributed to the operation; the predictor is where in the kill chain the model is applied, and the behaviours that mark the highest-risk actors have no ATT&CK identifiers."
methodology: "832 accounts banned for cyber-related Usage Policy violations between March 2025 and March 2026, selected from a larger banned population for having enough investigative detail to map. Observed activity summarized per account, TTPs extracted from those summaries and mapped to MITRE ATT&CK V18, producing 13,873 observations across 482 unique techniques and all 14 tactics. Each actor scored 0–100 on the AI Risk Enablement Score (ARiES). Vendor-internal telemetry; no external replication."
related:
  - "[[mitre-atlas]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[anthropic-threat-intelligence-reports]]"
  - "[[gtg-1002-ai-orchestrated-espionage]]"
  - "[[gtg-2002-vibe-hacking-extortion]]"
  - "[[gtg-5004-no-code-ransomware]]"
  - "[[capability-floor-collapse]]"
  - "[[verizon-dbir-2026]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[mitre]]"
  - "[[anthropic]]"
sources:
  - "[[.raw/articles/anthropic-llm-attack-navigator-2026-06-03.md]]"
  - "[[.raw/articles/anthropic-ai-enabled-cyber-threats-mitre-attack-2026-06-03.md]]"
  - "https://red.anthropic.com/2026/attack-navigator/"
  - "https://www.anthropic.com/news/AI-enabled-cyber-threats-mitre-attack"
---

# LLM ATT&CK Navigator

**Source:** Kyla Guru, Alex Moix, and Jacob Klein, [*Mapping AI-enabled cyber threats: Insights from the LLM ATT&CK Navigator*](https://red.anthropic.com/2026/attack-navigator/), Anthropic Frontier Red Team (2026-06-03), with a shorter policy-facing companion at [*What we learned mapping a year's worth of AI-enabled cyber threats*](https://www.anthropic.com/news/AI-enabled-cyber-threats-mitre-attack). Local copies in `.raw/articles/`.

## Key claim

The signals threat intelligence uses to rank an adversary do not survive contact with AI-enabled operations. Across 832 banned accounts, assessed technical sophistication correlates with the rest of the risk score at r = 0.28, technique breadth at r = 0.27, and interface choice not at all, since 80% of the population used Claude Code and the tool that looks like a sophistication marker is the population default.[^nav-predictors] What does discriminate is position in the kill chain: the 54 actors who used a model for lateral movement average 56.4 on Anthropic's risk score against a population mean of 46.8.[^nav-lateral] And the behaviours that separate the top of the distribution have no ATT&CK identifiers at all: autonomous kill-chain orchestration, real-time pivot decisions, execution without human intervention.[^nav-gap]

## Dataset

| Property | Value |
|---|---|
| Accounts | 832, banned for cyber-related Usage Policy violations |
| Window | March 2025 – March 2026 |
| Framework version | MITRE ATT&CK V18 |
| Observations | 13,873 actions |
| Techniques | 482 unique sub-techniques, across all 14 tactics |
| Matrix split | Enterprise techniques account for 99% of observations |
| Selection | A subset of the banned population, chosen for sufficient investigative detail to map |

The selection rule is the main sampling caveat and Anthropic states it plainly: these are the cases where the investigation produced enough detail to extract TTPs, which biases toward operations that were investigated in depth rather than auto-banned.[^nav-dataset]

## The ARiES score

Anthropic scores each actor 0–100 on the AI Risk Enablement Score, composed additively from three dimensions.

| Dimension | Range | Inputs |
|---|---|---|
| Threat | 0–35 | Clarity of intent, technical sophistication, threat-intelligence signals, evasion of detection. Technical sophistication is graded by a model from the actor's prompts and tool usage. |
| Vulnerability | 0–35 | The model's capacity to enable the requested harm, plus the risk profile of the interface. Programmatic and agentic-coding interfaces score highest. |
| Impact | 0–30 | Actual or potential real-world consequence attributable to the model's involvement, from safety classifiers and investigator assessment. |

**The addition is the design decision, and Anthropic argues for it explicitly.** The classical formulation is Threat × Vulnerability × Impact, where a zero on any factor collapses the product. That is the correct behaviour when the question is whether an attack will succeed. ARiES asks a different question: which AI-involved cases warrant defender attention. Under multiplication, an inexperienced user who accidentally produces a wormable exploit scores zero on intent and disappears; an actor with working malware and no observed deployment scores zero on impact and disappears. Both are exactly what a vendor's detection pipeline needs to surface, so the score preserves each dimension independently.[^nav-additive]

The stated cost is that ARiES is not a prediction of attack success. It measures how concerning a case is, which makes it a triage instrument rather than a risk quantification, and comparisons against multiplicative enterprise risk scores are not like-for-like.

## Findings

### Model use concentrates before the intrusion, and is drifting later

Development and evasion dominate. T1587 (Develop Capabilities) appears for 574 of 832 actors (69%), almost all of it T1587.001 (Malware Development) at 560 actors (67.3%). T1027 (Obfuscated Files or Information) reaches 64.7%, T1005 (Data from Local System) 55.9%, and T1562 (Impair Defenses) 54.9%.[^nav-prevalence] Defense evasion is the largest single tactic, present for 84.4% of actors, and Anthropic observes 32 of MITRE's 64 defense-evasion techniques.[^nav-evasion]

Late-stage tactics are rare as a share of observations: impact 2.8%, exfiltration 2.8%, privilege escalation 2.4%, lateral movement 0.7%, together 8.7%, which is less than defense evasion alone.[^nav-evasion]

Rare, and moving. Half over half, account discovery (T1087) rose 8.9% and automated exfiltration (T1020) rose 6.2%, while capability development (T1587) fell 12% and phishing (T1566) fell 8.6%.[^nav-drift] Both rising techniques presuppose the actor is already inside the network.

### The risk distribution shifted inside one year

The share of actors scoring medium risk or higher went from 33% in the first six months to 56% in the second, a cohort shift of about 22.6 percentage points.[^nav-shift] Anthropic allows that improved detection may account for part of it, and attributes the rest to low- and mid-skill actors moving into live-operations work that previously appeared only in a small high-risk cohort. The stated implication for defenders is that the shift happened "without requiring the actors themselves to become any more skilled".[^nav-shift]

### Lateral movement is the marker, and it is a decaying one

The high-risk tail concentrates on post-compromise, hands-on-keyboard techniques: T1021 (Remote Services), T1078.003 (Valid Accounts), T1003 (OS Credential Dumping), T1560 (Archive Collected Data), and T1505.003 (Web Shell), each three to five times more common in the top tier than in the population.[^nav-hirisk] Lateral movement predicts most strongly, at +10.5 points of average risk score.

Anthropic then undercuts its own indicator: those same operational techniques are where the whole population is drifting, so the marker erodes as the trend continues. The proposed replacement is scaffolding, meaning the architecture built around the model that lets it chain stages with minimal human input, which is not currently measurable from technique mappings.[^nav-scaffolding]

### GTG-1002 as the worked case for the taxonomy gap

[[gtg-1002-ai-orchestrated-espionage|GTG-1002]] scored the maximum 100 while using 30 techniques across 13 tactics, a profile comparable to dozens of medium-risk actors in the same dataset; the median actor uses 16 techniques and several low-risk actors exceed 30.[^nav-1002] The differentiator Anthropic names is the scaffolding: Claude Code on Kali Linux with open-source penetration-testing tools wired in as MCP servers, executing and reasoning rather than advising.

The report also refines the wiki's existing account of the campaign's human-machine split. The model operated autonomously through reconnaissance and internal discovery and adapted to unanticipated infrastructure, while the final exfiltration was human-directed: the download to the operator's machine ran through curl MCP tool calls. Anthropic reads that as the operator retaining the consequential decisions and delegating the operational work.[^nav-1002-split]

## Position on the taxonomy gap

Anthropic's proposal is cross-cutting categories added to ATT&CK covering agentic orchestration of a kill chain, real-time pivot decisions, AI-directed execution without human intervention, and autonomous target selection. The report states that active conversations with MITRE are under way.[^nav-mitre]

Three observations follow for the wiki's taxonomy pages.

**The proposal targets ATT&CK, not ATLAS, and the choice is correct.** These operations are attacks on conventional enterprise estates that happen to be conducted through a model. [[mitre-atlas|MITRE ATLAS]] catalogs adversarial techniques against machine-learning systems; nothing in this dataset attacks an AI system. Routing agentic-orchestration coverage into ATLAS because it involves AI would put enterprise intrusion tradecraft in the AI-security catalog, which is the boundary error the [[mitre-atlas|ATLAS page]] already draws in the other direction for the post-RCE legs of the OpenAI–Hugging Face chain.

**Every observation mapped, and that is the finding.** All 13,873 actions found a home in ATT&CK V18. The gap is not missing techniques but a missing dimension: the framework describes what was done and has no vocabulary for who or what decided to do it next. A taxonomy of actions cannot record the absence of an actor between two actions.

**Anthropic proposes cross-cutting categories rather than new techniques**, which is the same structural shape the wiki's [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] uses for its structural tests: conditions evaluated across a chain rather than entries enumerated within it. If MITRE adopts the proposal, the crosswalk gains its first externally-maintained cross-cutting axis.

## Limits

- **Vendor-internal and unreplicated.** Every figure comes from one vendor's enforcement pipeline. No external party has audited the mappings, the ARiES weights, or the selection rule.
- **Circularity partly addressed, not eliminated.** Technical sophistication is graded by a model from the actor's own prompts, then used as an ARiES input. Anthropic tests for the resulting circularity by removing the component, finding the top six actors unchanged in rank and Spearman ρ = 0.96 across all 832.[^nav-predictors] That answers whether the component drives the ranking; it does not address the model-grades-the-model dependency in the underlying label.
- **One internal inconsistency.** T1562 (Impair Defenses) is given as 54.9% in the prevalence list and 54.8% in the defense-evasion section of the same post. The difference is immaterial to any argument here; this page cites 54.9%.
- **Not independent of the DBIR.** See below.

> [!contradiction] The DBIR is not a second source for these findings
> [[verizon-dbir-2026|Verizon DBIR 2026]] is cited on several wiki pages as independent breach telemetry corroborating the AI-in-attacks picture. Anthropic states that it supplied eleven months of this same threat-actor dataset to that report.[^nav-dbir] The DBIR's GenAI section reports a median actor using AI across 15 documented techniques; the Navigator reports a median of 16 over a window shifted four months later. Treat the two as one telemetry source presented twice. The DBIR's *breach* findings — exploitation at 31% of initial access, 26% of KEV criticals patched, the 43-day median — come from the contributor breach corpus and remain independent of Anthropic.

## Relations

- **Supports** [[capability-floor-collapse|Capability Floor Collapse]] — supplies the population-scale evidence for what the August 2025 case studies show individually: risk decoupled from actor skill, and a cohort shift toward operational techniques with no corresponding skill increase.
- **Extends** [[mitre-atlas|MITRE ATLAS]] — adds an externally-argued gap in the sibling framework, and clarifies which of the two catalogs should carry agentic-orchestration coverage.
- **Extends** [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] — a candidate eighth taxonomy for the crosswalk, structured as cross-cutting categories rather than a catalog.
- **Tempers** [[verizon-dbir-2026|Verizon DBIR 2026]] — establishes shared provenance for the GenAI half of that report.

## See also

- [[anthropic-threat-intelligence-reports|Anthropic Threat Intelligence Reports]] — the case-study series this dataset generalizes
- [[gtg-1002-ai-orchestrated-espionage|GTG-1002: AI-Orchestrated Espionage Campaign]] — the maximum-score worked example
- [[mitre-atlas|MITRE ATLAS]] · [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] — the crosswalk pages this report bears on
- [[capability-floor-collapse|Capability Floor Collapse]] — the concept this dataset quantifies

## Notes

[^nav-predictors]: Guru, Moix, and Klein, [*Mapping AI-enabled cyber threats*](https://red.anthropic.com/2026/attack-navigator/), "High-risk actors and their tactics": technical sophistication r = 0.28 against the remaining risk components once removed from the composite; removing it leaves the top six actors in identical rank order, Spearman ρ = 0.96 across all 832; technique breadth r = 0.27; 80% of actors misused Claude Code.
[^nav-lateral]: Ibid.: 54 actors used lateral movement, average ARiES 56.4 against a population mean of 46.8, +10.5 points relative to actors not using it.
[^nav-gap]: Ibid., "A new era for MITRE ATT&CK".
[^nav-dataset]: Ibid., "About the dataset": 13,873 actions, 482 unique techniques, all 14 tactics, mapped against ATT&CK V18; Enterprise techniques 99% of observations.
[^nav-additive]: Ibid., "A note on the scoring formula".
[^nav-prevalence]: Ibid., "AI-assisted capability development": T1587 574/832 (69%), T1587.001 560 actors (67.3%), T1027 64.7%, T1005 55.9%, T1562 54.9%, T1055 30.3%. Only 22.5% of actors reach privilege escalation or impact stages; fewer than 12 use models for remote services.
[^nav-evasion]: Ibid., "AI-assisted evasion tactics": defense evasion present for 84.4% of actors; 32 of 64 defense-evasion techniques observed (25 enterprise, 7 mobile); late-stage tactics total 8.7% of observations.
[^nav-drift]: Ibid.: T1087 +8.9%, T1020 +6.2%, T1587 −12%, T1566 −8.6% between the first and second halves.
[^nav-shift]: Ibid., "Live exploitation actors on the rise": 33.5% to 56.1%, a 1.7× increase and a cohort shift of about 22.6 percentage points.
[^nav-hirisk]: Ibid., "High-risk actors and their tactics": T1021, T1078.003, T1003, T1560, T1505.003 each three to five times more common among the highest-risk actors than in the overall population.
[^nav-scaffolding]: Ibid., key findings: "the differentiator will become the scaffolding — the surrounding code, architecture, and tooling that makes AI models more capable."
[^nav-1002]: Ibid., "Novelty and sophistication in the age of AI agents": ARiES 100, 30 techniques across 13 tactics, median actor 16 techniques.
[^nav-1002-split]: Ibid.: "The final data extraction — downloading to the attacker's machine via curl MCP tool calls — was human-directed."
[^nav-mitre]: Ibid., "How we are using the Navigator to inform our safeguards" and "A new era for MITRE ATT&CK".
[^nav-dbir]: Ibid., footnote 1: "For the DBIR, we provided analysis of 11 months of threat actor data, and rounded this out to 12 months for this report." The DBIR figure appears at [[verizon-dbir-2026|Verizon DBIR 2026]], "GenAI in attacks".
