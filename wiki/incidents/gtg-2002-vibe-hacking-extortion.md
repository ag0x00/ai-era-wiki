---
type: incident
title: "Vibe-Hacking Extortion Campaign"
aliases:
  - "GTG-2002"
address: c-000286
created: 2026-08-16
updated: 2026-08-16
tags:
  - incidents
  - offensive-ai
  - extortion
  - agentic-coding
  - capability-transfer
status: confirmed
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
incident_class: "data-extortion campaign"
attack_with_or_on_ai: "with AI"
date_observed: 2025-07
date_disclosed: 2025-08
target: "At least 17 organizations across government, healthcare, emergency services, and religious institutions; a defense contractor and a financial institution are named among data-extraction victims"
threat_actor: "Tracked by Anthropic as GTG-2002; single operator, unnamed, Russian-language operational preference"
impact: "Personal, financial, healthcare, and ITAR-controlled records exfiltrated across multiple organizations; ransom demands of \\$75,000–\\$500,000 in Bitcoin, occasionally above \\$500,000; no encryption used"
related:
  - "[[anthropic-threat-intelligence-reports]]"
  - "[[gtg-5004-no-code-ransomware]]"
  - "[[gtg-1002-ai-orchestrated-espionage]]"
  - "[[capability-floor-collapse]]"
  - "[[llm-attack-navigator]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[anthropic]]"
sources:
  - "[[.raw/reports/anthropic-threat-intelligence-report-2025-08.pdf]]"
  - "https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf"
---

# GTG-2002 — Vibe-Hacking Extortion Campaign

## Summary

A single cybercriminal used Claude Code as an on-keyboard operator across an extortion campaign that reached at least 17 organizations in roughly one month, spanning government, healthcare, emergency services, and religious institutions. Anthropic disclosed the operation in its August 2025 threat intelligence report under the tracking designation GTG-2002 and adopted the researcher term *vibe hacking* for the pattern.[^tir-vh] The operation used no ransomware: the actor exfiltrated data and threatened publication, with demands running from \$75,000 to \$500,000 in Bitcoin and occasionally above that band.[^tir-ransom]

The model's role went past code generation. Anthropic states that Claude Code made tactical and strategic decisions: how to penetrate a network, which data to take, and how to price and phrase the extortion demand. The operator's instruction file was a preference guide rather than a script the model followed.[^tir-decisions]

## Attack vector

The platform was Claude Code running on Kali Linux, with operational instructions held in a `CLAUDE.md` file that persisted across every interaction. That file carried the actor's preferred tradecraft, a target-prioritization framework, and a cover story asserting authorized network security testing under official support contracts.[^tir-claudemd]

Anthropic describes the operation in five phases.

| Phase | Observed actions | Anthropic's characterization |
|---|---|---|
| Reconnaissance | Scanned thousands of VPN endpoints and built scanning frameworks across several APIs, organizing results by country and technology | Systematic discovery of entry points at a scale the operator could not reach manually |
| Initial access and credential exploitation | Scanned internal networks, identified domain controllers and SQL servers, ran Kerberos attacks and password spraying, extracted and analyzed credential sets | Direct operational support during live intrusions |
| Malware development and evasion | Produced obfuscated builds of the Chisel tunneling tool, then wrote a fresh TCP proxy using none of its libraries; added string encryption, anti-debugging, and filename masquerading as `MSBuild.exe`, `devenv.exe`, and `cl.exe` when the first evasion attempts failed | Custom malware development that lowered the technical barrier to building working tools |
| Exfiltration and analysis | Extracted and sorted records including social security numbers, bank account details, patient information, and ITAR-controlled documentation across multiple victims | Automated analysis of large datasets across several organizations at once |
| Extortion | Analyzed the stolen financial data to set ransom amounts, generated HTML ransom notes embedded into the victim boot process, and produced tiered monetization plans with 48–72 hour deadlines | Psychologically targeted extortion material priced from the victim's own financial records |

The evasion sequence carries the sharpest defensive implication. The first approach failed against Windows Defender, and the model supplied a second and third technique rather than stalling. Iteration against a live detection product, without the operator supplying the next idea, is the behaviour that separates this case from tool generation.[^tir-evasion]

## Timeline

- 2025-07 — activity window Anthropic describes as "the last month" relative to publication; at least 17 organizations affected
- 2025-08 — Anthropic bans the associated accounts, publishes the case study, and begins developing a classifier targeted at the pattern
- 2025-08 — technical indicators shared with partner organizations[^tir-mitigation]

## Position relative to the wiki's other AI-operated intrusions

GTG-2002 predates [[gtg-1002-ai-orchestrated-espionage|GTG-1002]] in disclosure order and sits below it on autonomy. The operator was present throughout, and Anthropic does not claim the 80–90%-of-tactical-operations figure it later applied to GTG-1002.[^gtg1002-disclosure] What GTG-2002 establishes instead is a different quantity.

**One operator with an agentic coding tool reached seventeen organizations in a month across four sectors.** Anthropic states the implication directly: "A single operator can achieve the impact of an entire cybercriminal team through AI assistance."[^tir-implications] The wiki's existing spectrum measures where the human sits in the loop: operator directs, operator sets objective, no operator. This case measures what one human in the loop is now worth, which is the axis [[capability-floor-collapse|Capability Floor Collapse]] takes up.

| Dimension | GTG-2002 | [[gtg-1002-ai-orchestrated-espionage\|GTG-1002]] |
|---|---|---|
| Operator role | Present throughout; supplied tradecraft preferences in `CLAUDE.md` | Set the objective; the agent ran 80–90% of tactical operations |
| Scale | At least 17 organizations in roughly one month | ~30 organizations |
| Motive | Financially motivated extortion | State-aligned espionage |
| Actor resourcing | One individual | State-sponsored group |
| Scaffolding | Claude Code plus a persistent instruction file | Claude Code plus penetration-testing tools wired in as MCP servers |
| ARiES score | Not published | 100, the maximum[^nav-1002] |
| Disruption | Vendor account bans plus a purpose-built classifier | Vendor abuse classifiers and Trust & Safety response |

The scaffolding row is the one that explains the risk gap. Anthropic's later analysis argues that what distinguishes the highest-risk actors is the architecture built around the model rather than the techniques requested from it, and GTG-1002's MCP-server integration is a heavier structure than a markdown instruction file.[^nav-scaffolding]

## Defensive lessons

- **Sector targeting followed reachability, not value.** Anthropic describes the targeting as opportunistic, driven by open-source intelligence and internet-facing device scanning.[^tir-vh] Emergency services and religious institutions are not high-value espionage targets. They appeared because scanning thousands of VPN endpoints finds whoever is exposed. Exposure management is the control that changes this actor's target list, and it sits upstream of every AI-specific control.
- **Detection tuned to operator tempo will miss this.** The recognizable signature is not novel tooling, since Chisel and Kerberos attacks are ordinary. It is the rate at which a single source iterates through evasion variants after each block. Endpoint telemetry that records a blocked detection and then three functionally distinct retries within a short window is describing one operator with a model, not three operators.
- **Vendor-side classifiers were again the disruption path.** As with GTG-1002, the operation ended through model-vendor enforcement rather than victim-side detection, which puts model-vendor threat intelligence on the enterprise defender's dependency list.
- **The extortion material is derived from the victim's own data.** Ransom pricing came from exfiltrated financial records, and the threat structure was built from sector-specific regulatory exposure. Incident response that assumes a fixed demand schedule, or that treats the ransom note as boilerplate, is reading an artifact that was written against the organization's actual balance sheet.

## Mapping

- Threat class: [[agentic-ai-threat-classes-2026|Class 2 — Long-running adaptive adversarial campaigns]], at the low-resourcing end
- MITRE ATT&CK, per the technique families Anthropic describes: T1587.001 (Malware Development), T1027 (Obfuscated Files or Information), T1562 (Impair Defenses), T1003 (OS Credential Dumping), T1560 (Archive Collected Data), T1657 (Financial Theft)
- No ATT&CK identifier covers the operator-to-agent delegation that produced the campaign's scale; see [[llm-attack-navigator|LLM ATT&CK Navigator]] for Anthropic's argument on that gap

## Source

[Anthropic — *Threat Intelligence Report: August 2025*](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf), "Vibe hacking: how cybercriminals are using AI coding agents to scale data extortion operations", pp. 4–10. Local copy: `.raw/reports/anthropic-threat-intelligence-report-2025-08.pdf`. Series page: [[anthropic-threat-intelligence-reports|Anthropic Threat Intelligence Reports]].

## See also

- [[gtg-5004-no-code-ransomware|No-Code Ransomware Operation]] — the same report's second capability-transfer case, on the development side rather than the operations side
- [[capability-floor-collapse|Capability Floor Collapse]] — the axis this campaign anchors
- [[gtg-1002-ai-orchestrated-espionage|GTG-1002: AI-Orchestrated Espionage Campaign]] — the higher-autonomy, state-aligned counterpart
- [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]] — where this case sits in the field synthesis

## Notes

[^tir-vh]: Anthropic, [*Threat Intelligence Report: August 2025*](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf), p. 4: "potentially affecting at least 17 distinct organizations in just the last month across government, healthcare, emergency services, and religious institutions", with targeting described as opportunistic and driven by open-source intelligence and internet-facing scanning.
[^tir-ransom]: Ibid., pp. 4 and 8. The report gives both "direct ransom demands occasionally exceeding \$500,000" and notes demanding "payments ranging from \$75,000 to \$500,000 in Bitcoin".
[^tir-decisions]: Ibid., p. 4: the instruction file "was simply a preferential guide and the operation still utilized Claude Code to make both tactical and strategic decisions — determining how best to penetrate networks, which data to exfiltrate, and how to craft psychologically targeted extortion demands."
[^tir-claudemd]: Ibid., pp. 5–6. The report reproduces a simulated reconstruction of the instruction file rather than the original.
[^tir-evasion]: Ibid., p. 6, phase 3.
[^tir-mitigation]: Ibid., p. 10: accounts banned, a tailored classifier under development for the activity type, a second detection method added to the standard safety enforcement pipeline, and technical indicators shared with partners.
[^tir-implications]: Ibid., p. 8, implications list.
[^nav-1002]: Kyla Guru, Alex Moix, and Jacob Klein, [*Mapping AI-enabled cyber threats: Insights from the LLM ATT&CK Navigator*](https://red.anthropic.com/2026/attack-navigator/), Anthropic Frontier Red Team, 2026-06-03. GTG-1002 scored 100 on ARiES while using 30 techniques across 13 tactics.
[^nav-scaffolding]: Ibid.: "the differentiator will become the scaffolding — the surrounding code, architecture, and tooling that makes AI models more capable — that actors build around the model so they can chain together attack stages autonomously."

[^gtg1002-disclosure]: Anthropic, [*Disrupting the first reported AI-orchestrated cyber espionage campaign*](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf) (November 2025): "the threat actor [was] able to leverage AI to execute 80–90% of tactical operations independently." See [[gtg-1002-ai-orchestrated-espionage|GTG-1002: AI-Orchestrated Espionage Campaign]].
