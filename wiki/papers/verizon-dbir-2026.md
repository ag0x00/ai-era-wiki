---
type: paper
title: "Verizon DBIR 2026"
address: c-000106
created: 2026-05-24
updated: 2026-08-21
tags:
  - papers
  - vulnops
  - exposure-management
  - threat-intel
  - dbir
status: summarized
scope_axis: [sec-against-ai, ai-in-sec-defense]
year: 2026
authors: []
venue: "Verizon Business — 2026 Data Breach Investigations Report (19th edition)"
source_url: https://www.verizon.com/business/resources/reports/2026-dbir-data-breach-investigations-report.pdf
key_claim: "Vulnerability exploitation has become the leading initial-access vector for breaches while remediation slowed — only 26% of known-exploited critical vulnerabilities were patched in 2025, down from 38%. Threat actors now use GenAI across many attack stages, though almost entirely to reproduce known techniques rather than to invent novel ones."
methodology: "2026 Verizon DBIR, drawn from more than 31,000 security incidents and more than 22,000 confirmed data breaches across 145 countries, covering October 2024 through November 2025. Figures below are read from the primary report PDF."
related:
  - "[[vulnops|VulnOps]]"
  - "[[vulnops-implementation-roadmap|VulnOps Implementation Roadmap]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]]"
  - "[[continuous-threat-exposure-management|CTEM]]"
  - "[[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]"
  - "[[llm-attack-navigator|LLM ATT&CK Navigator]]"
  - "[[anthropic-threat-intelligence-reports|Anthropic Threat Intelligence Reports]]"
  - "[[capability-floor-collapse|Capability Floor Collapse]]"
  - "[[gtg-5004-no-code-ransomware|No-Code Ransomware Operation]]"
sources:
  - "[[verizon-dbir-2026]]"
  - https://www.verizon.com/business/resources/reports/2026-dbir-data-breach-investigations-report.pdf
---

# Verizon DBIR 2026 — Attackers Outrun Remediation

**Source:** Verizon Business, *2026 Data Breach Investigations Report* (19th edition), local copy `.raw/papers/verizon-dbir-2026.pdf`. The report's own title names its three headline themes: "Breakthroughs in vulnerabilities, AI-assisted cyber attacks and social engineering." Figures below are read from the primary PDF and carry **high** confidence.

## Key Claim

Vulnerability exploitation is now the single largest initial-access vector for breaches, and remediation moved the wrong way over the same period: the share of known-exploited critical vulnerabilities fully patched fell from 38% to 26%, and the median time to full resolution rose from 32 to 43 days. Threat actors use GenAI across targeting, initial access, and malware development, but almost entirely to reproduce well-understood techniques. Fewer than 2.5% of AI-assisted malware observations involved genuinely uncommon techniques.

## Methodology

The 2026 DBIR analyzed more than 31,000 security incidents and more than 22,000 confirmed data breaches across 145 countries, covering October 2024 through November 2025, the largest breach corpus in the report's history. It is observational: it counts what contributing organizations and public disclosures reported, not a controlled study. The contributor base expanded this year with ransomware-payment and extortion-campaign data sources, which the report flags as a factor in several year-over-year shifts. Anthropic appears among the named research partners.

## Notable Findings

### Vulnerability exploitation leads initial access

- **Exploitation of vulnerabilities reached 31% of initial-access vectors, up from 20% the prior year (a 55% increase), and is now the most common vector** (Figure 10, n=20,023). This is the first DBIR in which it tops the list.
- **Credential abuse fell to 13%, down from 22% in the 2025 DBIR.** Part of the drop is methodological: the report added Pretexting to its tracked initial-access vectors this year, which absorbed some share; without that addition credential abuse would read 16%. Credentials remain pervasive — counting any point in the breach progression, credential abuse still sits on top at 39%.

### Remediation moved backward

- **Only 26% of critical vulnerabilities — defined as entries in the CISA Known Exploited Vulnerabilities (KEV) catalog — were fully remediated in 2025, down from 38% the prior year.**
- **The median time for full resolution rose to 43 days, almost two weeks longer than the prior year's 32 days.**
- In the median case, organizations had **50% more critical vulnerabilities to patch** than the year before. The report's framing: too many vulnerabilities, not enough time to patch them.

### System Intrusion and ransomware

- **The System Intrusion pattern accounts for 60% of breaches** and has been the top breach pattern since 2022; the report attributes its growth largely to ransomware. Within System Intrusion breaches, ransomware appears in 77%, and the two leading initial-access vectors — use of stolen credentials and exploit of vulnerability — are tied at 39% each.
- **Ransomware was involved in 48% of all breaches, up from 44%.** Monetization continued to weaken: **69% of ransomware victims did not pay**, and the median ransom paid fell to **\$139,875** from \$150,000. About **96% of ransomware victims were small and medium businesses**, often compromised opportunistically through stolen credentials (38%) or unpatched edge-device vulnerabilities (29%).

### Third-party exposure

- **Breaches with third-party involvement rose 60% year over year, reaching 48% of total breaches.** Third-party remediation lags badly: only 23% of third-party organizations fully remediated missing or misconfigured MFA on cloud accounts, and for weak passwords and permission misconfigurations the time to resolve half of all findings approached eight months.

### GenAI in attacks — widespread but not yet novel

- **Threat actors are using GenAI across attack stages: targeting, initial access, and malware and tool development.** The median observed actor researched or used AI assistance across **15 documented techniques**, with some using as many as 40 or 50.
- **The capability is mostly applied to known techniques, not new ones.** AI-assisted malware had a median of **55 existing known malware examples** performing the same function, and **fewer than 2.5%** of AI-assisted malware observations involved uncommon techniques with one or fewer known prior examples.

### Social engineering

- **The human element appeared in 62% of breaches**, up from 60%. Social Engineering was the third most common breach pattern at 16% of breaches. Phishing held at 16%; Pretexting reached 6% and is increasingly the initial action in ransomware and extortion. In phishing simulations, the median success ("click") rate for mobile-centric vectors such as voice and text was 40% higher than for email.

### 2025 events the report flags

- **March 2025:** a cascading GitHub Actions supply-chain breach exposed secrets across more than 23,000 repositories, corroborating the CI/CD exposures in [[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]].
- **December 2025:** mass exploitation of React2Shell (CVE-2025-55182), an RCE in React Server Components, reached 39% of cloud environments; the report also notes VoidLink, a malware framework written in six days by an AI agent.

## Strengths and Weaknesses

**Strengths.** The DBIR is the field's most-cited breach-telemetry source, and the 2026 edition rests on the largest corpus it has assembled. It is the first edition to place vulnerability exploitation above credentials for initial access, a breach-grounded counterpart to the benchmark-derived speed claims elsewhere in the wiki ([[moak-how-it-works|MOAK]], [[anthropic-glasswing-initial-update|Glasswing]]). The remediation figures (26% KEV patched, 43-day median) quantify the defender side of the gap that benchmark sources leave directional.

**Weaknesses.** Several year-over-year shifts are partly methodological: the credential-abuse drop reflects the new Pretexting category, and the ransomware and third-party figures benefit from an expanded contributor base, which the report states plainly. DBIR figures reflect a contributor base that skews toward organizations with mature logging. The GenAI findings are framed cautiously and should not be read as evidence of widespread novel AI-generated malware; the report's own data points the other way.

## Relations

- **Supports** [[vulnops|VulnOps]] — independent breach-telemetry evidence that periodic, manual vulnerability management is structurally outmatched. The 26%-patched / 43-day-median pair is the strongest non-vendor, non-benchmark quantification of the defender-side bottleneck the function exists to address.
- **Supports** [[zero-day-clock|Zero Day Clock]] — corroborates the exploitation-velocity story from breach data rather than red-team benchmarks.
- **Supports** [[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]] — the 48% third-party-involvement figure (+60% YoY) and the March 2025 GitHub Actions breach are direct quantitative and case anchors for the supply-chain scope.
- **Tempers** [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] — confirms widespread offensive GenAI use while showing it is concentrated on known techniques, which bounds the "novel AI malware" claim.

**Breach telemetry confirms the inversion; remediation is going backward.** Prior wiki evidence for the discovery-to-remediation inversion came from capability benchmarks ([[moak-how-it-works|MOAK]] 97.8% KEV exploitation) and a vendor coalition ([[anthropic-glasswing-initial-update|Glasswing]]). The DBIR 2026 grounds it in observed breaches: across 22,000+ confirmed breaches, vulnerability exploitation (31%) now outranks credentials (13%) for initial access. The sharper signal is on the defender side: the share of KEV criticals fully patched fell from 38% to 26% and the median resolution time rose from 32 to 43 days. As attackers got faster, defenders measurably slowed.

## The GenAI section is Anthropic telemetry, and the wiki had been reading it as independent

> [!contradiction] The GenAI half of this report is not a second source
> Anthropic states that it supplied eleven months of its banned-account threat-actor dataset to this report.[^nav-dbir] The DBIR's GenAI section reports a median actor using AI across 15 documented techniques; Anthropic's own [[llm-attack-navigator|LLM ATT&CK Navigator]] reports a median of 16 over a window shifted four months later. The two are one telemetry source published twice, and citing both in support of the same claim double-counts it. The distinction is confined to the GenAI section: the breach findings — exploitation at 31% of initial access, 26% of KEV criticals remediated, the 43-day median, the third-party figures — come from the contributor breach corpus and remain independent of Anthropic.

The novelty finding survives the provenance correction and gains a bound. AI-assisted malware carrying a median of 55 known prior examples, with under 2.5% of observations involving uncommon techniques, is consistent with the one case where a vendor could see the development process from the inside: [[gtg-5004-no-code-ransomware|GTG-5004]] shipped ransomware whose every component — ChaCha20, FreshyCalls, RecycledGate, reflective DLL injection, shadow-copy deletion — is published prior art, built by an actor Anthropic assesses as unable to implement any of it unaided.[^tir-5004]

That case also shows what the novelty metric does not measure. Counting prior examples answers whether the output is new. It does not answer whether the author could have produced it, and on that question the two sources point in opposite directions from the same data: the DBIR reads low novelty as a bound on the AI-malware threat, while Anthropic's case record reads the same low novelty alongside an author who could not code. The reconciliation is at [[capability-floor-collapse|Capability Floor Collapse]] — the collapse is in access to existing capability, not in the invention of new capability.

> [!gap] The direction of travel, not the ceiling
> The novelty ceiling holds for the reporting window. What moved instead is where in the kill chain the assistance is applied: across Anthropic's March 2025 – March 2026 window, AI-assisted account discovery rose 8.9% and automated exfiltration 6.2% while capability development fell 12% and phishing fell 8.6%, and the share of actors scoring medium risk or higher went from 33% to 56%.[^nav-drift] A repeat of the DBIR's novelty measurement will not detect that shift, because the techniques moving into use are as well-documented as the ones moving out. Open question: which measurement in a breach corpus would show it.

## Notes

[^nav-dbir]: Kyla Guru, Alex Moix, and Jacob Klein, [*Mapping AI-enabled cyber threats: Insights from the LLM ATT&CK Navigator*](https://red.anthropic.com/2026/attack-navigator/), Anthropic Frontier Red Team, 2026-06-03, footnote 1: "For the DBIR, we provided analysis of 11 months of threat actor data, and rounded this out to 12 months for this report." Summary at [[llm-attack-navigator|LLM ATT&CK Navigator]].

[^tir-5004]: Anthropic, [*Threat Intelligence Report: August 2025*](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf), pp. 15–17. See [[gtg-5004-no-code-ransomware|No-Code Ransomware Operation]].

[^nav-drift]: Guru, Moix, and Klein, [*Mapping AI-enabled cyber threats*](https://red.anthropic.com/2026/attack-navigator/): T1087 +8.9%, T1020 +6.2%, T1587 −12%, T1566 −8.6% between the two halves of the study window; medium-risk-or-higher share 33.5% to 56.1%.
