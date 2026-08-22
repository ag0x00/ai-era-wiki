---
type: entity
entity_type: product
title: "MDASH (Microsoft Agentic Scanning Harness)"
address: c-000029
created: 2026-05-13
updated: 2026-08-21
tags:
  - products
  - microsoft
  - mdash
  - agentic-soc
  - vuln-discovery
  - ai-in-sec-defense
  - ai-vuln-discovery
status: seed
scope_axis:
  - ai-in-sec-defense
vendor: "Microsoft"
ga_date: ""
homepage: "https://aka.ms/AI-drivenScanningHarness"
related:
  - "[[microsoft]]"
  - "[[mdash-defense-at-ai-speed]]"
  - "[[taesoo-kim]]"
  - "[[cybergym]]"
  - "[[microsoft-security-copilot]]"
  - "[[microsoft-zt4ai]]"
  - "[[xbow]]"
sources:
  - "https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/"
  - "https://aka.ms/AI-drivenScanningHarness"
---

# MDASH — Microsoft Multi-Model Agentic Scanning Harness

**Sources:** [Microsoft Security Blog — MDASH announcement (May 12, 2026)](https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/) · [Private Preview Signup (aka.ms)](https://aka.ms/AI-drivenScanningHarness)

MDASH (codename — **m**ulti-mo**d**el **a**gentic **s**canning **h**arness) is Microsoft's agentic vulnerability discovery and remediation system, built by Microsoft's **Autonomous Code Security (ACS)** team in collaboration with **Microsoft Windows Attack Research and Protection (WARP)**. It orchestrates **100+ specialized AI agents** across an ensemble of frontier and distilled models — auditors, debaters, dedup agents, and provers — to find, validate, and prove exploitable vulnerabilities end-to-end. First publicly announced May 12, 2026; in limited private preview as of mid-May 2026; product naming uses "codename MDASH" — the GA product name is not yet disclosed.

## Pipeline Architecture

Five-stage pipeline; targeting / validation / dedup / prove stages are model-agnostic by construction:

| Stage | Role | Output |
|---|---|---|
| **Prepare** | Ingests source target; builds language-aware indices; draws attack-surface and threat models from past commits. | Indices, attack-surface model |
| **Scan** | Specialized auditor agents reason over candidate code paths. | Candidate findings with hypotheses and evidence |
| **Validate** | Debater agents argue for and against each finding's reachability and exploitability. | Surviving findings with strengthened rationale |
| **Dedup** | Collapses semantically equivalent findings (patch-based grouping). | Distinct vulnerability candidates |
| **Prove** | Constructs and executes triggering inputs where the bug class admits (e.g., ASan in C/C++). | Validated PoC artifacts |

## Three Architectural Properties

1. **Ensemble of diverse models.** SOTA as heavy reasoner; **distilled models** as cost-effective debater for high-volume passes; a **second separate SOTA model** as independent counterpoint. Disagreement between models is itself a signal. *Microsoft does not disclose which specific models occupy these roles.*
2. **Specialized agents.** 100+ agents, each with its own role, prompt regime, tools, and stop criteria. Auditors do not reason like debaters; debaters do not reason like provers. Constructed through deep research with past CVEs and their patches.
3. **End-to-end pipeline with extensible plugins.** Domain plugins inject context the foundation models cannot see — kernel calling conventions, IRP rules, lock invariants, IPC trust boundaries, codec state machines, custom CodeQL databases. The **CLFS proving plugin** is a worked example, embedding on-disk container layout + block-validation sequence + in-memory state machine to construct triggering log files for candidate findings.

## Public Benchmark Results

| Benchmark | Result | Provenance |
|---|---|---|
| StorageDrive (21 planted vulns, private Microsoft interview driver) | **21/21 found, 0 false positives** | Microsoft-disclosed |
| `clfs.sys` historical recall (5 years, 28 MSRC cases) | **96%** | Microsoft-internal |
| `tcpip.sys` historical recall (5 years, 7 MSRC cases) | **100%** | Microsoft-internal |
| [[cybergym\|CyberGym]] public leaderboard (1,507 real-world vuln-repro tasks, level 1) | **88.45%** | Top score, ~5 points above #2 (83.1%) |
| May 2026 Patch Tuesday | **16 new CVEs** | 10 kernel-mode, 6 usermode; 4 Critical RCEs |

The CyberGym number is the only independently verifiable data point; the others are Microsoft-internal but anchored to a defensible ground truth (MSRC case database).

## Positioning

MDASH sits at the intersection of three wiki scope axes:

- **`ai-in-sec-defense`** (primary): Microsoft's defender-AI capability at the AppSec / vulnerability-research layer, distinct from [[microsoft-security-copilot|Security Copilot]] at the SOC layer. Second sourced anchor for [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] after [[xbow|XBOW]]/[[mythos|Mythos]]; the convergence with XBOW — two vendors, opposite sides of the stack, arriving at "the harness does the work, the model is one input" — is itself the load-bearing observation.
- **`sec-of-ai`** (tertiary): MDASH is itself an agentic system; the [[agentic-ai-security-cmm-2026|CMM]] questions about agent identity, action authority, and audit apply to MDASH's 100+ agents.

## Convergence with XBOW + Mythos

| | XBOW + Mythos | MDASH |
|---|---|---|
| Orientation | Offensive (live-site pentest) | Defensive (developer-side audit) |
| Model strategy | Single best frontier model, strong harness | Ensemble (SOTA + distilled + second-SOTA counterpoint) |
| Validation | Live-site interaction harness | Debate + dedup + automated prove (PoC construction) |
| Targets | Live web applications | Native Windows components (`tcpip.sys`, `ikeext.dll`, etc.) |
| Key insight (paraphrased) | "A model is a brain without a body" | "The harness does the work; the model is one input" |
| Public benchmark | XBOW's internal web exploit benchmark | CyberGym public leaderboard |
| Pricing | Mythos ~5× Opus at GA | Not disclosed; private preview |

The two systems are architecturally distinct but converge on the same architectural argument — orchestration outperforms model choice, and validation infrastructure determines real-world utility.

## Distribution

- **Status**: limited private preview as of May 2026.
- **Users**: Microsoft security engineering teams (production); a small set of customers (preview).
- **Signup**: [aka.ms/AI-drivenScanningHarness](https://aka.ms/AI-drivenScanningHarness).
- **GA timeline / pricing / SKU positioning**: not yet disclosed.

## Open Questions

- **Which models?** "Generally available AI models" is the only public attribution. Anthropic's [[mythos|Mythos]] is a plausible SOTA-reasoner candidate; OpenAI GPT and Microsoft-internal models are alternatives. Microsoft's silence is conspicuous.
- **GA productization**: standalone product, feature within Defender / Security Copilot, or service offering via consulting? The post does not commit.
- **Customer co-pilot pattern**: the post says customers "test" MDASH — implies hosted-service model rather than on-prem deployment. To be confirmed.
- **CyberGym configuration**: 88.45% is at `level 1` (vulnerable source + high-level description supplied). Performance on higher-difficulty levels (blind discovery) would be a stronger signal.
- **Relationship to MITRE ATLAS** or other vulnerability taxonomies: not addressed.

## See Also

- [[mdash-defense-at-ai-speed|Defense at AI Speed paper]] — primary source.
- [[microsoft|Microsoft]] — vendor.
- [[microsoft-security-copilot|Microsoft Security Copilot]] — adjacent Microsoft defender-AI product (SOC layer, not AppSec layer).
- [[cybergym|CyberGym]] — public benchmark MDASH leads.
- [[taesoo-kim|Taesoo Kim]] — paper author.
- [[xbow|XBOW]] / [[mythos|Mythos]] — offensive-side architectural counterparts.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — the wiki thesis MDASH co-anchors.
