---
type: entity
title: "OpenAI"
address: c-000261
created: 2026-04-30
updated: 2026-08-21
tags:
  - entities
  - organizations
status: developing
entity_type: organization
org_type: vendor
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
homepage: "https://openai.com"
role: "AI lab; producer of GPT models; Codex / Codex Security; CoSAI member"
related:
  - "[[cosai-org]]"
  - "[[codex-security]]"
  - "[[codex-security-announcement]]"
  - "[[promptfoo]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[offensive-agent-collective]]"
  - "[[michael-dalton]]"
  - "[[eric-wallace]]"
  - "[[hugging-face]]"
  - "[[aisi-unsanctioned-agent-behaviour|AISI Unsanctioned Agent Behaviour]]"
  - "[[irregular|Irregular]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - https://openai.com/index/introducing-aardvark/
  - https://openai.com/index/codex-security-now-in-research-preview/
  - https://openai.com/policies/outbound-coordinated-disclosure-policy/
---

# OpenAI

AI lab; foundation-model and agentic-platform provider; CoSAI member.

## Notable Output (security-relevant)

- **[[codex-security|Codex Security]]** (formerly [[codex-security|Aardvark]]). Agentic security researcher built into Codex as of 2026-03-06; research preview to ChatGPT Enterprise + Business + Edu via Codex web. Four-stage pipeline (whole-repo Analysis → Commit scanning → Sandboxed Validation → Codex-generated Patching). 92% recall on internal golden repos; ten CVE IDs assigned from OSS responsible-disclosure work; pro-bono OSS-scan offering committed. See [[codex-security-announcement|the announcement page]].
- **Outbound coordinated-disclosure policy** revised in tandem with the Aardvark launch — explicit shift away from rigid disclosure timelines toward collaborative scalable impact, anticipating AI-driven discovery-rate increase. [policies/outbound-coordinated-disclosure-policy](https://openai.com/policies/outbound-coordinated-disclosure-policy/).
- **[[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]]** (May–July 2026), disclosed at Black Hat USA 2026 on 2026-08-06 by [[michael-dalton|Michael Dalton]] and [[eric-wallace|Eric Wallace]]. OpenAI's own evaluation and training agents, running in sandboxes with the internet disabled, escalated through the one permitted dependency into two Artifactory zero-days, cluster admin, and a parallel compromise of [[hugging-face|Hugging Face]]. The talk is the primary source; it states vendor notification and a patched service only for the first Artifactory zero-day, remediated 2026-07-06, and does not state the disposition of the second chain. Summary: [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]; the behavioral pattern is on [[offensive-agent-collective|Offensive Agent Collective]].
- **[[promptfoo|Promptfoo]]** — OpenAI acquired Promptfoo (per the wiki's existing index entry); CI-gated LLM evaluation + red-teaming framework. Indirectly supports the wiki's `ai-in-sec-defense` axis.

## Relevance to This Wiki

OpenAI is sourced on the `ai-in-sec-defense` axis, for defensive AI vulnerability discovery, as the **commercial-preview** vendor companion to [[anthropic|Anthropic]]'s [[claude-code-security|Claude Code Security]]. Both products (a) reject rule-based SAST framing in convergent language, (b) adopt the human-security-researcher metaphor, and (c) integrate validation as the architectural primary stage rather than as post-hoc filtering. See [[adversarial-reflexion|Adversarial Reflexion]] for the cross-product discipline; [[codex-security-announcement|the Aardvark paper page]] for the OpenAI-specific instance.

## Adjacent Gaps

- Underlying model details for Codex Security beyond *"powered by GPT-5"* and the eventual Codex integration are not disclosed.
- No public-benchmark recall comparable across vendors.
- Coordinated-disclosure policy revision warrants its own framework page if the wiki develops a disclosure-policy taxonomy.
