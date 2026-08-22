---
type: entity
title: "AISLE"
address: c-000082
created: 2026-05-15
updated: 2026-08-21
tags:
  - entities
  - organizations
  - vuln-discovery
  - ai-native
status: developing
scope_axis: [ai-in-sec-defense]
entity_type: organization
org_type: vendor
role: "AI-native cybersecurity platform company. Builds an autonomous analyzer that discovers and proposes fixes for vulnerabilities in widely deployed open-source software. Surfaced 12 of 12 CVEs in the January 2026 coordinated OpenSSL release (including a CVSS 9.8 stack-buffer-overflow dating to 1998); 5 CVEs in curl (now partnered to secure curl's codebase); a 21-year-old FreeBSD remote-command-execution flaw (CVE-2026-42511). Tagline: *Zero is Everything*."
homepage: "https://aisle.com"
first_mentioned: "[[mythos-ready-briefing|Mythos-ready paper]]"
related:
  - "[[aisle-openssl-12-of-12|AISLE — 12 of 12 OpenSSL Vulnerabilities (Jan 2026)]]"
  - "[[ai-cybersecurity-after-mythos-jagged-frontier|AI Cybersecurity After Mythos: The Jagged Frontier]]"
  - "[[jagged-frontier|Jagged Frontier]]"
  - "[[stanislav-fort|Stanislav Fort]]"
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[unprompted-conference-march-2026|Unprompted Conference March 2026]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[openant|OpenAnt]]"
  - "[[codex-security|Codex Security]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[adversarial-reflexion|Adversarial Reflexion]]"
sources:
  - https://aisle.com
  - https://aisle.com/blog/aisle-discovered-12-out-of-12-openssl-vulnerabilities
  - https://aisle.com/blog/aisle-discovers-three-of-the-four-openssl-vulnerabilities-of-2025
  - https://aisle.com/blog/curl-adopts-aisle-after-its-ai-agents-discovered-5-cves
  - https://aisle.com/blog/aisle-discovers-cve-2026-42511-a-21-year-old-freebsd-remote-command-execution-vulnerability
  - https://aisle.com/blog/ai-cybersecurity-after-mythos-the-jagged-frontier
  - https://aisle.com/blog/how-aisles-autonomous-analyzer-found-command-injection-in-globs-cli-10m-weekly-downloads
  - https://www.schneier.com/blog/archives/2026/02/ai-found-twelve-new-vulnerabilities-in-openssl.html
  - "[[.raw/articles/aisle-12-of-12-openssl-vulnerabilities-2026-01.md]]"
  - "[[.raw/articles/aisle-autonomous-analyzer-2026-05-23.md]]"
---

# AISLE

**Sources:** [aisle.com](https://aisle.com) · [12-of-12 OpenSSL blog post](https://aisle.com/blog/aisle-discovered-12-out-of-12-openssl-vulnerabilities) · [Schneier on Security commentary](https://www.schneier.com/blog/archives/2026/02/ai-found-twelve-new-vulnerabilities-in-openssl.html)

## Description

AI-native cybersecurity platform company building an autonomous analyzer that discovers (and proposes fixes for) vulnerabilities in widely deployed open-source software. The company positions itself against the *"Shift Left"* SAST genealogy with a *"Shift to AI"* framing (per Ondrej Burianek's testimonial on the homepage). Tagline: *"Zero is Everything"*, meaning autonomous vulnerability discovery and remediation at scale.

## Relevance to This Wiki

AISLE is the wiki's anchor entity for **AI-driven discovery of decade-class latent vulnerabilities in widely deployed open-source cryptographic infrastructure**. Three signal disclosures so far:

1. **12 of 12 OpenSSL CVEs** in the January 2026 coordinated release, including **CVE-2025-15467**, a CVSS 9.8 stack-buffer-overflow in CMS AuthEnvelopedData parsing **dating to 1998**. See the [[aisle-openssl-12-of-12|dedicated paper page]]. Spans 8+ subsystems (CMS, QUIC, TLS 1.3, post-quantum ML-DSA signatures, PKCS#12, OCB, TimeStamp, PKCS#7).
2. **5 CVEs in curl** plus a follow-on partnership: *"curl Now Uses Our AI to Secure Its Code"* (May 13, 2026; Joshua Rogers writing for AISLE). The partnership matters because [[mythos-ready-briefing|the Mythos-ready briefing]] cited curl's earlier *discontinuation of its bug bounty over AI-generated "slop"*. The AISLE-curl tie-up is the **operational resolution** to that pattern: AI-supported quality findings rather than AI-generated noise.
3. **CVE-2026-42511**, a 21-year-old FreeBSD remote-command-execution vulnerability (May 7, 2026; Joshua Rogers blog post).

Per Bruce Schneier's testimonial on the AISLE homepage: *"AISLE is credited for surfacing 13 of 14 OpenSSL CVEs assigned in 2025, and 15 total across both releases. This is a historically unusual concentration for any single research team, let alone an AI-driven one."*

Position relative to peer AI vulnerability-discovery instruments:

| Instrument | Domain | Primary FP-control mechanism (per [[adversarial-reflexion\|Adversarial Reflexion]]) |
| :--- | :--- | :--- |
| [[openant\|OpenAnt]] (Knostic, OSS) | App-code vuln discovery | Constrained-attacker-persona + explicit trace + tool-use verification |
| [[codex-security\|Codex Security]] / Aardvark (OpenAI) | App-code vuln discovery | Sandboxed exploit-trigger validation |
| [[claude-code-security\|Claude Code Security]] (Anthropic) | App-code vuln discovery | Self-critique prove/disprove |
| [[mdash\|MDASH]] (Microsoft) | App-code vuln discovery | Ensemble + debater + prover-stage |
| **AISLE** | App-code vuln discovery (esp. cryptographic libraries) | **Stage-3 triage and verification ("false-positive discrimination").** Disclosed in the *Jagged Frontier* post: a five-stage hybrid AI-plus-symbolic cyber-reasoning system whose make-or-break stage validates candidates into working PoCs before disclosure. |

## The autonomous analyzer (disclosed mechanism)

AISLE's [[ai-cybersecurity-after-mythos-jagged-frontier|*Jagged Frontier* post]] (April 2026) and its glob-CLI disclosure (November 2025) describe the analyzer as a **hybrid of AI and symbolic reasoning**: a cyber-reasoning system, not a single model. Its central thesis is *"the moat is the system, not the model."* The pipeline runs five modular stages:

1. **Broad-spectrum scanning.** Navigate a large codebase to identify candidate functions worth examining. AISLE deliberately uses cheap, small models here (its "thousand adequate detectives" argument) rather than one expensive frontier model.
2. **Vulnerability detection.** Find flaws in isolated code regions.
3. **Triage and verification.** Separate true positives from false positives, which AISLE calls the make-or-break capability and ties to curl's decision to end its bug bounty over AI-generated noise. The stage produces a validated PoC to confirm CVE-class impact rather than mere "bad practice."
4. **Patch generation.** Propose a fix. In the glob case the maintainers shipped AISLE's proposed patch.
5. **Exploit construction.** Turn a vulnerability into a working attack; optional, and held back for defensive use.

The architectural claim is **model-agnostic orchestration**: the durable advantage is the scaffolding, not any one frontier model. That scaffolding covers targeting, cost-optimized model routing, validation, and integration with maintainer trust, where success is measured by report acceptance. AISLE reports that small open-weight models reproduce much of Claude Mythos's flagship results in isolated-code tests, which it reads as confirmation that capability is [[jagged-frontier|jagged]] (it does not scale smoothly with model size) and that no single model is best across tasks.

The post stops short of naming which symbolic techniques the system uses (taint analysis, symbolic execution, SMT solving) or how the AI and symbolic components are wired together. The five-stage shape and the model-agnostic thesis are disclosed; the internal wiring is not.

**Disclosure record (cumulative, per the post):** 180+ externally validated CVEs across 30+ projects, including 15 in OpenSSL (12 in one release, one at CVSS 9.8) and 5 in curl, some dating back 25+ years. A representative recent case is **CVE-2025-64756**, a command-injection RCE (CVSS 7.5, CWE-78) in the `glob` Node.js package (10M+ weekly downloads), found and fixed autonomously with a four-day disclosure-to-patch turnaround.

## People

- **[[stanislav-fort|Stanislav Fort]]**: AISLE founder and chief scientist; an early scientist at [[anthropic|Anthropic]]; author of the 12-of-12 OpenSSL disclosure and *"AI Cybersecurity After Mythos: The Jagged Frontier"* (April 7, 2026, same day as the Mythos Preview release).
- **Petr Šimeček**, **Tomas Dulka**, **Luigino Camastra**: AISLE researchers contributing to the OpenSSL discoveries (per the blog post acknowledgments).
- **Joshua Rogers**: AISLE blog author; wrote the curl-partnership and FreeBSD CVE-2026-42511 posts.
- **Ondrej Vlcek**: co-founder; former CEO of Avast.
- **Jaya Baloo**: co-founder; former CISO of Rapid7.

## Endorsements

Publicly testimonial endorsements from:

- **Bruce Schneier** (Inrupt; Harvard Kennedy School): historically-unusual-concentration framing.
- **J. Michael Daniel**: vulnerability management framing.
- **Aernout Reijmer**: perfect-storm framing (overstretched teams, shrinking exploit time, rising regulation).
- **Daniel Stenberg**: curl creator; *"An excellent new project. A powerful analyzer that highlights code areas that need more attention in ways the old generation of code tools have not been able to."*
- Ataccama, Ondrej Burianek, David Dolezal: practitioner endorsements.

## Adjacent / Open

- **Mechanism gap now closed (2026-05-23).** AISLE disclosed the five-stage cyber-reasoning-system shape and its model-agnostic thesis (see above), so the [[adversarial-reflexion|cross-product comparison]] cell is filled: stage-3 false-positive discrimination with PoC validation. The internal symbolic-reasoning techniques remain undisclosed, the residual gap.
- **Founders captured (2026-05-23):** Stanislav Fort (chief scientist, ex-Anthropic), Ondrej Vlcek (ex-Avast CEO), Jaya Baloo (ex-Rapid7 CISO). Headcount and funding still not captured.
- **AISLE on the [[unprompted-conference-march-2026|Unprompted March 2026]] key-claim.** Already named in the conference's overall key-claim alongside FENRIR, Promp2Pwn, and XBOW as systems running autonomous bug-finding agents at production scale. The conference page should cross-link to this entity.
