---
type: practice
title: "AI-BOM: AI Bill of Materials"
created: 2026-04-30
updated: 2026-09-29
tags:
  - practices
  - supply-chain
  - ai-bom
  - sbom
  - agentic-ai
status: developing
scope_axis:
  - sec-of-ai
maturity: early
addresses_threat: "Supply chain opacity (ASI04), model provenance gaps, untracked skill/plugin dependencies, data poisoning via unattested training data"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[supply-chain-security-for-agents]]"
  - "[[security-controls-for-ai-stacks]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[clawhavoc]]"
  - "[[litellm-supply-chain-compromise]]"
  - "[[jfrog-ssc-state-of-union-2026]]"
  - "[[nist-ai-600-1]]"
sources:
  - "[[.raw/papers/emerging-cybersecurity-practices-for-agentic-ai-applications.md]]"
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Live JFrog, SLSA and arXiv sources checked; CISA PDF access failed and archived source set was not verified in full."
---

# AI-BOM: AI Bill of Materials

## Definition

An **AI Bill of Materials (AI-BOM)** is a machine-readable record of the known components in a released AI system, analogous to a Software Bill of Materials (SBOM). For an agent, it can include models or hosted-model references, skills, MCP servers, libraries, instruction files, and available provenance for data and build artifacts. Supplier-held details that are unavailable remain explicit gaps.

The term is used in two related but distinct senses:

1. **Static AI-BOM**: a manifest produced at build/deploy time listing all AI system components and their provenance. Enables supply chain auditing.
2. **Runtime AI-BOM** (Miggo Security's usage): continuous discovery and tracking of what AI components are actually running in production — analogous to a CMDB but for AI artifacts. Enables behavioral drift detection.

## Significance

JFrog's 2026 research identified **495 malicious Hugging Face models** carrying live payloads. Its survey found that **53% of organizations self-host models** and **97% claim certified model governance**.[^jfrog-ssc] Three Q1 2026 supply chain incidents ([[clawhavoc|ClawHavoc — Agentic Skill Marketplace Supply Chain Attack]], [[sandworm-mode-npm-worm|SANDWORM_MODE npm worm — AI Toolchain Poisoning]], [[litellm-supply-chain-compromise|LiteLLM Supply Chain Compromise (Google ADK Dependency)]]) also show attacks on plugins, skills, and framework dependencies.

An AI-BOM lets the team answer which model and skills were released, where each MCP server came from, and what provenance is available for data and build artifacts. The runtime record then tests whether production still matches that release.

The component inventory an AI-BOM provides is the concrete answer to the [[nist-ai-600-1|NIST AI 600-1]] GenAI Profile's value-chain and component-integration risk category, which flags the opacity of third-party models, data, and software a GenAI system inherits. The Profile supplies the BOM ingredients in its Suggested Actions — provenance fields (`GV-1.6-003`), approved-provider lists (`GV-6.1-007`), and model/system cards (`MG-3.1-005`) — without assembling them into a single artifact; an AI-BOM makes that inherited chain enumerable rather than implicit.

## Components to Track

An AI-BOM for an agentic deployment should cover:

| Component Category        | What to Track                                          | Why It Matters                                  |
|---------------------------|--------------------------------------------------------|-------------------------------------------------|
| Model or hosted-model reference | Name, disclosed version, provider; digest for weights the organization holds | Model substitution, backdoored weights |
| Skills / plugins          | Name, version, publisher, install source, SHA-256, behavioral scope | ClawHavoc-class supply chain attacks       |
| MCP servers               | Name, version, origin, transport security, allowed tools | Tool poisoning, unauthorized tool exposure     |
| Cognitive identity files  | SOUL.md, IDENTITY.md — hash, change history           | Behavioral hijacking without code changes       |
| Framework dependencies    | LangChain, CrewAI, AutoGEN, etc. — version, license   | Dependency confusion, LiteLLM-class compromises |
| RAG data sources          | Corpus version, last scan date, access controls       | RAG poisoning, [[indirect-prompt-injection\|indirect prompt injection]]        |
| Orchestration code        | Version, signing, SLSA provenance level               | Code-level tampering                            |

## Format and Standards

- **CycloneDX ML extension**: supports model metadata, dataset references, and algorithm documentation in a machine-readable bill.
- **CISA and G7 minimum elements**: *Software Bill of Materials for AI — Minimum Elements*, published 2026-05-12, enumerates what an AI-BOM contains in seven clusters: metadata, system-level properties, models, dataset properties, infrastructure, security properties and key performance indicators. It prescribes no format, states that its elements are not mandatory, and puts requirements, standards, legislation and implementation detail outside its scope, so it sets the content list and creates no obligation to produce one.[^g7-aisbom]
- **SPDX**: another machine-readable bill format for software and AI components; check that the chosen profile covers the deployment's model and tool fields.
- **SLSA (Supply chain Levels for Software Artifacts)**: build provenance for release artifacts the organization produces. D8-BUILD tests SLSA Build L2 or higher when a build produces a release artifact.

Add deployment-specific fields where needed:

- Cognitive identity file hashes
- MCP server behavioral scope declarations
- Agent-to-agent communication topology
- Skill permission scopes (what tools/APIs can this skill invoke?)

## Runtime AI-BOM (Miggo Pattern)

**Miggo Security** describes its Runtime Defense Platform as using an AI-BOM discovery approach:

1. At deploy time: inventory all AI components (model, framework, skills, MCP servers).
2. At runtime: use **DeepTracing** to observe actual component behavior — what tools each component invokes, what data it accesses, what network destinations it calls.
3. Continuously: compare runtime behavior against the inventoried-at-deploy baseline. Deviation = alert.

This extends the static AI-BOM into a live behavioral inventory, analogous to what EDR does for processes vs. what a static CMDB does for assets.

## Maturity Progression

The [[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Engineering and Supply Assurance]] deep dive grades the AI-BOM alongside secure design, implementation, testing, and release. The following rows describe only its AI-BOM-related criteria; they do not establish a D8 domain level by themselves.

| CMM level | Applicable AI-BOM outcome |
|---|---|
| L2 | D8-INVENTORY records each deployed component, its source and version, with a digest for held files or images. |
| L3 | D8-AIBOM produces a machine-readable bill for each agent version the organization releases. D8-SIGN covers release artifacts the organization builds. |
| L4 | D8-AIBOM-RUNTIME compares observed components with the approved release bill. D8-LINEAGE records the lineage of models the organization produces. |
| L5 | D8-AIBOM-DRIFT closes or escalates differences within a stated tolerance; D8-VERIFY-GATE refuses unapproved component sets on every promotion path. |

A hosted-model consumer can still need an agent AI-BOM for a version it releases. D8-LINEAGE applies only where an agent calls a model the organization produces. D8-AIBOM-RUNTIME requires the release bill as its comparison baseline.

## Implementation Priorities

1. **Start with model inventory**: know what model version is running in each agent.
2. **Add skills/plugins**: every installed skill should be tracked with source, hash, install date.
3. **Layer in MCP servers**: as MCP adoption grows, MCP server provenance becomes critical.
4. **Automate generation**: build AI-BOM generation into the CI/CD pipeline. cdxgen's `aibom` command takes Hugging Face package URLs and direct Modelfile or GGUF inputs, emits CycloneDX 1.7 and submits the result to a Dependency-Track server; Anchore Syft catalogues GGUF and SafeTensors components in the same pipeline.[^aibom-tooling]
5. **Connect release and runtime records to incident review**: the release bill identifies approved parts; runtime reconciliation shows which parts were observed when an event occurred.

## Known Gaps

- The CISA and G7 minimum elements leave format and implementation detail open; an agent bill still needs fields for its cognitive files, MCP scope, and skill permissions.[^g7-aisbom]
- The Q1 2026 source survey describes Miggo's runtime discovery pattern, but does not establish market-wide runtime coverage.
- The Anchore capability page lists vulnerability scanning of AI artifacts as unsupported. A pipeline using that tool needs a separate method to consume findings about model or agent components.[^aibom-tooling]
- A 2026 preprint generated **97,940** AI-BOMs from public Hugging Face records with more than 100 downloads. The generated artifacts exposed gaps in model-card and provenance data; the study did not measure whether publishers distribute their own bills.[^aibom-completeness]
- No AI-specific exploitability profile exists in the documents read. The OWASP CycloneDX authoritative guide to AI/ML-BOM of June 2026 does not mention VEX, and the CISA and G7 guidance's nearest element links to external vulnerability databases instead of asserting exploitability.[^aibom-vex] [[agentic-ai-security-reference-architecture|The reference architecture]] places component provenance at the release boundary; it does not prescribe a vulnerability-consumption method for the bill.

## Notes

[^jfrog-ssc]: [JFrog — 2026 Software Supply Chain Security State of the Union (announcement)](https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs), 2026, report p.5–6. 495 malicious Hugging Face models carrying live payloads; 53% of organizations self-host AI models; 97% claim certified model governance. See [[jfrog-ssc-state-of-union-2026|JFrog 2026 SSC State of the Union]].
[^g7-aisbom]: [CISA — Software Bill of Materials for AI: Minimum Elements](https://www.cisa.gov/resources-tools/resources/software-bill-materials-ai-minimum-elements), published 2026-05-12, with the guidance served at [BSI — SBOM for AI: minimum elements (PDF)](https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/KI/SBOM-for-AI_minimum-elements.pdf); retrieved 2026-09-18. The output of the G7 Cybersecurity Working Group between August 2025 and February 2026.
[^aibom-tooling]: [cdxgen — README](https://github.com/cdxgen/cdxgen/blob/master/README.md) and [Anchore OSS — AI capabilities](https://oss.anchore.com/docs/capabilities/ai/), both retrieved 2026-09-18. cdxgen targets CycloneDX 1.6, 1.7 and 2.0 with 1.7 as the default and submits to Dependency-Track; the Anchore capability table lists a GGUF cataloguer and SafeTensors support and lists vulnerability scanning of AI artifacts as unsupported at this time.
[^aibom-completeness]: ["A Large-Scale Measurement of AI Bill of Materials Completeness in Hugging Face Models"](https://arxiv.org/html/2607.17242) (arXiv preprint, 2026-07-19), retrieved 2026-09-29. The researchers collected 2,942,466 public model records, retained models with more than 100 downloads, and generated 97,940 bills with the OWASP AIBOM Generator. The measured fields reflect repository information available to that generator.
[^aibom-vex]: [OWASP CycloneDX — Authoritative Guide to AI/ML-BOM (PDF)](https://cyclonedx.org/guides/OWASP_CycloneDX-Authoritative-Guide-to-AI-ML-BOM-en.pdf), First Edition Revision 1, dated 2026-06-10, retrieved 2026-09-18: a case-insensitive search of the extracted full text returns no occurrence of VEX.

## See Also

- [[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]] — the broader supply chain practice this feeds into
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] — AI-BOM closes the flagged data-layer gap
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — [[agentic-ai-security-cmm-d8-supply-chain|D8 Engineering and Supply Assurance]] grades the release AI-BOM at L3 (D8-AIBOM), runtime reconciliation at L4 (D8-AIBOM-RUNTIME), and time-bound drift handling at L5 (D8-AIBOM-DRIFT); cross-vendor BOM federation is not a scored criterion
