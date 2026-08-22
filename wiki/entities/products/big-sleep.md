---
type: entity
entity_type: product
title: "Big Sleep (Google Project Zero + DeepMind)"
address: c-000035
created: 2026-05-13
updated: 2026-08-21
tags:
  - products
  - google
  - project-zero
  - deepmind
  - big-sleep
  - vuln-discovery
  - ai-vuln-discovery
  - llm-agents
status: developing
scope_axis:
  - ai-in-sec-defense
vendor: "Google"
homepage: "https://projectzero.google/"
aliases:
  - "Project Naptime (predecessor framework)"
related:
  - "[[google]]"
  - "[[google-big-sleep-projectzero]]"
  - "[[codemender]]"
  - "[[google-codemender-deepmind]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[mdash]]"
  - "[[xbow]]"
  - "[[mythos]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[google-cloud-codemender-preview]]"
sources:
  - "https://projectzero.google/2024/10/from-naptime-to-big-sleep.html"
  - "https://cloud.google.com/blog/products/identity-security/cloud-ciso-perspectives-our-big-sleep-agent-makes-big-leap"
  - "https://www.anthropic.com/glasswing"
---

# Big Sleep (Google Project Zero + DeepMind)

**Sources:** [Project Zero — From Naptime to Big Sleep (Oct 2024)](https://projectzero.google/2024/10/from-naptime-to-big-sleep.html) · [Google Cloud Blog — Big Sleep agent makes a big leap](https://cloud.google.com/blog/products/identity-security/cloud-ciso-perspectives-our-big-sleep-agent-makes-big-leap)

**Big Sleep** is Google's AI agent for vulnerability discovery — a collaboration between **Google Project Zero** and **Google DeepMind**. It grew out of the earlier **Project Naptime** framework that achieved state-of-the-art on Meta's [[cyberseceval|CyberSecEval2]] benchmarks. Big Sleep's signature methodology is **variant analysis**: given a previously-fixed vulnerability (commit message + diff), find similar patterns elsewhere in the codebase. Project Zero positions this narrower task as a better fit for current LLMs than open-ended vulnerability discovery.

## Disclosed Capability Milestones

| Date | Milestone | Source |
|---|---|---|
| **June 2024** | Project Naptime announced — LLM-assisted vuln research framework; state-of-the-art on Meta CyberSecEval2 | Project Zero (predecessor post) |
| **October 2024** | Naptime → Big Sleep rebrand; **first real-world vulnerability** disclosed (SQLite stack buffer underflow); reported and patched same day, before any release | [[google-big-sleep-projectzero\|Project Zero, Oct 2024]] |
| **July 2025** | **SQLite CVE-2025-6965** disclosed — vulnerability "known only to threat actors and at risk of being exploited"; first time AI agent "directly foiled efforts to exploit a vulnerability in the wild" | Google Cloud Blog |
| **August 2025** | Public report: ~20 security vulnerabilities found | TechCrunch coverage |
| **May 2026** | Named in [[anthropic-glasswing-announcement\|Anthropic Glasswing announcement]] as Google's parallel AI-cyber tool alongside [[codemender\|CodeMender]] | Heather Adkins (VP Security Engineering) quote |
| **July 2026** | Not named. CodeMender enters managed preview on Google Cloud; Big Sleep remains vendor-internal, and the two agents' availability diverges | [[google-cloud-codemender-preview\|Google Cloud, Jul 2026]] |

## Methodology

Big Sleep's core operating mode is **variant analysis**:

1. Input: a previously-fixed vulnerability (commit message + diff).
2. Search: scan the current repository (at HEAD) for related patterns that may not have been fixed.
3. Output: candidate findings with reasoning trace.

This framing is chosen for three reasons (per Project Zero's October 2024 post):

- **Real-world exploit-variant pattern**: ["over 40% of the 0-days discovered were variants of previously reported vulnerabilities"](https://blog.google/threat-analysis-group/0-days-exploited-wild-2022/). Fuzzing fails to catch variants; attackers use manual variant analysis cost-effectively.
- **LLM task fit**: variant analysis "remove[s] a lot of ambiguity from vulnerability research, and start[s] from a concrete, well-founded theory: 'This was a previous bug; there is probably another similar one somewhere.'"
- **Asymmetric defender advantage**: pre-release discovery means "no scope for attackers to compete: the vulnerabilities are fixed before attackers even have a chance to use them."

## Architectural Position

Big Sleep is paired with [[codemender|CodeMender]] (Google DeepMind, Oct 2025) as Google's two-pronged vuln-discovery + patching stack:

| Capability | Agent |
|---|---|
| **Discovery / variant analysis** | Big Sleep |
| **Patching / proactive rewrite** | CodeMender |

The integration architecture between the two is not documented in public sources.

## Position in the Wiki

Big Sleep is Google's defender-side analogue to:

- [[mythos|Anthropic Claude Mythos Preview]] (vendor frontier model used for discovery)
- [[mdash|Microsoft MDASH]] (orchestrated multi-model harness)
- [[xbow|XBOW]] (offensive-orientation harness)

All four are anchors of [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]], converging on the architectural argument that orchestration outperforms raw model capability. Big Sleep's distinguishing feature is the **variant-analysis specialization** — a narrower task framing that makes the discovery problem more tractable than open-ended search.

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-2026|CMM]] D7 (Observability & Detection) L5+** — Big Sleep is a defender-side discovery primitive; CVE-2025-6965 is the canonical example of AI-foiled in-the-wild exploitation.
- **[[agentic-ai-security-cmm-2026|CMM]] D8 (Supply Chain & AI-BOM)** — pre-release OSS vulnerability discovery (SQLite as primary example) hardens upstream supply-chain.

## Open Questions

- **Model attribution**: Project Zero does not name the LLM. Gemini-family is presumed (DeepMind co-developed) but unconfirmed.
- **Productization timeline**: Big Sleep remains research-stage with select customer access via Google Cloud Security. Public GA timeline / pricing / customer base not disclosed.
- **Operational integration**: how Big Sleep findings feed [[codemender|CodeMender]] patching is not documented. The [[google-cloud-codemender-preview|July 2026 CodeMender preview]] does not mention Big Sleep, and the shipped product does its own scanning rather than consuming Big Sleep output.
- **Glasswing role**: Google is a [[glasswing|Project Glasswing]] partner with Mythos access via Vertex AI. Whether Big Sleep itself uses Mythos, Gemini, or both is unclear.

## See Also

- [[google-big-sleep-projectzero|Big Sleep foundational paper (Oct 2024)]] — source summary.
- [[codemender|CodeMender]] — patching-side counterpart.
- [[google|Google]] — vendor.
- [[anthropic-glasswing-announcement|Glasswing announcement]] — May 2026 coalition naming Big Sleep.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — wiki thesis.
- [[mdash|MDASH]] / [[xbow|XBOW]] / [[mythos|Claude Mythos Preview]] — adjacent ecosystem.
- [[cybergym|CyberGym]] — public leaderboard (Big Sleep is not listed; CyberSecEval2 was Naptime's benchmark).
