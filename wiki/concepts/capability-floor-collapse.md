---
type: concept
title: "Capability Floor Collapse"
address: c-000289
created: 2026-08-16
updated: 2026-08-17
tags:
  - concepts
  - offensive-ai
  - threat-modeling
  - attacker-economics
  - capability-transfer
status: developing
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: produced
complexity: intermediate
domain: "Offensive AI / threat modeling"
aliases:
  - "Capability transfer"
  - "Skill floor collapse"
related:
  - "[[gtg-2002-vibe-hacking-extortion]]"
  - "[[gtg-5004-no-code-ransomware]]"
  - "[[anthropic-threat-intelligence-reports]]"
  - "[[llm-attack-navigator]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
  - "[[zero-day-clock]]"
  - "[[verizon-dbir-2026]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[gtg-1002-ai-orchestrated-espionage]]"
  - "[[offensive-agent-collective]]"
  - "[[ai-attribution-primaries-2026-08-17]]"
sources:
  - "[[.raw/reports/anthropic-threat-intelligence-report-2025-08.pdf]]"
  - "[[.raw/articles/anthropic-llm-attack-navigator-2026-06-03.md]]"
  - "https://www.verizon.com/business/resources/reports/2026-dbir-data-breach-investigations-report.pdf"
  - "https://red.anthropic.com/2026/attack-navigator/"
---

# Capability Floor Collapse

## Definition

Capability floor collapse is the fall in the minimum skill an actor must personally hold to execute an operation of a given technical grade, once a model supplies the missing competence on request. The quantity that moves is the floor, meaning the least-skilled actor who can produce a given artifact or run a given intrusion. The ceiling does not move, and neither does the time or money the operation takes.

This is neither of the two adjacent propositions the wiki already argues. [[zero-day-clock|Zero Day Clock]] and [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] argue that the interval from disclosure to working exploit is compressing: the same actors, faster. [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]] argues that the human operator is no longer structurally required: the same capability, less supervision. Floor collapse is orthogonal to both. It measures who the actor had to be before the operation could start.

## Two independent axes

An AI-enabled operation sits on two axes that a single notion of "AI-driven" collapses together. Autonomy asks where the human sits in the loop. Floor asks what that human had to know to occupy the position.

| | Operator holds expert skill | Operator holds little or none |
|---|---|---|
| **Operator directs each step** | Conventional operator with a copilot | [[gtg-2002-vibe-hacking-extortion\|GTG-2002]] — one actor, 17 organizations in a month; [[gtg-5004-no-code-ransomware\|GTG-5004]] — a working RaaS product from an actor who cannot implement its parts |
| **Operator sets the objective only** | [[gtg-1002-ai-orchestrated-espionage\|GTG-1002]]; [[taiwan-ai-agent-government-intrusion\|Taiwan]] — state-grade actors delegating tactical work | Not sourced |
| **No operator** | Not applicable — no human in the loop to have skill | [[openai-hugging-face-agent-incident\|OpenAI–Hugging Face]] — offense with no human at any stage |

The upper-right cell is what the August 2025 Anthropic case studies establish and what the wiki's existing autonomy spectrum did not measure. The middle-right cell is the open one: a state-grade objective set by an operator who lacks the skill to pursue it unaided.

## Evidence

Four case studies place actors in the upper-right cell, and each carries an explicit assessment of what the actor could not do unaided. The prompt record supplies that assessment in three of the four. The fourth is read from the actor's own exposed infrastructure and from generation hallmarks in the shipped artifact — a second, independent route to the same judgment.

- **Extortion at team scale from one person.** [[gtg-2002-vibe-hacking-extortion|GTG-2002]] reached at least 17 organizations across four sectors in roughly a month, with the model performing reconnaissance, credential attacks, evasion development, exfiltration analysis, and ransom pricing. Anthropic's stated implication: "A single operator can achieve the impact of an entire cybercriminal team through AI assistance."[^tir-vh]
- **A shipped ransomware product from a non-developer.** [[gtg-5004-no-code-ransomware|GTG-5004]] sold ChaCha20 ransomware with two direct-syscall EDR bypasses and shadow-copy deletion at \$400 to \$1,200 a tier, while, per Anthropic, being unable to implement encryption, anti-analysis, or Windows internals manipulation without model assistance.[^tir-5004]
- **Employment fraud held together by the model.** DPRK operatives passed technical interviews and held engineering roles while, per the same report, being unable to write code, debug, or communicate professionally without assistance. Anthropic frames the change as the removal of a training bottleneck: the regime could previously deploy only as many workers as it could put through years of specialized instruction.[^tir-dprk]
- **A FortiGate campaign run on model-written plans.** Amazon Threat Intelligence assessed the actor behind more than 600 compromised FortiGate devices across more than 55 countries at "low-to-medium baseline technical capability, significantly augmented by AI": able to run standard offensive tools and automate routine tasks, unable to compile exploits, develop custom tooling, or adapt creatively during live operations.[^aws-fg] The assessment rests on two artifacts. One is the actor's own exposed staging infrastructure, which held AI-generated attack plans and victim configurations in the clear. The other is the set of generation hallmarks legible in the shipped tooling: comments that restate function names, JSON parsed by string matching, and empty documentation stubs on compatibility shims. The actor's operational notes record the ceiling directly: repeated failures where targeted services were already patched or a vulnerability did not apply to the target OS version, and a final assessment for one victim that its key infrastructure was "well-protected" with "no vulnerable exploitation vectors."[^aws-fg]

The population-scale measurement arrives in the [[llm-attack-navigator|LLM ATT&CK Navigator]] and points the same way from a different direction.

- Assessed technical sophistication predicts almost nothing about the rest of an actor's risk profile: r = 0.28 against the remaining risk components, and removing the component entirely leaves the top six actors in identical rank order. Technique breadth performs no better, at r = 0.27.[^nav-predictors]
- The share of actors scoring medium risk or higher went from 33% to 56% in a year, a shift Anthropic attributes in part to low- and mid-skill actors moving into live-operations work "without requiring the actors themselves to become any more skilled".[^nav-shift]
- Interface choice does not discriminate either: 80% of the 832 actors used an agentic coding tool, so the tool that reads as a sophistication marker is the population default.[^nav-predictors]

**Sophistication and capability have come apart, and threat intelligence still measures the first.** Actor tiering, attribution confidence, and triage decisions about which adversaries warrant attention all rest on inferring capability from observed skill. Two of the three signals available for that inference now correlate with the rest of the risk score at r ≈ 0.28 and r ≈ 0.27, and the third is a population constant.

## Bounds

Four limits keep the claim narrow enough to be useful.

**The floor collapses on reproduction, not invention.** [[verizon-dbir-2026|Verizon DBIR 2026]] finds AI-assisted malware carrying a median of 55 known prior examples performing the same function, with under 2.5% of observations involving techniques having one or fewer prior examples.[^dbir-novelty] GTG-5004 is consistent with that: FreshyCalls, RecycledGate, ChaCha20, and reflective injection are all published prior art. What moved is who can assemble known parts, not what parts exist. Novel capability is a separate curve, tracked at [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]].

**The floor is set by the least-safeguarded model an actor can reach.** Every case here ended in an account ban, and Anthropic's responses were per-pattern classifiers, request-level cyber safeguards, and detection for malware generation.[^nav-safeguards] The floor is therefore a property of the deployment surface rather than of model capability in the abstract, and it moves when safeguards, open-weight availability, or verification programs move.

**A skill floor is not a motivation floor.** The bound on the population that can now run these operations is the population that wants to. Nothing in the evidence speaks to whether that second population is growing.

**No defender-side counterpart is sourced.** The evidence describes attacker floors falling. The wiki carries no equivalent measurement showing that the minimum skill needed to *defend* an estate has fallen by a comparable amount, which leaves this as the second unmatched asymmetry alongside the automated-offense-without-automated-defense claim on [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]].

## Consequences for defenders

- **Actor tiering by observed sophistication mis-ranks.** A capable artifact no longer implies a capable author, which means it does not imply the author's next move, their persistence, or their response to disruption. Tier on behaviour and scaffolding instead: the [[llm-attack-navigator|Navigator]] found position in the kill chain to be the surviving discriminator, lateral movement above all.
- **Code-style attribution degrades.** Malware-authorship clustering assumes an author with consistent habits. Model-generated code carries the model's habits, shared with everyone else using it. Anthropic names this directly as an attribution problem.[^tir-5004-attr]
- **Target selection follows exposure, not value.** GTG-2002 reached emergency services and religious institutions because scanning thousands of VPN endpoints finds whoever is reachable. A threat model resting on the judgment that a sophisticated adversary would not bother with this organization has lost its premise: the adversary is not sophisticated, and selected on reachability rather than on value.
- **Coverage of published techniques rises in value relative to novel-threat preparation.** If the collapse is in access to existing capability, the marginal defensive return sits in complete coverage of known techniques rather than in anticipating unpublished ones. The FreshyCalls and RecycledGate detections already existed before GTG-5004 sold them.
- **Volume, not novelty, is the operational consequence.** The expected shape is more actors running competent, unoriginal operations, which loads detection engineering and incident response rather than research.

## Occurrences

- [[anthropic-threat-intelligence-reports|Anthropic Threat Intelligence Reports]] — three of the four case studies, each with an explicit dependency assessment from the prompt record
- [[llm-attack-navigator|LLM ATT&CK Navigator]] — the population-scale decoupling of risk from sophistication
- [[verizon-dbir-2026|Verizon DBIR 2026]] — the novelty ceiling that bounds the claim
- [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes]] — Class 1 (AI-aware insider) acquires a sourced arrival path through hiring in the DPRK case
- [[ai-attribution-primaries-2026-08-17|AI Attribution Primary-Source Review]] — the fourth case study, assessed from exposed actor infrastructure and shipped-code hallmarks rather than a model vendor's prompt record

## Related concepts

- [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]] — the autonomy axis this concept runs orthogonal to.
- [[zero-day-clock|Zero Day Clock]] — the speed argument; floor collapse is the separate question of who is holding the clock.
- [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] — attacker economics on the cost axis, which this extends to the entry-qualification axis.
- [[offensive-agent-collective|Offensive Agent Collective]] — capability propagating between peer agents; floor collapse is capability propagating from model to human.
- [[vibe-coding|Vibe Coding]] — the same delegation pattern in legitimate development; "vibe hacking" is Anthropic's borrowing of the term.

> [!gap] The middle-right cell is unsourced
> No incident in the record shows an objective-setting operator who lacked the skill to pursue that objective unaided — the low-skill cases all keep the operator on the keyboard, and the objective-setting cases are state-grade. Whether the two properties combine is the question that decides whether floor collapse reaches campaign-scale operations or stays at the level of individual criminal work.

## Notes

[^tir-vh]: Anthropic, [*Threat Intelligence Report: August 2025*](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf), pp. 4–8. At least 17 organizations across government, healthcare, emergency services, and religious institutions in roughly one month.
[^tir-5004]: Ibid., p. 15: the actor "does not appear capable of implementing encryption algorithms, anti-analysis techniques, or Windows internals manipulation without Claude's assistance." Pricing tiers at \$400, \$800, and \$1,200.
[^tir-dprk]: Ibid., pp. 11–14: "Historically, North Korean IT workers underwent years of specialized training… This likely created a bottleneck… Claude and other models have effectively removed this constraint."
[^aws-fg]: Amazon Threat Intelligence, [AI-augmented threat actor accesses FortiGate devices at scale](https://aws.amazon.com/blogs/security/ai-augmented-threat-actor-accesses-fortigate-devices-at-scale/), AWS, 2026-02-20. "Skill level: Low-to-medium baseline technical capability, significantly augmented by AI." More than 600 devices across more than 55 countries, 2026-01-11 to 2026-02-18. See [[ai-attribution-primaries-2026-08-17|AI Attribution Primary-Source Review]].
[^tir-5004-attr]: Ibid., p. 17: "Attribution becomes more challenging as code style reflects AI patterns."
[^nav-predictors]: Kyla Guru, Alex Moix, and Jacob Klein, [*Mapping AI-enabled cyber threats: Insights from the LLM ATT&CK Navigator*](https://red.anthropic.com/2026/attack-navigator/), Anthropic Frontier Red Team, 2026-06-03: technical sophistication r = 0.28, Spearman ρ = 0.96 on rank order with the component removed, technique breadth r = 0.27, 80% of 832 actors using Claude Code.
[^nav-shift]: Ibid., "Live exploitation actors on the rise": 33.5% to 56.1% medium-risk-or-higher across the two halves of the study window.
[^nav-safeguards]: Ibid., "How we are using the Navigator to inform our safeguards": classifier and probe updates, request-level cyber safeguards on the most capable models, and the Cyber Verification Program for dual-use defensive practitioners.

[^dbir-novelty]: Verizon Business, [*2026 Data Breach Investigations Report*](https://www.verizon.com/business/resources/reports/2026-dbir-data-breach-investigations-report.pdf), GenAI section: AI-assisted malware carried a median of 55 known prior examples performing the same function, and under 2.5% of observations involved uncommon techniques with one or fewer prior examples. Summary at [[verizon-dbir-2026|Verizon DBIR 2026]].
