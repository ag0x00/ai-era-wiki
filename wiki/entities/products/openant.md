---
type: entity
title: "OpenAnt"
address: c-000060
created: 2026-05-15
updated: 2026-08-21
tags:
  - entities
  - product
  - tool
  - vuln-discovery
  - knostic
  - open-source
status: seed
scope_axis: [ai-in-sec-defense]
entity_type: product
role: "Open-source LLM-based vulnerability discovery tool from Knostic. Six-stage pipeline that combines static call-graph analysis with agentic LLM stages and Docker-sandboxed dynamic verification to produce verified-exploitable findings while minimizing false positives via constrained-attacker-persona Adversarial Reflexion."
homepage: https://openant.knostic.ai/
github: https://github.com/knostic/OpenAnt
license: "TBD — confirm on repo inspection"
maintainer: "[[knostic|Knostic]]"
research_lead: "[[nahum-korda|Nahum Korda]]"
first_mentioned: "[[openant-announcement|Introducing OpenAnt blog]]"
related:
  - "[[knostic|Knostic]]"
  - "[[openant-announcement|OpenAnt announcement post]]"
  - "[[adversarial-reflexion|Adversarial Reflexion]]"
  - "[[nahum-korda|Nahum Korda]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[big-sleep|Big Sleep]]"
  - "[[codemender|CodeMender]]"
  - "[[mdash|MDASH]]"
  - "[[mythos|Mythos]]"
  - "[[xbow|XBOW]]"
  - "[[codex-security|Codex Security]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[agentic-ai-security-cmm-measurement-protocol|Agentic AI Security CMM Measurement Protocol]]"
sources:
  - https://openant.knostic.ai/
  - https://github.com/knostic/OpenAnt
  - https://www.knostic.ai/blog/openant
  - "[[.raw/articles/openant-2026-05-15.md]]"
---

# OpenAnt

**Sources:** [Project page — openant.knostic.ai](https://openant.knostic.ai/) · [GitHub — `knostic/OpenAnt`](https://github.com/knostic/OpenAnt) · [Announcement — Knostic blog](https://www.knostic.ai/blog/openant) · OSS-scan-program contact: `oss-scan@knostic.ai`

## Description

OpenAnt is an open-source LLM-based vulnerability discovery tool whose unit of analysis is a *unit* — a function plus its caller / callee call-graph metadata. Six-stage pipeline: static code parse → static reachability traversal → agentic exposure classification (Sonnet) → vulnerability discovery (Claude Opus) → exploitability verification (Claude Opus with agentic tool use under a constrained-attacker persona, called *Adversarial Reflexion*) → Docker-sandboxed dynamic verification. The pipeline is designed for *false-positive control by architectural constraint*: candidate units are ratcheted down by ~5 orders of magnitude (OpenSSL: 15,232 → 3 confirmed exploitable, 99.98% reduction) at a token cost of ~\$443 rather than the ~\$329,000 a naive Opus-on-every-unit pass would incur.

## Relevance to This Wiki

Fourth sourced AI vulnerability-discovery production path, alongside [[big-sleep|Big Sleep]] (Google), [[codemender|CodeMender]] (Google DeepMind), and [[mdash|MDASH]] (Microsoft). The first open-source entry — and the only one where the full pipeline source is auditable and the per-stage cost is published. Strengthens [[frontier-ai-for-vuln-discovery|the wiki's frontier-AI thesis]] with a second mechanism (constrained-persona verification with explicit trace) for the *"harness does the work, the model is one input"* observation, joining MDASH's ensemble + debate mechanism.

## Outputs

- **CLI / library** — point at a source tree, run the six-stage pipeline against the default Anthropic-API model stack; the model can be swapped to any provider. Free; token cost incurred by the user.
- **OSS scan program** — Knostic-hosted free scans for open-source projects; submit at `oss-scan@knostic.ai`.
- **Managed offering** — waitlist via `knostic.ai/openant`.
- **GitHub repository** — `knostic/OpenAnt`.

## Notable Design Choices

- **Adversarial Reflexion with constrained attacker persona.** Stage-5 exploitability verification forbids the model from assuming server access, credentials, or local-file access. For CLI tools and libraries: *no ability to run CLI commands*; the exploit must trigger *remotely*. Every step must be traced explicitly against the actual codebase via tool use. Eliminates the agreeable-judge false-positive class — see [[adversarial-reflexion|the Adversarial Reflexion concept page]] for the generalization.
- **The "unit" abstraction.** Function + call graph + caller / callee metadata. Decomposes the codebase into LLM-context-sized chunks with enough surrounding semantics for exploitability reasoning. Foundation data structure for every downstream stage.
- **Static-then-agentic-then-dynamic phasing.** Stages 1–2 are pure call-graph traversal with no LLM cost — they carry the bulk of the candidate reduction (OpenSSL 97.4% at Stage 2 alone). Stages 3–5 are agentic LLM stages. Stage 6 is Docker-sandboxed dynamic execution. The phasing isolates the cost driver (agentic stages) from the reduction driver (static analysis).
- **Per-stage cost publication.** The blog publishes per-unit cost ranges (\$0.13–\$10.92 for Stage 3, \$0.14–\$10.54 for Stage 5) and total cost across five real OSS projects. Cost discipline is a first-class concern; the announcement frames OpenAnt as *"free as in puppy"* — care and feeding via token cost.
- **No competing-product framing.** Explicit positioning: *"we have zero intention of competing with"* [[codex-security|Codex Security]] (announced as Aardvark) and [[claude-code-security|Claude Code Security]]. OpenAnt's niche is *OSS-maintainer-side defender tooling*. [[agentic-ai-security-cmm-measurement-protocol|The CMM measurement protocol]] cites OpenAnt's constrained-persona verification as one of five sourced instruments implementing false-positive control by architectural constraint, the answer its Stage 2 cross-domain question treats as production-grade. All three place false-positive control in a dedicated verification stage: OpenAnt's constrained-persona Adversarial Reflexion plus Docker-sandboxed dynamic test, Codex Security's sandboxed exploit trigger, Claude Code Security's multi-stage self-critique. Codex Security and OpenAnt both execute the candidate exploit; Claude Code Security discloses no sandboxed dynamic execution.

## Cross-Project Filter Ratios (Feb 2026)

| Repository | Language | Total units | After reachability | Verified-exploitable | Total cost (USD) |
| :--- | :--- | --: | --: | --: | --: |
| OpenSSL | C | 15,232 | 390 (97.4% reduction) | 3 | \$442.65 |
| WordPress | PHP | 12,177 | 393 (96.8% reduction) | 20 | \$239.45 |
| LangChain | Python | 6,701 | 143 (97.9% reduction) | 1 | \$51.48 |
| Rails | Ruby | 89 | 89 (0% reduction) | 2 | \$25.18 |
| Grafana | TypeScript + Go | 18,500 | 2,379 (87.1% reduction) | 86 | \$1,080.86 |

Reachability filtering generalizes well in C / PHP / Python (all ~97% reduction) and TypeScript+Go (87%), but collapses entirely on Rails (0% reduction). Stage 3 (agentic classification) carries the Rails reduction load — at correspondingly higher cost per unit.

## Adjacent Gaps

- **License clarification.** Repository linked but license terms not in the announcement body. Confirm on repo inspection.
- **False-negative measurement.** The announcement reports filter ratios and verified-exploit counts but not what the pipeline misses. A CVE-corpus replay or [[cybergym|CyberGym]]-adjacent eval would let OpenAnt's recall be compared to MDASH (88.45% on CyberGym), raw Mythos (83.1%), and XBOW × Mythos (42-55% FN reduction over Opus 4.6).
- **Dynamic-test methodological weakness in C codebases.** Acknowledged in the announcement (Known Issue #1). Pronounced for memory-safety classes (pointer arithmetic, complex control flow).
- **Cost-estimate volatility.** Observed ~2× swing from estimate to actual; driver is error-recovery loops (e.g., invalid-JSON automatic correction).
