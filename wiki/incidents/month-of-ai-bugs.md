---
type: incident
title: "Month of AI Bugs Disclosures"
created: 2026-04-30
updated: 2026-04-30
tags:
  - incidents
  - prompt-injection
  - agentic-ai
  - red-teaming
  - disclosure-series
status: developing
incident_class: "rolling-disclosure"
attack_with_or_on_ai: "on AI"
date_observed: 2025-08
date_disclosed: 2025-08
target: "Every major frontier model and major agentic development kit"
threat_actor: "Demonstrated by Johann Rehberger and contributors (responsible disclosure)"
impact: "Dozens of responsibly-disclosed prompt-injection, indirect-injection, tool-abuse, and exfiltration vulnerabilities across the agentic-AI vendor landscape during a single calendar month, including the [[jules-ai-kill-chain|Jules AI compromise]]."
related:
  - "[[jules-ai-kill-chain]]"
  - "[[indirect-prompt-injection]]"
  - "[[tool-abuse-chains]]"
  - "[[johann-rehberger]]"
  - "[[unit-42-prompt-injection-observations]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[securing-your-agents-talk]]"
sources:
  - "[[.raw/talks/securing-your-agents-2026-04-30.md]]"
---

# Month of AI Bugs (August 2025)

## Summary

A coordinated disclosure series in **August 2025**, organized by [[johann-rehberger|Johann Rehberger]] (Embrace The Red), modeled on the earlier-era "Month of Browser Bugs" / "Month of Bugs" tradition from offensive security. Over the calendar month, dozens of responsibly-reported successful attacks were cataloged against **every frontier model and every major agentic development kit**.

The series is significant not because any individual disclosure was novel in mechanism — most were [[indirect-prompt-injection|indirect prompt injections]] leading to [[tool-abuse-chains|tool-abuse chains]] or data exfiltration — but because the **breadth and consistency** of the failures across vendors established that the agentic-AI security gap is structural, not vendor-specific.

The most-cited disclosure from the series is the **[[jules-ai-kill-chain|Jules AI kill chain]]**, a five-stage compromise of Google's coding agent.

## Significance

The Month of AI Bugs reframed the agentic-AI security conversation in three ways:

1. **From "exists in the lab" to "exists in production."** Before August 2025, vendor security marketing could plausibly claim that frontier models had injection resistance "in production deployments." The series demonstrated that vendor agent products were vulnerable in the same configurations customers were using.
2. **From "single-vendor problem" to "structural problem."** Failures appeared across Google, OpenAI, Anthropic, Microsoft, Meta, and major open-source frameworks. No vendor was clean. This shifted the framing from "Vendor X has a bug" to "the agentic-AI threat model has not been resolved by anyone."
3. **From "research curiosity" to "responsible-disclosure infrastructure."** The series generated formal disclosures, CVE assignments where applicable, and vendor patches. It established a precedent that AI-agent vulnerabilities flow through the same disclosure channels as traditional security bugs.

## Cross-Cutting Patterns From the Series

Drawing on [[securing-your-agents-talk|Securing Your Agents]] (slide 13–14) and Rehberger's public summaries:

- **Indirect injection dominated.** Direct "ignore previous instructions" attempts were the minority. The high-impact disclosures used hidden injections in GitHub issues, calendar invites, web pages, document metadata, and email bodies.
- **Coding agents were over-represented as targets.** They have file/network access, persistent state, and routinely ingest user-influenceable content (issues, PRs, READMEs, commit messages).
- **Persistence-in-agent was a recurring theme.** Multiple disclosures showed attackers using the agent's own write capabilities to embed payloads that survive session restarts.
- **No vendor had egress filtering by default.** Outbound-network capability was a load-bearing element of nearly every exfiltration chain.
- **Markdown image rendering and URL-fetch side channels** were a consistent exfiltration pattern across chat-style products.

## Connection to Other Wiki Pages

- **[[jules-ai-kill-chain|Jules AI Kill Chain — Indirect Injection to Full Remote Control]]** — the canonical case study from the series.
- **[[unit-42-prompt-injection-observations|Unit 42 In-the-Wild Prompt Injection Observations]]** — Palo Alto Networks' production-telemetry follow-up that confirmed the same patterns at scale outside Rehberger's controlled disclosures.
- **[[indirect-prompt-injection|Indirect Prompt Injection]]** — the dominant attack class.
- **[[tool-abuse-chains|Tool-Abuse Chains]]** — the dominant exploitation pattern post-injection.

## Defensive Takeaways the Series Established

1. **Assume injection succeeds; design for containment.** This became the default posture in subsequent enterprise agent-security writing (see [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]]).
2. **Egress filtering is non-optional** for any agent with both private-data and untrusted-content exposure (the [[lethal-trifecta|Lethal Trifecta]]).
3. **Sandboxing is necessary, not paranoid.** See [[agent-sandboxing|Agent Sandboxing]].
4. **Anomaly detection on tool-call sequences** catches what input-layer detection misses. See [[agent-observability|Agent Observability]].
5. **Coding agents need bespoke threat modeling** distinct from chat-style agents.

## Sources

- Primary aggregator: Johann Rehberger, Embrace The Red blog, August 2025 series.
- Public summary referenced in: [[securing-your-agents-talk|Securing Your Agents]], Bill McIntyre, 2026 (slide 13).

## See Also

- [[johann-rehberger|Johann Rehberger]] — series organizer
- [[jules-ai-kill-chain|Jules AI Kill Chain — Indirect Injection to Full Remote Control]] — most-cited single disclosure
- [[indirect-prompt-injection|Indirect Prompt Injection]] — dominant vector
- [[unit-42-prompt-injection-observations|Unit 42 In-the-Wild Prompt Injection Observations]] — production-telemetry counterpart
