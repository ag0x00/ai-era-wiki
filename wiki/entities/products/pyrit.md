---
type: entity
entity_type: product
title: "PyRIT (Python Risk Identification Tool)"
homepage: "https://github.com/microsoft/PyRIT"
created: 2026-05-02
updated: 2026-06-21
tags:
  - entities
  - products
  - red-team
  - microsoft
  - open-source
  - orchestration
status: developing
scope_axis:
  - sec-of-ai
publisher: "Microsoft AI Red Team"
license: "MIT"
language: "Python"
canonical_repo: "https://github.com/microsoft/PyRIT"
docs: "https://microsoft.github.io/PyRIT/"
latest_version: "v0.13.0 (Apr 17, 2026)"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[garak]]"
  - "[[promptfoo]]"
  - "[[mindgard-cart]]"
  - "[[microsoft-rai]]"
  - "[[mitre-atlas]]"
sources:
  - "https://github.com/microsoft/PyRIT"
  - "https://microsoft.github.io/PyRIT/"
  - "https://github.com/microsoft/PyRIT/releases"
---

# PyRIT — Python Risk Identification Tool for generative AI

**Sources:** [PyRIT (repo)](https://github.com/microsoft/PyRIT) · [PyRIT docs](https://microsoft.github.io/PyRIT/) · [PyRIT releases](https://github.com/microsoft/PyRIT/releases)

Microsoft AI Red Team's open-source Python framework for **orchestrating multi-turn and single-turn adversarial attacks** against generative-AI systems. The wiki's [[agentic-ai-security-cmm-2026|CMM]] cites PyRIT as the **"orchestration / multi-turn"** attack category in the D7 L4 four-quadrant red-team coverage requirement.

## Function

PyRIT is an **orchestration framework**, not a fixed probe library. It composes:

| Component | Role |
|---|---|
| Targets | Adapters for OpenAI, Azure OpenAI, [[anthropic\|Anthropic]], Google, HuggingFace, custom HTTP/WebSocket, web apps via Playwright |
| Prompt converters | Encoding, persona injection, language transforms applied before sending |
| Scorers | True/false, Likert scale, classification, backed by LLMs, Azure AI Content Safety, or user logic |
| Memory | Stateful storage for multi-turn attacks |
| Orchestrators | Multi-turn attack strategies: Crescendo, TAP (Tree of Attacks with Pruning), Skeleton Key |
| Datasets | Adversarial-prompt corpora |

v0.13.0 (April 2026) introduced `TargetConfiguration` (replacing `TargetCapabilities`) and `AttackTechniqueRegistry` for composable attacks, plus ISO 42001-aligned harm definitions and a VisualLeakBench dataset loader.

## Repo move (April 2026)

The canonical repo is now **`microsoft/PyRIT`** (formerly `Azure/PyRIT`, archived March 27 2026). Wiki references using the old URL still redirect but should be updated. As of May 2026: 3.8k stars, 747 forks, 20 releases.

## Limitations

| Gap | Filled by |
|---|---|
| Fixed probe library (DAN, GCG, encoding etc.) | [[garak\|Garak]] |
| Regression-style pass/fail dashboards | [[promptfoo\|Promptfoo]] |
| 24/7 managed continuous service | [[mindgard-cart\|Mindgard CART]] |

This is the basis for the wiki's **four-quadrant** D7 L4 evidence requirement: single-tool coverage is explicitly not L4.

## Direct quotes

- *"An open source framework built to empower security professionals and engineers to proactively identify risks in generative AI systems."* (repo README)
- *"Automated and human-led AI red teaming — a flexible, extensible framework for assessing the security and safety of generative AI systems at scale."* (microsoft.github.io/PyRIT)

## Use in this wiki

- [[agentic-ai-security-cmm-2026|CMM]] D7 L4: orchestration / multi-turn red-team category
- [[agentic-ai-security-cmm-measurement-protocol|Measurement Protocol]]: interview script asks *"which tools were used (Promptfoo / PyRIT / Garak / Mindgard CART)?"*
- Cross-checked in [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes 2026]] §Class 2 (long-running adaptive APT campaigns). Multi-turn orchestration is what scales an attack, and PyRIT is the canonical OSS toolkit attackers and defenders both use.

## Caveats

- **API instability across minor versions**: v0.12 → v0.13 changed the target abstraction. Wiki-referenced code snippets predating April 2026 are stale.
- **Multimodal coverage is thin**: VisualLeakBench landed in v0.13.0 but vision/audio/voice attack libraries remain limited compared to text.
- **MCP / tool-use red-team primitives are sparse**: PyRIT can drive tool-using targets but ships limited canned tool-abuse scenarios. This is the seam [[promptfoo|Promptfoo]]'s BOLA/BFLA plugins fill at the agentic layer.

## See Also

- [[garak|Garak]]: probe library counterpart (single-shot probes)
- [[promptfoo|Promptfoo]]: regression-suite counterpart (CI-gated)
- [[mindgard-cart|Mindgard CART]]: continuous managed counterpart
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]: D7 L4 evidence anchor
