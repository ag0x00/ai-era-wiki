---
type: practice
title: "AI-BOM: AI Bill of Materials"
created: 2026-04-30
updated: 2026-06-23
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
---

# AI-BOM: AI Bill of Materials

## Definition

An **AI Bill of Materials (AI-BOM)** is a structured inventory of all components that compose an agentic AI system — analogous to a Software Bill of Materials (SBOM) for traditional software — extended to capture the artifact categories unique to AI deployments: model weights, training data attestations, skills/plugins, MCP servers, and cognitive identity files.

The term is used in two related but distinct senses:

1. **Static AI-BOM**: a manifest produced at build/deploy time listing all AI system components and their provenance. Enables supply chain auditing.
2. **Runtime AI-BOM** (Miggo Security's usage): continuous discovery and tracking of what AI components are actually running in production — analogous to a CMDB but for AI artifacts. Enables behavioral drift detection.

## Significance

ML-BOM adoption lags **48% behind** SBOM requirements as of June 2025 (Lineaje survey). JFrog reported a **6.5-fold increase** in malicious models on Hugging Face in 2024–2025. Its 2026 report adds detail: the JFrog Security Research team identified **495 malicious models on Hugging Face** carrying live payloads (reverse shells, credential harvesting, command execution), while **53% of organizations self-host models** from such registries and **97% claim certified model governance**. The gap between claimed governance and actual exposure is what the component inventory below addresses.[^jfrog-ssc] Meanwhile, three Q1 2026 supply chain incidents ([[clawhavoc|ClawHavoc — Agentic Skill Marketplace Supply Chain Attack]], [[sandworm-mode-npm-worm|SANDWORM_MODE npm worm — AI Toolchain Poisoning]], [[litellm-supply-chain-compromise|LiteLLM Supply Chain Compromise (Google ADK Dependency)]]) demonstrate that attackers are exploiting AI supply chains without requiring model compromise — they target the plugins, skills, and framework dependencies.

Without an AI-BOM, security teams cannot answer: "What model version is running in production? What skills are installed on this agent? Where did that MCP server come from? Has this training data been attested?"

The component inventory an AI-BOM provides is the concrete answer to the [[nist-ai-600-1|NIST AI 600-1]] GenAI Profile's value-chain and component-integration risk category, which flags the opacity of third-party models, data, and software a GenAI system inherits. The Profile supplies the BOM ingredients in its Suggested Actions — provenance fields (`GV-1.6-003`), approved-provider lists (`GV-6.1-007`), and model/system cards (`MG-3.1-005`) — without assembling them into a single artifact; an AI-BOM makes that inherited chain enumerable rather than implicit.

## Components to Track

An AI-BOM for an agentic deployment should cover:

| Component Category        | What to Track                                          | Why It Matters                                  |
|---------------------------|--------------------------------------------------------|-------------------------------------------------|
| Model weights             | Name, version, provider, SHA-256, training data attestation | Model substitution, backdoored weights         |
| Skills / plugins          | Name, version, publisher, install source, SHA-256, behavioral scope | ClawHavoc-class supply chain attacks       |
| MCP servers               | Name, version, origin, transport security, allowed tools | Tool poisoning, unauthorized tool exposure     |
| Cognitive identity files  | SOUL.md, IDENTITY.md — hash, change history           | Behavioral hijacking without code changes       |
| Framework dependencies    | LangChain, CrewAI, AutoGEN, etc. — version, license   | Dependency confusion, LiteLLM-class compromises |
| RAG data sources          | Corpus version, last scan date, access controls       | RAG poisoning, [[indirect-prompt-injection\|indirect prompt injection]]        |
| Orchestration code        | Version, signing, SLSA provenance level               | Code-level tampering                            |

## Format and Standards

- **CycloneDX ML extension**: the most AI-specific format; supports model metadata, dataset references, algorithm documentation. Recommended for static AI-BOMs.
- **SPDX**: more mature tooling ecosystem; less AI-specific but acceptable for framework-level dependencies.
- **SLSA (Supply chain Levels for Software Artifacts)**: the provenance framework; aim for SLSA Level 2+ for models deployed in production.

Agentic-specific fields not yet covered by existing standards:
- Cognitive identity file hashes
- MCP server behavioral scope declarations
- Agent-to-agent communication topology
- Skill permission scopes (what tools/APIs can this skill invoke?)

## Runtime AI-BOM (Miggo Pattern)

**Miggo Security**'s Runtime Defense Platform uses an AI-BOM discovery approach:

1. At deploy time: inventory all AI components (model, framework, skills, MCP servers).
2. At runtime: use **DeepTracing** to observe actual component behavior — what tools each component invokes, what data it accesses, what network destinations it calls.
3. Continuously: compare runtime behavior against the inventoried-at-deploy baseline. Deviation = alert.

This extends the static AI-BOM into a live behavioral inventory, analogous to what EDR does for processes vs. what a static CMDB does for assets.

## Maturity Progression

Align AI-BOM maturity with the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] **D8 Supply Chain & AI-BOM** domain:

| CMM Level | AI-BOM Capability                                                       |
|-----------|-------------------------------------------------------------------------|
| Level 1   | No AI component inventory                                               |
| Level 2   | Manual model inventory; ad hoc tracking                                 |
| Level 3   | Automated AI-BOM generation at build time; SHA-256 for all components   |
| Level 4   | Signed AI-BOMs; ML-BOM for all production models; runtime discovery     |
| Level 5   | Full provenance verification (SLSA); continuous runtime BOM diffing; threat intel integration |

Level 4 corresponds to "ML-BOM for all production models" in the CMM's Domain 5 (Supply Chain) criteria.

## Implementation Priorities

1. **Start with model inventory**: know what model version is running in each agent.
2. **Add skills/plugins**: every installed skill should be tracked with source, hash, install date.
3. **Layer in MCP servers**: as MCP adoption grows, MCP server provenance becomes critical.
4. **Automate generation**: build AI-BOM generation into the CI/CD pipeline, not as a manual step.
5. **Feed to SIEM**: AI-BOM data enables correlation — when an incident occurs, the BOM tells you what was running.

## Known Gaps

- No universal standard for agentic-specific AI-BOM fields (cognitive files, MCP scope, skill permissions).
- Runtime AI-BOM tooling is nascent — Miggo is the most specific implementation evidence available as of Q1 2026.
- No enforcement mechanism equivalent to SBOM mandates (e.g., Executive Order 14028 for software) specifically for AI components.

## Notes

[^jfrog-ssc]: [JFrog — 2026 Software Supply Chain Security State of the Union (announcement)](https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs), 2026, report p.5–6. 495 malicious Hugging Face models carrying live payloads; 53% of organizations self-host AI models; 97% claim certified model governance. See [[jfrog-ssc-state-of-union-2026|JFrog 2026 SSC State of the Union]].

## See Also

- [[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]] — the broader supply chain practice this feeds into
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] — AI-BOM closes the flagged data-layer gap
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — **D8 Supply Chain & AI-BOM** progression criteria reference AI-BOM at L3 (CycloneDX/SPDX 3.0 build-time), L4 (sigstore + runtime reconciliation), L5 (cross-vendor federation, SLSA Level 4 for AI artifacts)
