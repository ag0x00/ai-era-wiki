---
type: entity
title: "Google"
created: 2026-04-30
updated: 2026-08-16
tags:
  - entities
  - organizations
  - glasswing
status: active
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
entity_type: organization
org_type: vendor
homepage: "https://about.google"
role: "Hyperscaler; publisher of Google SAIF; SAIF data donor to CoSAI; developer of A2A protocol and Google ADK; Project Glasswing launch partner (May 2026)"
related:
  - "[[google-saif]]"
  - "[[google-agentic-soc]]"
  - "[[cosai]]"
  - "[[cosai-org]]"
  - "[[glasswing]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[mythos]]"
  - "[[big-sleep]]"
  - "[[codemender]]"
  - "[[google-big-sleep-projectzero]]"
  - "[[google-codemender-deepmind]]"
  - "[[gemini-cli]]"
  - "[[gemini-cli-workspace-trust-rce]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "https://www.anthropic.com/glasswing"
---

# Google

**Sources:** [Google (homepage)](https://about.google) · [Google SAIF](https://saif.google) · [Google Cloud Security](https://cloud.google.com/security)

> [!gap] Stub — pending full migration
> Google publishes [[google-saif|SAIF]] and donated SAIF data to [[cosai|CoSAI]] in July 2024.

## AI Security Contributions (from ai-security-standards-in-q1-2026)

- **[[google-saif|SAIF]]** — AI security framework; SAIF Risk Map and Risk Assessment donated to CoSAI in 2024
- **[[a2a-protocol|A2A Protocol]]** (v1.0.0, released 2026-03-12) — Agent-to-Agent protocol with signed Agent Cards (§8.4) and opacity principle; donated to Linux Foundation 2025-06-23; hosted under LF's Agentic AI Foundation
- **Google ADK Go 1.0** (March 31, 2026) — ships with `before_model_callback` hooks, OpenTelemetry integration, and Model Armor integration — reference implementation of platform-level enforcement
- **CoSAI Premier Sponsor** — key contributor to MCP Security White Paper

## Coding-Agent Surface

Google ships [[gemini-cli|Gemini CLI]] as `@google/gemini-cli`, with a first-party GitHub Action wrapper, `google-github-actions/run-gemini-cli`, that places the harness in the CI-runner deployment shape. On 2026-04-24 Google published [GHSA-wpqr-6v78-jr5g](https://github.com/advisories/GHSA-wpqr-6v78-jr5g), a CVSS 10.0 advisory against both, bundling two independently reported defects: automatic workspace-folder trust in headless mode, and `--yolo` suppressing the fine-grained tool allowlist. See [[gemini-cli-workspace-trust-rce|the incident record]] for the mechanisms and the eight-day report-to-patch timeline. Google made workspace trust explicit and enforced the allowlist under `--yolo` — a secure default that breaks existing pipelines by design, and says so in the advisory.

## Workspace Security

[[nicolas-lidzborski|Nicolas Lidzborski]] (Principal Software Engineer, Google Workspace security; ~3 years on GenAI security) presented a three-year retrospective at [[unprompted-conference-march-2026|Unprompted March 2026]]: [[securing-workspace-genai-at-google-talk|Securing Workspace GenAI at Google]]. The talk introduces the wiki's [[prompt-as-code|Prompt as Code]] structural framing, names [[agency-gap|Agency Gap]] / [[orchestration-hijacking|Orchestration Hijacking]] / [[recursive-prompt-injection|Recursive Prompt Injection (and Semantic Gaslighting)]] as threat sub-classes, and documents the "Architecting the Fortress" four-layer structural blueprint paired with [[plan-validate-execute|Plan-Validate-Execute]] as Google's canonical HITL pattern for high-stakes irreversible actions. Cross-validates the [[lethal-trifecta|Lethal Trifecta]] framing from the productivity-environment angle (calendar invites + email + smart-home control as concrete real-world impact surface).

## Project Glasswing partnership (May 2026)

Google is a named launch partner in [[glasswing|Project Glasswing]] (Anthropic's coalition initiative applying Mythos Preview to defensive vulnerability discovery on critical software). **Heather Adkins** (VP of Security Engineering) is the quoted executive. Google makes Mythos Preview available to Glasswing participants via **Vertex AI**, and operates two parallel Google-internal AI-powered cybersecurity tools — [[big-sleep|Big Sleep]] (discovery, Project Zero + DeepMind, 2024) and [[codemender|CodeMender]] (patching, DeepMind, 2025) — described in Adkins's quote as Google's tools to "find and fix critical software flaws." See [[anthropic-glasswing-announcement|the Glasswing announcement]] for coalition context.

## Agentic SOC (defender-side)

On the defender side, Google ships an agentic SOC across Google Security Operations and Google Threat Intelligence — Gemini agents for alert triage and investigation, malware analysis, detection engineering, threat hunting, and third-party context, governed by per-agent cryptographic identity. See [[google-agentic-soc|Google Agentic SOC]] for the product surface and its integration into Google SecOps and Google Unified Security.

## AI-Powered Cybersecurity Stack

Google operates a two-agent AI security stack on the DeepMind side, paired with infrastructure participation across Google Cloud Security:

| Agent | Role | Origin | First public milestone |
|---|---|---|---|
| [[big-sleep\|Big Sleep]] | Variant-analysis vulnerability discovery | Project Zero + DeepMind | [[google-big-sleep-projectzero\|Oct 2024 paper]] — first AI-discovered real-world exploitable memory-safety bug (SQLite); [Cloud CISO Perspectives blog](https://cloud.google.com/blog/products/identity-security/cloud-ciso-perspectives-our-big-sleep-agent-makes-big-leap) reports July 2025 SQLite CVE-2025-6965 as the first AI-foiled in-the-wild exploit |
| [[codemender\|CodeMender]] | Patching (reactive + proactive code rewrite) | DeepMind | [[google-codemender-deepmind\|Oct 2025 paper]] — 72 OSS patches upstreamed in 6 months; libwebp `-fbounds-safety` annotations as the proactive-class example |

The two agents are designed as discovery → patching counterparts; integration architecture between them is not publicly documented.

Their availability has since diverged. Big Sleep remains vendor-internal. CodeMender entered preview as a managed Google Cloud product on 2026-07-21 ([[google-cloud-codemender-preview|source summary]]), sold through the Gemini Enterprise Agent Platform; through AI Threat Defense, with [[wiz|Wiz]] orchestrating; and paired with a cyber-specialized Gemini 3.5 Flash Cyber model restricted to a small set of governments and trusted partners. The preview post does not mention Big Sleep, so the discovery-to-patching handoff remains undocumented on the product side.

### Predecessor framework

- **Project Naptime** (June 2024) — LLM-assisted vuln research framework that achieved state-of-the-art on Meta's [[cyberseceval|CyberSecEval2]] benchmark; the direct precursor to Big Sleep.

### Lineage

Google's AI-cybersecurity stack lineage (per CodeMender announcement): OSS-Fuzz → [AI-powered fuzzing (Aug 2023)](https://security.googleblog.com/2023/08/ai-powered-fuzzing-breaking-bug-hunting.html) → Project Naptime (June 2024) → Big Sleep (Oct 2024) → CodeMender (Oct 2025) → [[google-cloud-codemender-preview|CodeMender managed preview on Google Cloud (Jul 2026)]].

## CaMeL pattern

Google DeepMind published the [[camel-pattern|CaMeL pattern]] (March 2025, arXiv 2503.12599) — privileged + quarantined LLM split with a structured output channel. Research-stage as of Q2 2026; it is the architecturally pure form of the channel-separation insight that [[prompt-as-code|Prompt as Code]] points to.

> [!warning] Q1 2026 supply chain incident
> A LiteLLM supply chain compromise was detected in Google ADK dependencies (March 24, 2026), demonstrating that even Google's AI toolchain is not immune to the supply chain risks active in this period.

## Agent runtime isolation

Google supplies two open-source isolation primitives the wiki tracks: [[gvisor|gVisor]] (the user-space-kernel container sandbox, Apache 2.0) and, building on it, [[gke-agent-sandbox|Agent Sandbox]] — a Kubernetes SIG Apps subproject announced at Cloud Next '26 that turns the per-task agent sandbox into a first-class Kubernetes resource (Sandbox / SandboxTemplate / SandboxClaim CRDs, gVisor default, managed GKE delivery at 300 sandboxes/sec). Its design bet is that Kubernetes itself should be the agent runtime, with isolation delivered as an open primitive that runs on any cluster rather than a proprietary feature. This is the one runtime-security row where GCP leads with a portable open primitive rather than trailing AWS and Azure. See [[agent-sandbox-isolation-landscape|the isolation landscape comparison]].
