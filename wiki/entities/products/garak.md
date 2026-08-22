---
type: entity
entity_type: product
title: "Garak"
created: 2026-05-02
updated: 2026-06-21
tags:
  - entities
  - products
  - red-team
  - nvidia
  - open-source
  - probe-library
status: developing
scope_axis:
  - sec-of-ai
publisher: "NVIDIA"
homepage: "https://github.com/NVIDIA/garak"
license: "Apache 2.0"
language: "Python"
canonical_repo: "https://github.com/NVIDIA/garak"
docs: "https://docs.garak.ai/"
latest_version: "v0.15.0 (May 1, 2026)"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[pyrit]]"
  - "[[promptfoo]]"
  - "[[mindgard-cart]]"
  - "[[mitre-atlas]]"
  - "[[owasp-llm-top-10]]"
sources:
  - "https://github.com/NVIDIA/garak"
  - "https://docs.garak.ai/"
---

# Garak — Generative AI Red-teaming & Assessment Kit

**Sources:** [Garak (NVIDIA/garak repo)](https://github.com/NVIDIA/garak) · [Garak docs](https://docs.garak.ai/)

NVIDIA's open-source **LLM vulnerability scanner**. A probe-and-detector library that runs a fixed taxonomy of attack probes against a target model and reports per-probe pass/fail rates. The wiki's [[agentic-ai-security-cmm-2026|CMM]] cites Garak as the **"probe library"** attack category in the D7 L4 four-quadrant red-team coverage requirement.

## Function

> *"checks if an LLM can be made to fail in a way we don't want."* — repo README

Garak's mental model: **probes × generators × detectors**.

| Layer | Examples |
|---|---|
| **Probes** (attack categories) | `encoding`, `promptinject`, `gcg`, `dan`, `malwaregen`, `xss`, `leakreplay`, `packagehallucination`, `snowball`, `misleading`, `donotanswer`, `goodside`, `badchars`, `atkgen`, `continuation`, `glitch`, `grandma`, `lmrc`, `realtoxicityprompts`, `av_spam_scanning`, `blank` (~18+ categories; exact total varies per release) |
| **Generators** (target model APIs) | HuggingFace Hub (Pipeline, Inference API, private endpoints), OpenAI, AWS Bedrock, Replicate, Cohere, Groq, NVIDIA NIM, ggml/llama.cpp, REST (custom YAML), LiteLLM, plus test generators |
| **Detectors** (judges) | Each probe declares `primary_detector` + `extended_detectors`; harness wires the matching detector(s) automatically |

Output: JSONL hit logs, `garak.log`, plus an HTML report.

## Distinction from PyRIT

| Property | Garak | [[pyrit\|PyRIT]] |
|---|---|---|
| Attack shape | Single-shot, fixed | Multi-turn, stateful, composable |
| Coverage style | Breadth (catalog of named probes) | Depth (orchestration of attack sequences) |
| Run mode | Batch scan | Interactive / scripted orchestration |
| Best for | "Run the canon against this model" | "Build a novel multi-turn attack" |

The wiki's CMM D7 L4 requires **both** — single-tool coverage is not L4.

## Coverage caveats

- **`docs.garak.ai/garak/probes` returned 404** during research; the canonical probe inventory lives in the GitHub source tree (`garak/probes/`) and the docs site is "work in progress." Wiki citations should reference the source tree, not the docs page, for an authoritative list.
- **Probe count drifts release-to-release.** Do not cite a fixed number without pinning a Garak version.
- **Largely chat-completion-shaped.** Agentic / tool-use / MCP attack surfaces are not natively covered.

## Direct quotes

- *"checks if an LLM can be made to fail in a way we don't want."* — github.com/NVIDIA/garak
- *"Generative AI Red-teaming & Assessment Kit"* — repo README

## Use in this wiki

- [[agentic-ai-security-cmm-2026|CMM]] D7 L4 — probe-library red-team category
- [[agentic-ai-security-cmm-measurement-protocol|Measurement Protocol]] — listed as one of four required tools at L4 ("single-tool coverage is not L4")
- Closes the **breadth-of-known-CVE-style-probe-coverage** seam: Garak gives reproducible coverage of named jailbreak/injection/leak categories that PyRIT and Promptfoo would have to re-implement.

## See Also

- [[pyrit|PyRIT]] — orchestration counterpart
- [[promptfoo|Promptfoo]] — regression-suite counterpart
- [[mindgard-cart|Mindgard CART]] — continuous managed counterpart
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — D7 L4 evidence anchor
