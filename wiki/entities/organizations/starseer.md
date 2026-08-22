---
type: entity
entity_type: organization
org_type: vendor
title: "Starseer"
created: 2026-05-03
updated: 2026-05-23
tags:
  - organizations
  - ai-security
  - detection-engineering
  - mechanistic-interpretability
  - starseer
status: seed
no_public_url: "No public homepage, product page, or GitHub confirmed at talk date (March 2026). Hurd referenced a Starseer blog but provided no URL. Update this field when public artifacts surface."
related:
  - "[[carl-hurd]]"
  - "[[glass-box-security-talk]]"
  - "[[glass-box-security]]"
  - "[[mechanistic-interpretability-for-defense]]"
  - "[[agent-observability]]"
sources:
  - .raw/talks/2026-03-03_Carl-Hurd_Glass-Box-Security_transcript.md
---

# Starseer

**Sources:** No public homepage confirmed as of talk date (March 2026). Primary source: [[glass-box-security-talk|Hurd's Unprompted March 2026 talk]].

Early-stage AI security startup co-founded by [[carl-hurd|Carl Hurd]] (CTO) and at least one other co-founder (unnamed in the talk transcript). Founded 2025. Building next-generation detection engineering tooling for AI systems grounded in mechanistic interpretability.

## Product direction

Starseer is building what Hurd calls a **"Glass-Box Security"** capability: forward-pass hooks into neural network residual streams that capture intent (via cosine similarity of activation vectors) and strength (via scalar projection / dot product), enabling behavior-based detection rules expressed in YARA-style syntax with AI-native semantic modules.

The design targets the gap between existing plaintext-surface controls (eBPF/ETW + prompt firewalls) and what is actually happening inside a model's reasoning process. Key engineering choices described in the March 2026 talk:

- **Canary model for interpretability** — instruments smaller open-weight models (e.g., Llama 3.1) inline or async alongside frontier models, to avoid requiring API-only SaaS users to abandon managed inference endpoints entirely.
- **Residual-stream-only hooks** — avoids the quadratic cost of self-attention head monitoring.
- **Per-model empirical calibration** — concept vectors and detection thresholds are derived per model instance.
- **YARA + Cedar integration** — YARA for semantic activation-level rules; Cedar for deterministic action-gate policies; built on existing detection-engineering tooling ecosystems.

## Category

Starseer is a runtime instrumentation vendor — closer to the "runtime-instrumentation camp" than the "inline gateway camp" in the agentic-AI-security seed-cohort split described in [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]]. The design requires either self-hosted inference or an instrumented sidecar/canary model; it does not operate purely at the network layer.

> [!gap] Limited public information
> No public GitHub, product page, or funding announcement was available from the talk or transcript. Starseer's blog is referenced by Hurd but no URL was given. This page should be updated when Starseer publishes public artifacts.
