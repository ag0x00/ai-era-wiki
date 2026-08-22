---
type: concept
title: "AI Agent Layered Council"
created: 2026-05-02
updated: 2026-08-21
tags:
  - concepts
  - governance
  - operating-model
  - agentic-ai
  - cio-playbook
  - gartner
status: developing
complexity: intermediate
domain: governance
aliases:
  - "AI Agent Council"
  - "Agentic AI Council"
  - "Agentic AI Layered Council"
related:
  - "[[scaling-agentic-ai-cios-talk]]"
  - "[[gartner]]"
  - "[[guardian-agent-metagovernance]]"
  - "[[agent-catalog]]"
  - "[[decision-rights]]"
  - "[[shadow-automation]]"
  - "[[oversight-layer]]"
  - "[[nist-ai-rmf]]"
sources:
  - "[[.raw/talks/scaling-agentic-ai-cios-2026-05-01.md]]"
---

# AI Agent Layered Council

The **AI Agent Layered Council** is a [[gartner|Gartner]]-coined organizing primitive for **co-led** scaling of agentic AI across the C-suite. The CIO chairs but does not own. Each peer (CFO, COO, General Counsel, Head of Procurement, CHRO) carries a specific play whose effect is to **push accountability for variable cost, business outcomes, liability, and people-impact outward** to the business units actually deploying agents — while IT keeps the **foundational platform, catalog, governance, and digital-coach layer** centralized.

Introduced in [[scaling-agentic-ai-cios-talk|Gartner's Scaling Agentic AI talk (May 2026)]] by Brandon Gummer and Remy Gulzar.

## The structural argument

The talk frames it as a binary:

- **Option A** — CIO co-leads with C-suite peers via the council.
- **Option B** — CIO does not. Business units deploy agents anyway. CIO inherits de-facto ownership of every bad agentic decision the business makes.

Three Gartner data points anchor the argument:

| Stat | Implication |
|---|---|
| AI investment grew **52% outside IT budget** last year | Option B is already the default; council formation is catch-up, not pre-emption |
| Only **37%** of business-unit-led digital delivery succeeds when IT is excluded | Option B has a measurable failure rate |
| **<1%** of 1.4M 2025 layoffs attributable to AI; **20–21×** more facing job *redesign* | The CHRO needs help most, but the panic narrative is wrong |

## The five plays

Each row is the *peer*, the *thing they care about most*, and the *play* the CIO uses to recruit them.

| Peer | They care about | The play |
|---|---|---|
| **CFO** | Cost optimization | Variable [[agent-token-chargeback\|chargeback infrastructure]] for token spend; per-use-case budgets; cross-BU multi-agent harmonization visibility |
| **COO** | Business outcomes that matter | Outcome-driven metrics at the **right altitude** — not technology-ops (uptime), not top-line (loss ratio), but use-case outcomes (quote-to-bind ratio) |
| **General Counsel** | License to operate | Real-time, contextualized, auditable, transparent liability; distributed accountability model (RACI / RAPID); regular indemnity stress-tests; "[[distributed-kill-switch\|codifying care]]" |
| **Head of Procurement** | Vendor risk + duplication | Centralized [[agent-catalog\|agentic AI catalog]]; insertion at "zero day" upstream of RFP/RFI; **comply-or-explain** instead of **comply-or-die**; duty of care shifted upstream to vendors |
| **CHRO** | Workforce | Co-update job profiles; rebadge service desk → digital coaches; behavioral outcome metrics; communities of practice; ring-fenced learning time; [[distributed-kill-switch\|distributed kill switch]] |

## Basis for the term "layered"

The "layered" qualifier in the council's name maps to the **agentic AI layer** — Gartner's term for the cross-cutting layer of agent platforms, catalogs, and governance that sits across all functional silos. The council governs *that layer*. Each peer's play touches the same layer from a different angle:

- CFO play → cost layer
- COO play → outcome-measurement layer
- Legal play → liability + audit layer
- Procurement play → vendor + catalog layer
- CHRO play → people + change-management layer

## Relation to other wiki concepts

| Wiki page | Relationship |
|---|---|
| [[guardian-agent-metagovernance\|Guardian Agent Metagovernance]] | The metagovernance page covers *technical* controls *for* the oversight layer. The council is the *organizational* metagovernance — who owns what. They pair. |
| [[oversight-layer\|Oversight Layer (PDP + PEP for Agentic AI)]] | The council is the human accountability structure that operates the oversight layer. Without the council, the PDP/PEP architecture has no organizational owner outside IT. |
| [[decision-rights\|Decision Rights for AI Agents]] | The council *is* the cross-functional decision-rights body for portfolio-level agentic decisions (which agents to fund, where to enforce, what use cases to promote to design patterns) |
| [[shadow-automation\|Shadow Automation]] / [[shadow-ai\|Shadow AI]] | The council exists in part because shadow deployments are inevitable; the catalog + comply-or-explain plays are its containment mechanism |
| [[agent-catalog\|AI Agent Catalog]] | The catalog is the council's shared-state primitive — the artifact every peer's play references |

## Placement in the CMM

The council is the organizational form of the cross-functional accountability the [[nist-ai-rmf|NIST AI RMF]] GOVERN function calls for: GOVERN requires roles, responsibilities, and a culture for managing AI risk to be established at the organizational level, and a co-led body with a named play per C-suite seat is one way to populate those roles instead of leaving them with IT alone.

The Council framing is a **D1 (Governance & Accountability) + D2 (Identity & Authorization)** primitive in the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]:

- **L2 evidence**: a chartered cross-functional governance body for agentic AI exists, with documented members from at least three of (CFO, COO, GC, Procurement, CHRO).
- **L3 evidence**: each council seat has a *documented play* (variable chargeback, outcome metrics at right altitude, liability stress-tests, agent catalog, job-redesign program) with a named owner and quarterly review cadence.
- **L4 evidence**: council decisions feed runtime enforcement (e.g., catalog admissions feed PDP policy; behavioral outcome metrics feed agent-tier reassessment).
- **L5 evidence**: council operates across hyperscaler / vendor boundaries with a unified decision-rights matrix.

## Limits and caveats

> [!gap] Marketing-coined organizing principle
> The "AI Agent Layered Council" is a Gartner-coined concept introduced in May 2026. It has not (yet) been observed in non-Gartner literature, and there is no body of academic or industry-survey evidence on how organizations actually staff such councils, how they make decisions, or how often they fail. Treat as a useful organizing principle, not industry consensus.

> [!contradiction] Council ≠ steering committee
> Many enterprises already have an "AI steering committee" or similar. Per Gartner's framing, those existing bodies are *insufficient* — they are typically governance/oversight bodies that vote on requests, not co-led operating bodies with named plays per peer. Don't claim council maturity by renaming your steering committee.

## See Also

- [[scaling-agentic-ai-cios-talk|Scaling Agentic AI: A Leadership Guide for CIOs]] — primary source
- [[guardian-agent-metagovernance|Guardian Agent Metagovernance]] — technical metagovernance counterpart
- [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] — the architectural layer the council operates
- [[agent-catalog|AI Agent Catalog]] — the council's shared-state primitive
- [[agent-token-chargeback|Agent Token Chargeback]] — the CFO play
- [[distributed-kill-switch|Distributed Kill Switch]] — the CHRO + Legal play
- [[brandon-gummer|Brandon Gummer]] · [[remy-gulzar|Remy Gulzar]] — speakers who introduced the concept

<!-- sources:auto -->
## Sources

- [Scaling Agentic AI: A Leadership Guide for CIOs](https://stream.stream-ext.bizzabo.com/U00liQQ00l2Th5l5Bkc302pY02k01IzHU8P3OqHaCDwYzvxw.m3u8)
<!-- /sources -->
