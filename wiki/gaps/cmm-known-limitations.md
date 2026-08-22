---
type: gap-analysis
title: "CMM Known Limitations (current state)"
created: 2026-05-06
updated: 2026-05-06
tags: [gaps, cmm, known-limitations, current-state]
status: developing
scope_axis:
  - sec-of-ai
target: "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[standards-validation-methodology-2026-05]]"
---

# CMM Known Limitations (current state)

Current-state limitations of [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — reviewed and restated 2026-05-06.

This page replaces §5 ("Risks and overclaims") of the older [[agentic-cmm-vs-standards-validation|Validation page]]. Three of the original seven items were addressed by CMM revisions during 2026-05; one was wrong (CSA ATF five-stage); the rest are restated below against the *current* CMM. As future revisions close items, archive them here with a `[!check]` note rather than silently deleting.

## Still-current limitations

### 1. `D5 L3` — combined MCP+A2A+LLM gateway treated as a settled standard

The clause requires "an agent-aware proxy / gateway between agent and external tools enforcing per-tool RBAC (AgentGateway in Linux Foundation, Solo Enterprise, Cloudflare AI Gateway, Kong AI Gateway, or equivalent); HTTPS / TLS 1.3 + OAuth/mTLS for inter-agent [[a2a-protocol|A2A v1.0]] communication per spec §7."

The [[a2a-protocol|A2A v1.0.0 spec]] (LF-governed since June 2025) covers transport (§7) and Agent Card signing (§8.4) but **not** message-level integrity, replay protection, or cryptographic agent identity. These remain vendor-side ([[multi-agent-runtime-security|Oktsec-class enforcement]]) or proposal-side. Treating the combined MCP+A2A+LLM proxy as a settled `L3` (org-wide standard) requirement is aggressive without an org-authored A2A enforcement profile — which the CMM does call for ("orgs MUST document their own A2A enforcement profile, including signing algorithm and replay-protection layering"), but the org-authored-profile burden is the limitation.

**Status:** [verified-current]. Needs primary-source recheck on A2A v1.0.0 spec when next reviewed.

### 2. `D4 L5+` — TEE-backed guardrail attestation has no auditor schema

The L5+ clause requires "cryptographic attestation that guardrails executed in a TEE (AWS Nitro Enclaves-class)." This was originally `D4 L5` and was moved to L5+ in the 2026-05-04 L5/L5+ split (acknowledging it's research-stage). The remaining concern: even at L5+, auditors evaluating "TEE attestation chain" will find no standard chain-of-custody schema to evaluate against. The claim is auditable in principle (an attestation log either exists or doesn't) but the chain-of-custody schema is org-authored.

**Status:** [verified-current], reduced impact (L5+ is explicitly aspirational).

### 3. `D2 L5` — Microsoft Agent 365 Registry "or equivalent" remains underspecified

The clause references "Microsoft Agent 365 Registry or equivalent unified governance." Agent 365 GA was 2026-05-01; deployment evidence is now possible but not yet published at scale. "Or equivalent" softens the dependency on a single vendor, but no other product offers the documented Agent 365 capability set, so "equivalent" is currently undefined in practice. A CISO at L5 needs to either pick Agent 365 or build the equivalent capability set themselves.

**Status:** [verified-current]. Re-check by 2026-Q3 once Agent 365 deployment evidence and competing-product feature parity are observable.

### 4. `D1 L5` — AIUC-1 quarterly cadence and single-auditor capacity

The clause requires "AIUC-1 certified." AIUC-1 updates quarterly (Q2 2026 update focused on MCP / third-party / agent identity per AIUC's own statements); a `L5` claim is implicitly "currently certified against the most recent quarterly refresh," which the CMM language doesn't quite articulate (the L5 row says "AIUC-1 certified against the most recent quarterly refresh OR ISO/IEC 42001 certified" + "most-recent cert dated within last quarter" in the auditor-evidence column — better than the original 2026-04-30 framing but still a moving target). Schellman is currently the only accredited auditor — single-auditor capacity is a real gating constraint for organizations attempting L5 certification.

**Status:** [verified-current], partly addressed by the "most-recent cert within last quarter" language. Capacity constraint is structural, not fixable in CMM language.

### 5. `D6 L3+` — `IDENTITY.md` / `SOUL.md` filename conventions are not industry-standard

The CMM mandates SHA-256 of `SOUL.md` / `IDENTITY.md` / system prompts as cognitive file integrity. These specific filenames are vault conventions inherited from this project; in practice agent identity files use a wider variety of names (`.cursorrules`, `Claude.md`, `claude.md`, `.augment-guidelines`, `system_prompt.txt`, vendor-specific paths). The 2026-05-06 verification overlay added the AIUC-1 B008.6 anchor for the underlying primitive (cryptographic checksums for tamper detection), but the *scoping* to identity files is the load-bearing CMM contribution, and the file-set being protected is not yet standardized.

**Status:** [new-2026-05-06]. The CFI primitive is sound; the file-discovery layer is the gap. Future CMM revision should describe a discovery rule ("any file the agent reads as system context at startup or per-session"), not a hardcoded filename list.

## Limitations addressed by CMM revisions (archived)

The following items appeared in §5 of the older validation page and have been resolved by CMM revisions during May 2026. Kept as historical record so future readers don't reintroduce them.

> [!check] `D3 L4` CSA ATF five-stage promotion gates (resolved 2026-05-06)
> Original concern: "CSA ATF five-stage promotion gates not yet fully specified in published guidance." Refuted by 2026-05-06 verification: ATF v0.9.1 has **four** maturity levels (Intern / Junior / Senior / Principal) with concrete promotion criteria (minimum time, accuracy thresholds, availability targets, named security validations, sign-off matrix). The CMM's `D3 L4` clause was rewritten 2026-05-06 to match the actual ATF v0.9.1 spec; only the Principal-tier hardware-bound identity / policy-as-code primitives remain abstract enough to need org-authored rubric. See the 2026-05-06 follow-up log entry for details.

> [!check] `D6 L5` provably bounded poisoning rate citing Nature Medicine 2024 0.001% (resolved 2026-05-04)
> Original concern: "A medical-imaging study's empirical threshold is not a transferable assurance bound for arbitrary RAG corpora." The 2026-05-04 CMM revision softened the language: "**documented poisoning-rate bound based on domain-appropriate empirical evidence** (the corpus owner sets the threshold and cites the supporting study; the Nature Medicine 2024 0.001% medical-imaging finding is one example, not a general bound)."

> [!check] `D7 L4` four red-team tools treated as interchangeable (resolved 2026-05-04)
> Original concern: "Promptfoo / Mindgard CART / PyRIT / Garak have very different scopes; treating them as interchangeable understates the work." The 2026-05-04 revision added category-distinct framing: "**distinct attack categories** — orchestration / multi-turn (PyRIT), probe library (Garak), regression suite (Promptfoo), and continuous CART (Mindgard CART or equivalent). Single-tool coverage is not L4."

## Contribution guide

When a future review surfaces a new CMM limitation:
1. Add a numbered subsection under **Still-current limitations** with the concrete CMM clause cited.
2. Tag with `[verified-current]` (primary-source-checked), `[wiki-summary]` (only summary checked), or `[new-YYYY-MM-DD]` (newly identified).
3. Recommend a fix or note why it's structural (not fixable in CMM language).
4. When a CMM revision resolves it, move the item to **Limitations addressed by CMM revisions** with a `[!check]` callout summarizing the resolution.

Per-standard reviews from the audit backlog ([[standards-validation-methodology-2026-05|Standards Validation Methodology]]) will likely surface additional CMM limitations as they execute. Those should be filed here as well.

## Relations

- Replaces: [[agentic-cmm-vs-standards-validation|Validation page]] §5 (which is now demoted to historical snapshot).
- Targets: [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]].
- Methodology: [[standards-validation-methodology-2026-05|Standards Validation Methodology]] for how new limitations are sourced and verified.
