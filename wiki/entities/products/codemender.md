---
type: entity
entity_type: product
title: "CodeMender (Google DeepMind)"
address: c-000036
created: 2026-05-13
updated: 2026-09-01
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
  - "[[autonomous-code-security-google-talk]]"
  - "[[four-flynn]]"
  - "[[vvah|VVAH]]"
  - "[[defending-code-harness|defending-code-harness]]"
  - "[[oss-ai-vuln-discovery-harness-landscape|OSS AI Vuln-Discovery Harness Landscape]]"
  - "[[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]]"
sources:
  - "https://deepmind.google/blog/introducing-codemender-an-ai-agent-for-code-security/"
  - "https://cloud.google.com/blog/products/identity-security/find-and-fix-software-vulnerabilities-with-codemender/"
  - "https://www.anthropic.com/glasswing"
  - "https://unpromptedcon.org/abstract-march2026/"
  - ".raw/talks/2026-03-03_Heather-Adkins-and-Four-Flynn_Evaluating-Threats-Automating-Defense_transcript.md"
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
---

# CodeMender (Google DeepMind)

**Sources:** [Google DeepMind — Introducing CodeMender (Oct 2025)](https://deepmind.google/blog/introducing-codemender-an-ai-agent-for-code-security/) · [Google Cloud — CodeMender in preview (Jul 2026)](https://cloud.google.com/blog/products/identity-security/find-and-fix-software-vulnerabilities-with-codemender/) · [product documentation](https://docs.cloud.google.com/gemini-enterprise-agent-platform/codemender) · [[google-codemender-deepmind|research source-summary]] · [[google-cloud-codemender-preview|preview source-summary]]

**CodeMender** is Google's AI agent for *patching* software vulnerabilities, the patching-side counterpart to [[big-sleep|Big Sleep]]'s discovery. DeepMind announced it as research on October 6, 2025; Google Cloud placed it in preview as a managed product on July 21, 2026. The research agent operates **reactively** (patching newly-found vulnerabilities) and **proactively** (rewriting existing code to eliminate entire vulnerability classes). By the 2025 announcement date, the team had upstreamed **72 security patches** to OSS projects, including codebases as large as **4.5 million lines of code**. The October 2025 announcement stated that human researchers review every patch before submission. Google described the gate differently five months later: a pluggable stack of verifiers decides which candidate patches survive, and Flynn stated the design intent as an engine that "runs completely without human intervention", naming no reviewer at submission.[^google-talk] The July 2026 product keeps a person in the loop, with developers retaining control before anything is committed. The research programme's stated goal and the shipped product's control differ; the wiki has no source describing a single review step moving between them.

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

Google grouped the validators in four on the March 2026 deck: dynamic analysis (fuzzing, sanitizers), static analysis (AST-based checks, formal verification), differential testing, and LLM judges and critics. The set is pluggable, and Flynn located the programme's differentiator there rather than in patch generation — "some of the secret sauce of what we've been building is actually in these verification stages." The agent draws multiple samples to produce several candidate patches. Code is fuzzed before and after the patch to establish that functionality survived, formal verification attempts to prove the patched section functionally equivalent, and a further round of fuzzing replays the malicious input to confirm the vulnerability is gone. When no candidate clears the stack, the validation failures are fed back into the model's context, and the agent produces a fresh set that is validated and ranked for submission in turn.[^google-talk]

The 2025 announcement set four quality dimensions a patch clears before it reaches human review: fixes the *root cause* rather than the symptom, functionally correct, no regressions, follows style guidelines. Flynn restated the bar as three requirements in March 2026 — the patch fixes the security vulnerability, it does not break functionality, and it honours the idioms of the developer who wrote the code so the diff is easy to digest.[^google-talk]

## Two Operating Modes

### Reactive: patch a newly-discovered vulnerability

CodeMender debugs the root cause and devises a patch. Two examples from the announcement:

- **Heap buffer overflow** where the actual problem was "incorrect stack management of XML elements during parsing." The agent identified that the crash report was misleading and located the true defect.
- **Non-trivial patch dealing with complex object lifetime issues**, requiring modification of a custom C-code generator inside the project.

### Proactive: rewrite existing code to eliminate vulnerability classes

Worked example: applying `-fbounds-safety` annotations to **libwebp** (a widely-used image compression library). Once applied, the compiler adds bounds checks that would have rendered **CVE-2023-4863** (the libwebp zero-click iOS exploit used in BLASTPASS / NSO Group operations) "unexploitable forever."

This is the highest-value mode: patching one vulnerability stops one exploit; rewriting a vulnerability class stops a category of exploits.

## Results

### October 2025 research announcement

- **72 security patches upstreamed** to OSS projects in the 6 months prior to announcement.
- Target codebases include some as large as **4.5 million lines of code**.
- "Many of [the patches] have already been accepted and upstreamed."
- Human researchers reviewed every patch before submission.

### March 2026 conference figures

Flynn reported **178 autonomously generated fixes** landed in open source, which the deck splits **48 patched and 130 hardening**.[^google-talk] Most of the shipped volume is proactive rewriting that removes a vulnerability class rather than reactive repair of a reported bug. libwebp is the named worked example of hardening a critical library, and the internal Chrome work is described as automatically generated patches that harden pointers in the codebase.

The two counts do not form a series. 48 is fewer than the 72 upstreamed by October 2025, so they rest on different bases and this page reports them separately rather than as a trajectory.

## Google Cloud preview (July 2026)

Google Cloud moved CodeMender from vendor-internal research to a managed, customer-facing agent in preview on 2026-07-21 ([[google-cloud-codemender-preview|source summary]]). The product pipeline is scan, verify, remediate:

| Stage | Function |
|---|---|
| **Scan** | Reasons over repository context rather than matching patterns; targets memory corruption, injection, web security issues, cryptographic flaws, insecure data handling. Languages: C/C++, Go, Java, Python, Ruby, Rust, TypeScript |
| **Verify** | Builds and runs proof-of-concept exploits in a customer-managed sandbox to establish that a finding is reachable before it is patched |
| **Remediate** | Generates a patch, checks it with an [[llm-as-a-judge\|LLM-as-a-judge]] for functional disruption, and delivers a code diff for developer approval |

The verify stage is new relative to the research description. It repurposes [[autonomous-exploit-generation|proof-of-concept exploit construction]] as a triage control: exploitability decides whether a finding warrants a patch and where it ranks.

The product's stated scan scope is wider than the scope the research programme reports results for. Asked at [un]prompted in March 2026 about business-logic vulnerabilities, Adkins placed the research on infrastructure components that handle untrusted input — she named V8 and FFmpeg — and not on business applications.[^google-talk] Nothing published reconciles the two, so a buyer evaluating the product against a business application has no figure that covers their case: the zero-false-positive rate and the 178-fix count are both drawn from the narrower population. See [[autonomous-code-security-google-talk|the talk summary]] for the exchange.

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

The pattern now runs outside the coalition as well as inside it. Semgrep's July 2026 survey records the same validation shape in open-source harnesses under permissive licences: [[vvah|VVAH]] scores a proposed fix with an agentic panel of a security architect, a pen-tester and a cross-repo analyzer, and [[defending-code-harness|defending-code-harness]] verifies a patch on a four-tier ladder running compiles, proof-of-concept stops, tests pass, survives re-attack.[^semgrep] The survey also measures how uncommon the patching capability remains — of the five harnesses it tables on execution, proof-of-concept and patch output, three generate a patch and two generate none, with only one of the three verifying its patch by execution.

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-2026|CMM]] D6 (Data, Memory & RAG) L5+** — proactive rewriting of vulnerable data-handling code (libwebp, XML parsers) is a D6-adjacent primitive.
- **[[agentic-ai-security-cmm-2026|CMM]] D8 (Supply Chain & AI-BOM) L5+** — upstreaming patches to OSS at the 4.5M-LOC scale is a supply-chain hardening primitive.
- **[[agentic-ai-security-cmm-2026|CMM]] D9 (Operations & Human Factors)** — the human-review control is described differently at each stage of the programme's public account. October 2025 placed it at patch approval, which is [[plan-validate-execute|Plan-Validate-Execute]] applied to autonomous patch generation. March 2026 presented verification as the gate and stated full autonomy as the design intent, naming no reviewer at submission.[^google-talk] The July 2026 product keeps developer approval before anything is committed. The mapping holds at each stage; the wiki has no source describing one control moving between them.
- **[[agentic-ai-security-reference-architecture|RA]] Observability Plane** — patch validation extends agent-output auditing.

## Open Questions

- **Efficacy at preview**: the Google Cloud launch publishes no recall, precision, false-positive, or patch-count data. The March 2026 conference figures cover the research programme's open-source output, not the product, and are activity counts rather than efficacy measurements.[^google-talk] The claim that exploit simulation cuts false positives in the shipped pipeline stays unevidenced.
- **Maintainer acceptance rate**: neither the 72 patches upstreamed by October 2025 nor the 178 fixes reported in March 2026 carry an accepted-versus-rejected breakdown.
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
- [[autonomous-code-security-google-talk|Autonomous Code Security at Google]]: March 2026 talk disclosing the verifier stack and the 178-fix output.

[^google-talk]: Heather Adkins and Four Flynn, *Evaluating Threats & Automating Defense: How Google is Advancing Code Security*, [\[un\]prompted, San Francisco](https://www.youtube.com/watch?v=B_7RpP90rUk) (2026-03-03): Big Sleep at zero false positives end-to-end on deep memory-safety bugs, with a working exploit built as proof of vulnerability; CodeMender at 178 open-source fixes, 48 patched and 130 hardening; verification presented as the gate, and full autonomy stated as the design intent. See [[autonomous-code-security-google-talk|the talk summary]].
[^semgrep]: [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses), July 2026 (no day-level date exposed; author not named). The execution/PoC/patch table (three of five generate a patch) is human-written; the VVAH panel composition and the defending-code-harness tier ladder are from Semgrep's LLM-generated repository summaries. Summarized at [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].
