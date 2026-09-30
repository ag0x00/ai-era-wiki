---
type: entity
entity_type: organization
org_type: vendor
parent_org: "[[google]]"
title: "Wiz"
created: 2026-04-30
updated: 2026-09-30
tags:
  - entities
  - organizations
  - cnapp
  - ai-spm
  - cloud-security
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
role: "Cloud-native application protection platform (CNAPP) vendor; AI-SPM launch (2023); now part of Google Cloud Security; Red Agent Opus-powered continuous pentester (2026)"
related:
  - "[[onyx-platform]]"
  - "[[wiz-ai-app]]"
  - "[[wiz-ai-app-launch]]"
  - "[[wiz-ai-spm]]"
  - "[[ai-spm]]"
  - "[[google]]"
  - "[[codemender]]"
  - "[[google-cloud-codemender-preview]]"
  - "[[claude-partners-opus-cybersecurity]]"
  - "[[taming-shai-hulud-with-ai-talk]]"
  - "[[unprompted-conference-march-2026]]"
sources:
  - "[[.raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md]]"
  - "https://onyx.security/platform"
  - "[[.raw/articles/introducing-wiz-ai-app-2026-09-30.md]]"
  - "https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/wiz-acquisition/"
  - "https://www.wiz.io/"
  - "https://www.wiz.io/blog/ai-security-posture-management"
  - "https://www.wiz.io/blog/wizdom-product-launches-2025"
  - "https://www.wiz.io/blog/red-agent-claude-opus"
  - "[[.raw/articles/claude-partners-opus-cybersecurity-2026-05-23.md]]"
  - "[[.raw/articles/find-and-fix-software-vulnerabilities-with-codemender-2026-09-18.md]]"
verified: 2026-09-30
verified_against:
  - ".raw/articles/claude-partners-opus-cybersecurity-2026-05-23.md"
  - ".raw/articles/introducing-wiz-ai-app-2026-09-30.md"
  - ".raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md"
verified_findings: 0
verified_note: "AI-APP, Red Agent and Onyx claims checked against archives and official pages; earlier CodeMender source not reopened."
---

# Wiz

Wiz is a cloud-native application protection platform (CNAPP) vendor founded in 2020. In [its 2023 AI-SPM launch](https://www.wiz.io/blog/ai-security-posture-management), the company claimed to be the first CNAPP with native AI security capabilities. Wiz expanded the offering to agent and MCP discovery in 2025. [Google completed its acquisition of Wiz in March 2026](https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/wiz-acquisition/); Wiz now operates within Google Cloud Security.

## Core platform

Wiz CNAPP combines: CSPM (cloud security posture), CWPP (cloud workload protection), CIEM (cloud entitlements), DSPM (data security posture), code/IaC scanning, and now AI-SPM. The **Wiz Security Graph** correlates cloud, identity, data, and code exposures; the [[wiz-ai-app|Wiz AI-APP]] announcement extends that graph to AI models, agents, tools, and runtime activity, as described in [[wiz-ai-app-launch|Wiz AI-APP Launch]].[^wiz-ai-app]

## AI-relevant offerings

| Product | Role |
|---|---|
| **[[wiz-ai-spm\|Wiz AI-SPM]]** | AI Security Posture Management — model inventory, configuration risk, attack-path correlation to AI assets, AI-BOM, runtime monitoring of agent behavior |
| **[[wiz-ai-app\|Wiz AI-APP]]** | Graph-powered AI application inventory, attack-path analysis, and runtime detection[^wiz-ai-app] |

## Red Agent — Opus-powered offensive testing (2026)

[Wiz Red Agent](https://www.wiz.io/blog/red-agent-claude-opus) uses Claude Opus models to test production web applications and APIs. Wiz reports that it scans more than 150,000 production assets per week, surfaces thousands of high- and critical-severity findings, and achieved zero false positives after its testing period.[^red-agent] The stated method analyzes application logic, chains steps, and adapts to server responses. These are vendor-reported results; the [[claude-partners-opus-cybersecurity|Opus partner ecosystem]] repeats them without an independent evaluation. Wiz VP AI & Threat Research Alon Schindel said, *"Security teams are no longer limited by a lack of data, but by the ability to act on it."*[^opus-partners]

## Green Agent and AI Threat Defense (2026)

Google Cloud's [[codemender|CodeMender]] preview (2026-07-21) is the first published account of how Wiz composes with Google Cloud security products after the acquisition ([[google-cloud-codemender-preview|source summary]]). Within **AI Threat Defense**, Wiz "orchestrates agentic application security": it calls CodeMender to scan code, enriches the findings in the Wiz Security Graph with deployment context, and triggers Red Agent for AI pentesting. The [Wiz Green Agent](https://www.wiz.io/blog/introducing-wiz-green-agent) then directs CodeMender to generate and test patches carrying that application context, and Google describes Wiz as the command center for governing and scaling remediation across the offering.

Red Agent and Green Agent form a paired attack-and-repair loop over one asset graph. The graph is what distinguishes this from repository-only scanning: production reachability, not just source reachability, feeds prioritization. Google states CodeMender scanning under Wiz orchestration is "coming soon," so the integration is announced rather than shipped.

The premise Google states for the offering is the adversary's own use of AI: adversarial AI threats accelerate attacks on code, and security teams need machine-speed defenses that automate code remediation. Red Agent and Green Agent therefore run offensive and defensive AI against the same estate for one purpose, which is to close a finding before an AI-driven attacker reaches it.

## Notable 2025–2026 events

- **Wizdom 2025** [expanded AI-SPM](https://www.wiz.io/blog/wizdom-product-launches-2025) to agent and MCP discovery, with agent posture and runtime risks connected in the Security Graph
- **OpenAI Platform connector** added to AI-SPM
- **NVIDIA Enterprise AI Factory integration**
- **Google Cloud acquisition** — [completed in March 2026](https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/wiz-acquisition/); the AI Threat Defense composition is described above

## At Unprompted (March 2026)

Rami McCarthy presented [[taming-shai-hulud-with-ai-talk|Zeal of the Convert: Taming Shai-Hulud with AI]] at the [[unprompted-conference-march-2026|Unprompted Conference (March 2026)]] — a practitioner post-mortem on responding to the 2025 Shai-Hulud supply-chain campaigns by moving from single-purpose scrapers to multi-agent triage engines that parallelize victimology and automate secret-impact analysis.

## Wiki references

- [[wiz-ai-spm|Wiz AI-SPM]] — primary product page
- [[ai-spm|AI Security Posture Management]] practice page
- [[agentic-ai-security-reference-architecture|RA]] Data plane (supply-chain scanning) and Observability plane (AI-SPM)

## AI-SPM positioning

Wiz's [AI-SPM launch](https://www.wiz.io/blog/ai-security-posture-management) describes graph-based attack-path analysis across AI services, identities, data, and cloud exposures. The company presents this correlation within its broader CNAPP platform.

The [[onyx-platform|Onyx Platform (Onyx AI Control Plane)]] [also advertises AI-SPM configuration hardening](https://onyx.security/platform), alongside an inline MCP gateway. These vendor descriptions establish product scope but do not compare coverage or effectiveness.

[^wiz-ai-app]: [Wiz — Introducing Wiz AI Application Protection Platform](https://www.wiz.io/blog/introducing-wiz-ai-app), 2026-03-23, the graph-based AI-APP announcement.
[^red-agent]: [Wiz — Red Agent and Claude Opus](https://www.wiz.io/blog/red-agent-claude-opus), 2026-04-30, vendor-reported weekly asset volume, severity counts, and zero-false-positive claim after its testing period.
[^opus-partners]: [Anthropic — How our partners are putting Opus to work for cybersecurity](https://claude.com/blog/how-our-partners-are-putting-opus-to-work-for-cybersecurity), 2026-05-21, the quoted Wiz executive statement and repeated Red Agent results.
