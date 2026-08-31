---
type: concept
title: "Zero Day Clock"
address: c-000070
created: 2026-05-15
updated: 2026-08-31
tags:
  - concepts
  - zero-day-clock
  - time-to-exploit
  - tte
  - quantitative-anchor
status: developing
scope_axis: [sec-against-ai, ai-in-sec-defense]
related:
  - "[[ai-attribution-primaries-2026-08-17|AI Attribution Primary-Source Review]]"
  - "[[sergej-epp|Sergej Epp]]"
  - "[[sysdig|Sysdig]]"
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[moak|MOAK]]"
  - "[[moak-how-it-works|MOAK How It Works]]"
  - "[[autonomous-exploit-generation|Autonomous Exploit Generation]]"
  - "[[mythos-ready-security-program|Mythos-ready Security Program]]"
  - "[[unprompted-conference-march-2026|Unprompted Conference March 2026]]"
  - "[[vulnops|VulnOps]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]"
  - "[[anthropic-glasswing-initial-update|Glasswing initial update]]"
  - "[[mozilla|Mozilla]]"
  - "[[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]"
  - "[[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]]"
  - "[[dream-taiwan-multi-agent-ai-attack|Taiwan Multi-Agent Attack Reconstruction]]"
  - "[[vulnerability-research-agentic-age-keynote]]"
  - "[[vulnerability-properties]]"
  - "[[arizona-state-university]]"
  - "[[capability-floor-collapse|Capability Floor Collapse]]"
  - "[[gtg-5004-no-code-ransomware|No-Code Ransomware Operation]]"
  - "[[llm-attack-navigator|LLM ATT&CK Navigator]]"
  - "[[autonomous-code-security-google-talk|Autonomous Code Security at Google]]"
  - "[[cybergym|CyberGym Benchmark]]"
sources:
  - "[[.raw/articles/zero-day-clock-the-collapse-2026-05-25.md]]"
  - "[[.raw/articles/zero-day-clock-call-to-action-2026-05-25.md]]"
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[anthropic-glasswing-initial-update|Glasswing initial update]]"
  - "https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia"
  - "https://www.cybergym.io/cybergym/"
homepage: "https://zerodayclock.com"
coined_by:
  - "[[sysdig]]"
---

# Zero Day Clock

The **Zero Day Clock** is a data-and-visualization instrument that tracks *Time-to-Exploit* (TTE) — the gap between CVE disclosure and first observed exploitation — across time. It was launched at the [[unprompted-conference-march-2026|Unprompted Conference]] in March 2026 by [[sergej-epp|Sergej Epp]] (CISO, [[sysdig|Sysdig]]) and collaborators, and is published at [zerodayclock.com](https://zerodayclock.com). The underlying dataset is a live, growing set of CVE-exploit pairs drawn from CISA KEV, VulnCheck KEV, and XDB.[^dataset]

## The Data

The Zero Day Clock reports the **median** time-to-exploit by year. The series begins in 2018 and shows a near-exponential collapse:[^collapse]

| Year | Median TTE | Note |
| :--- | :--- | :--- |
| 2018 | 771 days | Series begins; over two years from disclosure to first observed exploit |
| 2021 | 84 days | ~9× compression in three years; Log4Shell exploited within hours of disclosure |
| 2023 | 6.36 days | ~40% of exploited flaws were zero-days; over 44% exploited within 24 hours |
| 2024 | 4 hours | — |
| 2025 | zero-day | Median exploitation now occurs on or before disclosure |
| 2026 | zero-day | Sustained |

From 771 days in 2018 to a median of zero days by 2025, the window of exposure has effectively closed. For the median exploited vulnerability, the exploit now arrives on or before the public advisory.[^collapse]

> [!contradiction] Correction (2026-05-25): the prior figures did not match the primary source
> An earlier version of this page carried a year-by-year table labelled "Mean TTE (10% trimmed)" running 2.3 years (2018) → 56 days (2024) → 23.2 days (2025) → 9 hours (2026). Those values are **not found in the primary Zero Day Clock or in the [[mythos-ready-briefing|Mythos-ready briefing]]** the wiki cited for them. The briefing states only that time-to-exploit is "now under one day in 2026" and reproduces a clock diagram; it contains no year-by-year figures and no "9 hours." The live Zero Day Clock reports a **median** (not a trimmed mean): 771 days in 2018, 4 hours in 2024, zero-day in 2025–2026. The fabricated mean table and the "9 hours" figure have been removed here and on the dependent pages ([[vulnops|VulnOps]], [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]], [[mythos-ready-briefing|Mythos-ready briefing]], [[mythos-ready-security-program|Mythos-ready playbook]], [[sergej-epp|Sergej Epp]], and the homepage), which now follow zerodayclock.com/collapse.

## The Exposure Arithmetic

The clock frames the current state as a structural inversion. An exploit is created within an hour of a patch or advisory; attacks begin within 24 hours; the median organization needs about 20 days to test and deploy the same patch. By that arithmetic an organization is exposed for roughly 99.9% of the vulnerability lifecycle, and monthly patch cycles offer little protection.[^math] The act of fixing a vulnerability accelerates its exploitation: AI can reverse-engineer a patch into a working exploit in minutes, a restatement of the decades-old observation that *the patch is the advisory*.[^collapse]

## Measurement scope of the curve

> [!quote] Caveat, per the Mythos-ready briefing
> "It is worth noting that the historical collapse in time-to-exploit has not yet produced a proportional increase in the impact of exploitation. Many of the most consequential incidents of recent years involved credential abuse, social engineering, or supply chain compromise rather than zero-day exploitation. The Zero Day Clock trend is a leading indicator of where attacker capability is heading, not a direct measure of current damage."
> — [[mythos-ready-briefing|Mythos-ready briefing]], Appendix A

The clock measures where attacker capability is heading. Current losses are still dominated by credential abuse, social engineering, and supply chain compromise. The argument is that AI-driven capability eventually flows into the impact channel; the clock is the leading indicator of when.

Four further caveats bound the reading:

- **Defender-biased sample.** CISA KEV + VulnCheck KEV + XDB captures vulnerabilities *observed* exploited, not vulnerabilities that could be exploited but have not been seen yet. If AI-driven discovery outruns KEV listing, the curve understates the true pace.
- **Residence time before discovery sits outside the clock entirely.** The clock starts at disclosure, so it cannot measure how long a vulnerability existed before anyone found it. [[cybergym|CyberGym]]'s open-ended runs put a figure on that interval: ten deduplicated zero-days still present in current releases had each persisted an average of **969 days** before an agent found them.[^cybergym-site] A separate campaign across **431 OSS-Fuzz projects** confirmed 7 zero-days under GPT-4.1 and 22 under GPT-5, four of the latter overlapping with the former. Those vulnerabilities were exploitable throughout their residence and appear in no time-to-exploit series.
- **CVE-exploit pairs miss un-cataloged AI findings.** The [[mozilla|Mozilla]]-via-Mythos finding (271 vulnerabilities, 3 warranted CVEs) indicates that most AI-discovered vulnerabilities are never assigned CVEs, which makes any CVE-based TTE a lower bound on AI-driven discovery-to-exploit pace. [[anthropic-glasswing-initial-update|Anthropic's Glasswing update]] confirms the 271 figure directly (Firefox 150, more than ten times the Firefox 148 count under Opus 4.6).
- **AI-augmented campaigns that involve no vulnerability stay outside the dataset entirely.** Amazon Threat Intelligence's FortiGate campaign used AI-generated attack plans and reconnaissance tooling to compromise more than 600 devices across more than 55 countries, with no FortiGate vulnerability exploited: access ran on exposed management ports and single-factor credentials.[^aws-fg] No CVE means no clock starts. The campaign is a negative datum outside the curve's scope. See the [[ai-attribution-primaries-2026-08-17|AI Attribution Primary-Source Review]].

## Exploitation with no disclosure event

TTE measures the interval between a public advisory and first observed exploitation, so it presumes an advisory exists to start the clock. A class of vulnerability now exists for which no such event occurs. In the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]], four vulnerabilities were discovered, weaponized, and exploited by the same autonomous system, against the estates that were running it: two in [[artifactory|JFrog Artifactory]] and two chained in [[hugging-face|Hugging Face]].[^bh-openai-hf] Nobody published anything to start a clock against. Discovery-to-exploitation ran to effectively zero on the inside while TTE stayed undefined on the outside.

For this class the ordering inverts. Exploitation is the first event, and vendor notification, when it arrives, is a consequence of it: the first Artifactory flaw was exploited from 2026-06-26, and the vendor was notified and the patch deployed on 2026-07-06 as part of remediation. Two intervals stay measurable without an advisory to anchor them:

- **Foothold to full control.** Under 13 hours from code execution on a single Hugging Face dataset-worker pod to cluster admin across multiple clusters, through two chained zero-days.[^bh-openai-hf]
- **Exploitation to detection.** Eight days from the first Artifactory RCE on 2026-06-26 to OpenAI's incident declaration on 2026-07-04, which was triggered by the service failing under agent load rather than by a security signal. Twenty-three days to the first security detection proper, a workload alert on the internal privilege escalation on 2026-07-19, three days after Hugging Face published.[^bh-openai-hf]

This compounds the defender-biased-sample caveat above rather than sitting beside it. A vulnerability exploited by the system that found it, inside infrastructure the finder already occupies, never enters CISA KEV, VulnCheck KEV, or XDB, and so cannot move the curve in either direction.

A parallel, adversary-side case makes the same point without CVEs at all. In the [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]], the compromises exploited were largely logic and configuration flaws (unauthenticated debug endpoints, an `alg: none` JWT gap, SSO over-trust) that never register as CVEs, discovered and exploited by the same multi-agent framework across a four-day operation with no advisory anywhere in the chain. Dream Security's own framing of the finding states the clock's asymmetry directly: *"the cost of running a competent attack has collapsed, but the cost of defending against one has not."*[^dream-taiwan]

## Operational consequences of the collapse

- **It is the empirical basis for the window-of-exposure argument** in the [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] thesis and Risk #1 / Risk #9 of the [[mythos-ready-security-program|Mythos-ready playbook]]. The same claim was previously argued through vendor executive quotes; the clock supplies measured figures.
- **It is the motivation for [[vulnops|VulnOps]].** When the median exploit arrives on or before disclosure, periodic vulnerability management — a quarterly pen test plus patch-as-CVE-arrives — is structurally outmatched. VulnOps is the continuous response to a continuous-discovery, continuous-exploitation environment.
- **It pairs with the remediation-side lag.** The clock measures attacker-side TTE; the [[anthropic-glasswing-initial-update|Glasswing funnel]] (6,202 estimated high/critical found, 75 patched) measures defender-side discovery-to-patch lag. The two curves define the window of exposure, and they are diverging — the operational case for VulnOps. As Glasswing puts it, the constraint has moved from finding vulnerabilities to verifying, disclosing, and patching them.
- **MOAK supplies the mechanism.** [[moak-how-it-works|MOAK's five-agent pipeline]] autonomously exploits 174 of 178 CISA KEVs (97.8%) within hours of disclosure, with Claude Opus 4.6 reaching 98% on post-knowledge-cutoff KEVs. The clock documents that exploitation now precedes or coincides with disclosure; MOAK demonstrates how — no human bottleneck in the exploit-derivation loop.
- **The queue in front of the clock may stop being sortable.** [[autonomous-code-security-google-talk|Heather Adkins argued at [un]prompted in March 2026]] that agentic frameworks are close to finding every vulnerability in every system, and that CVSS will therefore stop being meaningful as a ranking instrument. The clock measures how fast exploitation follows disclosure; her claim is about whether the disclosures in front of it can still be ordered by severity.

Corroborating the trend from outside the clock's own dataset: VulnCheck reported that, of 159 vulnerabilities first observed exploited in Q1 2025, 28.3% had exploitation evidence within a day of disclosure — nearly one in three.[^vulncheck] Rapid7's 2026 Cyber Threat Landscape Report, on a separate dataset, found confirmed exploitation of newly disclosed high/critical vulnerabilities up 105% year over year (to 146 in 2025) and mean time-to-exploit down from 61.0 to 28.5 days.[^rapid7] The Rapid7 figure is a mean and the clock's is a median; the gap is expected, because a near-zero median with a multi-week mean describes a right-skewed distribution — most exploited flaws are weaponized at or before disclosure, while a slow-exploited tail lifts the mean.

**The clock measures the interval, not the population holding it.** Every figure on this page is a time: days from disclosure to exploitation, hours from KEV publication to a working exploit. Anthropic's vendor telemetry measures the orthogonal quantity, which is who is now able to hold the clock at all — [[gtg-5004-no-code-ransomware|GTG-5004]] shipped ransomware with two direct-syscall EDR bypasses while, per Anthropic's assessment of the prompt record, being unable to implement any of its components unaided, and across 832 banned accounts assessed technical sophistication correlates with the remaining components of Anthropic's risk score at r = 0.28.[^floor] A collapsing interval and a collapsing entry qualification compound: the exposure window is defined by the fastest adversary, and the population able to move at that speed is no longer bounded by how many people can write the exploit. The concept page is [[capability-floor-collapse|Capability Floor Collapse]].

On the remediation side, the Qualys 2026 benchmark puts the mean time to remediation for the most-delayed complex applications (Java, .NET, Citrix) at 5 months and 10 days, even as roughly 40 million of 150 million deployed patches now ship autonomously.[^qualys] The attacker-side and defender-side curves are moving in opposite directions.

## Call to Action — Ten Demands

The Zero Day Clock pairs its data with a ten-point policy agenda, each attributed to a named proponent:[^cta]

1. **Hold the makers accountable** — software-vendor liability for shipping insecure products (Jen Easterly; Bruce Schneier).
2. **Build security into the platform** — "shift down," so applications inherit secure defaults from frameworks and infrastructure (Phil Venables).
3. **Stop patching, start rebuilding** — distributed, immutable, ephemeral systems, the DIE triad (Sounil Yu; Heather Adkins).
4. **Eliminate the root cause** — memory-safe languages for new critical code; approximately 70% of critical flaws in large C/C++ codebases are memory-safety bugs (Mark Russinovich).[^memsafe]
5. **Open-source the defense** — make AI defensive tooling free to every defender, including those priced out of six-figure contracts (Daniel Miessler; Loris Degioanni).
6. **Regulation for machine speed** — safe harbors and pre-authorized response for autonomous defense, instead of quarterly-audit assumptions (Rob T. Lee).
7. **Bridge the gap between hackers and policy** (Jeff Moss).
8. **Zero trust everywhere** (John Kindervag).
9. **Treat cyber as statecraft.**
10. **Fund the defense.**

### Scope of the memory-safety demand

Carrying out Demand 4 eliminates the property set the language addresses and leaves the rest standing. The evidence is a shipped one: 79 CVEs dropped against a Rust reimplementation of coreutils *after* it shipped in the current Ubuntu release, carrying time-of-check/time-of-use flaws in critical utilities rather than memory corruption.[^asu-keynote] The demand is calibrated against the roughly 70% memory-safety share cited above;[^memsafe] a rewrite inherits the remaining 30% into new code that has no analysis history.

Agentic rewriting makes the demand tractable at a scale it was not tractable at before, and does not change that arithmetic. [[vulnerability-research-agentic-age-keynote|Shoshitaishvili's Black Hat 2026 keynote]] reports agents reimplementing libssl, libpng, and libxml as millions of lines of Rust, in which the crypto library reproduced the classic non-memory-safety cryptographic attacks the original was vulnerable to — under explicit instruction naming those historical vulnerabilities and forbidding them. [[vulnerability-properties|Vulnerability properties]] survive a change of language; instructing the model not to reproduce them is not a control.

Demand 4 stands: it retires one property class rather than delivering a clean codebase, and a rewrite planned as a security control needs its post-rewrite threat model derived from the original's non-memory-safety vulnerability history.

## Notes

[^dataset]: [Zero Day Clock — Explorer](https://zerodayclock.com/explorer), Sysdig and collaborators, 2026. Live dataset of CVE-exploit pairs sourced from CISA KEV, VulnCheck KEV, and XDB. The explorer is a JavaScript-rendered live counter; the exact pair count is not readable from a static fetch (early-2026 secondary analyses cite figures around 3,500).
[^collapse]: [The Collapse — Zero Day Clock](https://zerodayclock.com/collapse), 2026. Median TTE by year: 771 days (2018), 84 days (2021), 6.36 days (2023), 4 hours (2024), zero-day (2025–2026); ~40% of 2023 exploited flaws were zero-days and over 44% exploited within 24 hours. Local copy: `.raw/articles/zero-day-clock-the-collapse-2026-05-25.md`.
[^math]: [The Collapse — Zero Day Clock](https://zerodayclock.com/collapse), 2026, "The Math" section. Exploit created in under one hour; attacks begin within 24 hours; median patch time approximately 20 days; organizations exposed for roughly 99.9% of the vulnerability lifecycle.
[^cta]: [Call to Action — Zero Day Clock](https://zerodayclock.com/call-to-action), 2026. Ten demands with named proponents (Easterly, Schneier, Venables, Yu, Adkins, Russinovich, Miessler, Degioanni, Lee, Moss, Kindervag). Local copy: `.raw/articles/zero-day-clock-call-to-action-2026-05-25.md`.
[^memsafe]: [Call to Action — Zero Day Clock](https://zerodayclock.com/call-to-action), 2026, citing the 2024 White House ONCD report "Back to the Building Blocks." Approximately 70% of critical vulnerabilities in large C/C++ codebases are memory-safety bugs.
[^rapid7]: [Rapid7 — 2026 Cyber Threat Landscape Report](https://www.rapid7.com/research/report/global-threat-landscape-report-2026/), 2026, as reported by [CSO Online](https://www.csoonline.com/article/4156005/patch-windows-collapse-as-time-to-exploit-accelerates.html). Confirmed exploitation of newly disclosed high/critical vulnerabilities rose to 146 in 2025 from 71 in 2024 (+105%); mean time-to-exploit fell from 61.0 to 28.5 days.
[^qualys]: [Qualys — Enterprise Patch & Remediation Benchmark 2026](https://blog.qualys.com/qualys-insights/2026/04/20/enterprise-patch-remediation-benchmark-2026), April 2026. Mean time to remediation for the most-delayed complex applications (Java, .NET, Citrix) of 5 months 10 days; about 40 million of roughly 150 million deployed patches were autonomous (no human in the loop). See [[qualys-patch-remediation-benchmark-2026|Qualys Patch & Remediation Benchmark 2026]].
[^bh-openai-hf]: Michael Dalton and Eric Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident — A Technical Reconstruction*, Black Hat USA 2026 (2026-08-06). Four zero-days exploited by the agents that found them; Artifactory outage and OpenAI incident declaration 2026-07-04; Hugging Face publication 2026-07-16; OpenAI workload alert 2026-07-19; incidents linked 2026-07-20. See [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]].
[^floor]: Anthropic, [*Threat Intelligence Report: August 2025*](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf), pp. 15–17; and Kyla Guru, Alex Moix, and Jacob Klein, [*Mapping AI-enabled cyber threats: Insights from the LLM ATT&CK Navigator*](https://red.anthropic.com/2026/attack-navigator/), Anthropic Frontier Red Team, 2026-06-03 (r = 0.28 across 832 accounts, March 2025 – March 2026).

[^vulncheck]: [VulnCheck — 2025 Q1 Trends in Vulnerability Exploitation](https://www.vulncheck.com/blog/exploitation-trends-q1-2025), Patrick Garrity, April 2025. Of 159 vulnerabilities first reported exploited in the wild in Q1 2025, 28.3% had exploitation evidence within one day of CVE publication. See [[vulncheck-exploitation-trends-q1-2025|VulnCheck Q1 2025 exploitation trends]].
[^dream-taiwan]: Dream Research Labs, [Taiwan Multi-Agent Attack Reconstruction](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia) (2026-08-12). See [[dream-taiwan-multi-agent-ai-attack|the source summary]] and [[taiwan-ai-agent-government-intrusion|the incident record]].
[^aws-fg]: Amazon Threat Intelligence, [AI-augmented threat actor accesses FortiGate devices at scale](https://aws.amazon.com/blogs/security/ai-augmented-threat-actor-accesses-fortigate-devices-at-scale/), AWS (2026-02-20). More than 600 devices across more than 55 countries, 2026-01-11 to 2026-02-18, with no exploitation of any FortiGate vulnerability. See [[ai-attribution-primaries-2026-08-17|AI Attribution Primary-Source Review]].
[^cybergym-site]: UC Berkeley RDI, [CyberGym](https://www.cybergym.io/cybergym/) (fetched 2026-08-31). Published at ICLR 2026, [OpenReview `2YvbLQEdYt`](https://openreview.net/forum?id=2YvbLQEdYt); preprint [arXiv:2506.02548](https://arxiv.org/abs/2506.02548). Local copy: `.raw/articles/cybergym-benchmark-2026-08-31.md`.

<!-- sources:auto -->
## Sources

- [The Collapse — Zero Day Clock](https://zerodayclock.com/collapse)
- [Call to Action — Zero Day Clock](https://zerodayclock.com/call-to-action)
- [Zero Day Clock](https://zerodayclock.com)
<!-- /sources -->

[^asu-keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06). See [[vulnerability-research-agentic-age-keynote|the talk summary]].
