---
type: concept
title: "Citizen Coders"
address: c-000072
created: 2026-05-15
updated: 2026-08-21
tags:
  - concepts
  - citizen-coders
  - shadow-it
  - mythos-era
  - inventory
status: seed
no_public_url: "term coined in the internal Mythos-ready paper; no separate canonical external page"
scope_axis: [sec-against-ai, sec-of-ai]
related:
  - "[[mythos-ready-security-program|Mythos-ready Security Program]]"
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[shadow-ai|Shadow AI]]"
  - "[[shadow-automation|Shadow Automation]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[vibe-coding|Vibe Coding]]"
  - "[[supply-chain-security-for-agents|Supply Chain Security for Agents]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[securing-agentic-coding]]"
  - "[[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]"
  - "[[slopsquatting|Slopsquatting]]"
sources:
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
---

# Citizen Coders

**Citizen Coders** — surfaced by the [[mythos-ready-briefing|Mythos-ready briefing]] (April 2026) — names the proliferation of coding agents to **non-developer users**. Using a coding agent in 2026 is *"now easier than using Excel; all you need is English."* The structural consequence: code, infrastructure, and dependencies enter the organization through users who were never previously in scope for software-engineering security controls, fragmenting central IT visibility and creating new inventory + supply-chain gaps.

## Significance

The Mythos-era organizational threat surface is **bigger than what the security team can inventory** — and it is *getting bigger faster* as coding agents proliferate to non-developers. The Mythos-ready briefing surfaces this in two places:

> *"Shadow IT will fragment central control as coding agents proliferate to Citizen Coders, employees develop their own infrastructure, and threat intelligence is lagging behind vulnerability discovery and exploitation."*
> — [[mythos-ready-briefing|§IV The Mythos-ready Security Program]]

> *"The proliferation of coding agents to non-developer users further fragments central IT visibility."*
> — Risk Register #6 (Incomplete Asset and Exposure Inventory)

## Relationship to Existing Wiki Concepts

- **Sibling to [[shadow-ai|Shadow AI]]** and **[[shadow-automation|Shadow Automation]]**: Shadow AI is *unauthorized AI tool usage by end users* (Samsung-leak class); Shadow Automation is *ungoverned agents accessing repos / prod / credentials at developer pace* (a Knostic framing); **Citizen Coders is the further generalization** — non-developers writing software with agentic assistance, often without the security team's knowledge that *software is being written at all*.
- **Adjacent to [[vibe-coding|Vibe Coding]]** (Karpathy-coined, formalized in [[pwc-agentic-sdlc-in-practice|PwC's 2026 Agentic SDLC report]]): vibe coding is the *method* (natural-language intent rather than exact specifications); Citizen Coders is the *user class* doing it.
- **Operational consequence for [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]**: every Citizen Coder's `.claude/` (or equivalent) tree is a supply-chain artifact the enterprise has no visibility into. The fragmentation is exactly what [[agentshield|AgentShield]]-style audit was designed to surface — but only on harnesses the security team knows exist.
- **Operational consequence for [[supply-chain-security-for-agents|Supply Chain Security for Agents]]**: Citizen Coders install MCP servers, skills, and IDE extensions on a long tail of endpoints the security team does not centrally provision.
- **Exposure to [[slopsquatting|slopsquatting]]**: [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] places Citizen Coders on the exposed side of the hallucinated-package attack class, because a non-developer generating code through an assistant is the least likely to check a suggested package name against its registry history. The verification step that catches an invented dependency is the one this user class is least equipped to perform.

## Operational Response

From the Mythos-ready playbook's Priority Actions:

- **PA 2 — Require AI Agent Adoption.** Formalize agent usage *as part of all security functions, with mandatory security controls and oversight in place.* The same framing extends to non-security functions: usage cannot be optional if security wants to maintain visibility.
- **PA 7 — Inventory and Reduce Attack Surface.** Use agents themselves to accelerate inventory across the full organization, including infrastructure assembled by non-developers.
- **PA 3 — Defend Your Agents.** *"Define scope boundaries, blast-radius limits, escalation logic, and human override mechanisms"* applies to Citizen Coder agents as much as to security-owned agents.

## Deployment-shape exposure

A citizen coder's exposure is set by which [[generative-coding-deployment-shape-2026|deployment variant]] they are handed. The interactive shape asks them to adjudicate permission prompts about shell commands they may not be able to evaluate, and this is the population most likely to allowlist broadly to clear the friction. The delegated-cloud shape asks less of them and contains more, because the isolation boundary is the vendor's rather than the operator's. Where the audience is non-specialist, the containment argument for delegated or sandboxed variants is stronger than the productivity argument.

## See Also

- [[mythos-ready-briefing|Mythos-ready briefing]] — naming source.
- [[shadow-ai|Shadow AI]] · [[shadow-automation|Shadow Automation]] · [[vibe-coding|Vibe Coding]] — adjacent concepts.
- [[pwc-agentic-sdlc-in-practice|PwC Middle East 2026 Agentic SDLC report]] — Pioneer-tier adoption data point (38% of regional teams already augmenting ≥6 of 7 SDLC stages); broader empirical context for the shift Citizen Coders represents at the user-class level.
