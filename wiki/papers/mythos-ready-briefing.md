---
type: paper
title: "Mythos-Ready Security Program"
address: c-000067
created: 2026-05-15
updated: 2026-08-21
tags:
  - papers
  - strategy-briefing
  - csa
  - sans
  - unprompted
  - owasp
  - vulnops
status: summarized
scope_axis: [ai-in-sec-defense, sec-against-ai]
year: 2026
authors:
  - "[[gadi-evron|Gadi Evron]]"
  - "Rich Mogull"
  - "Robert T. Lee"
contributing_authors:
  - "Jen Easterly"
  - "Chris Inglis"
  - "[[heather-adkins|Heather Adkins]]"
  - "[[sounil-yu|Sounil Yu]]"
  - "Katie Moussouris"
  - "James Lyne"
  - "Maxim Kovalsky"
  - "Joshua Saxe"
  - "Bruce Schneier"
  - "Phil Venables"
  - "Rob Joyce"
  - "Jim Reavis"
  - "John N. Stewart"
  - "Dave Lewis"
  - "John Yeoh"
  - "Ramy Houssaini"
venue: "Joint CSA CISO Community / SANS / Unprompted / OWASP Gen AI Security Project — Expedited Strategy Briefing"
version: "1.0"
original_release: 2026-04-12
last_updated_source: 2026-05-01
license: "CC BY-NC 4.0"
source_url: ""
no_public_url: "PDF distributed via Cloud Security Alliance CISO Community channels (contact cisos@cloudsecurityalliance.org); the briefing references a canonical 'latest version' link in its frontmatter but the URL was not captured in the local copy."
archived_copy: ".raw/papers/mythos-ready-csa-sans-unprompted-v1.0-2026-04-12.pdf"
key_claim: "AI-driven vulnerability discovery has created a structural and durable shift in offense/defense asymmetry: time-to-exploit has collapsed to under one day in 2026; a 'Mythos-ready' security program is the operational response that prioritizes minimum viable resilience, AI-agent adoption across all security functions, agent harness defense, continuous patching capacity, and a permanent VulnOps function staffed and automated like DevOps."
methodology: "Strategy briefing assembled by community consensus across three lead authors and 17 contributing authors with 75+ named reviewers (CISOs and practitioners). Six sections: I Executive Summary, II Key Takeaways for the CISO, III Introduction (with timeline of LLM-based offensive capabilities Jun 2025 – Apr 2026), IV The Mythos-ready Security Program (10 Questions + Risk Register + Priority Actions), V Executive and Board Briefing template, VI Conclusions and Recommendations. Two appendices: A Historical Precedent (DARPA Cyber Grand Challenge 2016 → present), B Mythos Risk Register Legend (OWASP LLM 2025 + OWASP Agentic 2026 + MITRE ATLAS + NIST CSF 2.0 + CSA AI Control Matrix v1.0.3)."
related:
  - "[[mythos-ready-security-program|Mythos-ready Security Program]]"
  - "[[vulnops|VulnOps — Vulnerability Operations]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[cyber-poverty-line|Cyber Poverty Line]]"
  - "[[citizen-coders|Citizen Coders]]"
  - "[[mythos|Claude Mythos Preview]]"
  - "[[anthropic-glasswing-announcement|Project Glasswing]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]"
  - "[[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack 2026]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
  - "[[openant|OpenAnt]]"
  - "[[codex-security|Codex Security]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[big-sleep|Big Sleep]]"
  - "[[xbow|XBOW]]"
  - "[[gadi-evron|Gadi Evron]]"
  - "[[wendy-nather|Wendy Nather]]"
  - "[[sergej-epp|Sergej Epp]]"
  - "[[csa|Cloud Security Alliance]]"
  - "[[sans-institute|SANS Institute]]"
  - "[[owasp|OWASP]]"
  - "[[unprompted-conference-march-2026|Unprompted Conference March 2026]]"
sources:
  - "[[.raw/papers/mythos-ready-csa-sans-unprompted-v1.0-2026-04-12.pdf]]"
aliases:
  - papers/mythos-ready-csa-sans-unprompted-2026-04-12
  - mythos-ready-csa-sans-unprompted-2026-04-12

---

# The "AI Vulnerability Storm" — Building a "Mythos-ready" Security Program

**Source:** *The "AI Vulnerability Storm": Building a "Mythos-ready" Security Program — Expedited Strategy Briefing* (Cloud Security Alliance CISO Community + SANS + Unprompted + OWASP Gen AI Security Project + the wider community). Version 1.0; original release 2026-04-12; last updated 2026-05-01. License: CC BY-NC 4.0. Contact: `cisos@cloudsecurityalliance.org`. Local copy: `.raw/papers/mythos-ready-csa-sans-unprompted-v1.0-2026-04-12.pdf` (md5 `4af643cd72df73713795b65b5d18cb83`).

## Key Claim

AI-driven vulnerability discovery has crossed an operational threshold: capabilities that previously required nation-state resources are now broadly accessible, time-to-exploit has collapsed to **under one day in 2026** per the [[zero-day-clock|Zero Day Clock]], and existing patch cycles / response processes / risk metrics were not designed for this environment. A *Mythos-ready security program* is the operational response — anchored on five propositions: (1) **use LLM-based vulnerability discovery and remediation capabilities now** (already mature; build toward [[vulnops|VulnOps]]); (2) **update risk metrics** because pre-AI assumptions about exploit timelines and incident frequency no longer hold; (3) **accelerate teams via coding agents across all security functions** (GRC, incident response — not just code); (4) **prepare to respond to more simultaneous high-severity incidents** with hardened mitigating controls (segmentation, egress filtering, Zero Trust, phishing-resistant MFA, secrets rotation); (5) **focus on the basics** (segmentation, patching, IAM, defense-in-depth/breadth) while building **collective defense** through ISACs / CERTs / sector coordinating groups, especially for organizations below [[cyber-poverty-line|Wendy Nather's Cyber Poverty Line]].

## Methodology

Community-consensus strategy briefing, not a peer-reviewed paper. Three lead authors ([[gadi-evron|Gadi Evron]], Rich Mogull, Robert T. Lee), 17 contributing authors (including [[heather-adkins|Heather Adkins]], [[sounil-yu|Sounil Yu]], Bruce Schneier, Jen Easterly, Chris Inglis, Rob Joyce, Katie Moussouris, Phil Venables, Joshua Saxe, James Lyne, Maxim Kovalsky, Jim Reavis, John N. Stewart, Dave Lewis, John Yeoh, Ramy Houssaini), 75+ named reviewers across CISO and practitioner roles. Six sections plus two appendices, totaling 29 pages.

## Notable Findings

### 1. Quantitative anchors for the "AI Vulnerability Storm"

- **[[zero-day-clock|Zero Day Clock]] (Sergej Epp, March 2026)**: the briefing reproduces a Zero Day Clock diagram and states that time-to-exploit is *"now under one day in 2026"* (down to hours); it gives no year-by-year figures. The live Zero Day Clock reports a **median** TTE of 771 days (2018) falling to zero-day by 2025 — see [[zero-day-clock|Zero Day Clock]].
- **Mythos vs Claude Opus 4.6 on Firefox (Anthropic internal lab)**: Mythos generated **181 working exploits**; Opus 4.6 succeeded **only twice** under the same conditions — ~90× autonomy/reliability jump.
- **Mythos exploit success rate (Apr 7 2026 announcement)**: 72%. Thousands of zero-days across every major OS and browser. 27-year-old OpenBSD bug.
- **Mozilla / Firefox**: 271 vulnerabilities discovered using Mythos (of which 3 warranted CVEs).
- **AISLE (Feb 5 2026)**: 12 OpenSSL zero-days including a CVSS 9.8 vulnerability dating to 1998.
- **DARPA AIxCC finals (Aug 8 2025, DEF CON 33)**: 54 vulnerabilities in **4 hours of compute** across **54 million lines of code**.
- **[[big-sleep|Google Big Sleep]] (Aug 5 2025)**: 20 real-world zero-days in OSS projects including FFmpeg and ImageMagick, autonomously found + reproduced.
- **[[xbow|XBOW]] (Jun 24 2025)**: #1 on HackerOne US leaderboard — first autonomous system to outperform all human hackers on the platform.
- **Anthropic Chinese state-sponsored disclosure (Nov 14 2025)**: First publicly disclosed AI-orchestrated espionage campaign — Claude Code used to autonomously run full attack chains (reconnaissance through exfiltration) across ~30 global targets; detected mid-September 2025.
- **Sysdig 8-minute admin compromise** (early 2026, detailed in [[unprompted-conference-march-2026|Sergej Epp's Unprompted talk]]).
- **Linux kernel**: bug reports climbed from **2 to 10 per week** through early 2026, initially hallucinated but now all verified real; code is being removed from the kernel to reduce attack surface due to LLM-driven research.
- **curl project** discontinued bug bounty over AI-generated "slop"; now reports are an increasing share of AI-supported quality findings.

### 2. Timeline corrections / precision additions for the wiki

- **Aardvark original launch**: **30 October 2025** as private beta (not 2025-unspecified as the wiki previously held). Renamed Codex Security in March 2026 — see [[codex-security|the product page]].
- **Anthropic + Heather Adkins / Gadi Evron singularity warning**: September 2025 — autonomous vulnerability discovery + exploitation forecast **~6 months away** at that point. Bruce Schneier joined in October 2025 and the three introduced [[vulnops|VulnOps]] as a concept.
- **Anthropic Chinese state-sponsored disclosure**: 2025-11-14 disclosure of the **first publicly disclosed AI-orchestrated espionage campaign**.
- **Claude Mythos Preview & Project Glasswing announcement**: April 7 2026 (per the briefing's timeline). The wiki previously framed this as May 12 2026 — but the Glasswing *coalition* announcement (May 12 2026) is distinct from the Mythos Preview release (April 7 2026). The briefing's April 7 date refers to the model preview release; the wiki's May 12 anchor remains correct for the coalition / partner program announcement.

### 3. Mythos & Glasswing — what's structurally new

Three Mythos technological distinctives the briefing names:

- **Exploits without scaffolding** — Mythos generates working exploits in a *one-shot* prompt without elaborate scaffolding or agent configuration. The 181-vs-2 Firefox lab result is the quantitative anchor.
- **Complex chained vulnerabilities** — Mythos identifies vulnerabilities composed of multiple primitives chained together (e.g., multiple memory-corruption bugs combined into one exploit path).
- **"One-shot" single-prompt capability** — significant accomplishment without elaborate scaffolding.

Strategically, Mythos broke into mainstream media beyond technical security communities, reaching boardrooms — raising urgency and opening new resources/funding across the industry. **[[anthropic-glasswing-announcement|Project Glasswing]]** is described as *"possibly the largest multi-party vulnerability coordination effort in history."* OpenAI's parallel **Trusted Access** program (announced earlier in 2026, recently expanded) is named explicitly. The briefing flags the central limitation: Glasswing covers a curated partner ecosystem and most organizations that build or maintain critical software will not have early access; competitive landscape is narrowing — comparable offensive capabilities expected in other frontier models within months and in open-weight models within 6 months to a year, making the defensive advantage of early access *time-limited by definition*.

### 4. The Mythos-ready Security Program — what the briefing operationalizes

The full operational instrument is filed as a dedicated playbook page: see [[mythos-ready-security-program|Mythos-ready Security Program (Playbook)]]. The three load-bearing artifacts:

- **10 Questions to Understand Your Security Program State and Influence** — triage instrument: AI stance / employee agentic-coding access / OSS legal posture / control over repos and agentic supply chain (MCP servers, plugins, skills) / cooling-off gate between code change and production / security operational vs advisory / fastest security-driven production change in last year / crown-jewels tracking / urgent-work prioritization with third parties / executive working definition of urgency.
- **Risk Register (DRAFT)** — 13 risks across Critical (5), High (7), Medium (1) categories, each mapped to OWASP LLM 2025, OWASP Agentic 2026, MITRE ATLAS, NIST CSF 2.0, and CSA AI Control Matrix V1.0.3 codes. Critical examples: Accelerated Threat Exploitation, Insufficient AI Automation Capabilities, Unmanaged AI Agent Attack Surface, Inadequate Incident Detection and Response Velocity, Cybersecurity Risk Model Outdated.
- **Priority Actions (DRAFT)** — 11 PAs across Risk Control / Operational Enabler / Governance categories, with start-time (this week / this month / next 90 days / next 6 months) and time horizon (ongoing / 45 days / 90 days / 6 months / 12 months). The list culminates in **PA 11 — Stand Up VulnOps** as the 6-to-12-month long-term play.

### 5. VulnOps as the central long-horizon concept

> *"Long-term, there is no alternative to building a permanent Vulnerability Operations (VulnOps) function, staffed and automated like DevOps, but for autonomous vulnerability research and remediation. Owns continuous discovery of zero-day vulnerabilities across your entire software estate (from your own code to third-party software), and establishes automated remediation pipelines. Design VulnOps around triage discipline from the start."* — PA 11

Filed as a dedicated concept page: see [[vulnops|VulnOps — Vulnerability Operations]]. The concept is jointly attributed to [[gadi-evron|Gadi Evron]], [[heather-adkins|Heather Adkins]], and Bruce Schneier (October 2025 industry-warning collaboration).

### 6. Operational human-cost framing

Three honest observations the briefing names that other vendor strategic-forecasts often elide:

- **"We cannot outwork machine-speed threats."** The cadence and volume of vulnerability disclosures will exceed anything the industry has experienced. Re-prioritize, automate, and **prepare for burnout** — request additional headcount and budget reserve capacity.
- **Security teams are caught in a vice** — AI simultaneously accelerates vulnerability-report volume + organizational code-shipping volume + attack surface expansion, against a workforce already at capacity, while staff also operate with **increased uncertainty about their roles** under AI augmentation.
- **The path forward is doubling down on fundamentals + hands-on adoption of agents at every level, from the CISO down.** Every security role is becoming an *AI builder* role; using a coding agent is now *easier than using Excel*. All you need is English.

### 7. The Y2K analogy

> *"We have done this before. Y2K was a systemic threat with a hard deadline, and the industry met it through coordinated, disciplined effort. This is the same kind of problem, requiring the same kind of response, with more powerful tools available to defenders."*

The closing frame argues that the *coordinated industry response* template applies — and that being Mythos-ready means closing the gap between *how fast vulnerabilities are found* and *how fast the organization can respond*.

### 8. Tools recommended in PA 1 (Point Agents at Your Code and Pipelines)

The briefing names the specific tools an organization should turn agents toward immediately:

- **Commercial**: [[claude-code-security|Claude Code Security]] (Anthropic), [[codex-security|Codex Security]] (OpenAI).
- **Open source**: [[openant|OpenAnt]] (Knostic), **[[raptor|raptor]]** (Claude Code framework for autonomous vulnerability research), **`exploitation-validator`** (agentic skill), **agentic skills from Trail of Bits**.

This is the operational answer to *"which tools do I deploy on Monday morning?"* — the closest the wiki has yet seen to a vetted shortlist.

## Strengths and Weaknesses

**Strengths.** Community-consensus authorship across CSA + SANS + Unprompted + OWASP Gen AI Security Project gives the briefing weight that any single-vendor source cannot match. The 75+ named reviewers — many serving or former CISOs — establish provenance for the operational recommendations. The Risk Register cross-walked to OWASP LLM 2025 + OWASP Agentic 2026 + MITRE ATLAS + NIST CSF 2.0 + CSA AI Control Matrix V1.0.3 is the wiki's first multi-framework Mythos-era risk register. Honest about human cost (burnout, uncertainty, role-evolution stress) in a way that vendor strategic-forecasts rarely are. The Y2K analogy is rhetorically and structurally apt. The 10-Questions instrument is short, sharp, and unambiguous — a practitioner could run a board update around it on Monday.

**Weaknesses and open scope.**

- **Not a peer-reviewed paper.** It is an *expedited strategy briefing* with community authorship; some upstream data points (181-vs-2 Firefox, 271 Mozilla via Mythos, AISLE 12 OpenSSL zero-days, Gambit Mexican-government report) are cited inline without independent corroboration in the briefing body. These warrant follow-up ingests against primary sources.
- **The 11 Priority Actions assume an aggressive time table** — the briefing acknowledges this and notes that organizational size, complexity, and budget may make some recommendations unrealistic in their stated horizons.
- **Some recommendations are mutually constraining** — e.g., the requirement to *delay patching due to supply-chain risks* with a cooldown period directly competes with the need to *patch faster*; the briefing acknowledges this calls for nuance in policy.
- **MOAK ("Mother of All KEVs")** is named as a stealth startup that uses public frontier models to autonomously create exploits from submitted CVEs — but with no founder, URL, or technical detail. Inline citation; warrants follow-up if MOAK surfaces a public release.
- **No explicit cross-walk to the wiki's [[agentic-ai-security-cmm-2026|CMM 2026]]** — the framework refs in the Risk Register target NIST CSF + MITRE ATLAS + OWASP + CSA AICM v1.0.3; a CMM cross-walk would let an organization map its current CMM tier to the briefing's PA timeline. Candidate wiki work.

## Relations

- **Supports** [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] thesis — adds Mythos vs Opus 4.6 Firefox 181-vs-2 quantitative anchor, the 72% Mythos exploit success rate, Mozilla 271 vulns, AISLE 12 OpenSSL zero-days, DARPA AIxCC 54-vulns-in-4h data point, and the Anthropic Chinese state-sponsored disclosure (Nov 14 2025) attribution. Refines the Aardvark launch date to **30 October 2025**.
- **Supports** [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] thesis — the *AI Vulnerability Storm* framing and the Zero Day Clock TTE-collapse data point are the most quantitatively precise sources the wiki has on the "window-of-exposure collapse" argument.
- **Supports** [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack 2026]] thesis — [[vulnops|VulnOps]] is the candidate Layer-8 *AI-driven vulnerability operations* function complementing the candidate Layer 4½ harness-config audit and Layer 7 AI-driven vuln-discovery layers parked from earlier ingests.
- **Adjacent to** [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — the Risk Register's framework refs (NIST CSF 2.0 / OWASP LLM 2025 / OWASP Agentic 2026 / MITRE ATLAS / CSA AICM v1.0.3) overlap with the CMM Crosswalk page but on a different surface (the CMM grades *agent security maturity*; the Risk Register catalogs *Mythos-era enterprise risk*). Cross-walking the two is a deferred wiki task.
- **Names** [[gadi-evron|Gadi Evron]] (lead author, Knostic CEO, VulnOps co-introducer); [[heather-adkins|Heather Adkins]] (Google CISO, VulnOps co-introducer); [[sergej-epp|Sergej Epp]] ([[zero-day-clock|Zero Day Clock]] creator, Sysdig CISO, Unprompted speaker); [[wendy-nather|Wendy Nather]] ([[cyber-poverty-line|Cyber Poverty Line]] concept creator); [[sounil-yu|Sounil Yu]] (Knostic CTO, contributing author).

**First community-consensus strategic briefing of the Mythos era.** Unlike vendor strategic forecasts ([[anthropic-2026-agentic-coding-trends|Anthropic's 2026 Agentic Coding Trends]], etc.) or single-org research outputs, this briefing is co-authored across **CSA + SANS + Unprompted + OWASP Gen AI Security Project** with 75+ named reviewers. It combines (a) a quantitative AI-Vulnerability-Storm framing anchored on the [[zero-day-clock|Zero Day Clock]] TTE-collapse data, (b) a 13-row Risk Register cross-walked to OWASP LLM 2025 + OWASP Agentic 2026 + MITRE ATLAS + NIST CSF 2.0 + CSA AICM v1.0.3, (c) an 11-row Priority Actions table with aggressive time horizons, (d) the [[vulnops|VulnOps]] long-horizon function as the durable answer to autonomous vulnerability research and remediation, and (e) explicit operational human-cost framing (burnout, role-uncertainty, *"every role is becoming an AI builder role"*).

> [!gap] Upstream data-point follow-ups
> Mozilla 271 vulns via Mythos / 3 CVEs; **MOAK** (Mother of All KEVs) stealth-startup naming — none of these are independently sourced on the wiki yet. Each is an ingest candidate.

> [!check] Closed gaps — 2026-05-15
> **Gambit report on AI-led Mexican-government compromise** → resolved by [[mexican-government-ai-breach|the dedicated incident page]] + [[gambit-security|Gambit Security org page]] (single operator + Claude Code + GPT-4.1 + 9 agencies + ~415M citizen records). **Anthropic 2025-11-14 Chinese state-sponsored disclosure** → already documented as [[gtg-1002-ai-orchestrated-espionage|GTG-1002]] (Anthropic's PRC-nexus campaign disclosure). **CyberGym direct sourcing** → resolved on [[cybergym|the CyberGym concept page]] (UC Berkeley team Wang/Shi/He/Cai/Zhang/Song; 1,507 instances across 188 OSS-Fuzz projects; arXiv 2506.02548; github.com/sunblaze-ucb/cybergym; 35 zero-days discovered + 17 incomplete patches + 10 unique zero-days at 969-day mean persistence). **AISLE** → resolved by [[aisle|the AISLE org page]] + [[aisle-openssl-12-of-12|the 12-of-12 OpenSSL disclosure paper]] (12 CVEs incl. CVSS 9.8 stack-buffer-overflow dating to 1998; 8+ subsystems; 5 AISLE-authored fixes incorporated directly; 6 pre-release catches; collaborative model with OpenSSL Foundation; testimonial endorsements from Bruce Schneier and Daniel Stenberg / curl creator; companion AISLE-curl partnership May 13 2026 + FreeBSD CVE-2026-42511 21-year-old RCE May 7 2026).
