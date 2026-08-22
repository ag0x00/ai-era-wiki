---
type: paper
title: "Frontier Red Team Vulnerability Research"
address: c-000141
created: 2026-05-23
updated: 2026-08-16
tags:
  - papers
  - ai-vuln-discovery
  - sec-against-ai
  - coordinated-disclosure
  - frontier-red-team
status: developing
source_url: "https://red.anthropic.com/2026/zero-days/"
scope_axis:
  - ai-in-sec-defense
  - sec-against-ai
authors:
  - "Nicholas Carlini"
  - "Keane Lucas"
  - "Evyatar Ben Asher"
  - "Newton Cheng"
related:
  - "[[anthropic]]"
  - "[[mythos]]"
  - "[[claude-code-security]]"
  - "[[anthropic-glasswing-announcement|Project Glasswing announcement]]"
  - "[[anthropic-glasswing-initial-update|Glasswing one-month update]]"
  - "[[mozilla]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[vulnops|VulnOps]]"
  - "[[autonomous-exploit-generation|Autonomous Exploit Generation]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[vulnerability-research-agentic-age-keynote]]"
sources:
  - "https://red.anthropic.com/2026/zero-days/"
  - "https://red.anthropic.com/2026/firefox/"
  - "https://red.anthropic.com/2026/cvd/"
  - "[[.raw/articles/anthropic-frt-zero-days-2026-05-23.md]]"
  - "[[.raw/articles/anthropic-frt-firefox-2026-05-23.md]]"
  - "[[.raw/articles/anthropic-frt-cvd-2026-05-23.md]]"
---

# Anthropic Frontier Red Team — Vulnerability Research Series (2026)

**Sources:** [0-days (Feb 5, 2026)](https://red.anthropic.com/2026/zero-days/) · [Firefox / Mozilla (Mar 6, 2026)](https://red.anthropic.com/2026/firefox/) · [CVD dashboard (live)](https://red.anthropic.com/2026/cvd/)

Three posts from Anthropic's Frontier Red Team document the methodology, a worked case study, and the operational disclosure pipeline behind the firm's open-source vulnerability research. They are the primary-source backbone for the [[claude-code-security|Claude Code Security]] product and [[anthropic-glasswing-announcement|Project Glasswing]]. The series tracks one program across three layers: the capability claim (zero-days, February), a controlled case study with a named partner (Firefox, March), and the live disclosure ledger (CVD dashboard).

## 0-days: the capability claim (Feb 5, 2026)

The foundational post reports that [[mythos|Claude Opus 4.6]] discovered and validated **more than 500 high-severity vulnerabilities** in open-source software with no specialized tooling. Several had survived decades in code that had absorbed millions of CPU-hours of fuzzing.

The mechanism is the point. Claude does not fuzz: it does not generate random inputs and watch for crashes. It reads and reasons about code the way a human researcher does. One technique recurs: the model studies past security fixes, then searches the codebase for structurally similar bugs that were never patched. Worked examples span GhostScript, an OpenSC buffer (4096 bytes), and CGIF's LZW dictionary assumptions (`MAX_DICT_LEN` of 4096 entries).

The post draws two forward-looking conclusions. First, discovery speed and scale may soon exceed expert humans, which would make the industry-standard 90-day disclosure window insufficient. Second, Anthropic frames activation-measuring detectors ("probes") with real-time intervention as a model-layer safeguard against misuse.

> [!gap] Probe-architecture detail needs a verbatim re-check
> The fetched copy paraphrased the "cyber-specific probes" safeguard. The hard figures (500+, the 4096-byte structures, the 90-day window, the February 5 date and author list) are reliable; treat the probe mechanism as directionally accurate pending a verbatim read of the live post before quoting.

## Firefox: the controlled case study (Mar 6, 2026)

Anthropic and [[mozilla|Mozilla]] ran a two-week collaboration in which Claude Opus 4.6 scanned roughly 6,000 Firefox C++ files. It surfaced **22 vulnerabilities, 14 rated high-severity** by Mozilla, close to a fifth of all high-severity Firefox bugs remediated in all of 2025, compressed into two weeks. The first use-after-free appeared within 20 minutes; the team submitted 112 unique reports, and the fixes shipped in Firefox 148.0. (A later, larger pass with [[mythos|Claude Mythos Preview]] produced the **271** Firefox-150 figure recorded on the [[zero-day-clock|Zero Day Clock]] page.)

The post's central finding is an asymmetry: **discovery is now cheap and fast, but weaponization is not.** Only 2 of several hundred exploit attempts succeeded, at roughly \$4,000 in API credits. Anthropic argues this gap currently favors defenders, because patching agents can fix bugs faster than attackers can build working exploits, while cautioning that a future model that breaks the exploitation barrier would demand new safeguards.

The post credits **task verifiers** as the methodological enabler: Claude checks its own candidate findings against a trusted external tool, which keeps a high-volume scan from drowning in false positives.

## CVD dashboard: the disclosure pipeline (live)

The coordinated-vulnerability-disclosure dashboard quantifies the full discovery-to-patch funnel. As of the May 22, 2026 snapshot:

| Stage | Count |
|---|---|
| Findings discovered (Claude Mythos Preview) | 23,019 |
| Triaged by external firms | 1,900 |
| Confirmed valid | 1,726 (90.8% true-positive) |
| Disclosed to maintainers | 1,596 |
| Acknowledged | 1,451 |
| Patched upstream | 97 |
| Security advisories (CVE or GHSA) | 88 |
| OSS projects affected | 281 |

Anthropic partners with six external security firms for independent human triage before disclosure. The dashboard makes the [[anthropic-glasswing-initial-update|Glasswing bottleneck inversion]] concrete: the program generates far more findings than it discloses because **human triage is the rate-limiting step**, not discovery. Anthropic also cautions that the true-positive count is a weak proxy for impact and prefers patch count as the firmer metric — agreement between Claude's severity ratings and the external firms was 58.7% exact and 94.4% within one band across 463 compared vulnerabilities. A disclosure ledger uses SHA-3-512 hash commitments for tamper-evidence.

An independent academic pipeline reports the same funnel shape with no vendor interest behind it. [[vulnerability-research-agentic-age-keynote|Yan Shoshitaishvili's Black Hat USA 2026 keynote]] states his lab finds vulnerabilities at roughly ten times the rate it can report them, for the same reason Anthropic's dashboard shows: a report worth sending needs a proposed fix and a reasonable analysis, not a bare finding.[^asu-keynote] That convergence argues human triage is a structural bottleneck of the field rather than a limit specific to Anthropic's operating model.

## Significance

The series supplies the primary-source detail under several wiki claims that previously rested on secondary citations:

- The **500+ figure** cited on the [[claude-code-security|Claude Code Security]] and [[anthropic-glasswing-announcement|Glasswing]] pages originates here, with the reasoning-over-code-not-fuzzing mechanism attached.
- The **discovery-vs-exploitation gap** is the sharpest sourced statement of the asymmetry the [[zero-day-clock|Zero Day Clock]]'s caveat describes: attacker *capability* is racing ahead of attacker *impact*, because turning a found bug into a working exploit stays expensive.
- The **CVD funnel** is the operational evidence for [[vulnops|VulnOps]]: when discovery scales past human triage and patching, the scarce resource shifts downstream, exactly as the find→fix-gap argument predicts.

**Defender advantage is a claim about today, not a law.** The Firefox post's "defenders have the advantage" rests entirely on the exploitation barrier holding. The same post names the condition under which it breaks: a model that closes the find→exploit gap. The advantage is contingent, and the contingency is a model-capability threshold — the same threshold the [[autonomous-exploit-generation|Autonomous Exploit Generation]] page tracks crossing on the offensive side.

## Adjacent / Open

- **Frontier Red Team author network.** Nicholas Carlini, Keane Lucas, Evyatar Ben Asher, Newton Cheng, Hasnain Lakhani, David Forsythe, Kyla Guru, and Daniel Freeman recur across the posts. No dedicated person pages yet; create one for Carlini if the FRT line of work is cited further.
- **90-day disclosure window.** The zero-days post questions its adequacy under machine-scale discovery. The wiki has no page on disclosure-window policy; a candidate if more sources address it.
- **Does disclosure net-harm at all.** A separate, harder question sits alongside the timing one. A May 2026 lab study of embedded-device disclosure, run without agentic discovery, found that disclosing one vulnerability endangered roughly three times as many devices as it secured, with the exact ratio depending on what counts as endangerment.[^asu-keynote] The zero-days post's question is whether 90 days is fast enough; this one is whether disclosure itself is net-positive at any speed. The two are different problems and neither replaces the other.

[^asu-keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06). See [[vulnerability-research-agentic-age-keynote|the talk summary]].
