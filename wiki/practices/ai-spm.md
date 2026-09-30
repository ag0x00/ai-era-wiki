---
type: practice
title: "AI Security Posture Management (AI-SPM)"
created: 2026-05-01
updated: 2026-09-30
tags:
  - practices
  - posture-management
  - ai-spm
  - ai-bom
  - inventory
status: developing
scope_axis:
  - sec-of-ai
maturity: emerging
addresses_threat: "Misconfigured AI infrastructure: open indexes, stale embeddings, weak allow-lists, missing audit logging, untracked models / prompts / connectors"
related:
  - "[[onyx-platform]]"
  - "[[palo-alto-prisma-airs]]"
  - "[[wiz-ai-app]]"
  - "[[wiz-ai-app-launch]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[ai-data-security]]"
  - "[[ai-bom]]"
  - "[[dspm]]"
  - "[[guardian-agent]]"
  - "[[agent-observability]]"
  - "[[security-controls-for-ai-stacks]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[standards-review-eu-ai-act-2026-Q2]]"
sources:
  - "[[.raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md]]"
  - "https://onyx.security/platform"
  - "https://docs.paloaltonetworks.com/prisma-airs/ai-inventory/agent-discovery/ai-agent-discovery-with-cortex-cloud"
  - "https://www.wiz.io/blog/ai-security-posture-management"
  - "[[.raw/articles/introducing-wiz-ai-app-2026-09-30.md]]"
  - "[[.raw/articles/knostic-ai-data-security-2026-05-01.md]]"
  - "https://www.knostic.ai/blog/ai-data-security"
verified: 2026-09-30
verified_against:
  - ".raw/articles/introducing-wiz-ai-app-2026-09-30.md"
  - ".raw/articles/knostic-ai-data-security-2026-05-01.md"
  - ".raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md"
verified_findings: 0
verified_note: "Wiz, Knostic and Onyx archives reviewed in this pass; added PA posture linkage against live Cortex AI-SPM docs, including license and region limits."
---

# AI Security Posture Management (AI-SPM)

**AI-SPM** is the AI analog to **CSPM** (Cloud Security Posture Management): a continuous discipline of inventorying AI assets, detecting misconfigurations, and tracking remediation across an enterprise's AI footprint. It treats AI infrastructure as a posture surface rather than a one-time security review.

## Definition

A complete AI inventory plus continuous misconfiguration detection across:

- **Models**: production, staging, fine-tuned variants, embedding models
- **Prompts**: system prompts, prompt templates, chain templates
- **Tools / connectors**: MCP servers, API integrations, plugins
- **Datasets and indexes**: training data, RAG corpora, vector stores
- **Caches and logs**: prompt caches, response caches, audit logs

For each asset: owner, environment, applicable policies, last-verified-state.

## Distinction from CSPM

CSPM checks whether your cloud resources are configured against published baselines (CIS, AWS Well-Architected, etc.). AI-SPM has fewer published baselines and more emergent risk patterns. Common AI-specific misconfigurations:

- **Open indexes**: vector stores or RAG corpora reachable without authentication
- **Stale embeddings**: vector representations of documents that have since been deleted, redacted, or had permissions tightened (the index still contains the original meaning)
- **Weak allow-lists**: agent tool calls permitted to broad domains rather than specific endpoints
- **Missing logging**: model calls invoked without OpenTelemetry instrumentation; retrieval paths not traced
- **Permission drift** between document stores and AI retrieval: document permissions tightened but the AI's index still serves the old content
- **Shadow connectors**: MCP servers or plugins installed locally without inventory
- **Default system prompts**: prompts shipped with vendor defaults that contain confidentiality leaks (LLM07:2025)

## Operations

The [Knostic article on AI data security](https://www.knostic.ai/blog/ai-data-security) and CMM-aligned guidance converge on these AI-SPM operational primitives:

1. **Inventory everything.** Models, prompts, tools, connectors, datasets, indexes, caches, logs.
2. **Map each asset to owners, environments, and policies.** No orphaned assets.
3. **Continuously check for misconfigurations.** Drift, not the state at ingest, is the dominant risk.
4. **Align checks to [[nist-ai-rmf|AI RMF]] "Measure" and "Manage."** Risk-based, prioritized.
5. **Record exceptions with time limits and reviewers.** Compliance trail.
6. **Demonstrate event logging and post-market monitoring** — [[eu-ai-act|EU AI Act]] Art. 12 (automatic event logging over the system lifetime) and Art. 72 (post-market monitoring plan) are the binding-law drivers for AI-SPM telemetry and inventory; both are outcome obligations with no telemetry-schema specification ([[standards-review-eu-ai-act-2026-Q2|2026-Q2 EU AI Act review]]).
7. **Generate board-level posture summaries** with risk-rated backlogs and trend lines.
8. **Automate ticketing for violations**, verify closure.
9. **Continuously validate posture by red-teaming** and replaying risky prompts in staging.
10. **Document control library** and link each control to a published standard (NIST AI RMF, [[iso-iec-42001|ISO 42001]], OWASP ASI, [[owasp-aivss|AIVSS]], [[csa-maestro|CSA MAESTRO]]).

## Relationship to AI-BOM

An [[ai-bom|AI-BOM]] records components for a release or an observed runtime state. AI-SPM uses component records to check configuration and drift. The two are paired:

- AI-BOM declares: "This system depends on model X v1.2.3, embedding model Y, MCP server Z, dataset D, prompt P version 14."
- AI-SPM asserts: "Model X is reachable, embedding model Y is current, MCP server Z is running with policy P, dataset D's permissions match the AI's retrieval index, prompt P version 14 has no `LLM07:2025` system-prompt-leakage issues, and any of those changing fires an alert."

The approved AI-BOM supplies a reference state; the posture process compares it with observed components and configurations.

## Relationship to DSPM

[[dspm|DSPM]] (Data Security Posture Management) maps where sensitive data lives in the enterprise. AI-SPM links those data labels to AI models, indexes, and connectors. The Knostic article describes DSPM feeding risk signals to AI guardrails; the integration below adds AI-SPM's asset mapping between them:

```
DSPM  ── feeds ──>  AI-SPM  ── feeds ──>  AI guardrails
```

DSPM signals (this repo holds Confidential PII) flow into AI-SPM (the RAG index that pulls from this repo must enforce that label) which flows into runtime guardrails (block answers that surface this label without proper authorization).

## Relationship to Guardian Agents

Gartner's continuous-assurance category for [[guardian-agent|guardian agents]] — AI agent posture management, security testing, risk and control validation, compliance reporting — restates the AI-SPM discipline at the agent-asset level: an agent's own configuration, permissions, and behavior over time. The asset list above sits underneath that layer as the infrastructure-asset inventory a guardian-agent deployment depends on and does not itself supply, so a program running a guardian-agent product for agent-level assurance still needs AI-SPM's model, prompt, tool, and dataset inventory in place under it.

## CMM Mapping

AI-SPM is a [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] **D7 Observability** capability, with crossover to **D8 Supply Chain** (AI-BOM coupling) and **D6 Data, Memory & RAG** (DSPM coupling). Mature implementations integrate with CSPM and SIEM rather than running standalone.

## Tooling Categories (Q2 2026)

Three converging categories, plus a narrower harness-scoped fourth:

1. **Pure-play AI-SPM vendors** (emerging): explicit AI inventory + posture checks
2. **Cloud posture extensions**: CNAPP products adding AI discovery and configuration checks, such as [[wiz-ai-spm|Wiz AI-SPM]] from [[wiz|Wiz]] ([2023 launch](https://www.wiz.io/blog/ai-security-posture-management))
3. **[[microsoft-agent-365|Microsoft Agent 365]]**: agent governance across Defender, Entra, and Purview
4. **Harness-config scanners**: narrow, single-harness AI-SPM operating on the agent-configuration tree itself (hooks, MCP server manifests, subagents, slash commands, skill manifests, `CLAUDE.md`). Open-source: [[agentshield|AgentShield]] (Claude Code config; rules for secrets, permissions, hooks, MCP servers, and agents with provenance-aware `runtimeConfidence` weighting).

Wiz's [[wiz-ai-app|Wiz AI-APP]] announcement describes a CNAPP extension that joins inventory to attack-path analysis and runtime detection. The [[wiz-ai-app-launch|Wiz AI-APP Launch]] summarizes the vendor's claims and their evidence limits.[^wiz-ai-app]

The [[onyx-platform|Onyx Platform (Onyx AI Control Plane)]] [advertises AI-SPM configuration hardening](https://onyx.security/platform) alongside agent and MCP supply-chain risk checks. Its product page also describes an inline MCP gateway; deployment evidence is needed to assess that control's behavior.

[[palo-alto-prisma-airs|Palo Alto Prisma AIRS (AI Runtime Security)]] [displays posture findings from Cortex Cloud AI-SPM](https://docs.paloaltonetworks.com/prisma-airs/ai-inventory/agent-discovery/ai-agent-discovery-with-cortex-cloud) when its tenant is connected to Cortex Cloud with an active AI-SPM license. The [August 2026 feature](https://docs.paloaltonetworks.com/ai-runtime-security/new-features/by-date/prisma-airs/august-2026) is limited to Americas-region Strata Cloud Manager tenants.

The category is not yet stable. Treat product comparisons as tentative.

## Open Issues

- **Reference baselines.** The sources cited here do not define a CIS-equivalent AI infrastructure benchmark. A deployment must document which CSA, OWASP, and NIST guidance informs each local baseline.
- **Embedding-versus-source drift.** Detecting that an embedding still encodes content that the source has removed or tightened is non-trivial; it requires re-embedding-and-comparing or maintaining a content-hash trail.
- **Tool / MCP inventory.** MCP servers can be installed at the user level without enterprise visibility; integrating with [[mcp-security|MCP Security]] discovery is required.

## See Also

- [[ai-data-security|AI Data Security (Knostic blog, 2026)]]: primary source
- [[dspm|Data Security Posture Management (DSPM) for AI]]: data-side posture management
- [[ai-bom|AI-BOM: AI Bill of Materials]]: paired static inventory
- [[guardian-agent|Guardian Agent]]: agent-asset-level counterpart to this infrastructure-asset discipline
- [[agent-observability|Agent Observability]]: runtime telemetry that feeds posture checks
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] §Observability layer
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]: D7 Observability capability

[^wiz-ai-app]: [Wiz — Introducing Wiz AI Application Protection Platform](https://www.wiz.io/blog/introducing-wiz-ai-app), 2026-03-23, platform overview and inventory/risk/runtime sections.
