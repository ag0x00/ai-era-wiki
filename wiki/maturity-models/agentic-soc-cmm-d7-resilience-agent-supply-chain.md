---
type: maturity-model
title: "Agentic SOC CMM D7 Resilience and Agent Supply Chain"
address: c-000193
created: 2026-06-03
updated: 2026-06-23
tags:
  - maturity-models
  - cmm
  - agentic-soc
  - supply-chain
  - resilience
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
related:
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[mythos-ready-security-program]]"
  - "[[harness-config-as-supply-chain-artifact]]"
  - "[[supply-chain-security-for-agents]]"
  - "[[ai-bom]]"
  - "[[mcp-security]]"
  - "[[non-human-identity]]"
  - "[[cyber-poverty-line]]"
sources:
  - "[[agentic-soc-cmm]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[mythos-ready-security-program]]"
  - "[[harness-config-as-supply-chain-artifact]]"
---

# Agentic SOC CMM D7 Resilience and Agent Supply Chain

Companion deep-dive to [[agentic-soc-cmm|the Agentic SOC CMM]]'s D7 domain. D7 measures two coupled capabilities: whether the SOC governs its own defender agents as a software supply chain, and whether the agent fleet keeps the SOC operational under failure. The supply-chain half covers provenance and integrity of the model, tools, MCP servers, prompts, retrieval pipelines, and escalation logic that compose a defender agent; the resilience half covers fail-safe behavior, degradation modes, and what happens when an agent or its model is compromised or unavailable. The domain scores the [[agentic-soc-reference-architecture|reference architecture]]'s orchestration, identity/action-authority, and policy/enforcement planes from the angle of *what the agents themselves are made of and how they fail*.

D7 is an **autonomy gate**. With D8, it sits at the top of [[agentic-soc-cmm|the CMM]]'s gating ladder: a function reaches L4 (delegated) only when the agent stack running it is secured and resilient at scale. The reasoning is specific to a SOC. A defender agent is privileged — it can contain a host, block an identity, quarantine a mailbox, or isolate a workload — so a compromised defender agent is an attacker that already holds response authority. D7 (the integrity and resilience of that agent) and D4 (the identity and action-authority that bounds it) are jointly the containment for that case. D7 keeps the agent from being subverted or silently degraded; D4 limits the damage if it is.

> [!gap] Shared layer — cross-referenced, not duplicated
> D7 overlaps the application-security CMM's supply-chain domain, [[agentic-ai-security-cmm-2026|D8 Supply Chain & AI-BOM]]. The mechanisms (AI-BOM, signed releases, registry and pre-install scanning, runtime reconciliation, the model-consumer vs. model-producer split) are defined there and not restated here. This page carries only the SOC-specific delta: defender agents hold response authority, so their supply-chain compromise is an active-attacker scenario rather than a data-integrity one, and the resilience half (fail-safe behavior of a privileged fleet) has no counterpart in the application-security domain.

## Control landscape (dated)

The supply-chain controls are the same primitives the application-security stack uses, applied to agents that can take destructive defensive actions. The resilience controls are largely operational design (degradation modes, kill switches, continuity) with little dedicated product. Marked GA vs. preview/experimental honestly; dated and swappable.

| Layer | What ships today | Status (mid-2026) |
|---|---|---|
| Artifact provenance & signing | Sigstore / cosign for model and skill artifacts; SLSA provenance (Build L1–L3; no L4 in v1.0); CycloneDX ML-BOM and SPDX 3.0 AI extensions for the [[ai-bom\|AI-BOM]] | Signing and ML-BOM tooling GA; reproducible builds for stochastic model weights remain unsolved |
| MCP-server provenance | [[mcp-security\|MCP]] registry namespace provenance; OAuth/auth surface per the MCP spec; pre-install and registry scanning of servers and skills | Registry gives namespace provenance only; cryptographic name→binary signing for MCP servers does not exist yet |
| Agent-harness integrity | Cognitive-file integrity (SHA-256 baselines) over prompts, tool definitions, and identity files; SAST-style scanning of the [[harness-config-as-supply-chain-artifact\|harness configuration tree]] (hooks, tool manifests, retrieval config) | Hashing is trivial; harness-config SAST is early and not a converged product category |
| Runtime reconciliation | Runtime [[ai-bom\|AI-BOM]] / AI-SPM discovery reconciling running components against the build-time manifest ([[ai-spm\|AI-SPM]] platforms) | GA on major clouds; drift policy is the operator's to define |
| Fleet resilience | Per-agent kill switch and orphaned-agent reaper; model-provider failover / multi-model routing in the orchestrator; deterministic fallback playbooks | Kill switch and failover are configuration patterns, not packaged products; fail-safe defaults are a design choice |

The platform-native column matters for incumbents. A single-cloud SOC reaches the verification-and-reconciliation tier (signature checks, runtime AI-BOM, AI-SPM discovery) for *acquired* agent artifacts inside existing AI-SPM entitlements, because the SOC is a model *consumer*, not a model producer.

## Capability levels

Cumulative: Level N assumes every Level N−1 criterion. Each level states a capability specific to securing and sustaining the SOC's own agent fleet.

- **L1 — Initial.** Defender agents run with no provenance and no failure plan. Models, tools, and MCP servers are pulled ad hoc; prompts and tool definitions are edited in place with no integrity baseline; there is no defined behavior for a model outage or a compromised agent.
- **L2 — Developing.** A manual inventory lists the models, skills, MCP servers, and harness components each agent uses. Artifact and library versions are tracked; vendor-attested model cards are collected; a basic kill switch can disable an agent. A degradation expectation is written down: which functions fall back to human operation if the agent fleet is unavailable.
- **L3 — Defined.** Supply-chain controls are standardized across the fleet: a build-time AI-BOM exists per defender agent; model and skill releases are signed and signatures are verified at install; MCP servers and skills pass registry and pre-install scanning; cognitive-file integrity baselines cover prompts, tool definitions, and escalation logic. Fail-safe behavior is defined per function: a privileged agent fails *closed* on its containment actions (no action without verification) rather than failing toward unbounded action. The orphaned-agent reaper runs on a measurable SLA. This is the supply-chain and resilience floor that, with D8, supports **function autonomy at L3 (autonomous in-bounds)** — an agent may act within bounds only once its composition is verified and its failure mode is bounded.
- **L4 — Managed.** Quantitative supply-chain and resilience posture is tracked continuously. Every model and skill artifact is signed (Sigstore/cosign or equivalent); a runtime AI-BOM reconciles against the build-time manifest under a defined drift policy; harness configuration is scanned in CI and drift from baseline is alerted. Resilience is tested, not assumed: model-provider failover and deterministic fallback playbooks are exercised on a cadence, and a compromised-defender-agent scenario is run as a tabletop or live drill — covering revocation of the agent's identity (the D4 seam), containment of its in-flight actions, and reconstruction from a known-good baseline. SLSA L3 provenance (signed, hermetic, isolated builds) is achievable for the agent artifacts the SOC builds. This is the level that, with D8 at the same maturity, gates **function autonomy at L4 (delegated)**: full delegation requires the agent stack to be secured and resilient at scale.
- **L5 — Optimizing.** A closed loop runs in production: every supply-chain finding (an unsigned artifact, a failed reconciliation, a flagged MCP server) and every resilience event (a failover, a degraded agent) produces a controls update within a published SLA. The MCP/dependency feed is wired to auto-quarantine a known-bad server or skill without waiting on a human. Compromised-agent recovery is rehearsed quarterly with a measured recovery objective. The fleet degrades gracefully under model loss — functions step down their autonomy automatically rather than going dark or running unverified.
- **L5+ — Leading Edge.** All of L5, plus capabilities ahead of the shipping baseline: cryptographic name→binary signing for the SOC's MCP servers and skills; cross-vendor AI-BOM federation reconciling agents across more than one platform; and an active named contribution to a supply-chain standard for agents (CycloneDX ML-BOM, SPDX AI extensions, an MCP-signing proposal) — spec or PR authorship, not membership.

## Right-sizing by org profile

| Band | Realistic D7 target | Why |
|---|---|---|
| Solo / small | L2, borrowed to L3 | Near or below the [[cyber-poverty-line\|cyber poverty line]]. The agent supply chain is the provider's: an MSSP/MDR or platform vendor signs, scans, and reconciles the artifacts, and the resilience plan is "the function reverts to the provider or to manual." The team owns a written inventory and a kill switch, not a build pipeline |
| Mid | L3, selective L4 | In-house SOC with constrained engineering. AI-BOM and signature verification on the agents the team delegates to (triage, detection); harness-config baselines on the same. Resilience drills on the highest-blast-radius function (response) before others. L4 only where a function is actually delegated |
| Enterprise | L4, selective L5 | A full fleet of privileged defender agents justifies continuous reconciliation, CI-gated harness scanning, rehearsed compromised-agent recovery, and an auto-quarantine loop. L5 is earned where blast radius is highest — the response and containment agents |

A small team at L2 with a tightly scoped kill switch and a provider-owned supply chain is right-sized, not immature. D7 cost rises with the number of agents the SOC *builds and delegates to*, not with the number it runs under supervision.

## Cost model

The dominant cost is engineering and drill labor, not licensing. Signing, ML-BOM, and AI-SPM tooling is largely GA or inside existing entitlements; the spend is the pipeline work to apply it to agents and the recurring labor of rehearsing failure.

| Level | Tooling / licensing | Operational labor | Run-rate note |
|---|---|---|---|
| L2 | ~0 (inventory in a spreadsheet; kill switch in the orchestrator) | ~0.1–0.25 FTE to inventory the fleet and document a degradation plan | — |
| L3 | ~0 for consumers (signing/verification, registry scanning mostly in existing tooling) | ~0.25–0.5 FTE: AI-BOM generation, signature verification, cognitive-file baselines, reaper upkeep | — |
| L4 | AI-SPM / runtime-AI-BOM platform where used | the dominant cost: CI integration for harness scanning, runtime reconciliation upkeep, and recurring failover + compromised-agent drills | Drills are recurring, not one-time |
| L5 | feed integration for auto-quarantine | closed-loop controls-update labor + quarterly recovery rehearsal with a measured objective | Price the rehearsal cadence, not the first setup |

**D7 spend is the labor to treat agents as a build pipeline and to rehearse their failure, not a tool purchase.** A SOC that buys an AI-SPM platform and never drills its compromised-agent recovery has bought visibility, not resilience.

## Open questions

- The gating threshold (which D7 maturity supports which function-autonomy level) inherits [[agentic-soc-cmm|the CMM]]'s MDPI ↔ [[soc-cmm|SOC-CMM]] correspondence and is not yet empirically calibrated.
- No cryptographic name→binary signing exists for MCP servers today; the L5+ line names a capability the ecosystem has not shipped, so it should be re-checked as the MCP registry matures.
- Reproducible builds for stochastic model weights are unsolved, so SLSA-style provenance for the model artifact itself stops at signed-attestation provenance, not bit-for-bit reproducibility.
- The resilience half (graceful degradation of a privileged fleet, automatic autonomy step-down under model loss) has little dedicated product; the level criteria describe design patterns the SOC assembles, and the maturity bar will firm up as products in this space appear.
- The boundary between D7 (agent integrity and resilience) and D4 (identity and action-authority) on the compromised-defender-agent scenario is a seam, not a clean cut; the recovery drill exercises both, and the two domains are scored together for that case.

## Relations

- Companion deep-dive to [[agentic-soc-cmm#axis-2--the-eight-maturity-domains|the Agentic SOC CMM]]'s D7 domain and its [[agentic-soc-cmm#the-gating-rule|gating rule]] — with D8, one of the two L4 (delegated) autonomy gates.
- Scores the orchestration, identity/action-authority, and policy/enforcement planes of [[agentic-soc-reference-architecture|the Agentic SOC Reference Architecture]] from the agent-composition and fail-safe angle.
- Shared layer with [[agentic-ai-security-cmm-2026|the Agentic AI Security CMM]]'s D8 Supply Chain & AI-BOM — the mechanisms live there; this page carries the privileged-defender-agent delta.
- Program-level companion: [[mythos-ready-security-program|the Mythos-ready Security Program]]'s PA 3, "Defend Your Agents," which audits the agent harness — prompts, tool definitions, retrieval pipelines, escalation logic — with the same rigor as the agent's permissions.
- Sibling autonomy gate D4 (Agent Identity & Action-Authority) is the joint containment for a compromised defender agent.
- Domain concepts: [[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]], the [[ai-bom|AI-BOM]], [[mcp-security|MCP Security]], [[non-human-identity|non-human identity]], and the [[harness-config-as-supply-chain-artifact|harness configuration tree as a supply-chain artifact]].
