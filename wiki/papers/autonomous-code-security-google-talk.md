---
type: talk
title: "Autonomous Code Security at Google"
address: c-000321
created: 2026-08-24
updated: 2026-08-24
tags:
  - papers
  - talks
  - google
  - deepmind
  - big-sleep
  - codemender
  - ai-vuln-discovery
  - vuln-patching
  - vulnops
  - memory-safety
  - ai-in-sec-defense
status: summarized
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
year: 2026
authors: ["Heather Adkins", "Four Flynn"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 3, 2026 (Day 1 / Stage 1)"
source_url: "https://unpromptedcon.org/abstract-march2026/"
archived_copy: ".raw/talks/2026-03-03_Heather-Adkins-and-Four-Flynn_Evaluating-Threats-Automating-Defense_transcript.md"
key_claim: "Google presents Big Sleep and CodeMender as one autonomous find-and-fix pipeline in which an exploit built as proof of vulnerability holds the false-positive rate at zero on deep memory-safety bugs, and a pluggable verifier stack rather than human review gates the patches; of 178 fixes landed in open source, 130 are proactive hardening and 48 are patches."
methodology: "Conference talk by two Google security executives presenting Big Sleep and CodeMender's operating figures from an 8-slide deck. Primary sources captured: the speaker transcript (~3,500 words) and the deck, both archived under .raw/talks/. Figures are the presenters' own and carry no external audit."
contradicts: []
supports:
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[autonomous-exploit-generation]]"
  - "[[vulnops]]"
related:
  - "[[big-sleep]]"
  - "[[codemender]]"
  - "[[google]]"
  - "[[heather-adkins]]"
  - "[[four-flynn]]"
  - "[[google-codemender-deepmind]]"
  - "[[google-big-sleep-projectzero]]"
  - "[[llm-as-a-judge]]"
  - "[[zero-day-clock]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[google-cloud-codemender-preview]]"
  - "[[ai-vuln-discovery-benchmark-landscape]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
  - "[[frontier-ai-for-vuln-discovery]]"
sources:
  - "[[.raw/talks/2026-03-03_Heather-Adkins-and-Four-Flynn_Evaluating-Threats-Automating-Defense_transcript.md]]"
  - "[[.raw/talks/2026-03-03_Heather-Adkins-and-Four-Flynn_Evaluating-Threats-Automating-Defense_slides.pdf]]"
  - "https://unpromptedcon.org/abstract-march2026/"
  - "https://www.youtube.com/watch?v=B_7RpP90rUk"
---

# Autonomous Code Security at Google

**Source:** [un]prompted, San Francisco, March 3, 2026 — [conference abstracts](https://unpromptedcon.org/abstract-march2026/) · [video](https://www.youtube.com/watch?v=B_7RpP90rUk) · [[unprompted-conference-march-2026|conference catalog]]. Local copies: `.raw/talks/2026-03-03_Heather-Adkins-and-Four-Flynn_Evaluating-Threats-Automating-Defense_transcript.md` and the deck beside it.

[[heather-adkins|Heather Adkins]] (VP Security Engineering, [[google|Google]]) and [[four-flynn|Four Flynn]] (VP Security and Privacy, Google DeepMind) presented [[big-sleep|Big Sleep]] and [[codemender|CodeMender]] as two halves of one pipeline, under a stated goal of eliminating every software vulnerability on Earth. Google gives operating figures for both programmes here, and they revise two things recorded from the October 2025 and July 2026 announcements: what the published patch counts count, and where the quality gate sits.

## The exploit-as-proof control

Adkins stated that Big Sleep runs "end-to-end, fully automated, without human involvement" at a false-positive rate of zero. The verification stage holds that rate: before the system reports a finding, it builds a working exploit for it. The reported set contains demonstrated bugs, not unconfirmed candidates. Gemini writes the report, stepping through every function in the path, and the report targets a developer with no vulnerability-research background.

The deck names five phases:

| Phase | Contents |
|---|---|
| 1 — Inputs | Vulnerability info, target code, variant analysis |
| 2 — Agentic reasoning loop | Multi-turn LLM interaction, hypothesis and revision |
| 3 — Core toolset | Code browser, Python interpreter, GDB debugger |
| 4 — Feedback | Feedback data, crash verification |
| 5 — Final output | Proof of vulnerability, high-quality report, zero false positives |

Phase 1 encodes what a human researcher accumulates over years in one codebase: its past vulnerabilities, its variants, its architecture. Adkins described the design target as recreating the expertise of Google's Project Zero team rather than prompting a model for bugs.

Two limits sit on the zero-false-positive figure, both stated on stage. The claim covers **deep memory-safety bugs**, and Adkins excluded shallow cross-site scripting and integer overflows from what the research targets. The exploit-as-proof control also transfers poorly to other vulnerability classes. Asked whether the techniques reach vulnerability classes with a weaker signal than a segfault, Flynn answered that the discovery techniques carry over to web and shallow bugs but that "the verification techniques of the vulnerability are different." A class where no crash exists gives phase 4 nothing to verify, so the false-positive rate is a property of that verification stage and not of the reasoning loop.

Google still runs fuzzing at scale and maintains OSS-Fuzz. Adkins offered the relationship between the two as evidence that the research is aimed deep enough: "Big Sleep is finding things the fuzzers are missing."

Adkins also cited a 2017 RAND study for the human baseline the speed target is measured against — about a month for an expert to find a deeply embedded bug, and about 22 days to build a working exploit. The deck states the automated target as "moving from months to minutes."

## The patch count decomposes into hardening

Flynn reported 178 autonomously generated fixes landed in open source. The deck breaks that number down: **48 patched, 130 hardening**. Most of what CodeMender has shipped to open source is proactive rewriting that removes a vulnerability class rather than reactive repair of a specific reported bug. The deck names libwebp as the worked example of hardening a critical library, and Flynn described the internal Chrome work as automatically generated patches that harden pointers in the codebase.

This decomposition does not form a series with the figure the wiki already carries. The [[google-codemender-deepmind|October 2025 DeepMind announcement]] reported 72 security patches upstreamed, and 48 is fewer than 72, so the two numbers count different things. Read them as two measurements on different bases rather than as a trajectory.

Autonomous fixing therefore delivers most of its volume where the correctness bar is easiest to hold — a mechanical transformation applied to a whole class, checkable by fuzzing and differential testing — and least of it where a fix must be reasoned to one specific root cause.

## Verification as the gate

CodeMender draws multiple samples to produce several candidate patches, then puts each through a validation stack the deck groups in four:

| Verifier group | Contents |
|---|---|
| Dynamic analysis | Fuzzing, sanitizers |
| Static analysis | AST-based checks, formal verification |
| Differential testing | — |
| LLM judges and critics | — |

Flynn described what each is for. The code is fuzzed before and after the patch to establish that functionality survived; formal verification attempts to prove the patched section functionally equivalent; a further round of fuzzing replays the malicious input to confirm the vulnerability is gone; and an [[llm-as-a-judge|LLM judge]] reviews the patch under a carefully constructed pre-prompt. The set is pluggable, and Flynn located the programme's differentiator there: "some of the secret sauce of what we've been building is actually in these verification stages." When no candidate clears the stack, the validation failures are fed back into the model's context and the agent produces a fresh set, validated and ranked in turn.

Flynn stated the design intent as "a full end-to-end discovery and fixing engine, right? That runs completely without human intervention," naming no reviewer at submission, with patches that clear validation becoming candidates to send to the community. The human-review gate recorded from the [[google-codemender-deepmind|October 2025 research description]] and this account are each accurate for their date. The research programme's stated goal is full autonomy; the wiki has no source describing a single review step moving between the two accounts.

Flynn volunteered the objection to his own position — that the programme is "maybe being too cautious in making sure that we have highly verified outputs for the community" — and defended the trade-off on adoption grounds, contrasting it with other folks who take a different trade-off toward quicker time to market while "tolerating more developer toil."

## Vulnerability abundance breaks prioritization

Adkins framed the programme against a supply problem rather than a discovery problem. Open-source pen-testing frameworks already exist, and by her own back-of-the-napkin estimate roughly a billion dollars of venture funding is going into vulnerability discovery, pen testing, red teaming, and simulation startups. Between attackers and defenders, she argued, the field is close to finding every vulnerability in every system.

The consequence she drew is a ranking failure rather than a defensive one: "We'll have to change the CVSS scoring system because it won't be meaningful anymore." A severity score is a triage instrument, and it assumes findings are scarce enough to sort. The leading indicators: a National Vulnerability Database backlog of 30,000 unanalyzed vulnerabilities, which she checked the morning of the talk, and a 35% rise between 2024 and 2025 in logged vulnerabilities receiving a CVE, against a population where not every discovered bug gets a CVE at all.

Flynn's name for the resulting condition is the **vulnpocalypse**, offered from the stage as the term to take away from the conference.

## Open problems the presenters name

The deck closes on three conundrums, stated as unsolved:

- **Supporting OSS maintainers struggling with volume and AI slop.** The maintainers who receive autonomously generated reports and patches are the constraint on the whole model.
- **Redeploying auto-mended code at scale.** Flynn was explicit that this is one of the hardest problems in patching and that he has no approach: "one of the hardest problems with patching is actually places in the world that struggle to actually apply patches in a timely manner… I don't know how to solve that with AI."
- **Whether to patch C++ at all, or rewrite it in Rust.** Posed as an open allocation question, with automated patching and memory-safe rewriting as competing uses of the same effort.

Two further limits came out in questions. The research targets infrastructure components that handle untrusted input — Adkins named V8 and FFmpeg — and not business applications, so business-logic vulnerability classes sit outside what the reported figures cover. Asked to compare Big Sleep with OpenAI's Aardvark, the speakers reported no side-by-side comparison, because "neither team has published full details," though they offered that both efforts use, in their words, "agentic reasoning" behind them — leaving the two systems uncompared on any common benchmark.

## Placement

The talk supplies operating evidence for the [[frontier-ai-for-vuln-discovery|frontier AI for vulnerability discovery]] thesis from the vendor whose programme this wiki has sourced in most detail, and it sharpens two of that thesis's open questions rather than closing them. The zero-false-positive figure holds only for bug classes an exploit can be built for. [[autonomous-exploit-generation|Autonomous exploit generation]] already draws that same boundary around proof-of-concept construction used as a triage control. The 48/130 split gives the [[vulnops|VulnOps]] argument the first measure of where autonomous fixing lands that this wiki has sourced.

Adkins's prioritization argument belongs to the demand side of the same problem. The [[zero-day-clock|zero-day clock]] measures how fast exploitation follows disclosure; her claim is that the queue in front of that clock is about to stop being sortable by severity at all.

Flynn's redeployment conundrum lands on [[sdlc-in-the-ai-attacker-era|SDLC in the AI-attacker era]]. That page bounds what the remediation side can assume, and the vendor automating the most patching states that generating a verified fix leaves one of the hardest problems undone: the fix still has to reach the running estate.
