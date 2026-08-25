---
type: paper
title: "CodeMender Preview on Google Cloud"
address: c-000236
created: 2026-07-26
updated: 2026-08-24
tags:
  - papers
  - google
  - google-cloud
  - codemender
  - wiz
  - vuln-patching
  - ai-vuln-discovery
  - vulnops
  - ai-in-sec-defense
status: summarized
origin: aggregated
scope_axis:
  - ai-in-sec-defense
publication_date: 2026-07-21
authors:
  - Michael Gerstenhaber
  - Clemens Viernickel
publisher: "Google Cloud"
source_url: https://cloud.google.com/blog/products/identity-security/find-and-fix-software-vulnerabilities-with-codemender/
archived_copy: ".raw/articles/google-cloud-codemender-preview-2026-07-21.md"
related:
  - "[[codemender]]"
  - "[[google-codemender-deepmind]]"
  - "[[google]]"
  - "[[wiz]]"
  - "[[wiz-ai-spm]]"
  - "[[big-sleep]]"
  - "[[vulnops]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[codex-security]]"
  - "[[claude-code-security]]"
  - "[[llm-as-a-judge]]"
  - "[[autonomous-exploit-generation]]"
  - "[[agentic-soc-ra-exposure-vulnops]]"
  - "[[autonomous-code-security-google-talk]]"
sources:
  - "https://cloud.google.com/blog/products/identity-security/find-and-fix-software-vulnerabilities-with-codemender/"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/codemender"
  - ".raw/talks/2026-03-03_Heather-Adkins-and-Four-Flynn_Evaluating-Threats-Automating-Defense_transcript.md"
---

# CodeMender Preview on Google Cloud

**Source:** [Now in Preview: Find and Fix Software Vulnerabilities with CodeMender (Google Cloud, 2026-07-21)](https://cloud.google.com/blog/products/identity-security/find-and-fix-software-vulnerabilities-with-codemender/) · archived extract at `.raw/articles/google-cloud-codemender-preview-2026-07-21.md`

Google Cloud placed [[codemender|CodeMender]] in preview as a managed code security agent on 2026-07-21, nine months after [[google-codemender-deepmind|the DeepMind research announcement]]. The post is a product launch: it defines a three-stage pipeline, three purchasing paths, and an enterprise data-handling posture, and publishes no efficacy figures of any kind.

The change is in availability class. CodeMender was a vendor-internal research agent whose output reached the outside world only as upstreamed open-source patches. It is now a product an enterprise can point at its own repositories.

## Pipeline

The product is organized as scan, verify, remediate.

| Stage | Function | Stated mechanism |
|---|---|---|
| **Scan** | Find candidate vulnerabilities | Reasons over the repository's context, goals, and functionality rather than matching patterns; targets memory corruption, injection, web security issues, cryptographic flaws, and insecure data handling. Languages: C/C++, Go, Java, Python, Ruby, Rust, TypeScript |
| **Verify** | Establish exploitability | Builds and runs proof-of-concept exploits in a sandbox the customer operates, to confirm a finding is reachable before a fix is written |
| **Remediate** | Produce and check a patch | Generates a patch, then applies an [[llm-as-a-judge\|LLM-as-a-judge]] check that the change does not disrupt existing functionality; output is a code diff for developer review |

The verify stage is the load-bearing addition relative to the 2025 research description. Google frames [[autonomous-exploit-generation|proof-of-concept exploit construction]] as a triage control: exploitability determines whether a finding is worth a patch and how it ranks. That inverts the usual role of exploit generation on this wiki, where it is catalogued as offensive capability.

Human approval is retained at the commit boundary. Developers receive diffs and decide; the agent does not merge. This matches the human-review gate in the research announcement and the approval gating in [[codex-security|Codex Security]] and [[claude-code-security|Claude Code Security]].

## Access paths

| Path | Terms |
|---|---|
| **Gemini Enterprise Agent Platform** | Preview, using generally available Gemini models |
| **AI Threat Defense** | CodeMender as a core component, orchestrated by [[wiz\|Wiz]] |
| **Gemini 3.5 Flash Cyber** | Restricted to "a small set of governments and trusted partners," with access planned to widen |

Model choice is explicit: customers select a model to trade cost, speed, and scanning depth against each other, and Google states support for third-party frontier models is planned for later in 2026. The 2025 research agent was described as running on Gemini Deep Think. The product is model-plural, and Google now describes the harness, rather than the reasoner, as the durable asset; it states the harness is kept "continuously updated with the latest Google DeepMind research."

The restricted path gates a cyber-specialized model behind government and trusted-partner status while the same agent ships on generally available models. That creates two capability speeds inside one product.

## Wiz orchestration

The Wiz path is the first published detail of how [[wiz|Wiz]] and Google Cloud security products compose after the acquisition. Within AI Threat Defense, Wiz "orchestrates agentic application security": it calls CodeMender to scan, enriches findings in the Wiz Security Graph with deployment context, and triggers [[wiz|Wiz Red Agent]] for AI pentesting. A **Wiz Green Agent** then directs CodeMender to generate and test patches carrying that application context.

Red Agent and Green Agent form a paired offense-and-repair loop over a shared asset graph. Red Agent is an Opus-powered continuous pentester; Green Agent is named here for the first time. Deployment context from the graph distinguishes this path from repository-only scanning: reachability in production becomes an input to prioritization, alongside reachability in source.

## Enterprise posture

Google states traffic routes through the customer's VPC, data is isolated and encrypted, and source code is subject to "zero retention." Sandboxes for exploit execution are customer-managed. Integration points named are CI/CD workflows, VS Code, and Antigravity; the stated scope covers first-party, open-source, and third-party code.

Salesforce, Robinhood, and Palo Alto Networks are quoted. Robinhood's Head of Security Operations states CodeMender found critical vulnerabilities that other AI-enabled tools missed. Palo Alto's quoted engineer calls it "genuinely ambitious about closing the loop from detection to fix." All three quotes are qualitative; none carries a number.

## Omissions

> [!gap] No efficacy data at preview
> The post publishes no recall or precision figures, no false-positive rate, no patch counts, no CVE counts, and no customer-reported metrics. Peer announcements on this axis led with numbers — [[codex-security|Codex Security]] with 92% recall on internal golden repos, [[wiz|Wiz Red Agent]] with a zero-false-positive claim across 150,000+ weekly assets, [[anthropic-frontier-red-team-vuln-research|Anthropic's Frontier Red Team]] with disclosure-funnel counts. CodeMender's own 2025 research announcement carried 72 upstreamed patches, and Google gave a further open-source count on a conference stage four months before this launch — 178 fixes, split 48 patched and 130 hardening.[^google-talk] The preview post drops that register entirely. The omission is product-scoped: activity counts exist for the research programme's open-source output and none for the shipped pipeline. Until Google publishes verification data, the exploit-simulation claim — that verify materially cuts false positives — is unevidenced on this wiki.

Pricing and a GA date are also unstated. The post does not mention [[big-sleep|Big Sleep]], the 72 upstreamed patches, or the libwebp `-fbounds-safety` work, so the relationship between the research agent's proactive class-elimination mode and the product's three stages is undocumented. Nothing in the product description corresponds to proactive rewriting of code to eliminate vulnerability classes; the shipped pipeline is reactive. The March 2026 figures measure what that omission leaves out: 130 of the research programme's 178 open-source fixes are hardening, so the product ships the smaller half of what the research agent does.

## Significance

This is the discovery-to-remediation loop sold as managed infrastructure. [[vulnops|VulnOps]] argues that the constraint has moved from finding vulnerabilities to verifying, prioritizing, and fixing them, and that enterprises need a standing function to absorb the volume. CodeMender's preview packages scan, exploit verification, and patch generation as one procurement, with the human decision point at diff review.

The pattern across vendors is now consistent. [[codex-security|Codex Security]], [[claude-code-security|Claude Code Security]], and CodeMender all run reason-over-code discovery, sandboxed validation, and patch generation under human approval, and all reject rule-based static analysis as the framing. Sandboxed validation, once a differentiator, is now a baseline. What differs is the surrounding estate: of the three, CodeMender alone is published as composing with a CNAPP asset graph and an offensive agent on the same platform. That claim reflects what the vendors have documented rather than what they have built — OpenAI and Anthropic may hold comparable integrations that no announcement describes.

## See Also

- [[codemender|CodeMender]] — product page.
- [[google-codemender-deepmind|CodeMender: AI Agent for Code Security]] — the 2025 research announcement this supersedes on availability.
- [[wiz|Wiz]] — orchestration layer in the AI Threat Defense path.
- [[vulnops|VulnOps]] — the function this product is sold into.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — wiki thesis.
- [[agentic-soc-ra-exposure-vulnops|Exposure & VulnOps]] — agentic SOC function this tooling serves.
- [[autonomous-code-security-google-talk|Autonomous Code Security at Google]] — March 2026 talk giving the 178-fix research-programme figures this launch omits.

[^google-talk]: Heather Adkins and Four Flynn, *Evaluating Threats & Automating Defense: How Google is Advancing Code Security*, [\[un\]prompted, San Francisco](https://www.youtube.com/watch?v=B_7RpP90rUk) (2026-03-03): Big Sleep at zero false positives end-to-end on deep memory-safety bugs, with a working exploit built as proof of vulnerability; CodeMender at 178 open-source fixes, 48 patched and 130 hardening; verification presented as the gate, and full autonomy stated as the design intent. See [[autonomous-code-security-google-talk|the talk summary]].
