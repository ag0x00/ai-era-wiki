---
type: paper
title: "AI Agents Are Here. So Are the Threats."
created: 2026-05-04
updated: 2026-08-21
tags:
  - papers
  - vendor-research
  - threat-research
  - agentic-ai
  - prompt-injection
  - tool-misuse
  - sandboxing
  - frameworks
status: summarized
year: 2025
authors: ["Jay Chen", "Royce Lu"]
venue: "Palo Alto Networks Unit 42 — Threat Research"
source_url: "https://unit42.paloaltonetworks.com/agentic-ai-threats/"
archived_copy: ".raw/articles/agentic-ai-threats-unit42-2025-05-01.md"
no_public_url: ""
key_claim: "Most agentic-AI vulnerabilities are framework-agnostic — they arise from insecure design, misconfiguration, and unsafe tool integration, not from flaws in CrewAI or AutoGen themselves. Demonstrated empirically by running nine identical attack scenarios across two functionally identical agent applications built on the two different frameworks."
methodology: "Built two functionally identical multi-agent investment-advisory applications — one in CrewAI, one in AutoGen — sharing the same instructions, models, and tools. Executed nine attack scenarios against each. Open-sourced the reference implementation as PaloAltoNetworks/stock_advisory_assistant on GitHub."
contradicts: []
supports:
  - "[[owasp-agentic-ai-top-10]]"
  - "[[indirect-prompt-injection]]"
  - "[[agent-sandboxing]]"
  - "[[supply-chain-security-for-agents]]"
related:
  - "[[palo-alto-networks]]"
  - "[[palo-alto-prisma-airs]]"
  - "[[crewai]]"
  - "[[autogen]]"
  - "[[unit-42-prompt-injection-observations]]"
  - "[[lethal-trifecta]]"
  - "[[mcp-security]]"
  - "[[guardrails-beyond-vibes-talk]]"
  - "[[hooking-coding-agents-with-cedar-talk]]"
  - "[[building-secure-agentic-systems-talk]]"
sources:
  - "https://unit42.paloaltonetworks.com/agentic-ai-threats/"
  - "[[.raw/articles/agentic-ai-threats-unit42-2025-05-01.md]]"
  - "https://github.com/PaloAltoNetworks/stock_advisory_assistant"
---

# AI Agents Are Here. So Are the Threats. (Unit 42, 2025-05-01)

**Source:** [Unit 42 — AI Agents Are Here. So Are the Threats.](https://unit42.paloaltonetworks.com/agentic-ai-threats/) (2025-05-01) by Jay Chen and Royce Lu. Local copy: `.raw/articles/agentic-ai-threats-unit42-2025-05-01.md`. Reference implementation: [PaloAltoNetworks/stock_advisory_assistant](https://github.com/PaloAltoNetworks/stock_advisory_assistant).

## Key Claim

**Most agentic-AI vulnerabilities are framework-agnostic.** Nine attack scenarios — covering information leakage, credential theft, tool exploitation, and remote code execution — succeed against functionally identical agent applications built on two different popular frameworks ([[crewai|CrewAI]] and [[autogen|AutoGen]]). The vulnerabilities arise from **insecure design patterns, misconfigurations, and unsafe tool integrations**, not from flaws in either framework. Mitigation requires layered defense-in-depth across prompts, runtime, tool integrations, and code execution sandboxes.

## Methodology

Built two functionally identical multi-agent investment-advisory applications:

- **Three cooperating agents per implementation**: orchestration agent (user interface, task delegation), news agent (search engine + web reader tools), stock agent (database + stock-data + Python code interpreter tools)
- **Identical instructions, models, and tool functionality** across both implementations — the only difference is the framework runtime ([[crewai|CrewAI]] vs [[autogen|AutoGen]])
- **Same nine attack payloads** executed against each, with framework-specific syntactic adjustments where the attack relies on framework-internal mechanisms (e.g. CrewAI's `delegate` vs AutoGen's `transfer_to_*`)
- **Reference implementation open-sourced** at `github.com/PaloAltoNetworks/stock_advisory_assistant` for reproducibility, with both vanilla and "reinforced" prompt variants

This is the first published study to systematically isolate **framework-agnostic** agentic vulnerabilities from framework-specific ones.

## The nine attack scenarios

Each scenario maps to one or more OWASP Agentic AI Threats categories. Reproducible via the open-source reference implementation.

| # | Attack | OWASP threats | Mitigations |
|---|---|---|---|
| 1 | **Identifying participant agents** — list all agents in the system | Prompt injection, intent breaking | Prompt hardening, content filtering |
| 2 | **Extracting agent instructions** — pull system prompts from each agent in the multi-agent group | Prompt injection, intent breaking, agent communication poisoning | Prompt hardening, content filtering |
| 3 | **Extracting agent tool schemas** — extract tool input/output schemas | Prompt injection, intent breaking, agent communication poisoning | Prompt hardening, content filtering |
| 4 | **Internal network access via web reader** — coerce the news agent's web-reader tool into fetching internal-network URLs | Prompt injection, tool misuse, communication poisoning | Prompt hardening, content filtering, **tool input sanitization** |
| 5 | **Sensitive data exfiltration via mounted volume** — read files from the code interpreter's mounted volume | Prompt injection, tool misuse, identity spoofing, RCE, communication poisoning | Prompt hardening, **code executor sandboxing**, content filtering |
| 6 | **Service account access token exfiltration via metadata service** — exfiltrate cloud service-account tokens via the metadata endpoint | Prompt injection, tool misuse, identity spoofing, RCE, communication poisoning | Prompt hardening, **code executor sandboxing**, content filtering |
| 7a | **SQL injection via database tool** — exfiltrate full database tables | Prompt injection, tool misuse, communication poisoning | Prompt hardening, **tool input sanitization**, **tool vulnerability scanning**, content filtering |
| 7b | **BOLA (Broken Object-Level Authorization) via database tool** — access another user's data by manipulating object references | Prompt injection, tool misuse, communication poisoning | **Tool vulnerability scanning** |
| 8 | **Indirect prompt injection via webpage** — leak the conversation history to an attacker via a malicious webpage retrieved by the news agent | Prompt injection, tool misuse, communication poisoning | Prompt hardening, content filtering |

The pattern is consistent: **all nine scenarios work the same way on both CrewAI and AutoGen** with only minor syntactic adjustments to the attack payloads.

## The five mitigation strategies (defense-in-depth)

Unit 42 explicitly states: **no single mitigation is sufficient**.

| Mitigation | What it covers | What it doesn't |
|---|---|---|
| **Prompt hardening** — narrowly-defined responsibilities; explicit prohibitions on disclosing instructions, coworkers, or tool schemas; constrained tool invocations | Raises the bar for prompt-only attacks | Bypassed by advanced injection techniques; insufficient on its own |
| **Content filtering** — inline inspection and blocking of agent inputs/outputs at runtime; vendor reference: [[palo-alto-prisma-airs\|Palo Alto Prisma AIRS]] | Tool schema extraction, tool misuse, memory manipulation, malicious code execution, sensitive data leakage, malicious URLs | Latency / cost; false positives / negatives |
| **Tool input sanitization** — type, format, range, and special-character validation at the tool boundary | SQL injection, command injection, SSRF via tool inputs | Doesn't catch logic flaws in the tool itself |
| **Tool vulnerability scanning** — SAST + DAST + SCA on every tool integrated into the agentic system | BOLA, dependency CVEs, insecure logic | Doesn't catch runtime misconfigurations |
| **Code executor sandboxing** — strict container runtime controls: restrict networking, limit mounted volumes (use tmpfs), drop Linux capabilities (CAP_NET_RAW, CAP_SYS_MODULE, CAP_SYS_ADMIN), block syscalls (kexec_load, mount, unmount, iopl, bpf), enforce resource quotas | Mounted-volume reads, metadata-service exfiltration, sandbox escape, cryptojacking | Default container configurations are insufficient — must be hardened |

These five strategies map directly onto the wiki's [[agentic-ai-security-reference-architecture|RA]] planes (Control / Runtime / Egress / Data / Observability) and the [[agentic-ai-security-cmm-2026|CMM]]'s D3 / D4 / D5 / D6 / D7 domains.

## Notable Findings

- **Framework-agnostic vulnerabilities dominate.** Nine identical attacks succeed across two architecturally distinct agent frameworks. The implication for the [[agentic-ai-security-cmm-2026|CMM]]: **L4 multi-tool red-team coverage** must include framework-portable test suites, not framework-specific ones. The reference implementation provides exactly such a suite.
- **Multi-agent communication poisoning is load-bearing.** 8 of 9 scenarios involve agent-to-agent communication poisoning as a contributing threat — the orchestrator's `delegate` (CrewAI) or `transfer_to_*` (AutoGen) mechanism is the weak link that lets attackers reach internal agents through the user-facing orchestrator. This **empirically confirms ASI07 / ASI08 / ASI10** (multi-agent threats from OWASP Agentic AI Top 10) as production-relevant, not just theoretical.
- **Code executor is the highest-impact attack surface.** Two of the nine scenarios (mounted volume, metadata service) become possible only because the stock agent has a code-interpreter tool. The code interpreter elevates impact from "information leakage" to "credential theft + cloud takeover." This validates the wiki's [[agent-sandboxing|agent-sandboxing]] practice page as a load-bearing CMM D4 control.
- **Tool boundaries are real boundaries.** SQL injection (#7a) and BOLA (#7b) are not new vulnerability classes — they're classic application-security flaws inherited by agent systems through tool integration. Tools that were "safe" when called by traditional code become exploitable when called by an LLM that can be prompted to construct adversarial inputs. This justifies the [[supply-chain-security-for-agents|supply-chain]] domain's emphasis on **scanning every tool integrated into the agentic system**.
- **Indirect prompt injection works as advertised.** Scenario #8 (conversation-history exfiltration via malicious webpage retrieved by the news agent) is a live demonstration of the [[indirect-prompt-injection|indirect prompt injection]] attack class. Combined with the [[unit-42-prompt-injection-observations|sister Unit 42 production-telemetry piece]] (March 2026), Unit 42 has now published both lab evidence (this article, May 2025) and production evidence (March 2026) for the same attack class.
- **Prisma AIRS is positioned as the runtime mitigation.** The article names [[palo-alto-prisma-airs|Palo Alto Prisma AIRS]] as the recommended runtime content-filter that catches tool schema extraction, tool misuse, memory manipulation, malicious code execution, sensitive data leakage, and malicious URLs. This is consistent with how Prisma AIRS is positioned in the wiki's [[agentic-ai-security-reference-architecture|RA]] (Runtime + Egress + Data + Observability planes).

## Strengths and Weaknesses

**Strengths:**

- **Reproducible.** The [open-source reference implementation](https://github.com/PaloAltoNetworks/stock_advisory_assistant) lets any team reproduce all nine attacks and test their own defenses.
- **Framework-agnostic by construction.** The two-framework methodology is the cleanest possible isolation of "framework flaw" vs "design flaw" — the two implementations share everything except the framework runtime.
- **Defense-in-depth honest about limits.** Unit 42 explicitly says no single mitigation is sufficient and shows the per-attack mitigation table that documents which strategies cover which scenarios.
- **Cloud-specific attacks included.** Scenarios #5 (mounted volume) and #6 (metadata service) are cloud-deployment-specific and reflect realistic attack paths against production agentic apps deployed on EC2 / GCE / Azure VM-class infrastructure.
- **Date.** This is May 2025 work — among the earliest systematic empirical studies of agentic-AI vulnerabilities. Predates Unprompted March 2026 by ~10 months.

**Weaknesses:**

- **Vendor positioning.** The article concludes by recommending Palo Alto Networks' Prisma AIRS as the canonical runtime mitigation. Substantively reasonable (Prisma AIRS does ship the relevant content-filter capabilities) but the reader should triangulate with vendor-neutral solutions ([[llamafirewall|LlamaFirewall]], [[lakera-guard|Lakera Guard]], NeMo Guardrails) before purchasing.
- **No multi-LLM-model evaluation.** All nine attacks are demonstrated against a single (unstated) underlying LLM. The [[1-8m-prompts-30-alerts-talk|Salesforce Rittinghouse]] and [[breaking-the-lethal-trifecta-talk|Stripe Bullen]] talks both show that attack success rate (ASR) varies meaningfully across model generations; the article doesn't discuss this.
- **No "reinforced prompts" empirical comparison.** The reference implementation includes both vanilla and "reinforced" prompt variants but the article doesn't quantify how much harder reinforced prompts make the attacks. This would be the empirical bound on how far "prompt hardening" alone can take you.
- **Scope limited to single-tenant.** The investment-advisory app is a single-tenant prototype; multi-tenant attack surfaces (cross-tenant agent contamination, tenant-id confusion) are not in scope.

## In the RA / CMM

- **CMM D7 L4 (multi-tool red-team coverage)** — the open-source reference implementation is a candidate for the **regression suite** slot alongside [[promptfoo|Promptfoo]], complementing [[pyrit|PyRIT]] (orchestration) + [[garak|Garak]] (probe library) + [[mindgard-cart|Mindgard CART]] (continuous CART). It's framework-aware in a way the others aren't.
- **CMM D4 (Runtime & Guardrails)** — empirically validates content filtering (L4) and code executor sandboxing (L5 — cited explicitly in the article's mitigation 5).
- **CMM D5 (Egress & Network)** — the metadata-service-token-exfiltration scenario (#6) is exactly the kind of cloud-instance-specific egress that [[smokescreen|Smokescreen]]-class controls were designed for; SSRF closure to internal IPs / metadata endpoints is a D5 baseline.
- **CMM D8 (Supply Chain & AI-BOM)** — tool vulnerability scanning (mitigation 4) is the article's framing of the supply-chain control.
- **RA Control plane** — agent-to-agent delegation requires explicit policy (per [[hooking-coding-agents-with-cedar-talk|Sondera Cedar harness]] for coding agents); this article's scenarios #2/#3 demonstrate the consequence of *not* having such policy in the multi-agent investment app.

## Relations

- **Supports** [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10 (ASI)]] — explicitly maps each scenario to OWASP Agentic AI Threats categories from the GenAI Security Project's Threats and Mitigations publication
- **Supports** [[indirect-prompt-injection|Indirect Prompt Injection]] — Scenario #8 is a live lab demonstration; pairs with [[unit-42-prompt-injection-observations|Unit 42 production-telemetry observations]] for evidence triangulation
- **Supports** [[agent-sandboxing|Agent Sandboxing]] practice page — Scenarios #5 + #6 are the canonical examples of code-executor sandbox failure modes
- **Supports** [[supply-chain-security-for-agents|Supply Chain Security for Agents]] — Scenarios #7a + #7b are tool-vulnerability-class attacks; tool input sanitization + scanning are the mitigations
- **Sister piece**: [[unit-42-prompt-injection-observations|Unit 42 In-the-Wild Prompt Injection Observations]] (March 2026) — production-telemetry observations of the same attack class. Together: lab + production evidence from the same vendor.
- **Architectural sibling**: [[guardrails-beyond-vibes-talk|Stripe Zhang/Shah talk on guardrails]] — Stripe's input-side defense program; same problem space, different vendor's answer.
- **Architectural sibling**: [[building-secure-agentic-systems-talk|Dropbox McMillin talk]] — defense-in-depth in a 19-agent / 73-tool fleet; complementary practitioner perspective.
