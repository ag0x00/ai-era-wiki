---
type: entity
entity_type: product
title: "CodeMender (Google DeepMind)"
address: c-000036
created: 2026-05-13
updated: 2026-08-21
tags:
  - products
  - google
  - google-cloud
  - deepmind
  - codemender
  - gemini-deep-think
  - vuln-patching
  - ai-vuln-discovery
  - vulnops
  - wiz
  - ai-in-sec-defense
status: developing
scope_axis:
  - ai-in-sec-defense
vendor: "Google"
ga_date: ""
preview_date: 2026-07-21
homepage: "https://docs.cloud.google.com/gemini-enterprise-agent-platform/codemender"
related:
  - "[[google]]"
  - "[[google-codemender-deepmind]]"
  - "[[google-cloud-codemender-preview]]"
  - "[[big-sleep]]"
  - "[[google-big-sleep-projectzero]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[mdash]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[wiz]]"
  - "[[vulnops]]"
  - "[[llm-as-a-judge]]"
  - "[[autonomous-exploit-generation]]"
  - "[[codex-security]]"
  - "[[claude-code-security]]"
sources:
  - "https://deepmind.google/blog/introducing-codemender-an-ai-agent-for-code-security/"
  - "https://cloud.google.com/blog/products/identity-security/find-and-fix-software-vulnerabilities-with-codemender/"
  - "https://www.anthropic.com/glasswing"
---

# CodeMender (Google DeepMind)

**Sources:** [Google DeepMind — Introducing CodeMender (Oct 2025)](https://deepmind.google/blog/introducing-codemender-an-ai-agent-for-code-security/) · [Google Cloud — CodeMender in preview (Jul 2026)](https://cloud.google.com/blog/products/identity-security/find-and-fix-software-vulnerabilities-with-codemender/) · [product documentation](https://docs.cloud.google.com/gemini-enterprise-agent-platform/codemender) · [[google-codemender-deepmind|research source-summary]] · [[google-cloud-codemender-preview|preview source-summary]]

**CodeMender** is Google's AI agent for *patching* software vulnerabilities, the patching-side counterpart to [[big-sleep|Big Sleep]]'s discovery. DeepMind announced it as research on October 6, 2025; Google Cloud placed it in preview as a managed product on July 21, 2026. The research agent operates **reactively** (patching newly-found vulnerabilities) and **proactively** (rewriting existing code to eliminate entire vulnerability classes). By the 2025 announcement date, the team had upstreamed **72 security patches** to OSS projects, including codebases as large as **4.5 million lines of code**. All patches are reviewed by human researchers before submission.

The shipped product is narrower than the research agent: it scans, verifies exploitability, and generates reactive patches. Proactive class elimination has no counterpart in the published product description.

## Architecture

CodeMender uses **Gemini Deep Think** as the core reasoner, paired with a toolbox for reasoning and validation:

| Component | Role |
|---|---|
| **Static analysis** | Code pattern, control flow, data flow scrutiny |
| **Dynamic analysis** | Runtime behavior validation |
| **Differential testing** | Compare behavior between original and patched code |
| **Fuzzing** | Identify input-driven failure modes |
| **SMT solvers** | Formal reasoning about constraints |
| **LLM-based critique tool** | Highlights diff between original and modified; verifies no regressions; agent self-corrects from feedback |
| **LLM-judge for functional equivalence** | Confirms semantic preservation across modifications |

Patches are **only surfaced for human review** when they satisfy four quality dimensions: fixes the *root cause* (not just symptom), functionally correct, no regressions, follows style guidelines.

## Two Operating Modes

### Reactive: patch a newly-discovered vulnerability

CodeMender debugs the root cause and devises a patch. Two examples from the announcement:

- **Heap buffer overflow** where the actual problem was "incorrect stack management of XML elements during parsing." The agent identified that the crash report was misleading and located the true defect.
- **Non-trivial patch dealing with complex object lifetime issues**, requiring modification of a custom C-code generator inside the project.

### Proactive: rewrite existing code to eliminate vulnerability classes

Worked example: applying `-fbounds-safety` annotations to **libwebp** (a widely-used image compression library). Once applied, the compiler adds bounds checks that would have rendered **CVE-2023-4863** (the libwebp zero-click iOS exploit used in BLASTPASS / NSO Group operations) "unexploitable forever."

This is the highest-value mode: patching one vulnerability stops one exploit; rewriting a vulnerability class stops a category of exploits.

## Results (as of October 2025)

- **72 security patches upstreamed** to OSS projects in the 6 months prior to announcement.
- Target codebases include some as large as **4.5 million lines of code**.
- "Many of [the patches] have already been accepted and upstreamed."
- All patches **human-reviewed before submission**.

## Google Cloud preview (July 2026)

Google Cloud moved CodeMender from vendor-internal research to a managed, customer-facing agent in preview on 2026-07-21 ([[google-cloud-codemender-preview|source summary]]). The product pipeline is scan, verify, remediate:

| Stage | Function |
|---|---|
| **Scan** | Reasons over repository context rather than matching patterns; targets memory corruption, injection, web security issues, cryptographic flaws, insecure data handling. Languages: C/C++, Go, Java, Python, Ruby, Rust, TypeScript |
| **Verify** | Builds and runs proof-of-concept exploits in a customer-managed sandbox to establish that a finding is reachable before it is patched |
| **Remediate** | Generates a patch, checks it with an [[llm-as-a-judge\|LLM-as-a-judge]] for functional disruption, and delivers a code diff for developer approval |

The verify stage is new relative to the research description. It repurposes [[autonomous-exploit-generation|proof-of-concept exploit construction]] as a triage control: exploitability decides whether a finding warrants a patch and where it ranks.

Three access paths exist. The **Gemini Enterprise Agent Platform** path runs on generally available Gemini models. In the **AI Threat Defense** path, [[wiz|Wiz]] orchestrates: it calls CodeMender to scan, enriches findings with deployment context from the Wiz Security Graph, and triggers Wiz Red Agent for pentesting; a **Wiz Green Agent** then directs CodeMender to generate and test context-enriched patches. A third path pairs CodeMender with **Gemini 3.5 Flash Cyber**, restricted to a small set of governments and trusted partners with access planned to widen.

Customers select the model, trading cost, speed, and scanning depth against each other, and Google states support for third-party frontier models is planned for later in 2026. The research agent ran on Gemini Deep Think; the product does not fix a single reasoner. Enterprise terms named are VPC traffic routing, data isolation and encryption, zero retention of source code, and customer-operated sandboxes. Integration points are CI/CD, VS Code, and Antigravity.

Salesforce, Robinhood, and Palo Alto Networks are quoted as customers. No efficacy figures, pricing, or GA date accompany the preview.

## Position in the Wiki

CodeMender pairs with [[big-sleep|Big Sleep]] as Google's two-pronged DeepMind-affiliated stack:

| Capability | Agent |
|---|---|
| **Discovery / variant analysis** | [[big-sleep\|Big Sleep]] |
| **Patching / proactive rewrite** | CodeMender |

The architectural pattern (multi-agent specialization + LLM-judge validation + automated regression checks) converges with [[mdash|Microsoft MDASH]]'s Prepare-Scan-Validate-Dedup-Prove pipeline (CodeMender being patching-oriented; MDASH discovery-oriented). The pattern is now visible across all three Glasswing partner stacks (Google's Big Sleep + CodeMender, Microsoft's MDASH, Anthropic's Mythos + Glasswing partners).

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-2026|CMM]] D6 (Data, Memory & RAG) L5+** — proactive rewriting of vulnerable data-handling code (libwebp, XML parsers) is a D6-adjacent primitive.
- **[[agentic-ai-security-cmm-2026|CMM]] D8 (Supply Chain & AI-BOM) L5+** — upstreaming patches to OSS at the 4.5M-LOC scale is a supply-chain hardening primitive.
- **[[agentic-ai-security-cmm-2026|CMM]] D9 (Operations & Human Factors)** — human-review-before-submission is the explicit HITL pattern; analogous to [[plan-validate-execute|Plan-Validate-Execute]].
- **[[agentic-ai-security-reference-architecture|RA]] Observability Plane** — patch validation extends agent-output auditing.

## Open Questions

- **Efficacy at preview**: the Google Cloud launch publishes no recall, precision, false-positive, or patch-count data. The claim that exploit simulation cuts false positives is unevidenced.
- **Maintainer acceptance rate**: 72 patches upstreamed; the accepted-versus-rejected breakdown is not disclosed.
- **GA terms**: preview since 2026-07-21. GA date and pricing not disclosed.
- **Proactive mode in the product**: the 2025 research announcement's highest-value mode — rewriting code to eliminate whole vulnerability classes — has no counterpart in the shipped scan/verify/remediate pipeline. Whether it is deferred, unmarketed, or dropped is unstated.
- **Integration with Big Sleep**: handoff architecture undocumented. The preview post does not mention Big Sleep.
- **Glasswing role**: Google is a [[glasswing|Project Glasswing]] partner; whether CodeMender is offered to Glasswing participants via Vertex AI is not in any source.
- **Gemini 3.5 Flash Cyber scope**: what the cyber-specialized model does that generally available Gemini models do not, and what qualifies an organization as a trusted partner, are undisclosed.
- **Technical-paper followups**: DeepMind promised technical papers and reports "in the coming months"; none yet ingested.

## See Also

- [[google-codemender-deepmind|CodeMender source-summary page]]: 2025 research announcement.
- [[google-cloud-codemender-preview|CodeMender Preview on Google Cloud]]: 2026 product launch.
- [[big-sleep|Big Sleep]]: discovery-side counterpart.
- [[google|Google]]: vendor.
- [[wiz|Wiz]]: orchestration layer in the AI Threat Defense access path.
- [[vulnops|VulnOps]]: the function the preview product is sold into.
- [[anthropic-glasswing-announcement|Glasswing announcement]]: May 2026 coalition naming CodeMender.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]: wiki thesis.
- [[codex-security|Codex Security]] and [[claude-code-security|Claude Code Security]]: the convergent scan/validate/patch products from OpenAI and Anthropic.
- [[mdash|MDASH]]: parallel multi-agent discovery system with similar critique+validation pattern.
- [[plan-validate-execute|Plan-Validate-Execute]]: the broader HITL design pattern CodeMender's human-review step instantiates.
