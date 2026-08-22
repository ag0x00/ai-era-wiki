---
type: talk
title: "Scaling Agentic AI: A Leadership Guide for CIOs"
created: 2026-05-01
updated: 2026-05-01
tags:
  - papers
  - talks
  - gartner
  - agentic-ai
  - governance
  - operating-model
  - cio-playbook
status: summarized
year: 2026
authors: ["Brandon Gummer", "Remy Gulzar"]
venue: "Gartner webinar (Bizzabo-streamed), 60 min"
source_url: ""
archived_copy: ".raw/talks/scaling-agentic-ai-cios-2026-05-01.md"
no_public_url: "Gartner on-demand webinar (Bizzabo replay, session-tokened m3u8 — no stable public landing page)"
key_claim: "Agentic AI cannot be scaled by the CIO alone. It requires a co-led 'AI Agent Layered Council' across CFO, COO, General Counsel, Procurement, and CHRO — each with a specific play that drives accountability, liability, and ownership *outward* from IT to the business."
methodology: "Practitioner-oriented synthesis of Gartner's 2026 CIO and Technology Executive Survey, Gartner Digital Vanguard research, Gartner human-parity-line measurement (Dec 2025), 2025 layoff analysis (n=1.4M), 2026 AI-readiness research, and named enterprise case studies (Vizient, Minter Ellison, Luyan Industries)."
contradicts: []
supports:
  - "[[guardian-agent-metagovernance]]"
  - "[[shadow-automation]]"
  - "[[shadow-ai]]"
  - "[[agent-catalog]]"
  - "[[decision-rights]]"
  - "[[least-agency-principle]]"
related:
  - "[[ai-agent-layered-council]]"
  - "[[human-parity-line]]"
  - "[[agent-token-chargeback]]"
  - "[[distributed-kill-switch]]"
  - "[[guardian-agents-market-guide]]"
  - "[[ai-trism]]"
  - "[[oversight-layer]]"
  - "[[brandon-gummer]]"
  - "[[remy-gulzar]]"
  - "[[gartner]]"
sources:
  - "[[.raw/talks/scaling-agentic-ai-cios-2026-05-01.md]]"
---

# Scaling Agentic AI: A Leadership Guide for CIOs

**Source:** Gartner webinar (Bizzabo-streamed, 60 min). No stable public URL — replay is session-tokened. Local transcript: `.raw/talks/scaling-agentic-ai-cios-2026-05-01.md`.

## Key Claim

The CIO cannot scale agentic AI alone — and waiting until they're forced to react means inheriting the de-facto ownership of every bad agentic decision the business makes. Therefore, form an **[[ai-agent-layered-council|AI Agent Layered Council]]** that co-leads agentic AI across the C-suite (CFO, COO, General Counsel, Procurement, CHRO). For each peer, the talk gives a specific play whose effect is to **push accountability for variable cost, business outcomes, liability, and people-impact outward** to the business units actually deploying agents — while IT provides the foundational platform, catalog, and governance.

**The structural argument.** Of the ten "must-do" plays in the talk, eight are about *who owns the consequences*, not *what controls to deploy*. The talk's contribution to the wiki is therefore on the **governance / operating-model layer** — adjacent to but distinct from the [[security-controls-for-ai-stacks|security-controls layer]] this wiki has emphasized. It pairs with [[guardian-agent-metagovernance|Guardian Agent Metagovernance (Guards for the Guardians)]] (controls *for* the oversight layer) by addressing the *organizational* metagovernance question: who owns what, and how do we keep the CIO from becoming the kill-switch holder of last resort.

## Speakers and provenance

- **Brandon Gummer** — Gartner Vice President Analyst (former CIO before joining Gartner). Owns sections on CFO, COO, CHRO, and the closing C-suite synthesis. *Name spelling unverified — transcribed by Whisper as "Gummer" / occasionally "Gummo"; verify before external citation.*
- **Remy Gulzar** — Gartner Vice President Analyst. Owns sections on legal counsel and procurement. *Name spelling unverified — transcribed as "Gulzar" / occasionally "Gosar".*
- **Rishika Kaushik** — moderator (Gartner).

The webinar is one of [[gartner|Gartner]]'s public-facing CIO sessions, streamed via Bizzabo and recorded for on-demand replay at gartner.com/webinars. Transcript is auto-generated (WhisperKit `large-v3`); verify quotes against the source video before external citation.

## Structure of the talk (60 min)

| Section | Topic | Speaker |
|---|---|---|
| 1 (5 min) | Why agentic AI must be **co-led**: option A vs option B | Brandon |
| 2 (35 min) | The **AI Agent Layered Council** — five C-suite plays | Brandon (CFO, COO, CHRO) + Remy (Legal, Procurement) |
| 3 (5 min) | Closing synthesis | Brandon |
| 4 (15 min) | Q&A | both |

## The "option A vs option B" framing

> **Option A:** CIO co-leads agentic AI with C-suite peers. Hard but addresses inevitability.
> **Option B:** CIO doesn't, business units go alone. Easy in the short term, but already happening anyway — and only **37%** of business-unit-led digital delivery succeeds when IT is excluded. The CIO ends up holding the bag without holding the wheel.

Option B is also already in motion: Gartner data shows **AI investment jumped 52% outside the IT budget last year**. The talk's argument: *delaying option A is pretending option B isn't already the default*.

> [!note] Time pressure: the human-parity line
> Brandon cites Gartner research showing AI crossed the **[[human-parity-line|human parity line]]** in December 2025 — the threshold at which human judges prefer AI's output as often as they do industry professionals' across 1,320 tasks in 42 job roles spanning the nine industries that contribute most to US GDP. This is *the* time-pressure argument the talk uses to justify acting now rather than later.

## The five-CXO playbook (the heart of the talk)

### CFO play — variable chargeback infrastructure for token spend

CFOs care about **cost optimization across the enterprise**. The lead-in: 52% AI spend growth outside IT means token costs are spiraling untracked. The play:

1. Establish **central agentic AI services** + a **variable chargeback infrastructure** so token spend is attributable to the consuming business unit and use case.
2. Set **token-budget thresholds per use case** to prevent runaway spend.
3. Use chargeback visibility to identify **multi-agent system harmonization** opportunities.

> [!example] Multi-agent harmonization (Brandon's example)
> Marketing deploys agentic AI for trade-promotion / digital-marketing / experience-management automation. Supply chain deploys agentic AI for demand-forecasting / inventory management. **Without harmonization, marketing creates demand shockwaves the supply chain cannot absorb → stockouts, dissatisfied customers.** Visibility from the chargeback layer surfaces the coupling. This is the operating-model analogue of the security-architecture concern about cross-agent emergent behavior.

This play is the conceptual seed of [[agent-token-chargeback|Agent Token Chargeback]].

### COO play — business-outcome-driven metrics, at the right altitude

COOs care about **business outcomes that matter, not technology operational metrics**. Three litmus tests Brandon offers:

1. **Don't use technology operational metrics** (uptime, quotes-generated). Too low.
2. **Don't fall for "time saved = money saved."** Almost never holds the way teams expect.
3. **Don't use top-line outcome metrics** (loss ratio, enterprise profitability). Too high — they cross many BUs and don't drive action.

The right altitude is **technology-outcome-driven metrics**: in the insurance example, **quote-to-bind ratio** sits between "uptime" (too low) and "loss ratio" (too high) and is the right outcome to baseline + report quarterly.

### Legal Counsel play — shift from documented liability to real-time, contextualized, auditable liability

Remy's framing: pre-AI legal liability practice was **documented, distributable, tested**. AI agents make decisions in real time, so liability must become **real-time, contextualized, auditable, transparent**.

Sub-plays:

- **Adopt an accountability model (RACI / RAPID)** distributed close to the people making decisions — *not* steered from a central faraway corporate function.
- **Stress-test contractual indemnity / warranties regularly.** Don't file them and forget.
- **Combine human + machine oversight** because humans cannot react at agent speed.
- **Multi-use audit trails** — for reporting, evidence, and *training data for both humans and machines*.
- **"Codifying care."** Build oversight that prevents the CIO from being the lone kill-switch holder; if you're standing alone with the kill switch, governance has already broken down.

> [!contradiction] Standards lag the technology
> Remy explicitly warns that **NIST AI RMF and ISO 42001 (he says "42K") were created in the past and don't tell you what's happening now**. The wiki already takes this position in [[ai-security-standards-in-q1-2026|AI Security Standards in Q1 2026]] — independently corroborated here from the Gartner side.

### Procurement play — agentic AI catalog + comply-or-explain + duty of care upstream

Remy's frame: **agents are services, not technology purchases.** Treat procurement of an agent like a shared service agreement — the agent does work for you.

Sub-plays:

- **Centralized [[agent-catalog|agentic AI catalog]]** so procurement can vet new requests against the existing stack and prevent duplication.
- **Insert IT requirements at "zero day"** of any new procurement request — upstream of RFP/RFI, so vendors that can't meet safe/responsible-use requirements never enter the funnel.
- **Comply-or-explain instead of comply-or-die** — recognize that **67% of employees use personally obtained AI** (ChatGPT, Gemini); a punitive "clamp it down" posture loses, a collaborative "what do you actually need" posture wins.
- **Shift duty of care upstream to vendors** via IP indemnity and safe-use warranties. Brandon's interjection: *"the largest vendors in the world now offer those terms as negotiated terms — but they won't bring them up; you have to ask."*
- **Mitigate agent exposure early in the process** — stress-test indemnity provisions on a regular cadence as conditions change.

This play is the seed for sharpening the [[agent-catalog|AI Agent Catalog]] page from a security-only inventory primitive into a **procurement-coordination primitive** as well.

### CHRO play — formalize the alliance, redesign jobs, distribute kill switches

Remy's data-point first: **less than 1% of the 1.4M layoffs in 2025 were directly attributable to AI** (Gartner analysis). But **20–21x more people** will face complete job *redesign*. The CHRO needs help most, not least.

Brandon's plays:

- **Co-update job profiles** + run **job-impact assessments** + **promotion litmus tests** with the CHRO.
- **Rebalance external staffing with AI** — internal employees with AI capabilities reduce dependence on professional services.
- **Rebadge service desk → digital coaches.** When BUs hit the agent catalog, a human is there to help them onboard and extract value.
- **Behavioral outcome metrics, not just adoption**: prevent over-dependence, reduce drudgery, monitor employee engagement.
- **Three communities of practice** in complement to platforms: agentic AI, data & analytics AI, tech best practices (CI/CD, DevSecOps, Agile, QA, system integration, architecture). Gartner research: **4.1× more reuse, 3.8× more coherent architecture** when all three exist.
- **Empathy maps + journey maps** for AI-driven job redesign.
- **Ring-fence time for learning** — protect 1 hour/week × 12 weeks for AI-literacy COPs.
- **Distribute the kill switch** to every team member ("[[distributed-kill-switch|one-vote veto]]"), not just the CIO.

> [!example] CHRO case studies cited
> - **Vizient** — empathy maps for Gen-AI-era job redesign; engaged employees in co-creating the future-state journey, leveraging the **Cunningham effect** (post slightly the wrong answer → engagement spikes; named for Wikipedia co-founder John Cunningham, though Brandon attributes it).
> - **Minter Ellison** (Australian law firm) — ring-fenced 1h/week × 12 weeks for AI-literacy COPs; outcomes good enough that the firm uses it as a talent-attraction story on its website.
> - **Luyan Industries** (China, automotive electronics) — gave **every team member a "one-vote veto"**: any employee can stop a project or agentic workflow in its tracks. Source for [[distributed-kill-switch|Distributed Kill Switch]].

## Closing synthesis

> "Don't become the de facto owner of all bad AI decisions."

Across all five plays, the common move is to **push accountability and ownership** outward to the business units (variable costs, business outcomes, liability, talent impact) while IT keeps the **foundational platform, catalog, governance, and digital-coach layer** centralized. The CIO does not abandon authority — they distribute *consequences* to where decisions are actually made.

## Q&A — additional points

- **Empathy maps deconstructed**: persona (the human picture, not just process) + current-state journey map → heat-map agentic-AI-eligible automation/decision points → future-state journey map. *Agents make decisions; if your "agentic AI" doesn't make decisions, it's hyper-automation, not agents.*
- **Department fragmentation handling**: governance + standards + use-case-level technology approval → **promote successful use cases into design patterns** (technical design, decision journey, governance) so they can be repeated without reinventing the wheel.
- **CIO ↔ CTO collaboration**: Gartner data — *2026 CIO and Tech Executive Survey* — shows agentic AI being pursued by IT (23%, of which 15% apps/software, 8% I&O), so CIO/CTO contention is real where the CTO owns apps. Resolve by **democratizing access to full-stack solutions** to BUs *while* the CTO retains platform ownership. Underlying evidence: the **top 20% most effective enterprises are 3.2× more likely to use a product-centric operating model**.
- **CEO accountability + EU digital sovereignty**: Brandon notes EU legislation has elevated digital-sovereignty liability to the **personal legal liability** of CEO, CIO, and board members — sharpening the case for the council and the procurement / legal plays.

## Strengths and weaknesses

**Strengths:**

- Operating-model rigor — every play has a stakeholder, a metric, a litmus test, and (often) a named case study.
- Strong data anchors — 52%, 37%, 67%, 1%, 21×, 4.1×, 3.8×, 3.2×, 70%-of-agents-fail-past-10-steps.
- The "comply-or-explain" framing is a useful sharpening of the wiki's existing [[shadow-ai|Shadow AI]] / [[shadow-automation|Shadow Automation]] narratives.

**Weaknesses:**

- **No specific security controls** — this is a CIO-leadership talk, not an architecture talk. Pair with [[guardian-agents-market-guide|Market Guide for Guardian Agents]] for the controls layer.
- **Light on threat models** — prompt injection, tool-abuse, lethal-trifecta vulnerabilities are absent.
- **Gartner-framed examples only** — Vizient, Minter Ellison, Luyan are cited but without external corroboration of outcomes.
- **"AI agent layered council" is a Gartner-coined concept** — useful but not yet observed in non-Gartner literature; treat as marketing-coined organizing principle, not industry consensus (yet).

## Relations to existing wiki pages

| Wiki page | Connection |
|---|---|
| [[guardian-agents-market-guide\|Gartner Market Guide for Guardian Agents (Feb 2026)]] | Same publisher; this talk is the *organizational* counterpart to the controls-side Market Guide |
| [[guardian-agent-metagovernance\|Guardian Agent Metagovernance (Guards for the Guardians)]] | This talk fills the *organizational metagovernance* layer the Market Guide left implicit |
| [[agent-catalog\|AI Agent Catalog]] | Sharpens the catalog from security inventory primitive → procurement coordination primitive |
| [[shadow-ai\|Shadow AI]] / [[shadow-automation\|Shadow Automation]] | Adds the 67%-of-employees-use-personal-AI stat + comply-or-explain framing |
| [[decision-rights\|Decision Rights for AI Agents]] | The council *is* the cross-functional decision-rights body for agentic AI portfolio decisions |
| [[least-agency-principle\|Least Agency Principle]] | The "distributed kill switch" / one-vote-veto pattern operationalizes block-tier decisions across non-IT staff |
| [[ai-trism\|Gartner AI TRiSM]] | Talk fits squarely in TRiSM's "operationalize" guidance (the part the standards have least to say about) |
| [[ai-security-standards-in-q1-2026\|AI Security Standards in Q1 2026: Agentic Threats Outpace Frameworks]] | Independent corroboration of "standards lag" thesis |

## See Also

- [[ai-agent-layered-council|AI Agent Layered Council]] — central concept introduced by this talk
- [[human-parity-line|Human Parity Line]] — Gartner's measurement; Dec-2025 crossing date
- [[agent-token-chargeback|Agent Token Chargeback]] — practice page for the CFO play
- [[distributed-kill-switch|Distributed Kill Switch]] — practice page for the one-vote-veto pattern
- [[brandon-gummer|Brandon Gummer]] · [[remy-gulzar|Remy Gulzar]] — speakers
- [[gartner|Gartner]] — publisher
