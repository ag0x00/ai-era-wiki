---
type: paper
title: "AI Security Standards: Agentic Threats Outpace Frameworks"
created: 2026-04-30
updated: 2026-08-22
tags:
  - papers
  - ai-security
  - frameworks
  - agentic-ai
  - standards
  - q1-2026
status: summarized
scope_axis:
  - sec-of-ai
year: 2026
authors: []
venue: "Internal research / working paper"
source_url: ""
archived_copy: ".raw/papers/ai-security-standards-in-q1-2026.md"
no_public_url: "original research — wiki-internal working paper, not yet published externally"
key_claim: "Coverage of agentic AI risks now exists across multiple frameworks, but enforcement is absent — no framework mandates platform-level security, verifiable agent identity, or testable AI-BOM; the open-source ecosystem (LlamaFirewall, AgentGateway, credential proxies) has outpaced standards bodies in delivering working controls."
methodology: "Structured framework-by-framework gap analysis covering NIST AI RMF, ISO/IEC 42001, MITRE ATLAS, OWASP, Google SAIF/CoSAI, and Microsoft RAI; cross-mapped against the OWASP ASI Top 10 agentic risk taxonomy; supplemented with threat landscape survey of Q1 2026 incidents (ClawHavoc, SANDWORM_MODE, Meta Sev 1, MCP CVEs) and emerging control architecture case studies."
contradicts: []
supports:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agent-observability]]"
related:
  - "[[nist-ai-rmf]]"
  - "[[mitre-atlas]]"
  - "[[owasp-llm-top-10]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[iso-iec-42001]]"
  - "[[google-saif]]"
  - "[[cosai]]"
  - "[[microsoft-rai]]"
  - "[[eu-ai-act]]"
  - "[[csa-maestro]]"
  - "[[owasp-aivss]]"
  - "[[nist-ai-600-1]]"
  - "[[standards-review-saif-cosai-2026-Q2]]"
  - "[[standards-review-iso-42001-27090-2026-Q2]]"
  - "[[threat-modeling-for-ai]]"
  - "[[threat-taxonomy-reconciliation]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# AI Security Standards in Q1 2026: Agentic Threats Outpace Frameworks

**Source:** wiki-internal working paper (no external URL). Local copy: `.raw/papers/ai-security-standards-in-q1-2026.md`.

## Key Claim

Q1 2026 delivered unprecedented framework productivity — OWASP ASI Top 10, MITRE ATLAS 84 techniques, Microsoft ZT4AI 700+ controls, CoSAI MCP taxonomy — but **enforcement remains entirely absent**. No framework mandates platform-level security enforcement, verifiable agent identity, or a validated AI-BOM. Enterprises that are fully compliant with every published standard can still rely entirely on bypassable prompt-level guardrails. The open-source ecosystem (LlamaFirewall, AgentGateway, credential proxies) has answered questions that standards bodies have not yet asked.

## Methodology

- Framework-by-framework structured analysis of: [[nist-ai-rmf|NIST AI RMF]], [[iso-iec-42001|ISO/IEC 42001]], [[mitre-atlas|MITRE ATLAS]], [[owasp-llm-top-10|OWASP LLM Top 10]], [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10]], [[google-saif|Google SAIF]], [[cosai|CoSAI]], [[microsoft-rai|Microsoft RAI]], [[csa-maestro|CSA ATF/MAESTRO]]
- Cross-mapping of all six frameworks against the 10 OWASP ASI categories to identify coverage gaps
- Q1 2026 threat landscape survey covering confirmed incidents, CVEs, and production attack observations
- Emerging control architecture analysis (credential proxies, LlamaFirewall, AgentGateway, FIDES)

## Notable Findings

### Framework activity (Q1 2026)

- **NIST** published AI 800-3, AI 800-4, IR 8605A (COSAiS annotated outline), and launched **CAISI** (February 17, 2026) — the first U.S. government program explicitly targeting agentic AI standards. AI RMF 1.0 remains unchanged; no 2.0 draft exists.
- **MITRE ATLAS** jumped from 66 to **84 techniques across 16 tactics** in two quarterly updates (v5.3.0 January, v5.4.0 February). New agentic techniques include "Publish Poisoned AI Agent Tool" and "Escape to Host."
- **OWASP** published the [[owasp-agentic-ai-top-10|Agentic Applications Top 10]] (ASI Top 10, December 2025), AIVSS v0.8 (March 19, 2026 — extends CVSS 4.0 with agentic amplification factors), and a Practical Guide for Secure MCP Server Development.
- **CoSAI** published Model Context Protocol (MCP) Security (2026-01-20), Agentic Identity and Access Management (2026-04-17), Principles for Secure-by-Design Agentic Systems, and onboarded Meta as a Premier Sponsor. Now 40+ industry partners. The deliverable dates and titles were reconciled in [[standards-review-saif-cosai-2026-Q2|the 2026-Q2 SAIF/CoSAI standards review]], which also flagged the "40 threats / 12 categories" MCP figure as not re-verifiable.
- **Microsoft** announced Zero Trust for AI (ZT4AI, 700+ controls), Agent 365 unified governance control plane (\$15/user/month), and direct OWASP ASI Top 10 mapping to Copilot Studio.
- **ISO/IEC 42001** unchanged; companion **ISO/IEC 42006:2025** (audit body requirements) finalized; **ISO/IEC 27090** (AI cybersecurity guidance) entered FDIS ballot March 2026 — still unpublished and guidance-only as of June 2026, per [[standards-review-iso-42001-27090-2026-Q2|the 2026-Q2 ISO/IEC 42001 + 27090 review]].
- **CSA** launched the Agentic Trust Framework (February 2, 2026) with five progressive autonomy promotion gates.

### Threat landscape (Q1 2026)

- **ClawHavoc** (January–February 2026): 1,184+ malicious skills uploaded to OpenClaw marketplace; first large-scale AI agent supply chain attack. Payload: Atomic macOS Stealer.
- **SANDWORM_MODE** (February 20): npm worm injecting malicious MCP servers into Claude Code, Cursor, and VS Code via AI toolchain poisoning.
- **Meta Sev 1** (March 18): autonomous AI agent breach — first confirmed enterprise-grade agentic incident. Proprietary code and business strategies exposed for 2 hours.
- **30+ MCP CVEs in 60 days**: 82% of 2,614 surveyed MCP implementations vulnerable to path traversal; 66% to code injection. *Superseded against the primary:* the figures originate with [Endor Labs](https://www.endorlabs.com/learn/classic-vulnerabilities-meet-ai-infrastructure-why-mcp-needs-appsec) (2026-01-23), which reports 82% *using file-system operations prone to* path traversal and 67% using code-injection APIs — an API-usage rate rather than a confirmed-vulnerability rate. See [[mcp-exposure-measurements|MCP Exposure Measurements]].
- **Memory poisoning confirmed in the wild**: Microsoft found 50+ examples of hidden memory manipulation instructions embedded in "Summarize with AI" buttons across 31 companies.
- **Indirect prompt injection in production**: Unit 42 documented 22 distinct techniques from live telemetry (March 3, 2026).

### Comparative gap analysis (OWASP ASI coverage matrix)

> [!note] ASI05/ASI09 were renamed in the published Dec-2025 ASI release (RCE; Human-Agent Trust Exploitation). Those two rows are re-scored against the corrected taxonomy per [[standards-review-owasp-agentic-aivss-2026-Q2|the 2026-Q2 OWASP review]]; the rest reflect the Q1-2026 source snapshot.

| ASI Category | NIST AI RMF | ISO 42001 | MITRE ATLAS | OWASP ASI | CoSAI | Microsoft ZT4AI | CSA ATF |
|---|---|---|---|---|---|---|---|
| ASI01: Agent Goal Hijack | ○ | ○ | ● | ● | ◐ | ● | ◐ |
| ASI02: Tool Misuse | ○ | ○ | ● | ● | ◐ | ◐ | ◐ |
| ASI03: Identity & Privilege | ○ | ○ | ◐ | ● | ◐ | ● | ● |
| ASI04: Supply Chain | ◐ | ○ | ● | ● | ● | ◐ | ○ |
| ASI05: Unexpected Code Execution (RCE) | ○ | ○ | ● | ● | ○ | ◐ | ◐ |
| ASI06: Memory Poisoning | ○ | ○ | ● | ● | ◐ | ◐ | ○ |
| ASI07: Insecure Inter-Agent | ○ | ○ | ◐ | ● | ● | ◐ | ◐ |
| ASI08: Cascading Failures | ○ | ○ | ○ | ● | ◐ | ◐ | ◐ |
| ASI09: Human-Agent Trust Exploitation | ● | ○ | ○ | ● | ○ | ◐ | ○ |
| ASI10: Rogue Agents | ○ | ○ | ◐ | ● | ◐ | ● | ● |

● = Specific controls or techniques documented | ◐ = Partial/conceptual coverage | ○ = No meaningful coverage

**Key finding:** Only OWASP ASI achieves full coverage, but as risk descriptions rather than enforceable controls. Microsoft ZT4AI has deepest control implementation but is Azure-ecosystem-locked. NIST, ISO, and CSA ATF have the weakest coverage of agentic-specific risk categories (ASI06–ASI08 in particular).

### Structural gaps across all frameworks

1. **Platform-level vs. prompt-level enforcement** — The most critical architectural blind spot. No framework distinguishes or mandates enforcement at the platform layer (below the model). Prompt-level guardrails are definitionally bypassable by prompt injection. Production implementations (Google ADK `before_model_callback`, LlamaFirewall, AgentGateway) demonstrate the pattern; no standard requires it.

2. **Agent identity** — Only 22% of organizations treat AI agents as independent, identity-bearing entities (Gravitee, February 2026). Machine-to-human identity ratio is 82:1 (CyberArk, 2025). No published standard defines minimum identity requirements. NIST CAISI's Agent Identity and Authorization Concept Paper acknowledges the gap but has no shipped guidance. Okta for AI Agents GA: April 30, 2026.

3. **AI-BOM pre-standardization** — No ratified standard. Parallel efforts: OWASP AIBOM Generator (CycloneDX), SPDX 3.0 AI/ML extensions, IBM Granite 4.0 disclosures. EU AI Act Article 11/Annex IV effectively mandates AI-BOM-like documentation for high-risk systems but no standard defines what that means.

4. **Proof-of-guardrail / attestation** — Concept of cryptographic attestation that guardrails actually executed (via TEE) is entirely unaddressed by any framework. LlamaFirewall and Miggo Security are closest production approximations.

5. **Cognitive file integrity** — SHA-256 baselines for agent behavioral definition files (SOUL.md, IDENTITY.md) is an emerging practice with no framework coverage or tooling equivalent.

### Emerging control architectures with production evidence

- **Credential proxies** (Keychains.dev, AgentKeys, AgentSecrets): agent never possesses credentials; proxy injects at network layer. Provides prompt-injection resistance, least privilege, and audit trail.
- **LlamaFirewall** (Meta, open-source): PromptGuard 2 (97.5% recall, 1% FPR) + AlignmentCheck + CodeShield. >90% attack reduction on AgentDojo benchmark; sub-100ms latency for 90% of inputs. In production at Meta.
- **AgentGateway** (Solo.io → Linux Foundation): Rust-based proxy with RBAC/JWT/TLS/CEL policies for MCP, A2A, and LLM routing.
- **FIDES** (Microsoft research): zero successful prompt injection attacks using information-flow control with dynamic taint-tracking on AgentDojo benchmark — strongest published defensive result.
- **Agent 365** (Microsoft, GA May 1, 2026): first commercial unified agent governance control plane.
- **Okta for AI Agents** (GA April 30, 2026): agent lifecycle governance with shadow AI discovery.

## Strengths and Weaknesses

**What this source does well:**
- Comprehensive cross-framework coverage matrix against OWASP ASI taxonomy is the clearest published comparison of framework gaps
- Incident timeline is specific and verifiable (CVE numbers, dates, affected packages)
- Control architecture section grounds recommendations in production implementations with benchmark data

**Limitations and caveats:**
- Several statistics from the baseline could not be re-verified from primary sources: "48% ML-BOM adoption lag," "84% agent compliance audit failure" (attributed to CSA), "42,000 exposed agentic AI instances" (BitSight figure disputed)
- MITRE ATLAS version numbers (v5.3.0, v5.4.0) and exact technique counts sourced primarily from Vectra AI (commercial vendor) rather than MITRE's official changelog — independently verify at atlas.mitre.org
- The 82:1 machine-to-human identity ratio is correctly attributed to CyberArk, not Okta (corrects the January 2026 baseline)

## Relations

- Supports: [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — this paper is one of the foundational sources informing the canonical CMM and its [[agentic-ai-security-cmm-crosswalk|standards crosswalk]]; the framework gap analysis here directly motivated the CMM's cumulative-level rule and the platform-vs-prompt enforcement inflection at end of Phase 2.
- Supports: [[threat-modeling-for-ai|Threat Modeling for AI]] — the platform-vs-prompt enforcement doctrine framed here is the spine's lead design principle; the framework-coverage gap matrix is the standards-side complement to the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix.
- Supports: [[agent-observability|Agent Observability]] — platform-level enforcement section directly validates the glass-box paradigm and OTel/lifecycle hooks approach
- Supports: [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]] and [[agent-identity-architecture|AI Agent Identity Architecture]] — confirms these as production patterns at scale
- See also: [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]], [[nist-ai-rmf|NIST AI Risk Management Framework (AI RMF)]], [[mitre-atlas|MITRE ATLAS]], [[iso-iec-42001|ISO/IEC 42001 — AI Management Systems]], [[google-saif|Google SAIF — Secure AI Framework]], [[cosai|CoSAI — Coalition for Secure AI]], [[microsoft-rai|Microsoft Responsible AI Standard (RAI)]], [[eu-ai-act|EU AI Act]], [[csa-maestro|CSA MAESTRO / CSA Agentic Trust Framework]], [[owasp-aivss|OWASP AI Vulnerability Scoring System (AIVSS)]]
