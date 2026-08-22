---
type: practice
title: "AI Security Posture Management (AI-SPM)"
created: 2026-05-01
updated: 2026-08-17
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
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[ai-data-security]]"
  - "[[ai-bom]]"
  - "[[dspm]]"
  - "[[agent-observability]]"
  - "[[security-controls-for-ai-stacks]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[standards-review-eu-ai-act-2026-Q2]]"
sources:
  - "[[.raw/articles/knostic-ai-data-security-2026-05-01.md]]"
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

The Knostic article and CMM-aligned guidance converge on these AI-SPM operational primitives:

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

[[ai-bom|AI-BOM]] is the static inventory artifact. AI-SPM is the dynamic posture discipline that consumes the AI-BOM and asserts continuous validity. The two are paired:

- AI-BOM declares: "This system depends on model X v1.2.3, embedding model Y, MCP server Z, dataset D, prompt P version 14."
- AI-SPM asserts: "Model X is reachable, embedding model Y is current, MCP server Z is running with policy P, dataset D's permissions match the AI's retrieval index, prompt P version 14 has no `LLM07:2025` system-prompt-leakage issues, and any of those changing fires an alert."

The two are mutually dependent. AI-SPM keeps the AI-BOM current, and the AI-BOM supplies the reference state that AI-SPM measures drift against.

## Relationship to DSPM

[[dspm|DSPM]] (Data Security Posture Management) maps where sensitive data lives in the enterprise. AI-SPM extends posture into AI-specific assets that DSPM tools do not natively cover. The Knostic article's stack:

```
DSPM  ── feeds ──>  AI-SPM  ── feeds ──>  AI guardrails
```

DSPM signals (this repo holds Confidential PII) flow into AI-SPM (the RAG index that pulls from this repo must enforce that label) which flows into runtime guardrails (block answers that surface this label without proper authorization).

## CMM Mapping

AI-SPM is a [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] **D7 Observability** capability, with crossover to **D8 Supply Chain** (AI-BOM coupling) and **D6 Data, Memory & RAG** (DSPM coupling). Mature implementations integrate with CSPM and SIEM rather than running standalone.

## Tooling Categories (Q2 2026)

Three converging categories, plus a narrower harness-scoped fourth:

1. **Pure-play AI-SPM vendors** (emerging): explicit AI inventory + posture checks
2. **CSPM extensions**: CSPM products adding AI asset types (Wiz, Orca, Lacework signaling movement)
3. **Microsoft Agent 365**: vertical integration (Defender + Entra + Purview); the first commercial unified agent governance control plane; see [[microsoft-rai|Microsoft Responsible AI Standard (RAI)]]
4. **Harness-config scanners**: narrow, single-harness AI-SPM operating on the agent-configuration tree itself (hooks, MCP server manifests, subagents, slash commands, skill manifests, `CLAUDE.md`). Open-source: [[agentshield|AgentShield]] (Claude Code config; 102 rules across Secrets / Permissions / Hooks / MCP Servers / Agents with provenance-aware `runtimeConfidence` weighting).

The category is not yet stable. Treat product comparisons as tentative.

## Open Issues

- **Reference baselines.** No CIS-equivalent benchmark exists for AI infrastructure yet. CSA, OWASP, and NIST are all candidates.
- **Embedding-versus-source drift.** Detecting that an embedding still encodes content that the source has removed or tightened is non-trivial; it requires re-embedding-and-comparing or maintaining a content-hash trail.
- **Tool / MCP inventory.** MCP servers can be installed at the user level without enterprise visibility; integrating with [[mcp-security|MCP Security]] discovery is required.

## See Also

- [[ai-data-security|AI Data Security (Knostic blog, 2026)]]: primary source
- [[dspm|Data Security Posture Management (DSPM) for AI]]: data-side posture management
- [[ai-bom|AI-BOM: AI Bill of Materials]]: paired static inventory
- [[agent-observability|Agent Observability]]: runtime telemetry that feeds posture checks
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] §Observability layer
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]: D7 Observability capability
