---
type: paper
title: "Project Glasswing: Initial Update"
address: c-000089
created: 2026-05-22
updated: 2026-08-21
tags:
  - papers
  - anthropic
  - glasswing
  - claude-mythos
  - vuln-discovery
  - ai-vuln-discovery
  - ai-in-sec-defense
  - sec-against-ai
  - coordinated-disclosure
  - triage-bottleneck
status: summarized
scope_axis:
  - ai-in-sec-defense
  - sec-against-ai
publication_date: 2026-05-22
authors: []
publisher: "Anthropic"
source_url: https://www.anthropic.com/research/glasswing-initial-update
archived_copy: ".raw/articles/anthropic-glasswing-initial-update-2026-05-22.md"
related:
  - "[[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]]"
  - "[[glasswing]]"
  - "[[mythos]]"
  - "[[anthropic]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[cloudflare]]"
  - "[[mozilla]]"
  - "[[aisi-uk]]"
  - "[[exploit-benchmarks]]"
  - "[[zero-day-clock]]"
  - "[[vulnops]]"
  - "[[xbow-mythos-evaluation]]"
  - "[[claude-code-security]]"
  - "[[frontier-ai-for-vuln-discovery]]"
sources:
  - "[[.raw/articles/anthropic-glasswing-initial-update-2026-05-22.md]]"
---

# Project Glasswing — Initial Update (One Month In)

**Source:** [Anthropic — Project Glasswing: An initial update](https://www.anthropic.com/research/glasswing-initial-update) (May 22, 2026). Local copy: `.raw/articles/anthropic-glasswing-initial-update-2026-05-22.md`.

## Source Summary

One month after the [[anthropic-glasswing-announcement|May 12 launch]], Anthropic reports that its ~50 partners have used [[mythos|Claude Mythos Preview]] to find **more than ten thousand high- or critical-severity vulnerabilities** across systemically important software. The update reframes the central problem: discovery is no longer the constraint on software security; verification, disclosure, and patching are. The post structures its disclosure around aggregate statistics and illustrative examples rather than full vulnerability detail, because most findings sit inside the 90-day coordinated-disclosure window and detailing them would put end users at risk.

**The bottleneck has inverted.** Anthropic states it directly: *"Progress on software security used to be limited by how quickly we could find new vulnerabilities. Now it's limited by how quickly we can verify, disclose, and patch the large numbers of vulnerabilities found by AI."* Several partners report bug-finding rates up more than tenfold. The scarce resource is now human triage-and-patch capacity, not discovery capability. This is the operational thesis of [[vulnops|VulnOps]] confirmed by primary-source coalition data, and the qualitative companion to the [[zero-day-clock|Zero Day Clock]]'s quantitative TTE-collapse curve.

## Partner Results (One Month)

- Most partners have each found **hundreds** of high- or critical-severity vulnerabilities; collectively **more than ten thousand**.
- **[[cloudflare|Cloudflare]]**: 2,000 bugs found (400 high- or critical-severity) across critical-path systems, at a false-positive rate the Cloudflare team considers **better than human testers**.
- **One Glasswing partner bank**: Mythos Preview helped detect and prevent a fraudulent **\$1.5M wire transfer** after a threat actor compromised a customer's email account and made spoofed phone calls — evidence that the model assists beyond code-level vulnerability discovery.

### External evaluations corroborating the preview

- **[[aisi-uk|UK AI Security Institute]]**: Mythos Preview is the **first model to solve both of AISI's cyber ranges** (multistep cyberattack simulations) end to end.
- **[[mozilla|Mozilla]]**: found and fixed **271 vulnerabilities in Firefox 150** while testing Mythos Preview — over **ten times** the count found in Firefox 148 with [[mythos|Claude Opus 4.6]].
- **[[xbow-mythos-evaluation|XBOW]]**: Mythos Preview is a *"significant step up over all existing models"* on its web exploit benchmark, with *"absolutely unprecedented precision"* on a token-for-token basis.
- **[[exploit-benchmarks|ExploitBench and ExploitGym]]**: two newly released academic benchmarks for exploit-development capability both show Mythos Preview as the strongest performer.

### Patch-volume signals across the industry

- **Palo Alto Networks**: latest release included **over five times** as many patches as usual.
- **Microsoft**: number of new patches will *"continue trending larger for some time."*
- **Oracle**: finding and fixing vulnerabilities across products and cloud **multiple times faster** than before.

## Open-Source Scanning Program

Anthropic has scanned **more than 1,000 open-source projects** over the past few months. Funnel as of this update:

| Stage | Count | Note |
|---|---|---|
| Estimated high/critical found | **6,202** | Out of 23,019 total including medium/low |
| Carefully assessed (6 independent firms + Anthropic) | **1,752** | The triage-limited subset |
| Confirmed true positives | **1,587 (90.6%)** | Post-triage validity rate |
| Confirmed high/critical | **1,094 (62.4%)** | Of the assessed subset |
| Projected high/critical (at current TP rate) | **~3,900** | Open-source code alone; expected to rise |
| Unvetted bugs disclosed (on maintainer request) | **1,129** | 175 estimated high/critical by Mythos |
| High/critical disclosed to maintainers | **~530** | A further 827 confirmed and pending disclosure |
| Patched | **75** | 65 with public advisories |
| Mean time to patch (high/critical) | **~2 weeks** | — |

The steep drop-off at each funnel stage reflects human effort. Anthropic published a [public dashboard of open-source vulnerabilities](https://red.anthropic.com/2026/cvd/) tracking the disclosure pipeline over time.

**Maintainer overload is now a named constraint.** Anthropic reports that open-source maintainers are *"severely capacity constrained,"* facing a deluge of low-quality AI-generated bug reports, and that **some maintainers have asked Anthropic to slow down its rate of disclosures** because they need more time to design patches. Patch volume is low for three reasons: (1) early in the 90-day window; (2) undercounting silent patches; (3) a genuine problem — even at a deliberately slow pace, Mythos Preview is adding to an already-overloaded security ecosystem. This is the supply-side companion to the bottleneck-inversion insight: the constraint is not just internal triage but the volunteer-maintainer capacity of the open-source commons.

### Named example — wolfSSL (CVE-2026-5194)

Mythos Preview constructed an exploit in **wolfSSL**, an open-source cryptography library used by billions of devices, that would let an attacker **forge certificates** — enabling a perfectly legitimate-looking fake website for a bank or email provider under attacker control. Now patched; assigned **[CVE-2026-5194](https://nvd.nist.gov/vuln/detail/CVE-2026-5194)**; full technical analysis to follow.

## Tools for Defenders Using Publicly Available Models

Anthropic frames the interim period — vulnerabilities discovered fast, patched slowly — as the present risk, and pushes generally available tooling to compress it:

- **[[claude-code-security|Claude Security]]** released in **public beta** for Claude Enterprise customers — scans codebases and generates proposed fixes. In three weeks since launch, **Claude Opus 4.7 has been used to patch over 2,100 vulnerabilities**. (Faster than open-source patching because enterprises fix their own code rather than relying on volunteer maintainers.)
- **Cyber Verification Program** — vetted security professionals using Anthropic models for legitimate cybersecurity work (vulnerability research, pentesting, red-teaming) may operate **without certain anti-misuse safeguards**.
- **Tooling shared on request** to qualifying security teams: the **skills** Anthropic and partners built, a **harness** that maps the codebase / spins up scanning subagents / triages findings / writes reports, and a **threat-model builder** that prioritizes the model's work.
- **Cisco** open-sourced its [Foundry Security Spec](https://blogs.cisco.com/ai/announcing-foundry-security-spec) so other defenders can build a similar evaluation system.

## Ecosystem Support and Release Posture

- Partnership with the **OpenSSF Alpha-Omega** project (Linux Foundation announced **\$12.5M** in grant funding from leading organizations) to help maintainers process and triage bug reports.
- Continued support for **[[exploit-benchmarks|ExploitBench and ExploitGym]]** and other quantitative benchmarks via the External Researcher Access Program.
- **Claude for Open Source** supports maintainers and contributors; Anthropic commits to scan any open-source package it adopts.
- **Release posture unchanged**: no company — including Anthropic — has safeguards strong enough to prevent Mythos-class models from being misused, so Anthropic has not released Mythos-class models publicly. Next steps: expand Glasswing with critical partners including **US and allied governments**; pursue general release once stronger safeguards exist.

## Contradictions / Resolutions

> [!check] Mozilla 271-vulnerability finding — now primary-sourced
> The [[zero-day-clock|Zero Day Clock page]] and [[frontier-ai-for-vuln-discovery|the vuln-discovery thesis]] previously cited the *"271 Mozilla vulns / 3 CVEs"* figure secondhand via the [[mythos-ready-briefing|Mythos-ready briefing]]. This update confirms the 271 number directly from Anthropic (Firefox 150, >10× the Firefox 148 count under Opus 4.6) and links Mozilla's own [hardening write-up](https://hacks.mozilla.org/2026/05/behind-the-scenes-hardening-firefox/). The open gap tracked in `hot.md` is closed on the count; CVE-assignment detail remains in Mozilla's posts.

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-2026|CMM]] D7 (Observability & Detection) L5+** — the open-source scanning funnel (6,202 → 1,752 assessed → 75 patched) is the canonical industrial-scale measurement of the discovery-vs-remediation gap; it should anchor the L5+ revision discussion.
- **[[agentic-ai-security-cmm-2026|CMM]] D8 (Supply Chain & AI-BOM)** — the maintainer-overload finding and the OpenSSF Alpha-Omega grant are supply-chain-security primitives; the constraint is now empirically the human-maintainer commons, not tooling.
- **[[vulnops|VulnOps]]** — this update is the strongest primary-source evidence for the VulnOps thesis: a permanent, DevOps-shaped function for continuous discovery-and-remediation, because discovery has outrun triage-and-patch.

## Limitations

- **Vendor-published aggregates.** Counts are Anthropic-reported; the 90.6% true-positive rate covers only the 1,752 assessed of 6,202 estimated high/critical, so the projected ~3,900 is an extrapolation.
- **Disclosure-window opacity.** Most findings are inside the 90-day window; the post gives illustrative examples (wolfSSL) and statistics, not full technical detail.
- **Self-estimated severity.** For directly disclosed unvetted bugs, severity is Mythos's own estimate, not independently confirmed.
- **No model-architecture detail.** Consistent with the original announcement, no training, scale, or methodology disclosure for Mythos.

## See Also

- [[anthropic-glasswing-announcement|Project Glasswing announcement (May 12, 2026)]] — the launch source this updates.
- [[glasswing|Project Glasswing (entity)]] — the coalition.
- [[mythos|Claude Mythos Preview]] — the model.
- [[vulnops|VulnOps]] — the operational thesis this update confirms.
- [[zero-day-clock|Zero Day Clock]] — the quantitative TTE-collapse companion.
- [[cloudflare|Cloudflare]] · [[mozilla|Mozilla]] · [[aisi-uk|UK AI Security Institute]] — partners/evaluators with named results.
- [[exploit-benchmarks|ExploitBench & ExploitGym]] — the new academic exploit-development benchmarks.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — the thesis this anchors.

- [[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]] — treats the bottleneck inversion documented here as the condition that makes [[vulnops|VulnOps]] a permanent standing function rather than a project.
