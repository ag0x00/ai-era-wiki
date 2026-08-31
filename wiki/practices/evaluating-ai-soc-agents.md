---
type: practice
title: "Evaluating AI SOC Agents: Gartner's Seven Questions"
address: c-000144
created: 2026-05-25
updated: 2026-08-31
tags:
  - practices
  - agentic-soc
  - soc-automation
  - procurement
  - ai-in-sec-defense
status: developing
maturity: emerging
addresses_threat: "Misaligned AI-SOC-agent procurement: adopting agents that demo well but do not improve TDIR outcomes, carry hidden cost or autonomy risk, or cannot be audited"
scope_axis: [ai-in-sec-defense]
related:
  - "[[end-to-end-harness-evaluation]]"
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[gartner|Gartner]]"
  - "[[mate-cd-cr-continuous-detection-response|Mate CD/CR]]"
  - "[[vulnops-l1-soc-extinction|From Threat Intel to VulnOps]]"
  - "[[plan-validate-execute|Plan-Validate-Execute]]"
  - "[[guardian-agents-market-guide|Gartner Guardian Agents Market Guide]]"
  - "[[measuring-agent-effectiveness-talk]]"
  - "[[ai-security-larsen-effect-talk]]"
sources:
  - https://www.bleepingcomputer.com/news/security/how-to-evaluate-ai-soc-agents-7-questions-gartner-says-you-should-be-asking/
---

# Evaluating AI SOC Agents — Gartner's Seven Questions

A buyer-side evaluation framework for AI SOC agents (Gartner's term for the agentic-SOC category), from the Gartner report *Validate the Promises of AI SOC Agents With These Key Questions* by analysts Craig Lawson and Andrew Davies. The framework's premise is an adoption-versus-outcomes gap: Gartner projects that 70% of large SOCs will pilot AI agents for Tier 1 and Tier 2 operations by 2028, but only 15% will achieve measurable improvement without structured evaluation.[^gartner] The questions below are the structure Gartner proposes to close that gap.

> [!note] Source provenance
> The accessible copy of this framework is a Prophet Security-sponsored republication on BleepingComputer; the framework and figures are Gartner's. The vendor-specific examples in the original have been omitted — the criteria are vendor-neutral.

## The Seven Questions

1. **Does it reduce the work your team does today?** Start from operational bottlenecks, not the vendor feature list. Ask which SOC functions are repetitive, low-value time sinks, which tasks are best suited to augmentation, and whether the agent is purpose-built for the specific SOC roles in question (alert triage and investigation differ from workflow-rule authoring).
2. **How do you measure outcomes beyond "alerts processed"?** Volume is misleading. Center evaluation on TDIR metrics (mean time to detect, mean time to respond, false-positive reduction), and treat **mean time to contain as the end goal**, because containment is where risk is reduced. Add qualitative outcomes such as analyst satisfaction and execution quality, not only speed. Ask for benchmarks from similar environments, and whether they came from a proof of concept or sustained production use.
3. **Will the vendor still exist in two years?** The category is early-stage and crowded. Ask the general-availability date, current customer base, and funding outlook. Treat likely acquisitions as a third-party vendor-management risk rather than a disqualifier. Scrutinize pricing models (alert volume, data volume, or token usage) and how cost behaves under load, since LLM-backed processing of high alert volumes can scale unpredictably.
4. **Does it make analysts better, or just busier differently?** Evaluate augmentation and upskilling, not only triage speed. Ask what training accompanies the tool, whether it creates learning opportunities (suggesting threat hunts, recommending practices), and whether it assists detection engineering. The tension to resolve: if the agent does all investigative legwork, junior analysts may never develop senior-analyst skills. Implementations that present transparent reasoning teach while they triage.
5. **What are the boundaries of AI autonomy?** Gartner distinguishes **human-in-the-loop** (approval for each action) from **human-on-the-loop** (broader latitude with strategic oversight); neither is inherently correct, and the choice depends on risk appetite, regulation, and system maturity. Ask which actions are autonomous versus approval-gated, how guardrails apply to high-impact actions (account disablement, network isolation), and whether autonomy is customizable by task or risk level. A sound fail-safe defaults to **escalation, not action**, under ambiguity. This is the procurement-side instance of the [[plan-validate-execute|Plan-Validate-Execute]] pattern.
6. **Will it work with your existing stack?** Evaluate native integration depth across SIEM, EDR, SOAR, and identity platforms rather than a logo wall. Ask whether the solution requires data centralization or can query across multiple security data sources in place, which is a material difference for hybrid or complex architectures.
7. **Can you see what it is doing?** Explainability may be the most important criterion. Ask how the agent explains decisions and actions, whether it provides human-readable audit trails for every automated action, and how it handles sensitive data and prevents model misuse or leakage. A "glass box" that documents each query, the data retrieved, and the reasoning lets analysts trace a conclusion to evidence instead of trusting a confidence score; without it, analysts either accept verdicts on faith or redo the work. Look for human feedback that actually influences future behavior.

## Cautions

Gartner's cautions section warns against overreliance on marketing claims, states that full autonomy is not viable today, and flags hidden costs in pricing models and integration complexity. The framework deliberately declines to name category winners.

## Placement

This is the buyer-side complement to the [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] thesis: where that page maps the vendor patterns (Microsoft copilot-plus-agents, CrowdStrike AIDR, and the converged-loop framings of [[mate-cd-cr-continuous-detection-response|Mate CD/CR]] and [[vulnops-l1-soc-extinction|Mallory]]), this framework supplies the criteria to separate operational improvement from marketing. Questions 5 and 7 (autonomy boundaries, explainability) restate the action-authority and continuous-evaluation capabilities that distinguish a real agentic SOC. The framework is evaluation guidance, not an independent benchmark — it tells a buyer what to ask, not how any product scores.

## See Also

- [[end-to-end-harness-evaluation|End-to-End Harness Evaluation]] — the same buyer's question applied to an autonomous discover-and-patch harness rather than a SOC agent.
- [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] — the synthesis page this evaluation guidance serves
- [[gartner|Gartner]] — the analyst firm behind the framework
- [[plan-validate-execute|Plan-Validate-Execute]] — the human-in/on-the-loop autonomy pattern Question 5 invokes
- [[guardian-agents-market-guide|Gartner Guardian Agents Market Guide]] — Gartner's adjacent agentic-AI-governance market analysis
- [[measuring-agent-effectiveness-talk|Measuring Security-Agent Effectiveness]] — the builder-side counterpart: multi-dimensional evaluation of the agent you ship
- [[ai-security-larsen-effect-talk|The AI Security Larsen Effect]] — the practitioner buyer-side counterpart: capability-based configure/buy/build vendor selection

## Notes

[^gartner]: [BleepingComputer (Prophet Security-sponsored) — How to evaluate AI SOC agents: 7 questions Gartner says you should be asking](https://www.bleepingcomputer.com/news/security/how-to-evaluate-ai-soc-agents-7-questions-gartner-says-you-should-be-asking/), 2026, summarizing the Gartner report *Validate the Promises of AI SOC Agents With These Key Questions* (Craig Lawson, Andrew Davies). Gartner projection: 70% of large SOCs will pilot AI agents for Tier 1/Tier 2 operations by 2028; only 15% will achieve measurable improvement without structured evaluation.
