---
type: playbook
title: "Mythos-ready Security Program"
address: c-000068
created: 2026-05-15
updated: 2026-08-21
tags:
  - playbooks
  - strategy
  - mythos
  - vulnops
  - ai-vulnerability-storm
status: developing
origin: produced
scope_axis: [ai-in-sec-defense, sec-against-ai]
authors:
  - "[[gadi-evron|Gadi Evron]]"
  - "Rich Mogull"
  - "Robert T. Lee"
related:
  - "[[mythos-ready-briefing|Mythos-ready paper page]]"
  - "[[vulnops|VulnOps]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[cyber-poverty-line|Cyber Poverty Line]]"
  - "[[citizen-coders|Citizen Coders]]"
  - "[[canadian-bank-secure-sdlc-ai-assessor-scorecard|Canadian-bank Assessor Scorecard]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
  - "[[agentic-soc-cmm|Agentic SOC Capability Maturity Model]]"
  - "[[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
  - "[[security-controls-for-ai-stacks|Security Controls for AI Stacks]]"
  - "[[openant|OpenAnt]]"
  - "[[codex-security|Codex Security]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[raptor|raptor]]"
sources:
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[.raw/papers/mythos-ready-csa-sans-unprompted-v1.0-2026-04-12.pdf]]"
---

# Mythos-ready Security Program — CISO Playbook

Operational instrument from *The "AI Vulnerability Storm": Building a "Mythos-ready" Security Program* (CSA + SANS + Unprompted + OWASP Gen AI Security Project, 2026-04-12, v1.0). Designed for *"the CISO who needs to walk into a room Monday morning with a plan."*

The playbook composes three load-bearing artifacts: a **10-question triage instrument**, a **13-row draft Risk Register** cross-walked to OWASP / MITRE ATLAS / NIST CSF 2.0 / CSA AICM, and an **11-row Priority Actions table** with explicit start times and time horizons. The 90-day Executive/Board Briefing template follows.

The playbook is the operational answer to the [[zero-day-clock|Zero Day Clock]]'s TTE collapse (median 771 days in 2018 to zero-day by 2025), to the [[mythos|Mythos]] / [[anthropic-glasswing-announcement|Glasswing]] capability step-change, and to the parallel pressure of [[citizen-coders|Citizen Coders]] proliferating coding agents into non-developer functions.

**Minimum viable resilience is the entry tier.** Before pursuing maturity, achieve *minimum viable resilience*: realign measurements (cost of exploitation, early detection of compromise, blast radius containment) to a higher tier. Many pre-AI program assumptions are now broken — TTE in hours, incident frequency rising, CVE system at scale risk, shadow IT fragmenting central control as [[citizen-coders|Citizen Coders]] proliferate, threat intelligence lagging behind discovery and exploitation.

## Section 1 — 10 Questions to Understand Your Security Program State and Influence

A short triage instrument to reach ground truth on program state and gauge influence on business functions. Use real examples, not policy statements.

| # | Question | Context |
| :--- | :--- | :--- |
| 1 | **What is our actual stance on AI today?** | Allowed, tolerated, restricted, or unknown. |
| 2 | **Can employees use agentic coding tools in the enterprise today?** | Looping LLM tool use including coding agents (regardless of writing code); do you have security guardrails in place? |
| 3 | **Can employees contribute to open source without legal ambiguity?** | A legal / IP question, not a tech philosophy question. |
| 4 | **Do we have disciplined control of repos, artifacts, and software, including for agentic supply chain (MCP servers, plugins, skills)?** | Source control, package paths, artifact provenance, what is actually allowed in CI/CD and through coding agents. |
| 5 | **Is there a real cooling-off / security gate between code change and production?** | Demonstrates enforcement of security in release cycles and control of software supply chain. |
| 6 | **Is security operational, or primarily advisory?** | Extent to which the security function can directly affect outcomes vs. review-and-escalate. |
| 7 | **What is the fastest this company has made a security-driven production change in the last year?** | Use a real example, not a policy statement. |
| 8 | **Are our critical "crown jewels" explicitly tracked and current?** | Not theoretically important systems — the actual few that matter most and their dependencies. |
| 9 | **Do we know how to get urgent work prioritized by our key third parties?** | Feature requests, bug reports, security escalations, relationship ownership, leverage. |
| 10 | **Does executive leadership have a working definition of urgency?** | *"If everything is a crisis, nothing is urgent."* |

## Section 2 — Mythos-ready Risk Register (DRAFT)

13 risks across three severity tiers. Each risk maps to framework references and to one or more Priority Actions in Section 3.

**Framework prefix legend** (per Appendix B of the source):
- `LLMxx` — OWASP Top 10 for LLM Applications 2025
- `ASIxx` — OWASP Top 10 for Agentic Applications 2026
- `AML.Txxxx` — MITRE ATLAS adversarial-ML techniques
- `GV/ID/PR/DE/RS` — NIST CSF 2.0 functions (Govern / Identify / Protect / Detect / Respond)
- `AICM: xxx` — CSA AI Control Matrix V1.0.3 controls

**Severity legend**: Critical = immediate exposure if unaddressed; High = significant exposure within 45 days; Medium = organizational risk requiring structured attention, no direct exploitable exposure but weakens higher-priority controls.

**Risk type legend**: Threat = external actor capability (raise cost, can't eliminate); Vulnerability = internal exploitable condition (addressable via remediation); Capability gap = defensive function missing or below required level; Governance = organizational/structural failure that amplifies every other risk.

### Critical (5)

| # | Risk | Type | Framework refs | Maps to PA |
| :--- | :--- | :--- | :--- | :--- |
| 1 | **Accelerated Threat Exploitation** — AI-autonomous exploit generation at machine speed | Threat | AML.T0040, AML.T0043, PR.PS, PR.IR, AICM: TVM, MDS, AIS | PA 4, 5 |
| 2 | **Insufficient AI Automation Capabilities** — defenders operating at human speed while attackers operate with AI augmentation | Capability gap | GV.OC, GV.RM, DE.CM, RS.MA, AICM: GRC, HRS, MDS | PA 1, 2 |
| 3 | **Unmanaged AI Agent Attack Surface** — privileged AI agents outside existing control frameworks | Vulnerability | LLM06:2025, ASI02, ASI03, AML.T0047, PR.AA, GV.SC, AICM: MDS, IAM, STA, AIS, CCC | PA 3 |
| 4 | **Inadequate Incident Detection and Response Velocity** — detection and response at human speed against machine-speed attacks | Capability gap | ASI08, AML.T0047, DE.CM, DE.AE, RS.MA, AICM: SEF, LOG | PA 9, 10 |
| 5 | **Cybersecurity Risk Model Outdated** — stakeholder decisions based on pre-AI risk models | Governance | GV.OC, GV.RM, RS.CO, AICM: GRC, A&A | PA 6 |

#### 1 — Accelerated Threat Exploitation

AI models have discovered vulnerabilities and created exploits for over a year. Mythos accelerates this, and non-frontier open-weight models can already achieve much of it at accessible cost. Each patch also becomes an exploit blueprint, because AI accelerates patch-diffing and reverse engineering of fixes.

#### 2 — Insufficient AI Automation Capabilities

The asymmetry is cultural as well as technological. Teams that do not adopt AI coding agents cannot match the speed or scale of AI-augmented threats, regardless of their technical skill.

#### 3 — Unmanaged AI Agent Attack Surface

Agents (often coding agents) are necessary to counter AI-speed threats, but they are privileged, insecure by default, a current focus of attackers, and not covered by existing security controls. They introduce defensive risk (insecure privileged agents inside the environment) and supply-chain risk (MCP servers, VS Code extensions, agentic skills, rules).

#### 4 — Inadequate Incident Detection and Response Velocity

AI has reduced the sophistication and time needed to construct complex attacks. Alert triage volumes, SIEM correlation speed, and containment authorization latency were designed for human-paced threats.

#### 5 — Cybersecurity Risk Model Outdated

Reporting metrics built on pre-AI assumptions about exploit timelines and attack complexity may no longer reflect actual exposure. Outdated models can lead to underfunding of the controls that prevent incidents.

### High (7)

| # | Risk | Type | Framework refs | Maps to PA |
| :--- | :--- | :--- | :--- | :--- |
| 6 | **Incomplete Asset and Exposure Inventory** | Vulnerability | ASI04, AML.T0000, ID.AM, GV.SC, AICM: UEM, DCS, MDS, STA | PA 7 |
| 7 | **Unsecured Software Delivery Pipeline** | Vulnerability | LLM01:2025, LLM05:2025, LLM08:2025, ASI01, AML.T0018, AML.T0051.001, PR.PS, ID.IM, AICM: AIS, CCC, TVM, STA | PA 1 |
| 8 | **Network Architecture Insufficient for Lateral Movement Containment** | Vulnerability | PR.IR, PR.PS, AICM: DCS, IAM | PA 8 |
| 9 | **Continuous Vulnerability Management Maturity Gap** — reactive posture, no VulnOps function | Capability gap | ASI10, ASI06, AML.T0018, ID.RA, ID.AM, DE.CM, AICM: TVM, AIS, STA, GRC | PA 11 |
| 10 | **Threat Detection Dependent on Lagging Intelligence** | Capability gap | AML.T0000, DE.CM, ID.RA, GV.OV, AICM: TVM, LOG | PA 9, 10 |
| 11 | **Innovation Governance and Oversight Deficit** | Governance | GV.OC, GV.RM, GV.RR, GV.OV, AICM: GRC, A&A | PA 2, 4 |
| 12 | **Regulatory and Liability Exposure from AI-Discovered Vulnerabilities** | Governance | GV.OC, GV.RM, GV.RR, AICM: GRC, A&A | PA 1, 4 |

#### 6 — Incomplete Asset and Exposure Inventory

AI-accelerated attacker capabilities change which assets are at highest risk. Attackers can scan an entire OS codebase at accessible cost and enumerate exposure faster than the organization can inventory it. Proliferation of coding agents to non-developers further fragments central IT visibility.

#### 7 — Unsecured Software Delivery Pipeline

Code from humans and AI agents ships without consistent security review. AI-generated code introduces vulnerabilities at higher volume than manual development: same defect rate, more code, more capable adversary. Without LLM-driven review integrated into the pipeline, exploitable flaws reach production before defenders can find them.

#### 8 — Network Architecture Insufficient for Lateral Movement Containment

A flat or insufficiently segmented network gives every successful exploit leverage. AI-driven attacks worsen this through automated multi-hop lateral movement. With AI-accelerated discovery, architectural segmentation becomes the primary control limiting blast radius.

#### 9 — Continuous Vulnerability Management Maturity Gap

Quarterly pen tests and reactive patching cycles cannot keep pace with continuous AI-driven discovery. Existing CVE/NVD infrastructure and patch prioritization workflows were built for dozens of critical CVEs per month, not hundreds.

#### 10 — Threat Detection Dependent on Lagging Intelligence

CVE- and KEV-based intelligence is structurally outpaced by AI discovery rates. Novel vulnerabilities have no KEV listing by definition.

#### 11 — Innovation Governance and Oversight Deficit

Without cross-functional governance, onboarding and deployment of any new control runs into approval friction that slows adoption. AI-accelerated attacker timelines give this friction a harder deadline.

#### 12 — Regulatory and Liability Exposure from AI-Discovered Vulnerabilities

The **EU AI Act (August 2026)** introduces automated audit, incident reporting, and cybersecurity requirements around AI. Existing regulations use reasonableness as a test. When AI can find significantly more vulnerabilities at accessible cost, the standard of what constitutes reasonable defensive effort shifts. Boards face direct-financial-exposure questions about whether they used available AI defensive tools.

### Medium (1)

| # | Risk | Type | Framework refs | Maps to PA |
| :--- | :--- | :--- | :--- | :--- |
| 13 | **AI Hype and Confusion Causing Systematic Inaction** | Governance | GV.OC, GV.RM, AICM: GRC, HRS | PA 1 |

#### 13 — AI Hype and Confusion Causing Systematic Inaction

Signal-to-noise collapse in threat and technology guidance. The volume of AI-related security guidance, commentary, and vendor claims exceeds anything the industry has experienced. Teams that dismiss the shift as hype, or exhaust attention on low-signal content, will miss critical threat-landscape changes they need to react to.

## Section 3 — Priority Actions (DRAFT, Aggressive Time Table)

11 actions with explicit start times and time horizons. *"For the CISO who needs to walk into a room Monday morning with a plan."*

| # | Action | Category | Risk | Start | Horizon |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | **Point Agents at Your Code and Pipelines** | Risk Control | Critical | This week | Ongoing |
| 2 | **Require AI Agent Adoption** | Operational Enabler | Critical | This week | Ongoing |
| 3 | **Defend Your Agents** | Risk Control | Critical | This month | 45 days |
| 4 | **Establish Innovation Acceleration Governance** | Governance | Critical | This week | 6 months |
| 5 | **Prepare for Continuous Patching** | Risk Control | Critical | This week | 45 days |
| 6 | **Update Risk Models and Reporting** | Governance | Critical | This week | 45 days |
| 7 | **Inventory and Reduce Attack Surface** | Risk Control | High | This month | 90 days |
| 8 | **Harden Your Environment** | Risk Control | High | This month | 6 months |
| 9 | **Build a Deception Capability** | Risk Control | High | Next 90 days | 6 months |
| 10 | **Build an Automated Response Capability** | Risk Control | High | Next 90 days | 12 months |
| 11 | **Stand Up VulnOps** | Risk Control | Critical | Next 6 months | 12 months |

### 1 — Point Agents at Your Code and Pipelines

Turn agents and LLM capabilities inward on your own code and dependencies. Start by asking an agent for a security review of any code, build toward a full audit within CI/CD, and shift left by adding capabilities directly into developers' coding agents. **All code, human or AI-generated, should pass LLM-driven security review before merge.** Commercial tools include [[claude-code-security|Claude Code Security]] (Anthropic) and [[codex-security|Codex Security]] (OpenAI). Open-source options include [[openant|OpenAnt]] (Knostic), [[raptor|raptor]] (a Claude Code framework), the `exploitation-validator` agentic skill, and agentic skills from Trail of Bits.

### 2 — Require AI Agent Adoption

Formalize AI agent usage (mostly coding agents) as part of all security functions, with mandatory security controls and oversight in place. While defensive AI technology has not yet caught up, these agents let staff stay effective in the new threat landscape and accelerate beyond *"human speed."* Optional adoption programs have not been shown to overcome cultural barriers, and adoption is a limiting factor for the rest of this table.

### 3 — Defend Your Agents

Without agents, most tasks on this list are untenable, but the agents themselves must be defended. They are not covered by existing controls and introduce both cyber-defense and agentic supply-chain risk. **The agent harness — prompts, tool definitions, retrieval pipelines, and escalation logic — is where the most consequential failures occur; audit it with the same rigor as the agent's permissions.** Before deploying agents in or adjacent to production environments, define scope boundaries, blast-radius limits, escalation logic, and human override mechanisms. *Do not wait for industry governance frameworks. Define your own now.*

### 4 — Establish Innovation Acceleration Governance

Stand up a cross-functional mechanism (Security, Legal, Engineering) to evaluate new offensive threats and accelerate onboarding of defensive technologies. Without it, every other action runs into approval friction that slows deployment to the attacker's advantage.

### 5 — Prepare for Continuous Patching

With increased vulnerability discovery and reporting — and [[anthropic-glasswing-announcement|Glasswing]] making [[mythos|Mythos]] available to significant software vendors — prepare triage and deployment capacity to handle a potential flood of patches as new critical vulnerabilities are disclosed.

### 6 — Update Risk Models and Reporting

Review and update security risk metrics, reporting, and business risk calculations to reflect AI-accelerated exploit timelines and attack complexity. Pre-AI assumptions about patch windows, exploit scarcity, and incident frequency may no longer hold, and outdated models can underfund controls. Communicate and collaborate with stakeholders; map and prioritize potential effects on business, reporting, and projections.

### 7 — Inventory and Reduce Attack Surface

Use agents to accelerate inventory and build toward full-coverage inventory over 45 days. Generate real SBOMs. Aggressively shut down unneeded or unmaintained functionality, phase out suppliers that no longer comply with updated vulnerability-management requirements, and isolate or air-gap at-risk systems. *You cannot patch, segment, or defend what you don't know exists.*

### 8 — Harden Your Environment

The basics remain valid. Implement egress filtering (*it blocked every public log4j exploit*).[^egress] Enforce deep segmentation and zero trust where possible. Lock down the dependency chain. Mandate phishing-resistant MFA for all privileged accounts. Every boundary increases attacker cost. AI can accelerate software minimization, which reduces the operational overhead of second-order functions such as patching — for example, minimizing base OS images and replacing third-party libraries with framework primitives.

### 9 — Build a Deception Capability

This capability is attack-tool and vulnerability independent: it identifies attacks and attackers based on TTPs. Deploy canaries and honey tokens, layer behavioral monitoring, pre-authorize containment actions, and build response playbooks that execute at machine speed.

### 10 — Build an Automated Response Capability

Improve detection engineering and incident response to be systemic and, to the degree possible, autonomous. Examples include asset and user behavioral analysis, pre-authorized containment actions, and response playbooks that execute at machine speed.

### 11 — Stand Up VulnOps

Long term, there is no alternative to building a permanent [[vulnops|Vulnerability Operations (VulnOps)]] function — staffed and automated like DevOps, but for autonomous vulnerability research and remediation. It owns continuous discovery of zero-day vulnerabilities across the entire software estate (own code through third-party) and establishes automated remediation pipelines. **Design VulnOps around triage discipline from the start.**

## Section 4 — Executive and Board Briefing Template

Two talking points and a five-component aggressive 90-day plan.

**Talking Point: AI Accelerates Both Sides.** AI is making us faster and more competitive; the same capabilities make attackers faster and more dangerous. Time-to-disruption compressed from weeks to hours; *permanent acceleration, not a temporary spike*. Turned inward, these tools let us find and fix our own weaknesses before adversaries do.

**Talking Point: An Aggressive Plan Is Needed.** An appropriately funded foundation lets programs adapt rather than merely react in a crisis. The speed and volume of what we must handle has changed. *This is not an open-ended AI initiative.*

**90-day aggressive plan** (clear owners and outcomes):

- **Increase People and Capacity.** Repurpose existing staff and onboard headcount / contractor capacity to handle increases in triage, remediation, and incidents — protect experienced staff from burnout, especially as the first wave of [[anthropic-glasswing-announcement|Glasswing]] patches hits.
- **Deploy AI Tooling.** Formalize AI agent usage across all security functions as standard practice: scanning own code, AI-driven review before code ships, augmenting teams with purpose-built agents.
- **Harden Infrastructure.** Update asset inventories; reduce unnecessary exposure; enforce segmentation, Zero Trust, egress filtering, phishing-resistant authentication. Validate across internal systems and key third-party providers (MSPs, SOCs).
- **Accelerate Procurement and Governance.** Align Security + Legal + Engineering on threat evaluation and fast-track priority defensive-technology onboarding. Current approval cycles are too slow for the coming environment.
- **Update Playbooks.** Update technical + communications response plans to execute at required speed and scale — including pre-authorized containment and coordination for *simultaneous* incidents.
- **Track Progress.** Regular check-ins throughout the 90-day period to capture results and identify roadblocks.

## Section 5 — How to Adapt

The full Risk Register and Priority Actions assume an aggressive time table that may not be realistic for every organization. Adapt by:

- **Organization size, complexity, and budget.** Complicated environments and entirely-SaaS environments adapt differently; some have agility, others have budget.
- **Mutual constraints.** Some recommendations are contradictory if followed as-is — e.g., *delay patching due to supply-chain risks with a cooldown period* directly competes with *patch faster*. Apply nuance in decision-making, policy, mitigating controls, or per-incident handling.
- **Below the [[cyber-poverty-line|Cyber Poverty Line]].** Engage ISACs, CERTs, and sector coordinating groups now. Defenders must leverage coordinating groups — especially when considering organizations that fall below the Cyber Poverty Line, as introduced by Wendy Nather.

## Adjacent Wiki Instruments

- [[canadian-bank-secure-sdlc-ai-assessor-scorecard|Canadian-Bank Assessor Scorecard]] — first playbook on the wiki; sector-specific (Canadian FRFI) ~65-question scorecard with L1-L5 section maturity. Use for *organization-specific maturity scoring* (compares an organization to a regulatory floor); use the Mythos-ready playbook for *industry-wide near-term operational response*.
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — measures *agent-security maturity* across nine domains; the Mythos-ready Risk Register is complementary (it catalogs *Mythos-era enterprise risk* across the broader cyber program). It scores the maturity behind PA 3 (Defend Your Agents), which is the securing-the-agents layer shared with the Agentic SOC pair below.
- [[agentic-soc-cmm|Agentic SOC Capability Maturity Model]] and [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] — the defender-operations pair. The CMM scores the maturity of the SOC capabilities several Priority Actions build, which partly fulfils the deferred CMM-to-PA crosswalk: **PA 9 (Build a Deception Capability)** and **PA 10 (Build an Automated Response Capability)** are the detection-engineering, response, and tradecraft (D6) functions the CMM scores, and **PA 11 (Stand Up VulnOps)** is its Exposure & VulnOps function. The CMM's gating rule — *maturity gates autonomy*, so a function runs only at the autonomy its weakest governing domain supports — is the discipline behind PA 10's "systemic and, to the degree possible, autonomous" qualifier: it names how far response can safely be delegated and what to mature next to delegate further. Where the Mythos-ready table says *do these things*, the Agentic SOC CMM says *how mature you are at them and how much you can safely automate*.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — production-paths thesis on the offensive/defensive AI-vuln-discovery axis. This playbook's PA 1 names the specific tools to deploy.
- [[owasp-ai-exchange|OWASP AI Exchange]] — states a governance entry tier of its own, and bounds the compliance-as-driver argument. Its `AI PROGRAM` control sets a three-step bare minimum for AI governance focused on security — inventory current AI use and ideas, run risk analysis to identify threats, controls and owners, then continue with the next GUARD step — inside an eight-step first iteration ([`/go/aiprogram/`](https://owaspai.org/go/aiprogram/)). That is ordered and unclocked, where this playbook's eleven Priority Actions carry explicit start times and cover the whole security program rather than AI governance alone. Its `CHECK COMPLIANCE` control also warns against treating legislation as the guide, because a law's scope need not include the organization's own stakes — the EU AI Act does not cover risks to company secrets ([`/go/checkcompliance/`](https://owaspai.org/go/checkcompliance/)). That bounds the reasonableness-shift argument in Risk 12 without changing it.
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] — its six-layer inventory holds no deception primitive, and records **PA 9 (Build a Deception Capability)** as the adjacent instrument that treats deception as first-class. The Exchange's AI-specific honeypots supply the AI-asset decoy examples that inventory has no layer to receive.

## Notes

[^egress]: Egress-filtering claim quoted from the source briefing. See [[mythos-ready-briefing|Mythos-ready paper]] and the original PDF (`.raw/papers/mythos-ready-csa-sans-unprompted-v1.0-2026-04-12.pdf`), Section 3, Priority Action 8.
