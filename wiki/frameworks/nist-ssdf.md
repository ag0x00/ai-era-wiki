---
type: framework
title: "NIST SSDF (SP 800-218)"
address: c-000047
created: 2026-05-14
updated: 2026-07-30
tags:
  - framework
  - nist
  - secure-sdlc
  - secure-by-design
  - eo-14028
  - ssdf
status: developing
scope_axis:
  - sec-of-ai
  - sec-against-ai
framework_class: secure-sdlc
issuer: "[[nist|NIST]]"
homepage: https://csrc.nist.gov/pubs/sp/800/218/final
doi: 10.6028/NIST.SP.800-218
current_version: "1.1"
first_published: 2022-02
last_substantive_update: 2022-02-03
authors:
  - "Murugiah Souppaya"
  - "Karen Scarfone"
  - "Donna Dodson"
aliases:
  - "SSDF"
  - "SSDF v1.1"
  - "NIST SP 800-218"
  - "Secure Software Development Framework"
related:
  - "[[nist]]"
  - "[[nist-sp-800-218a]]"
  - "[[nist-ai-rmf]]"
  - "[[nist-sp-800-162]]"
  - "[[microsoft-sdl]]"
  - "[[owasp-samm]]"
  - "[[secure-sdlc-framework-stack-2026]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[supply-chain-security-for-agents]]"
  - "[[securing-agentic-coding]]"
sources:
  - "[[.raw/papers/nist-sp-800-218.pdf]]"
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-218.pdf"
---

# NIST SSDF — Secure Software Development Framework (SP 800-218 v1.1)

**NIST SP 800-218 v1.1** (*Secure Software Development Framework: Recommendations for Mitigating the Risk of Software Vulnerabilities*) is NIST's outcomes-oriented secure-SDLC reference, published February 2022 by Souppaya, Scarfone, and Dodson. SSDF defines a core set of high-level secure software development practices that can be integrated into any SDLC model regardless of size, sector, technology, platform, or programming language. It is the federal anchor for secure-by-design software acquisition under **Executive Order 14028 ("Improving the Nation's Cybersecurity," May 2021)** and **OMB M-22-18**: agencies acquiring software must obtain SSDF self-attestation from suppliers.

The framework is **descriptive of outcomes, not prescriptive of tooling**: it defines what good secure development looks like (the practice statements) without specifying how to implement it (left to the references column and organizational risk-based adaptation). This positioning is deliberate. SSDF is intended as a *common vocabulary* for secure-SDLC, not a compliance instrument.

## Structure

SSDF v1.1 organizes practices into **four groups**:

| Group | Code | Scope | Practices in v1.1 |
|---|---|---|---|
| **Prepare the Organization** | PO | Ensure people, processes, and technology are prepared to perform secure software development | PO.1–PO.5 (5 practices) |
| **Protect the Software** | PS | Protect all components from tampering and unauthorized access | PS.1–PS.3 (3 practices) |
| **Produce Well-Secured Software** | PW | Produce software with minimal security vulnerabilities | PW.1, PW.2, PW.4–PW.9 (8 practices; PW.3 is vacant in v1.1 and was added later by [[nist-sp-800-218a\|SP 800-218A]]) |
| **Respond to Vulnerabilities** | RV | Identify residual vulnerabilities and respond appropriately | RV.1–RV.3 (3 practices) |

Each practice is defined by:

- **Practice statement**: outcome-level "what good looks like" (one or two sentences).
- **Tasks**: actions that may be needed to perform the practice. Each task has a unique identifier (e.g., `PO.1.1`).
- **Notional implementation examples**: illustrative options. Stated explicitly as not normative: *"no examples or combination of examples are required."*
- **References**: pointers to other standards/guidance with specific subsection mappings.

The reference column is structurally significant: it maps every task to specific subsections of **BSAFSS, BSIMM, EO 14028, IEC 62443, ISO 27034, MSSDL ([[microsoft-sdl|Microsoft SDL]]), NIST CSF, OWASP ASVS, OWASP MASVS, OWASP SAMM, PCI Secure SLC, SCAGILE, SCFPSSD, SCSIC, NIST SP 800-53, NIST SP 800-160, NIST SP 800-161, and NIST SP 800-181**. This is the master crosswalk that lets organizations citing SSDF auditors map back to any of the upstream sources.

**Microsoft SDL is an explicit named source.** SSDF v1.1's reference column cites `MSSDL` ([[microsoft-sdl|Microsoft Secure Development Lifecycle]]) for at least 8 distinct tasks (PO.1.2, PO.1.3, PO.2.2, PW.1, PW.2, PW.4, PW.7, PW.8 — see Table 1). This is the documentary anchor for the wiki claim that Microsoft SDL is a foundational influence on SSDF. NIST gathered input from Microsoft, Google, AWS, Oracle, IBM, SAFECode, Synopsys, and others during the public-comment phase; SSDF synthesizes their collective practice into a vendor-neutral common vocabulary.

## Four Recommendations (Executive Summary)

SSDF v1.1 articulates four organizational recommendations that map 1-to-1 to the practice groups:

1. **Organizations should ensure that their people, processes, and technology are prepared to perform secure software development** (PO).
2. **Organizations should protect all components of their software from tampering and unauthorized access** (PS).
3. **Organizations should produce well-secured software with minimal security vulnerabilities in its releases** (PW).
4. **Organizations should identify residual vulnerabilities in their software releases and respond appropriately to address those vulnerabilities and prevent similar ones from occurring in the future** (RV).

## Position in the Secure-SDLC Ecosystem

SSDF is the **federal-anchored, vendor-neutral, outcomes-oriented** secure-SDLC framework. Its role differs from peer frameworks along three axes:

| Framework | Class | Issuer | Distinctive role |
|---|---|---|---|
| **NIST SSDF (SP 800-218)** | Secure-SDLC framework (standard) | NIST | **Outcomes-oriented; federal regulatory anchor (EO 14028); vendor-neutral common vocabulary** |
| [[microsoft-sdl\|Microsoft SDL]] | Secure-SDLC framework (vendor) | [[microsoft\|Microsoft]] | Practice-oriented; concrete Microsoft tooling; named source for SSDF |
| [[owasp-samm\|OWASP SAMM v2]] | Secure-SDLC maturity model | OWASP | 5 functions × 3 practices × 3 levels; explicit assessment toolkit |
| BSIMM | Secure-SDLC industry benchmark | Synopsys / Black Duck | Descriptive survey of observed enterprise practices |

For most organizations, **SSDF answers the question "what outcomes are we trying to achieve"**, SAMM answers "where am I and what's next," BSIMM answers "what do peers actually do," and Microsoft SDL answers "give me a concrete vendor-tooled implementation." See [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack for 2026]] for the wiki's thesis on how these compose in 2026.

## Executive Order 14028 — The Regulatory Anchor

SSDF gained its federal weight from EO 14028 Section 4 ("Enhancing Software Supply Chain Security") which directed NIST to:

- Publish "guidelines outlining security measures that shall be applied to the Federal Government's use of critical software."
- Define "criteria for designating software as 'critical software'."
- **Identify practices that enhance the security of the software supply chain** — operationalized as SSDF v1.1.

OMB M-22-18 (September 2022) then required federal agencies to obtain SSDF self-attestation from third-party software producers before using their products. CISA published a Secure Software Development Attestation Form to operationalize that self-attestation. This regulatory pathway makes SSDF compliance functionally mandatory for any vendor selling to the US federal government.

**Appendix A of SSDF v1.1** maps SSDF practices to the specific EO 14028 clauses (4e(i)–(x)): the table that auditors actually use.

## Boundaries of SSDF

The framework is explicit about its limits:

- **Not a checklist.** *"The intention of the SSDF is not to create a checklist to follow, but to provide a basis for planning and implementing a risk-based approach to adopting secure software development practices."*
- **Not a complete software development methodology.** SSDF identifies practices but does not prescribe how to implement them. Organizations are expected to consult the references and apply organizational risk-based judgment.
- **Not all practices apply to all use cases.** Some are foundational, some advanced; practices are not prioritized in the table.
- **Practices are not equally weighted.** Risk, cost, feasibility, applicability, and automatability inform which practices to emphasize.
- **No CSF mapping is guaranteed.** SSDF *may* support NIST CSF Functions/Categories/Subcategories but "SSDF practices do not map to them and are typically the responsibility of different parties."

## The AI Extension — SP 800-218A

In response to **Executive Order 14110 (October 2023) Section 4.1.a**, NIST published [[nist-sp-800-218a|SP 800-218A — *Secure Software Development Practices for Generative AI and Dual-Use Foundation Models: An SSDF Community Profile*]] in July 2024. SP 800-218A is a **Community Profile**: a baseline of SSDF practices and tasks enhanced with recommendations, considerations, notes, and informative references specific to AI model development. It augments SSDF v1.1 with:

- **3 new tasks** explicitly marked `[Not part of SSDF 1.1]`: `PO.5.3` (continuous execution-environment monitoring), `PS.1.2` (protect training data), `PS.1.3` (protect model weights and configuration parameters)
- **1 new practice** (`PW.3`, Confirm the Integrity of Training, Testing, Fine-Tuning, and Aligning Data Before Use) with 3 new tasks (`PW.3.1`–`PW.3.3`)
- **Modifications** to several existing practices (PO.5, PS.1, PS.3.2, PW.2.1, PO.1.3) flagged `[Modified from SSDF 1.1]`
- **AI-specific recommendations, considerations, and notes** for ~30 existing tasks, plus **priorities** (High/Medium/Low), *not present in base SSDF*.

See [[nist-sp-800-218a|the 218A page]] for the full AI Profile analysis.

## Future Work and v2 Trajectory

As of mid-2026, SSDF v1.1 (Feb 2022) is the current published version. The publication's "Future Work" section anticipates expansion in three directions:

- **DevOps-specific guidance**: how organizations transition from current practices to SSDF practices in CI/CD pipelines.
- **Open-source software guidance**: how SSDF applies to OSS development and consumption.
- **Use-case-specific guidance**: sector-specific guidance (the AI Profile is the first instance).

A v2 update or a parallel revision has not been announced. Tracking this is one of the wiki's [[secure-sdlc-framework-stack-2026|framework-stack thesis]] open sub-questions: *"SLSA's relationship to SSDF — whether SSDF v2 (if it happens) absorbs SLSA-grade requirements or whether the two remain parallel."*

## In the RA / CMM

SSDF is a **policy-anchor framework** for the wiki's secure-SDLC ecosystem. It does not map to specific [[agentic-ai-security-reference-architecture|RA]] planes (it is a program-level framework, not a runtime architecture) but it does inform multiple [[agentic-ai-security-cmm-2026|CMM]] domains:

| CMM domain | SSDF tie |
|---|---|
| D1 — Governance & Accountability | PO.1 (Define Security Requirements), PO.2 (Roles and Responsibilities), PO.4 (Define Criteria) |
| D2 — Risk Management | PW.1 (Risk Modeling at Design Time) |
| D6 — Supply Chain & Component Governance | PS.3.2 (Provenance / SBOM / SLSA), PW.4 (Reuse Well-Secured Components), PO.1.3 (Communicate to Third Parties) |
| D7 — Observability & Anomaly Detection | PO.5.3 (Continuous Monitoring — added by 218A) |
| D9 — Incident Response & Recovery | RV.1–RV.3 (Identify, Remediate, Root-Cause Analyze) |

The [[agentic-ai-security-cmm-2026|CMM]]'s D6 cites both SP 800-218 and SP 800-218A as the supply-chain references. The [[agentic-ai-security-cmm-crosswalk|CMM crosswalk]] details each domain's SSDF anchors.

## Placement in this wiki

- **First dedicated page on the base SSDF** (the wiki previously had only a stub for the AI extension `[[nist-sp-800-218a|NIST SP 800-218A — SSDF Community Profile for Generative AI and Dual-Use Foundation Models]]`).
- **Anchor for the [[secure-sdlc-framework-stack-2026|2026 framework-stack thesis]].** SSDF is the prescribed "policy anchor" in that thesis's recommended stack; this page provides the citation surface.
- **Documentary anchor for the Microsoft SDL → SSDF lineage.** Table 1 cites `MSSDL` directly; the lineage claim is no longer a wiki assertion but a source-citable fact.
- **Federal regulatory anchor** for any wiki claim about EO 14028 / OMB M-22-18 secure-software-acquisition requirements.

> [!gap] The producing agent is outside SSDF's frame
> SSDF's practice groups govern the organization, the software, and the build environment. Where an autonomous agent holds repository credentials and executes commands inside that build environment, no practice names it as a subject with its own identity, permissions, or isolation requirements. [[nist-sp-800-218a|SP 800-218A]] extends the profile to AI systems being *built*, not to agents doing the building. Recorded as [[secure-sdlc-framework-stack-2026|Gap 4]] of the framework-stack thesis; the controls that exist today are vendor-side and catalogued in [[securing-agentic-coding|Securing Agentic Coding]].

## See also

- [[nist-sp-800-218a|NIST SP 800-218A — SSDF Community Profile for Generative AI and Dual-Use Foundation Models]] — the AI extension
- [[nist|NIST]] — issuing organization
- [[apostol-vassilev|Apostol Vassilev]] — NIST adversarial-ML lead (co-author of 218A and the AI 100-2e2023 taxonomy that 218A references)
- [[microsoft-sdl|Microsoft Secure Development Lifecycle (SDL)]] — peer secure-SDLC framework, explicit named source for SSDF
- [[owasp-samm|OWASP SAMM v2]] — peer maturity model
- [[nist-ai-rmf|NIST AI RMF 1.0]] — peer NIST AI-security framework (governance-side companion to SSDF's development-side scope)
- [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack for 2026]] — thesis on how SSDF / SAMM / AI overlays compose
- [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] — adjacent thesis
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — wiki CMM citing SSDF in multiple domains

<!-- sources:auto -->
## Sources

- [NIST SSDF (SP 800-218)](https://csrc.nist.gov/pubs/sp/800/218/final)
- [nvlpubs.nist.gov](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-218.pdf)
<!-- /sources -->
