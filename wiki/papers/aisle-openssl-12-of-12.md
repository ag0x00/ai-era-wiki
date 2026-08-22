---
type: paper
title: "AISLE: 12 of 12 OpenSSL Vulnerabilities"
address: c-000083
created: 2026-05-15
updated: 2026-08-21
tags:
  - papers
  - vuln-discovery
  - openssl
  - cryptographic-libraries
  - aisle
  - autonomous-analysis
status: summarized
scope_axis: [ai-in-sec-defense]
year: 2026
authors:
  - "[[stanislav-fort|Stanislav Fort]]"
contributing_authors:
  - "Petr Šimeček"
  - "Tomas Dulka"
  - "Luigino Camastra"
venue: "AISLE company blog"
source_url: https://aisle.com/blog/aisle-discovered-12-out-of-12-openssl-vulnerabilities
archived_copy: ".raw/articles/aisle-12-of-12-openssl-vulnerabilities-2026-01.md"
publication_date: 2026-01
key_claim: "An AI-driven autonomous analyzer discovered all 12 vulnerabilities in the January 2026 coordinated release of OpenSSL — including a CVSS 9.8 stack-buffer-overflow in CMS AuthEnvelopedData parsing whose vulnerable code dates to 1998 — across 8+ subsystems (CMS, QUIC, TLS 1.3, post-quantum ML-DSA, PKCS#12, OCB, TimeStamp, PKCS#7). 5 of 12 fixes were authored by AISLE and incorporated directly; 6 additional findings were caught pre-release and never required a CVE."
methodology: "AISLE Research Team began OpenSSL analysis in August 2025 using a proprietary autonomous analyzer. All discoveries were reported via OpenSSL's coordinated security-reporting process with full reproduction steps, root-cause analysis, and concrete patch proposals. The blog post describes operational collaboration between AISLE researchers and the OpenSSL Foundation; it does not disclose the analyzer's internal architecture."
related:
  - "[[aisle|AISLE]]"
  - "[[stanislav-fort|Stanislav Fort]]"
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[unprompted-conference-march-2026|Unprompted Conference March 2026]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[cybergym|CyberGym Benchmark]]"
  - "[[openant|OpenAnt]]"
  - "[[codex-security|Codex Security]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[adversarial-reflexion|Adversarial Reflexion]]"
sources:
  - "[[.raw/articles/aisle-12-of-12-openssl-vulnerabilities-2026-01.md]]"
  - https://aisle.com/blog/aisle-discovered-12-out-of-12-openssl-vulnerabilities
  - https://aisle.com/blog/aisle-discovers-three-of-the-four-openssl-vulnerabilities-of-2025
  - https://www.schneier.com/blog/archives/2026/02/ai-found-twelve-new-vulnerabilities-in-openssl.html
  - https://openssl-library.org/news/vulnerabilities/
aliases:
  - papers/aisle-openssl-12-of-12-2026
  - aisle-openssl-12-of-12-2026

---

# AISLE Discovered 12 out of 12 OpenSSL Vulnerabilities

**Source:** [AISLE blog — *AISLE Discovered 12 out of 12 OpenSSL Vulnerabilities*](https://aisle.com/blog/aisle-discovered-12-out-of-12-openssl-vulnerabilities) (January 2026). Author: [[stanislav-fort|Stanislav Fort]]. Contributing researchers: Petr Šimeček, Tomas Dulka, Luigino Camastra. Local copy: `.raw/articles/aisle-12-of-12-openssl-vulnerabilities-2026-01.md`.

## Key Claim

[[aisle|AISLE]]'s autonomous analyzer **discovered all 12 vulnerabilities** in the January 2026 coordinated release of [OpenSSL](https://openssl-library.org/news/vulnerabilities/) — including **CVE-2025-15467**, a **CVSS 9.8** stack-buffer-overflow in CMS AuthEnvelopedData parsing whose vulnerable code dates to **1998**. Span: 8+ subsystems (CMS, QUIC, TLS 1.3, post-quantum ML-DSA signatures, PKCS#12, OCB, TimeStamp, PKCS#7). 5 of 12 fixes were authored by AISLE and incorporated directly into OpenSSL. 6 additional findings were caught **before the vulnerable code ever appeared in a release** — the *"preventing vulnerabilities, not merely patching them after deployment"* framing.

## The 12 CVEs

### High and Moderate Severity

| CVE | Severity | Subsystem | Description |
| :--- | :--- | :--- | :--- |
| **CVE-2025-15467** | High (NIST CVSS v3 **9.8**) | CMS | **Stack buffer overflow** in CMS AuthEnvelopedData parsing — potential remote code execution under specific conditions. Vulnerable code dates to **1998**. |
| **CVE-2025-11187** | Moderate | PKCS#12 | PBMAC1 parameter-validation missing → stack-based buffer overflow |

### Low Severity (10 CVEs)

| CVE | Subsystem | Description |
| :--- | :--- | :--- |
| CVE-2025-15468 | QUIC | Crash in QUIC protocol cipher handling |
| CVE-2025-15469 | Post-quantum signatures (ML-DSA) | Silent truncation bug |
| CVE-2025-66199 | TLS 1.3 | Memory exhaustion via certificate compression |
| CVE-2025-68160 | I/O | Memory corruption in line-buffering — affects code back to OpenSSL 1.0.2 |
| CVE-2025-69418 | OCB mode | Encryption flaw on hardware-accelerated paths |
| CVE-2025-69419 | PKCS#12 | Memory corruption in character encoding |
| CVE-2025-69420 | TimeStamp | Crash in TimeStamp Response verification |
| CVE-2025-69421 | PKCS#12 | Crash in PKCS#12 decryption |
| CVE-2026-22795 | PKCS#12 | Crash in PKCS#12 parsing |
| CVE-2026-22796 | PKCS#7 | Crash in PKCS#7 signature verification — affects code back to OpenSSL 1.0.2 |

Three of the twelve bugs date to 1998–2000. They survived **millions of CPU-hours of fuzzing** by organizations including Google.

## Notable Findings

- **Watershed moment for AI-driven security on cryptographic libraries.** OpenSSL is *"one of the most deployed, battle-tested, and carefully maintained open-source projects in existence"*; even a single accepted vulnerability per release represents a rare achievement. 12 in one coordinated release from a single research team — let alone an AI-driven one — is structurally unusual. Per Bruce Schneier on AISLE's site: *"AISLE is credited for surfacing 13 of 14 OpenSSL CVEs assigned in 2025, and 15 total across both releases. This is a historically unusual concentration."*
- **AISLE proposed fixes adopted directly for 5 of 12 CVEs.** The blog frames this as collaboration rather than analyst-throw-over-fence: AISLE submits reproduction steps + root-cause analysis + concrete patch proposals, and in each case its proposed fixes *"either informed or were directly adopted by the OpenSSL team."*
- **6 additional findings caught pre-release** — never assigned a CVE because the fixes were merged before the vulnerable code appeared in a release. This is the strongest single signal in the disclosure that the AISLE workflow is operationalized continuously inside the development workflow, not just as point-in-time audit.
- **Time-to-remediation collapse.** Stanislav Fort: *"When autonomous discovery is paired with responsible disclosure, it collapses the time-to-remediation for the entire ecosystem."* This is the same TTR-collapse argument the [[zero-day-clock|Zero Day Clock]] frames empirically — but on the **defender side** rather than the attacker side. Defender-side TTR has historically been the bottleneck; AISLE's claim is that AI-driven defender TTR can compress as fast as the attacker-side TTE has.
- **Pairings with OpenSSL Foundation leadership.** **Tomáš Mráz** (CTO, OpenSSL Foundation): *"This release is fixing 12 security issues, all disclosed to us by AISLE. We appreciate the high quality of the reports and their constructive collaboration with us throughout the remediation."* **Matt Caswell** (Executive Director, OpenSSL Foundation): *"Keeping widely deployed cryptography secure requires tight coordination between maintainers and researchers. We appreciate AISLE's responsible disclosures and the quality of their engagement across these issues."* Maintainer-side validation is the structural difference between AI-driven *findings* and AI-driven *value*.

## Strengths and Weaknesses

**Strengths.** Cryptographic libraries are the hardest domain to claim novel findings in — *"finding a genuine security flaw in OpenSSL is extraordinarily difficult."* The 12-of-12 + 1998-vintage + 8-subsystem-span result establishes that AI-driven analysis can produce findings in a domain where traditional SAST has historically been brittle (complex logic errors, timing-dependent issues, deep subsystem interactions). The collaborative model (reproduction steps + root-cause analysis + patch proposals) is the production-grade version of AI-driven disclosure: the analyst gets actionable evidence and a starting fix, not just a finding to triage. The 6 pre-release catches are the strongest signal of operational integration.

**Weaknesses and open scope.**

- **The autonomous analyzer architecture is not disclosed.** Unlike [[openant|OpenAnt]] (six-stage pipeline), [[codex-security|Codex Security]]'s Aardvark (four-stage pipeline), or [[mdash-defense-at-ai-speed|MDASH]] (five-stage with named agent classes), AISLE has not published its harness internals. The wiki's [[adversarial-reflexion|cross-product FP-control mechanism comparison]] cannot yet place AISLE — see [[aisle|the org page]] for the mechanism-category gap.
- **No false-positive rate published.** AISLE reports 12 CVEs assigned and 6 pre-release catches; no comparable accept/reject ratio or per-finding triage cost is captured in this blog.
- **No third-party benchmark.** Comparisons to [[mdash|MDASH]] (88.45% on [[cybergym|CyberGym]]), [[mythos|raw Mythos]] (83.1% on CyberGym), or [[codex-security|Aardvark]] (92% on internal golden repos) require a common evaluation surface; the wiki's longstanding gap on this remains.
- **OpenSSL is the focused target.** AISLE's other surfaced disclosures (curl, FreeBSD) suggest broader OSS-cryptographic-and-systems scope, but the systemic coverage map across other widely-deployed OSS targets is not yet published.

## Relations

- **Supports** [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] thesis — AISLE is now the wiki's sourced anchor for *AI-driven discovery of decade-class latent vulnerabilities in widely deployed open-source cryptographic libraries*. Pairs with [[anthropic-glasswing-announcement|Glasswing]]'s 27-year-old OpenBSD vuln + 16-year-old FFmpeg vuln disclosures.
- **Strengthens** [[mythos-ready-briefing|Mythos-ready briefing]] — the AISLE-OpenSSL data point is cited inline by the briefing's timeline; the [[aisle|dedicated AISLE org page]] and this paper page now close that gap.
- **Cross-references [[unprompted-conference-march-2026|Unprompted Conference March 2026]]** — AISLE is named in the conference's overall key-claim alongside FENRIR, Promp2Pwn, and XBOW as systems running autonomous bug-finding agents at production scale.
- **Adjacent to** [[cybergym|CyberGym Benchmark]] — UC Berkeley team's *Open-Ended Discovery* mode published 35 zero-days + 10 unique zero-days at 969-day mean persistence; AISLE's 12-of-12 OpenSSL result sits in a similar conceptual frame but on a specific high-value target rather than across a benchmark corpus.

**AISLE's 6 pre-release catches are the operational signal.** The 12 assigned CVEs are the publicly visible artifact. The **6 additional findings caught before the vulnerable code ever appeared in a release** are the structural innovation — they document operational integration of autonomous analysis *into the development workflow*. This is the AI-driven realization of what the [[mythos-ready-security-program|Mythos-ready playbook]] calls *PA 1: Point Agents at Your Code and Pipelines*, with the additional commitment that *"all code (human or AI-generated) should pass LLM-driven security review before merge"* — applied at the OpenSSL upstream-development level rather than at the consumer-application level. Pre-release catches are the *no-CVE-needed* outcome the discipline is designed to produce.
