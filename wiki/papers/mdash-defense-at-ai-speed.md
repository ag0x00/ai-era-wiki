---
type: paper
title: "MDASH: Defense at AI Speed"
address: c-000028
created: 2026-05-13
updated: 2026-08-21
tags:
  - papers
  - microsoft
  - mdash
  - agentic-soc
  - vuln-discovery
  - ai-in-sec-defense
  - ai-vuln-discovery
  - multi-model
  - patch-tuesday
status: summarized
scope_axis:
  - ai-in-sec-defense
publication_date: 2026-05-12
authors:
  - "[[taesoo-kim|Taesoo Kim]]"
publisher: "Microsoft Security Blog"
source_url: https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/
archived_copy: ".raw/articles/microsoft-defense-at-ai-speed-2026-05-13.md"
no_public_url: ""
related:
  - "[[mdash]]"
  - "[[microsoft]]"
  - "[[taesoo-kim]]"
  - "[[cybergym]]"
  - "[[xbow-mythos-evaluation]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[agentic-soc-state-of-the-field]]"
sources:
  - "[[.raw/articles/microsoft-defense-at-ai-speed-2026-05-13.md]]"
aliases:
  - papers/microsoft-mdash-defense-at-ai-speed
  - microsoft-mdash-defense-at-ai-speed

---

# Defense at AI Speed — Microsoft's MDASH

**Source:** [Microsoft Security Blog — Defense at AI Speed (Taesoo Kim, May 12, 2026)](https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/). Local copy: `.raw/articles/microsoft-defense-at-ai-speed-2026-05-13.md`.

## Source Summary

Microsoft's first public announcement of [[mdash|MDASH]] (codename — **m**ulti-mo**d**el **a**gentic **s**canning **h**arness), an agentic vulnerability discovery and remediation system built by Microsoft's **Autonomous Code Security (ACS)** team in collaboration with **Microsoft Windows Attack Research and Protection (WARP)**. The system orchestrates **more than 100 specialized AI agents** across an ensemble of frontier and distilled models — auditors, debaters, dedup agents, and provers — to find, validate, and prove exploitable vulnerabilities end-to-end. The May 2026 Patch Tuesday cohort included **16 CVEs MDASH found** in the Windows networking and authentication stack, including four Critical RCEs (`tcpip.sys` SSRR UAF, `ikeext.dll` IKEv2 double-free, `netlogon.dll` CLDAP stack overflow, `dnsapi.dll` heap OOB). MDASH is in limited private preview for select customers.

The strategic frame: **AI vulnerability discovery has crossed from research curiosity into production-grade defense at enterprise scale, and the durable advantage lies in the agentic system around the model rather than any single model itself.** Three reinforcing arguments — composition for discovery, validation as its own pipeline, and model-agnostic harness durability.

## Key Contributions

### Quantitative claims

| Metric | Result |
|---|---|
| StorageDrive (private Microsoft interview driver, 21 planted vulnerabilities) | **21/21 found, 0 false positives** |
| `clfs.sys` MSRC historical recall (5 years, 28 cases) | **96% recall** |
| `tcpip.sys` MSRC historical recall (5 years, 7 cases) | **100% recall** |
| Public CyberGym leaderboard (1,507 real-world vuln-repro tasks across 188 OSS-Fuzz projects) | **88.45%** — top score, ~5 points above raw [[mythos\|Mythos Preview]] at **83.1%** (per [[anthropic-glasswing-announcement\|Anthropic's Glasswing announcement]] — the unnamed #2 entry referenced in MDASH's post) |
| May 2026 Patch Tuesday cohort | **16 new CVEs** (10 kernel-mode, 6 usermode; 4 Critical RCEs) |
| Agent count | 100+ specialized agents |

Microsoft is explicit about the boundaries: retrospective recall benchmarks are on internal code with a finite case count and tell us *"the system would have been useful had it existed at the time,"* not that the next 38 CLFS bugs will be found at the same rate. The forward-looking signal is the Patch Tuesday cohort itself.

### Architectural primitives

MDASH is structured as a five-stage pipeline taking a codebase and emitting validated, proven findings:

1. **Prepare** — ingests the source target, builds language-aware indices, draws attack surface and threat models from past commits.
2. **Scan** — specialized auditor agents over candidate code paths, emitting candidate findings with hypotheses and evidence.
3. **Validate** — a second cohort of *debater* agents argue for and against each finding's reachability and exploitability.
4. **Dedup** — collapses semantically equivalent findings (patch-based grouping).
5. **Prove** — constructs and executes triggering inputs where the bug class admits (e.g. ASan in C/C++); the prove stage validates pre-conditions dynamically and formulates the bug-triggering inputs.

Three properties make this work:

- **Ensemble of diverse models** — SOTA as heavy reasoner; **distilled models** as cost-effective debater for high-volume passes; a **second separate SOTA model** as independent counterpoint. Disagreement between models is itself a signal.
- **Specialized agents** — auditors do not reason like debaters, debaters do not reason like provers. Each pipeline stage has its own role, prompt regime, tools, and stop criteria.
- **End-to-end pipeline with extensible plugins** — plugins inject domain context the foundation models cannot see on their own (kernel calling conventions, IRP rules, lock invariants, IPC trust boundaries, codec state machines). The **CLFS proving plugin** is a worked example: it knows how to construct a triggering log file given a candidate finding, embedding on-disk container layout + block-validation sequence + in-memory state machine. The Windows team additionally extends reasoning with custom CodeQL databases.

The payoff is **portability across model generations**: targeting / validation / dedup / prove stages are model-agnostic by construction. When a new model lands, A/B testing it against the current panel is one configuration flip; customer investments (scope files, plugins, configurations, calibrations) all carry over.

### Two worked deep dives

- **CVE-2026-33827** (Critical RCE in `tcpip.sys`): SSRR-triggered race-condition UAF on `Path` reference-counted objects across non-trivial control flow with three independent concurrent free paths. Single-model harnesses miss this because the lifetime violation is not locally visible — the release and reuse are separated by an alternate branch, multiple validation checks, and several early-drop conditions; the decisive signal lives outside the immediate context, in an analogous-but-correct site elsewhere in the codebase. Detection requires cross-file pattern comparison, multi-step reachability, and concurrent-subsystem reasoning.
- **CVE-2026-33824** (Critical pre-auth LocalSystem RCE in `ikeext.dll`): IKEv2 SA_INIT + fragmentation triggers a textbook double-free. Bug spans **six source files** (`ike_A.c` through `ike_F.c`); strongest evidence that the bug is real is the correct version of the same pattern elsewhere in the codebase. Catching it requires recognizing the missing step at one site by reference to the present step at another — exactly the specialized-auditor + debate-stage pattern MDASH is built around.

### Strategic conclusions (paraphrased)

The post closes with three reinforcing claims:

1. **Discovery requires composition** that no single prompt can achieve. The Windows kernel races and alias chains are not visible to a model handed a single function; they are visible to a system that sequences cross-file comparison, multi-step reachability, debate, and end-to-end proof.
2. **Validation is the difference between a finding and a fix.** A scanner that flags candidate bugs is a scanner that produces a triage backlog. Validation is its own pipeline of agents and plugins.
3. **The system absorbs model improvements** — durability. The right question to ask of an AI vulnerability tool is not *which model does it use?* but *what does it do with the model, and what survives when the next model arrives?*

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-2026|CMM]] D7 (Observability & Detection) L4–L5** — MDASH is a defender-side instance of the [[red-teaming-for-ai-synthesis|four-quadrant red-team grid]]'s "continuous" quadrant applied inversely (continuous defender vuln discovery rather than continuous offensive testing).
- **[[agentic-ai-security-cmm-2026|CMM]] D8 (Supply Chain & AI-BOM)** — multi-model ensemble with explicit second-SOTA-counterpoint is an architectural pattern relevant to defender-side AI-BOM strategies.
- **[[agentic-ai-security-reference-architecture|RA]] Observability Plane** — MDASH operates at the audit layer; its agentic structure is a candidate primitive for the L5 reference stack.
- **[[microsoft-zt4ai|Microsoft ZT4AI]]** — MDASH is consistent with ZT4AI's framing of defender-side agentic systems as first-class identities; agent inventory and authority controls extend naturally.

## Convergence with [[xbow-mythos-evaluation|XBOW's Mythos Evaluation]]

Two independent vendors (XBOW + Anthropic, Microsoft) on opposite sides of the security stack (offensive, defensive) and using different model strategies (single best frontier model with strong harness; ensemble of diverse models with debate) **arrive at the same architectural insight**: the model is *one input*, and the harness around the model is the durable engineering. XBOW: *"a model is a brain without a body."* Microsoft: *"the harness does the work, and the model is one input."* The convergence corroborates [[frontier-ai-for-vuln-discovery|the wiki's vulnerability-discovery thesis]]: candidate generation against validated outcomes is the load-bearing asymmetry.

## Cross-Axis Implications

- **`ai-in-sec-defense`** (primary): second sourced anchor for [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] after XBOW. Microsoft's CyberGym-leaderboard 88.45% and 5-year MSRC recall are quantitative, publicly-comparable data points that move that thesis from "provisional" toward "convergent." MDASH is also a defender-side agentic system distinct from [[microsoft-security-copilot|Security Copilot]] — Copilot serves SOC operators (analyst, triage, conditional access); MDASH serves vulnerability researchers and DevSecOps. Microsoft now has named defender-AI capability at both the SOC layer and the AppSec layer.
- **`sec-against-ai`**: AI-augmented attackers gaining capability parity with MDASH-class systems would compress responsible-disclosure timelines further; reinforces the [[sdlc-in-the-ai-attacker-era|SDLC thesis]] argument.

## Limitations

- **Model identity is not disclosed.** Microsoft says "generally available AI models" but does not name SOTA models, distilled models, or counterpoint models.

**The MDASH model silence is explained by Glasswing coordination.** [[anthropic-glasswing-announcement|Anthropic's Glasswing announcement]] (same day, May 12 2026) confirms Microsoft as a Glasswing partner with Mythos Preview access. Microsoft tested Mythos against its own CTI-REALM open-source security benchmark and reported "substantial improvements." The MDASH announcement's "generally available AI models" phrasing reflects coordinated-launch constraint, not model-stack mystery. **Mythos Preview is almost certainly one of MDASH's orchestrated SOTA-reasoner models**, alongside likely OpenAI and Microsoft-internal counterparts.
- **Internal code, internal benchmarks.** StorageDrive, `clfs.sys`, and `tcpip.sys` are Microsoft-internal codebases. The CyberGym number is the one independently reproducible data point.
- **Single-author byline.** Taesoo Kim is named; team contributions (ACS, WARP) are noted but personnel breakdown is not disclosed.
- **No public Patch Tuesday CVE table reconciliation.** The 16 CVEs in the post are listed in a Microsoft-internal table; reconciliation with the actual May 2026 NVD entries is a follow-up audit.

## Open Questions Surfaced

- Which specific models does MDASH orchestrate? Anthropic's [[mythos|Mythos]] is a plausible candidate for the SOTA reasoner slot; Microsoft is unlikely to publicly confirm.
- How does the CyberGym `level 1` configuration (which provides vulnerable source + high-level vuln description) compare to harder levels (less context, blind discovery)? The 88.45% headline number is on the *easiest* level.
- Microsoft's commercial productization path: limited private preview today, but the harness is described as *"being used by Microsoft security engineering teams and tested by a small set of customers."* What is the GA timeline, pricing model, and SKU positioning relative to Defender and Security Copilot?
- How does [[mdash|MDASH]] relate to [[microsoft-security-copilot|Security Copilot]] architecturally? Same agent fleet, different routing? Separate stacks?

## See Also

- [[mdash|MDASH]] — the product page.
- [[microsoft|Microsoft]] — vendor.
- [[taesoo-kim|Taesoo Kim]] — author.
- [[cybergym|CyberGym benchmark]] — public benchmark MDASH leads.
- [[xbow-mythos-evaluation|XBOW's Mythos Evaluation]] — offensive-side companion paper.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — the wiki thesis MDASH co-anchors.
- [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] — adjacent thesis (defender-side AI broadly).
