---
type: paper
title: "Google Project Zero"
address: c-000033
created: 2026-05-13
updated: 2026-08-24
tags:
  - papers
  - google
  - project-zero
  - deepmind
  - big-sleep
  - vuln-discovery
  - ai-vuln-discovery
  - llm-agents
  - sqlite
status: summarized
scope_axis:
  - ai-in-sec-defense
publication_date: 2024-10-31
authors:
  - "Big Sleep team (Google Project Zero + Google DeepMind)"
publisher: "Google Project Zero"
source_url: https://projectzero.google/2024/10/from-naptime-to-big-sleep.html
archived_copy: ".raw/articles/google-big-sleep-projectzero-2024-10-31.md"
no_public_url: ""
related:
  - "[[big-sleep]]"
  - "[[google]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[codemender]]"
  - "[[google-codemender-deepmind]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[autonomous-code-security-google-talk]]"
sources:
  - "[[.raw/articles/google-big-sleep-projectzero-2024-10-31.md]]"
  - ".raw/talks/2026-03-03_Heather-Adkins-and-Four-Flynn_Evaluating-Threats-Automating-Defense_transcript.md"
---

# From Naptime to Big Sleep

**Source:** [Project Zero — From Naptime to Big Sleep (October 31, 2024)](https://projectzero.google/2024/10/from-naptime-to-big-sleep.html). Local copy: `.raw/articles/google-big-sleep-projectzero-2024-10-31.md`.

## Source Summary

Foundational technical announcement of **Big Sleep** — the Google Project Zero + Google DeepMind collaboration that grew out of the earlier **Project Naptime** framework for LLM-assisted vulnerability research. The post documents Big Sleep's **first real-world vulnerability**: an exploitable stack buffer underflow in SQLite, reported and patched before reaching any official release. Project Zero claims it as *"the first public example of an AI agent finding a previously unknown exploitable memory-safety issue in widely used real-world software."*

The strategic frame: **variant analysis** — given a previously-fixed vulnerability, find similar patterns elsewhere. This narrower task is "a better fit for current LLMs than the more general open-ended vulnerability research problem," reduces ambiguity, and starts from "a concrete, well-founded theory: 'This was a previous bug; there is probably another similar one somewhere.'" Project Zero's broader thesis: AI narrows the gap fuzzing leaves behind, with potential for "an asymmetric advantage for defenders."

## Key Contributions

### Methodology — variant-analysis framing

- Big Sleep is fed a previously-fixed vulnerability (commit message + diff) and asked to review the current repository (at HEAD) for related issues that might not have been fixed.
- Variant analysis is chosen because (a) "continued in-the-wild discovery of exploits for variants of previously found and patched vulnerabilities" — fuzzing fails to catch variants; (b) for attackers, manual variant analysis is cost-effective; (c) LLMs reduce ambiguity when given a starting point.
- The Naptime framework's [[cyberseceval|CyberSecEval2]] (Meta) state-of-the-art benchmarks were the prior milestone; Big Sleep is the productized continuation.

### First disclosed finding — SQLite stack buffer underflow

- **Vulnerability**: in `seriesBestIndex` of SQLite's `ext/misc/series.c`, a sentinel value `-1` in an index-typed field `iColumn` (used to indicate ROWID) was not handled correctly. The function computed `iCol = pConstraint->iColumn - SERIES_COLUMN_START`, which produces a negative value when `iColumn == -1`. The subsequent `aIdx[iCol] = i` then writes below the `aIdx` stack buffer, corrupting the next field (`pConstraint` pointer's low 32 bits).
- In debug builds an `assert(iCol >= 0 && iCol <= 2)` catches the condition; **release builds lack the assertion** and the corruption proceeds.
- Discovery context: Big Sleep was given recent SQLite commits (manually filtered to remove trivial and docs-only changes); for each commit, asked to find related issues at HEAD.
- Disclosure: reported "early October" 2024, fixed [same day](https://sqlite.org/src/info/41d58a014ce89356) by SQLite maintainers. Caught before any release, so no users impacted.
- Inspiration: Team Atlanta (the DARPA AIxCC team that became the seed for [[mdash|Microsoft MDASH]]) had earlier discovered a null-pointer dereference in SQLite at the AIxCC event — Project Zero used SQLite as their testing target to see if they could find something more serious. **This is the same Team Atlanta** that [[mdash-defense-at-ai-speed|MDASH's announcement]] credits as the source of several Microsoft ACS team members.

### Defensive framing

> "Finding vulnerabilities in software before it's even released, means that there's no scope for attackers to compete: the vulnerabilities are fixed before attackers even have a chance to use them. Fuzzing has helped significantly, but we need an approach that can help defenders to find the bugs that are difficult (or impossible) to find by fuzzing, and we're hopeful that AI can narrow this gap. We think that this is a promising path towards finally turning the tables and achieving an asymmetric advantage for defenders."

## Subsequent Public Milestones (post-paper)

While not part of this October 2024 post, public follow-ups establish Big Sleep's trajectory through 2025-2026:

- **August 2025**: Google reports Big Sleep has found ~20 security vulnerabilities ([TechCrunch coverage](https://techcrunch.com/2025/08/04/google-says-its-ai-based-bug-hunter-found-20-security-vulnerabilities/)).
- **July 2025**: SQLite **CVE-2025-6965** disclosure — Big Sleep finds a vulnerability that was "known only to threat actors and was at risk of being exploited." Google claims this is the first time an AI agent has directly foiled efforts to exploit a vulnerability in the wild.
- **May 2026**: [[anthropic-glasswing-announcement|Anthropic Glasswing announcement]] names Big Sleep in Heather Adkins's quote as Google's parallel AI-powered cybersecurity tool — positioning Big Sleep as Google's defender-side analogue to Anthropic's Mythos deployment.
- **March 2026**: Adkins discloses Big Sleep's five-phase architecture at [un]prompted and gives the false-positive rate as zero, end-to-end and without human involvement, on deep memory-safety bugs. The control is a working exploit built as proof of vulnerability before any finding is reported, and Gemini writes the report.[^google-talk] Findings go to a public issue tracker; as of the morning of the talk, all but five were fixed.

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-2026|CMM]] D7 (Observability & Detection) L5+** — Big Sleep is a defender-side variant-analysis primitive; its CVE-2025-6965 disclosure (first AI-foiled in-the-wild exploit) is a candidate L5+ Leading-Edge tier evidence item.
- **[[agentic-ai-security-cmm-2026|CMM]] D8 (Supply Chain & AI-BOM)** — pre-release vulnerability discovery in OSS dependencies (SQLite is used as a primary example) is a supply-chain primitive.
- **[[agentic-ai-security-reference-architecture|RA]] Observability Plane** — agentic vuln discovery sits on the defender side; Big Sleep is a candidate primitive.

## Convergence with Other Wiki Sources

- **Naptime-to-CyberSecEval2 lineage**: Meta's [[cyberseceval|CyberSecEval2]] benchmark predates Big Sleep; Project Naptime achieved state-of-the-art on it. CyberSecEval sits alongside [[cybergym|CyberGym]] as benchmark surface for AI vulnerability discovery.
- **Team Atlanta — Project Zero, then Microsoft**: Team Atlanta's DARPA AIxCC SQLite null-pointer-dereference work inspired Big Sleep's SQLite testing focus. Several Team Atlanta members later joined Microsoft's ACS team that built [[mdash|MDASH]]. The cross-organization personnel flow is the human-capital signal underlying the May 2026 tri-vendor convergence.
- **CodeMender symmetry**: [[google-codemender-deepmind|CodeMender (Oct 2025)]] is Google's parallel agent for the *patching* half of the workflow that Big Sleep solves on the *discovery* half. Both DeepMind-affiliated; both AI-agent design.

## Limitations

- **Single-finding paper.** This is one disclosed vulnerability with detailed walkthrough; broader recall numbers are not published.
- **Research-stage at publication.** Project Zero explicitly notes "Our project is still in the research stage"; the productization signal comes from subsequent posts.
- **No model attribution.** The post does not name which LLM Big Sleep uses (Gemini-family is presumed but unconfirmed in this source).
- **Variant analysis only.** This methodology assumes a previously-fixed vulnerability to seed each search. Open-ended discovery (find-anything-from-scratch) is explicitly out-of-scope for the framing.

## Open Questions

- Big Sleep's production capability surface. [[autonomous-code-security-google-talk|The March 2026 talk]] refreshed the technical detail layer with a five-phase architecture and a false-positive figure, so the disclosure gap against Mythos and MDASH is narrower than it was. What is still missing is a result against a shared corpus: Big Sleep appears on no public leaderboard, and Google has published no recall or precision figure for it.
- Relationship between Big Sleep and [[google-codemender-deepmind|CodeMender]]. Google states that CodeMender's input is a verified Big Sleep vulnerability and presents the pair as one end-to-end discovery-and-fixing engine.[^google-talk] The interface is still undocumented: no source states what artifact passes, how findings queue, or what happens when no patch clears validation.
- Google Cloud Security operationalization — Big Sleep is the Project Zero+DeepMind research surface, but how it surfaces to Google Cloud / Vertex AI customers is not in this post.

## See Also

- [[big-sleep|Big Sleep (product page)]] — the agent.
- [[google-codemender-deepmind|CodeMender paper]] — the patching counterpart.
- [[codemender|CodeMender (product page)]] — the patching agent.
- [[google|Google]] — vendor.
- [[anthropic-glasswing-announcement|Glasswing announcement]] — names Big Sleep + CodeMender as Google's parallel AI-cyber tools.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — wiki thesis Big Sleep co-anchors.
- [[mdash|MDASH]] / [[xbow|XBOW]] / [[mythos|Claude Mythos Preview]] — the other May 2026 anchors.
- [[autonomous-code-security-google-talk|Autonomous Code Security at Google]] — March 2026 talk disclosing the five-phase architecture.

[^google-talk]: Heather Adkins and Four Flynn, *Evaluating Threats & Automating Defense: How Google is Advancing Code Security*, [\[un\]prompted, San Francisco](https://www.youtube.com/watch?v=B_7RpP90rUk) (2026-03-03): Big Sleep at zero false positives end-to-end on deep memory-safety bugs, with a working exploit built as proof of vulnerability; CodeMender at 178 open-source fixes, 48 patched and 130 hardening; verification presented as the gate, and full autonomy stated as the design intent. See [[autonomous-code-security-google-talk|the talk summary]].
