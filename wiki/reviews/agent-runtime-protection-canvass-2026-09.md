---
type: review
title: "Agent Runtime Protection Market Canvass"
address: c-862722
created: 2026-09-16
updated: 2026-09-16
tags:
  - reviews
  - cmm
  - d4
  - guardrails
  - guardian-agent
  - vendors
  - runtime-protection
status: developing
scope_axis:
  - sec-of-ai
origin: produced
target: "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
methodology: "[[agentic-ai-security-cmm-recalibration-method-2026]]"
reviewer: "wiki (pass of 2026-09-16)"
review_completed: 2026-09-16
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[guardian-agent]]"
  - "[[guardian-agents-market-guide]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[oversight-layer]]"
  - "[[securing-agentic-coding]]"
  - "[[llamafirewall]]"
  - "[[chain-of-thought-monitorability]]"
  - "[[cmm-known-limitations]]"
  - "[[cmm-stress-test-canadian-fi-google-2026-09]]"
  - "[[onyx-platform]]"
  - "[[falcon-guardian]]"
  - "[[zenity]]"
  - "[[palo-alto-prisma-airs]]"
  - "[[lakera-guard]]"
  - "[[cyera-agent-guardian-release]]"
sources:
  - "https://onyx.security/platform/ai-security"
  - "https://docs.neuraltrust.ai"
  - "https://zenity.io/blog/closing-the-guardrail-gap-runtime-protection-for-openai-agentkit"
  - "https://noma.security/blog"
  - "https://straiker.ai/products/defend-ai"
  - "https://invariantlabs-ai.github.io/docs/mcp-scan/guardrails-reference/"
  - "https://invariantlabs.ai/guardrails"
  - "https://docs.hiddenlayer.ai/docs/products/console/overview"
  - "https://repello.ai/argus"
  - "https://blogs.cisco.com/ai/securing-ai-agents-with-cisco-ai-defense"
  - "https://github.com/cisco-ai-defense/ai-defense-python-sdk/releases/tag/v2.1.0"
  - "https://www.paloaltonetworks.com/ai-security/agent-security"
  - "https://www.f5.com/company/blog/what-are-ai-guardrails"
  - "https://www.globenewswire.com/news-release/2026/04/21/3278027/0/en/operant-ai-launches-codeinjectionguard-to-defend-ai-agents-against-runtime-code-injection-attacks.html"
  - "https://www.globenewswire.com/news-release/2026/05/04/3286769/0/en/operant-ai-launches-endpoint-protector-securing-shadow-ai-coding-agents-and-mcp-across-the-enterprise.html"
  - "https://techcommunity.microsoft.com/blog/microsoft-security-blog/authorization-and-governance-for-ai-agents-runtime-authorization-beyond-identity/4509161"
  - "https://techcommunity.microsoft.com/blog/microsoft-security-blog/securing-ai-agents-at-runtime-real-time-protection-and-threat-detection-for-micr/4541255"
  - "https://learn.microsoft.com/en-us/entra/agent-id/whats-new-agent-id"
  - "https://aws.amazon.com/about-aws/whats-new/2026/03/policy-amazon-bedrock-agentcore-generally-available/"
  - "https://www.anthropic.com/engineering/claude-code-auto-mode"
---

# Agent Runtime Protection Market Canvass

**Assessed 2026-09-16:** twenty-one products in the independent AI agent runtime-protection and guardian-agent category, graded from vendor documentation against the five runtime capabilities named by the L4 rung of [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]].

No product in the set holds a generally available implementation, two quarters old or older, of any of the five capabilities. Three products document a mechanism that touches one capability, and each carries at least one disqualifying condition. Invariant Labs' flow operator sits behind an early-access waitlist. Cisco AI Defense carries two: its agent runtime SDK reached general availability five days inside the exclusion window, and its tool-call inspection runs the platform's content classifiers instead of comparing intent against action. Operant AI's CodeInjectionGuard also carries two: it launched inside the window, and it scans commands and packages at run time, which takes a different subject from the static analysis of generated code that the capability names. Seven of the twenty-one market agentic runtime security over documentation that describes input and output filtering.

## Scope and method

### The five capabilities graded

The D4 L4 rung reads:

> **L4 — Managed.** Runtime control reaches the agent's reasoning and the semantics of its tool calls — chain-of-thought auditing, code-safety analysis, groundedness checking, framework-enforced trusted/untrusted context boundaries, and semantic validation of high-impact calls — and human approval stays mandatory for configured high-blast-radius operations however clean those checks come back. The load-bearing controls are preview, experimental, or specification-only, so an L4 program assembles this rung and evidences part of it from its own pipeline.

The canvass graded each product against the five capabilities that sentence names:

1. Chain-of-thought or alignment auditing, meaning goal-hijack detection on the agent's reasoning trace.
2. Code-safety analysis of code the agent generates.
3. Groundedness or hallucination checking.
4. Framework-enforced trusted and untrusted context boundaries.
5. Semantic validation of high-impact tool calls, covering intent-versus-action comparison, parsed impact and session-cumulative thresholds, together with mandatory human approval on high-blast-radius operations.

### The subject of the canvass

**This canvass grades a procurement route, and it does not grade whether L4 is achievable.** Recalibration rule 2 counts a control when it operates in the organization's production environment, whatever the underlying product's general-availability date, so an open-source or preview component running in production evidences an L4 criterion.[^rule2] [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4]] already names the route: a defensible L4 today is assembled from preview and open-source components, among them AlignmentCheck and CodeShield from [[llamafirewall|LlamaFirewall]], a groundedness checker, a sandbox for high-risk tasks, and a policy engine gating tool calls. An organization running that assembly holds L4 evidence today. This canvass tests the other route: whether the same rung can be reached by buying a product. On the evidence below it cannot, which prices L4 for this deployment shape as an integration project.

### The grading key

Each capability took one of seven grades: generally available, public preview, private or limited preview, waitlist or early access, open source, `CLAIMED, NO DOC`, and not covered. `CLAIMED, NO DOC` records a vendor claim with no documented mechanism behind it, and it is a verdict on the documentation rather than a gap in the search. Grades attach to capabilities and not to companies, so a product that is generally available as a whole grades as preview on a capability that is in preview.

### The date bar

Recalibration rule 2, the production-maturity qualifier in [[agentic-ai-security-cmm-recalibration-method-2026|CMM: Recalibration Method (Cadence and Cost)]], keeps a level criterion off bleeding-edge releases: a criterion avoids naming a capability whose only implementations are less than roughly two quarters past general availability.[^rule2] Two quarters back from the assessment date falls at approximately 2026-03-16, and every general-availability claim below is measured against that date. Dates therefore decide grades, which is why the canvass recorded release dates rather than release states.

### Sources

Every grade rests on a primary document read on 2026-09-16: vendor documentation, a release note, a dated press release, or a dated product announcement. Analyst listings and category rankings established no grade. Where a vendor's documentation was reached only through third-party coverage or a search summary, the entry carries low confidence and the footnote names what was read.

### Set membership

The set is the twenty-one products listed below. Twenty came from the named vendor list under assessment; CrowdStrike's [[falcon-guardian|Falcon Guardian]] was added from a category search of the same market. Products announced in this category outside that list were not graded, among them the [[cyera-agent-guardian-release|Cyera Agent Guardian release]].

## The canvass

Twenty-one products carry between them three documented mechanisms that touch an L4 capability and no generally available implementation of any of the five. Seven of the twenty-one belong to an acquiring platform vendor, four of them through deals announced within six weeks of each other in August and September 2025, and the acquirers have shipped platform integration rather than reasoning-layer capability.

| # | Vendor (product) | Corporate status | Enforcement position | Deployment model | Confidence |
|---|---|---|---|---|---|
| 1 | [[onyx-platform\|Onyx]] (Guardian Agent / AI Guard) | independent | in-path: alert, block, mask, steer, ask | inline inspection; proxy or SDK not documented | low |
| 2 | NeuralTrust (TrustGate / TrustGuard) | independent | in-path gateway | proxy and gateway | medium |
| 3 | [[zenity\|Zenity]] (Runtime Boundaries) | independent | in-path, deterministic enforcement | endpoint plus SaaS agent hooks | medium |
| 4 | Noma Security (AI-DR runtime protection) | independent | in-path: monitor, alert, block, mask | agent and endpoint hooks | medium |
| 5 | Straiker (Defend AI) | independent | in-path | SDK, API, webhook, sensor | low-medium |
| 6 | Pillar Security | independent | in-path adaptive guardrails | gateway or SDK, unconfirmed | low |
| 7 | WitnessAI (Agentic Control) | independent | in-path network enforcement | network level | medium |
| 8 | Prompt Security | acquired by SentinelOne, announced 2025-08-05 | in-path | proxy, browser, endpoint | medium |
| 9 | Aim Security (AI Firewall) | acquired by Cato Networks, announced 2025-09-03 | in-path firewall | SASE inline | low |
| 10 | Invariant Labs (Guardrails) | acquired by Snyk, 2025 | in-path, blocking rules | gateway, pre and post LLM and MCP calls | medium |
| 11 | Operant AI (Agent Protector / Endpoint Protector) | independent | in-path, blocks before execution | endpoint agent and hooks | medium |
| 12 | Lasso Security | independent | in-path gateway | MCP gateway; scanner core open source | low-medium |
| 13 | HiddenLayer (AIDR) | independent | in-path claimed, not detailed | SaaS console and integrations | medium |
| 14 | [[lakera-guard\|Lakera Guard]] | acquired by Check Point, announced 2025-09-16 | in-path | gateway and proxy | high |
| 15 | Robust Intelligence (Cisco AI Defense `agentsec`) | acquired by Cisco | in-path: monitor, enforce, off | SDK, dynamic client-library rewrite | medium |
| 16 | Protect AI ([[palo-alto-prisma-airs\|Prisma AIRS]] AI Agent Gateway) | acquired by Palo Alto Networks, closed April 2025 | in-path gateway once generally available | gateway | high |
| 17 | CalypsoAI | acquired by F5, announced 2025-09-11 | in-path | application delivery controller and gateway | medium |
| 18 | Troj.AI (TrojAI Defend) | independent; an A10 Networks relationship is unverified | in-path claimed | not documented | low |
| 19 | Vijil | independent | not documented | not documented | low |
| 20 | Repello AI (Argus) | independent | in-path, inside the agent loop | API and SDK | medium |
| 21 | CrowdStrike ([[falcon-guardian\|Falcon Guardian]]) | platform vendor; built in-house, not acquired | in-path for agent access control | endpoint sensor | high |

Capability grades follow. `partial` marks a mechanism that approximates the capability by another route, and `CLAIMED, NO DOC` marks a claim the documentation does not support.

| # | Vendor | 1 CoT audit | 2 Code safety | 3 Groundedness | 4 Context boundary | 5 Semantic tool call |
|---|---|---|---|---|---|---|
| 1 | Onyx | not covered | not covered | not covered | `CLAIMED, NO DOC` | claimed GA, no dated doc |
| 2 | NeuralTrust | not covered | not covered | not covered | not covered | not covered; input and output filtering |
| 3 | Zenity | not covered | not covered | not covered | partial: data-leak blocking | partial: intent claim, no method |
| 4 | Noma Security | `CLAIMED, NO DOC` | not covered | not covered | partial: undocumented context engine | partial: session-chain behaviour |
| 5 | Straiker | not covered | `CLAIMED, NO DOC` | `CLAIMED, NO DOC` | not covered | partial: MCP and tool risk detection |
| 6 | Pillar Security | not covered | not covered | not covered | partial: taint analysis | partial: behavioural deviation |
| 7 | WitnessAI | not covered | not covered | not covered | not covered | not covered; tool allow-listing |
| 8 | Prompt Security | not covered | not covered | not covered | not covered | not covered; filtering and DLP |
| 9 | Aim Security | not covered | not covered | not covered | not covered | not covered; policy firewall |
| 10 | Invariant Labs | not covered | not covered | not covered | documented, open source; product on waitlist | schema-level argument matching |
| 11 | Operant AI | not covered | adjacent mechanism, launched 2026-04-21 | not covered | not covered | not covered |
| 12 | Lasso Security | not covered | not covered | not covered | not covered | partial: undocumented baseline |
| 13 | HiddenLayer | not covered | not covered | not covered | not covered | not covered; detection and policy |
| 14 | Lakera | not covered | not covered | not covered | not covered | not covered; prompt and content guardrails |
| 15 | Cisco AI Defense | not covered | not covered | not covered | partial: indirect-injection detection | classifier-level, GA 2026-03-21 |
| 16 | Prisma AIRS | not covered | partial: artifact supply-chain scanning | not covered | not covered | limited preview |
| 17 | CalypsoAI | partial: decision explanation | not covered | not covered | not covered | not covered |
| 18 | Troj.AI | not covered | `CLAIMED, NO DOC` | not covered | not covered | partial: execution-trace analysis |
| 19 | Vijil | not covered | not covered | not covered | not covered | `CLAIMED, NO DOC` |
| 20 | Repello AI | not covered | not covered | not covered | not covered | not covered; prompt-layer filtering |
| 21 | Falcon Guardian | not covered | not covered | not covered | not covered | not covered; agent allow and deny |

### Findings by capability

#### Capability 1 — chain-of-thought and alignment auditing

No product in the set documents inspection of an agent's reasoning trace for goal hijack. Two vendors reach for the language and each describes a different mechanism. Noma Security writes that "[b]y governing the agent's internal thought process, Noma provides the only reliable defense",[^noma-cot] and the mechanism its own material describes is session and action-chain monitoring: "Noma tracks the full chain of agent actions across an entire session".[^noma-chain] A chain of executed actions carries no reasoning trace. CalypsoAI's Agentic Fingerprints and Outcome Analysis give "granular insight into every AI interaction, with detailed reasoning as to why a prompt was accepted or blocked",[^calypso] which reports why the guardrail decided as it did and reads nothing of the agent's own reasoning.

The underlying model property such a control reads is itself changing between model generations; see [[chain-of-thought-monitorability|Chain-of-Thought Monitorability]]. **Documented implementations: zero of twenty-one.**

#### Capability 2 — code-safety analysis of generated code

One product inspects code an agent is about to run, and it does so by another mechanism. Operant AI launched CodeInjectionGuard on 2026-04-21: "CodeInjectionGuard is available now as part of Operant AI's Agent Protector for teams deploying AI agents in development and production environments."[^operant-launch] Its documented mechanism intercepts pulled packages, matches known attack patterns and obfuscated code, monitors shell execution for credential harvesting and lateral movement, and intercepts reads on sensitive paths.[^operant-endpoint] That is runtime pattern and behaviour scanning at the endpoint, where the capability names static safety analysis of generated code on the CodeShield model. The launch date also sits inside rule 2's window: the product is under five months old against a bar of roughly two quarters.

Palo Alto's Prisma AIRS scans "supply chain vulnerabilities in agent artifacts, including agent code, MCP servers, and skills",[^prisma] which is composition analysis of build artifacts and takes a different subject from the code an agent writes at run time. **Documented implementations of the capability as written: zero; one adjacent mechanism, inside the window.**

#### Capability 3 — groundedness and hallucination checking

One vendor names the concept and none documents a method. Straiker lists "Application grounding & Output Safety … Detect and suppress application drift" among Defend AI's features,[^straiker] with no method, benchmark or dated availability statement behind the phrase. No other product in the set mentions groundedness, hallucination detection, or verification of an answer against its sources. **Documented implementations: zero of twenty-one.**

#### Capability 4 — trusted and untrusted context boundaries

The strongest documented mechanism in the canvass sits behind a waitlist. Invariant Labs, acquired by Snyk in 2025, documents a flow operator that "enables you to detect flows and ordering of operations",[^invariant-flow] blocking a tool from executing after the agent has read untrusted content. That is a dataflow rule enforcing a trust boundary, and it matches the capability as the ladder writes it. The product carrying it cannot be bought today, and its own landing page says so: "Sign up now for early access to our platform and to be notified as we release Guardrails."[^invariant-ea]

Three products enforce a narrower boundary. Zenity blocks data leaving across the boundary without enforcing the boundary generally. Pillar Security is reported to trace PII and secrets from source to destination through taint analysis, on third-party summaries rather than a Pillar document.[^pillar] Cisco AI Defense detects indirect prompt injection, framed as harmful commands inside content an agent reads and not as a general trust-boundary framework.[^cisco-blog] **Documented implementations: one, on an early-access waitlist.**

#### Capability 5 — semantic tool-call validation and mandatory approval

Eleven of the twenty-one products implement something against capability 5, more than against any other of the five, and the shortfalls fall into four shapes. Six implement a behavioural approximation graded `partial` above, two implement schema- or classifier-level inspection, one is in limited preview, and two claim the capability with no document behind the claim.

Cisco AI Defense inspects the arguments and results of MCP tool calls through the platform's general safety, privacy and security classifiers, which is the engine used for chat inspection applied to a new surface.[^cisco-docs] Invariant Labs matches tool-call parameters and content against regular expressions and PII detectors.[^invariant-flow] Palo Alto states the position plainly: "The AI Agent Gateway, currently available in limited preview, provides a central control plane to enforce agent runtime and identity security, governance and observability."[^prisma] Two products in the set gate tools by allow-list instead, which is a binary decision over a tool and carries no comparison of intent against action: WitnessAI enforces "a single, organization-wide approved-tool policy",[^witness] and Falcon Guardian's shipped control governs which agent types may run on an endpoint, with its AI-gateway layer pre-beta and general availability targeted for the fourth quarter of 2026.[^crowdstrike]

Human approval appears across the category as one selectable enforcement action. Onyx offers "[f]ive enforcement actions: alert, block, mask, steer, or ask (human in-the-loop)",[^onyx] so an operator who writes a policy without the fifth action gets no approval step at all. The L4 criterion holds approval mandatory for configured high-blast-radius operations however clean the automated checks come back, and no product in the set implements an approval that a clean automated check cannot skip. **Documented implementations of the capability as written: zero of twenty-one.**

### Distance between the category's name and its documented mechanism

**Seven of the twenty-one products market agentic runtime security over documentation that describes input and output filtering.** Prompt-injection classification, jailbreak detection, data-loss prevention, content filtering and tool allow-listing are the L2 and L3 layers of this domain, they have been generally available across the market for longer than the agentic framing has existed, and a product covering them well answers none of the five questions above. The distance between the category's name and its documented mechanism explains why the category reads as further along than the documentation supports.

- NeuralTrust markets TrustGuard as agent runtime security that "inspects every interaction and stops attacks at the moment of execution"; its TrustGuard documentation lists detectors for "[p]rompt injection, toxicity, off-topic use, hostile URLs, and documents" and for "PII and secrets leaving in a prompt, a completion, or a tool argument", and records that TrustGuard returns a verdict while the calling gateway, hook or SDK enforces it.[^neuraltrust]
- WitnessAI announces agentic control over "the tools and MCP servers AI agents can access", delivered as an organization-wide approved-tool policy, which is allow-listing at the network layer.[^witness]
- HiddenLayer markets AIDR as agentic runtime security; its console documentation describes monitoring of model input and output turned into enforceable runtime policies, and mentions none of the five capabilities.[^hiddenlayer]
- Lakera's acquisition announcement states the scope directly: its runtime "protects LLM inputs, outputs, and all data flowing through … enforcing guardrails against prompt attacks, data leakage, and content violations".[^lakera]
- Prompt Security is described by its acquirer as "a pioneer in securing AI in runtime", and the post-acquisition material describes enforcement against "prompt injection, sensitive data leakage, and misuse".[^promptsec]
- Repello AI positions Argus as "real-time runtime security for agentic AI systems that sits inside the agent loop", and the threat categories it names are prompt injection, jailbreaks, PII leakage and unsafe prompts.[^repello]
- Lasso Security markets Intent Security as behavioural validation for agentic AI, asking the operator to "[v]alidate that the user and agent's action did not deviate from their behavioral baseline" and to "[d]ecode the semantic meaning behind every request to detect hidden malicious commands", and publishes no method for computing the baseline.[^lasso]

### The dates behind the grades

The category's release dates cluster inside the last seven months, which is why the date bar excludes so much of it. Six products or capabilities in the set arrived between March and September 2026: Onyx in March, the Prisma AIRS AI Agent Gateway into limited preview in March, Operant Endpoint Protector in May, WitnessAI Agentic Control in June, Zenity Runtime Boundaries in July, and Falcon Guardian on 2026-09-01, fifteen days before this assessment. A rule written to keep level criteria off bleeding-edge releases excludes nearly all of that for another two quarters, whatever any of it covers.

Five dated items decided their own grades. Three are platform-native controls, checked alongside the independent set because each was named as a possible L4 anchor.

**Cisco AI Defense `agentsec` reached general availability on 2026-03-21**, five days after the cutoff. The release note for version 2.1.0 of the open-source AI Defense Python SDK introduces the "Agent Runtime SDK (`agentsec`) — New `aidefense.runtime.agentsec` module providing transparent, 2-line integration for runtime protection of LLM and MCP interactions",[^cisco-sdk] and the surrounding release sequence fixes the year.[^cisco-seq] The five-day margin decides the rule-2 verdict alone; the mechanism grade in capability 5 above stands on the documentation and does not depend on the date.

**Microsoft's runtime protection for Agent 365 reached general availability on 2026-07-27**, seven weeks before this assessment. Agent 365 itself reached general availability on 2026-05-01, and at that date Defender's runtime blocking and policy controls were scheduled for public preview in June 2026 rather than bundled with the platform. The split resolved later in two headings on one post: "Real-time protection for Microsoft Agent 365 tooling servers—now generally available" and "Threat detection for Microsoft Agent 365 agents—now in public preview".[^defender] Tool-server protection is therefore generally available and inside the window; agent threat detection remains in preview with no stated general-availability target.

**Microsoft Entra Agent ID reached general availability on 2026-05-01**,[^entra-ga] which places it inside the window whatever its capability coverage turns out to be.

**Policy in Amazon Bedrock AgentCore reached general availability on 2026-03-03**,[^agentcore] thirteen days before the cutoff and the only item in this pass to clear the date bar. Its mechanism compiles authorization rules into Cedar and constrains tool access by attribute and argument, which is the subject of the CMM's control and least-agency domain and not of capability 5. Clearing the date bar therefore places it against no L4 capability.

**Operant AI's CodeInjectionGuard launched on 2026-04-21**,[^operant-launch] and the 2026-05-04 date recorded in the first research round belongs to its re-bundling into Operant Endpoint Protector.[^operant-endpoint] Both dates sit inside the window, so the correction moves the anchor date and leaves the grade where it was.

One further dated figure bears on capability 5's approval requirement without belonging to any vendor in the set. Anthropic reports that "Claude Code users approve 93% of permission prompts",[^anthropic] a figure this page carries at medium confidence because the primary document discloses no sample size, time window, or definition of approval behind it.

### Corrections to the round-one record

The first research round recorded a Microsoft Entra Agent ID feature named Authorization Fabric as a shipped implementation of runtime authorization for agent tool calls. **That finding is withdrawn in full.** Authorization Fabric is a reference architecture that a reader builds, published on the Microsoft Security Blog on 2026-04-07. The post "introduces a reusable Authorization Fabric", "implemented as a Microsoft Entra-protected endpoint using Azure Functions/App Service authentication", and advises "[f]or a POC, keep it minimal—add hardening incrementally".[^authz-fabric] Its ALLOW, DENY, REQUIRE_APPROVAL and MASK values are decision outputs of that proposed design, and no product enforces them as states. The Microsoft Learn documentation for Entra Agent ID authorization, revised 2026-06-12, mentions Authorization Fabric nowhere, and names no policy decision point and none of those four values.[^entra-docs]

The retraction supports the D4 L4 criterion's own wording. The criterion says the load-bearing controls are preview, experimental, or specification-only, and Authorization Fabric is an instance of the third. This subsection exists so that a later reader who re-finds the post records it as guidance.

Two dates recorded in the first round are corrected in the section above: Operant's CodeInjectionGuard launched 2026-04-21, and Microsoft Defender's runtime coverage for Agent 365 was not bundled at Agent 365's own general availability.

## Out of scope

The canvass did not assess the four things below, and a reader must not infer any of them from it.

- **Input and output filtering.** The L2 and L3 layers of this domain are generally available across the whole market. A product covering them well scores nothing here, and `not covered` in the grade tables records the absence of an L4 capability and no judgment about a product's security value.
- **Product quality.** The canvass records what primary documentation says a product does. It ranks nothing and recommends nothing.
- **Whether L4 is achievable.** L4 is achievable today by assembly, as the scope section and [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4]] both state. The finding here concerns the procurement route and its cost.
- **Any ladder text.** No L1 to L5 criterion moved as a result of this canvass, and the evidence supports the D4 L4 criterion as written.

## Disposition

The canvass fed the decision pass on issue [#171](https://github.com/ag0x00/ai-era/issues/171), which answers recommendation 31 of the September 2026 stress test; see [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]]. On 2026-09-16 the realistic target for the generative-coding deployment shape in [[agentic-ai-security-cmm-2026|the Agentic AI Security Capability Maturity Model]] moved from L4 across all nine domains to `L3 → L4 (L4 in D8)`, matching what eight of that model's nine domain deep dives already right-size for the shape. [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]] records the canvass result in its L4 detail, and its ladder is unchanged. [[cmm-known-limitations|CMM Known Limitations]] item 20 moved to the addressed list on the same date.

The result also reaches five pages that carried a claim about what this market supplies: [[guardian-agent|Guardian Agent]], [[guardian-agents-market-guide|the Gartner Market Guide for Guardian Agents]], [[agentic-ai-security-reference-architecture|the Agentic AI Security Reference Architecture]], [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] and [[securing-agentic-coding|Securing Agentic Coding]].

## Notes

[^rule2]: Recalibration rule 2, the production-maturity qualifier, in [[agentic-ai-security-cmm-recalibration-method-2026|CMM: Recalibration Method (Cadence and Cost)]].
[^noma-cot]: Noma Security, "Securing Cursor's Agent Runtime: How Noma Leverages Cursor Hooks for Real-Time AI Guardrails", blog, 2025-12-23. https://noma.security/blog/securing-the-agentic-frontier-noma-unveils-the-first-real-time-agent-runtime-security-for-cursor/
[^noma-chain]: Noma Security, "Govern and Secure AI Agents Everywhere They Run", platform page, read 2026-09-16. https://www.noma.security/platform
[^calypso]: F5, "What are AI guardrails?", 2025-09-29. https://www.f5.com/company/blog/what-are-ai-guardrails
[^operant-launch]: Operant AI, "Operant AI Launches CodeInjectionGuard to Defend AI Agents Against Runtime Code Injection Attacks", GlobeNewswire, 2026-04-21. https://www.globenewswire.com/news-release/2026/04/21/3278027/0/en/operant-ai-launches-codeinjectionguard-to-defend-ai-agents-against-runtime-code-injection-attacks.html
[^operant-endpoint]: Operant AI, "Operant AI Launches Endpoint Protector, Securing Shadow AI, Coding Agents and MCP Across the Enterprise", GlobeNewswire, 2026-05-04. https://www.globenewswire.com/news-release/2026/05/04/3286769/0/en/operant-ai-launches-endpoint-protector-securing-shadow-ai-coding-agents-and-mcp-across-the-enterprise.html
[^prisma]: Palo Alto Networks, "AI Agent Security", read 2026-09-16. https://www.paloaltonetworks.com/ai-security/agent-security
[^straiker]: Straiker, "Defend AI", read 2026-09-16. https://straiker.ai/products/defend-ai
[^invariant-flow]: Invariant Labs, MCP-scan guardrails reference, read 2026-09-16. https://invariantlabs-ai.github.io/docs/mcp-scan/guardrails-reference/
[^invariant-ea]: Invariant Labs, Guardrails product page, read 2026-09-16. https://invariantlabs.ai/guardrails
[^pillar]: Pillar Security capability descriptions were reached through third-party summaries rather than a Pillar document; no primary Pillar documentation was read in this pass, and the entry is graded low confidence for that reason.
[^cisco-blog]: Cisco, "Securing AI Agents with Cisco AI Defense", 2026-06-29. https://blogs.cisco.com/ai/securing-ai-agents-with-cisco-ai-defense
[^cisco-docs]: [Cisco AI Defense Python SDK — README, MCP Inspection](https://github.com/cisco-ai-defense/ai-defense-python-sdk#mcp-inspection), read 2026-09-16: the SDK inspects "Model Context Protocol (MCP) JSON-RPC 2.0 messages for security, privacy, and safety violations in AI agent tool calls, resource access, and responses", through the same inspection API it applies to chat and HTTP traffic.
[^cisco-sdk]: Cisco AI Defense Python SDK, release notes for v2.1.0, 2026-03-21. https://github.com/cisco-ai-defense/ai-defense-python-sdk/releases/tag/v2.1.0
[^cisco-seq]: Release sequence for the same SDK: v2.0.0 on 2025-12-05, v2.1.0 on 21 March, v2.1.1 on 26 March, v2.1.2 on 2026-07-13, v2.1.3 on 2026-08-03, which fixes the v2.1.0 year as 2026. https://github.com/cisco-ai-defense/ai-defense-python-sdk/releases
[^witness]: Help Net Security, coverage of the WitnessAI Agentic Control announcement, 2026-06-17. Full URL not captured in the research record.
[^crowdstrike]: [CrowdStrike — CrowdStrike Unveils Falcon Guardian to Secure AI Agents Where They Execute: On the Endpoint at Runtime](https://www.crowdstrike.com/en-us/press-releases/crowdstrike-unveils-falcon-guardian-ai-agent-security/), 2026-09-01. Agent Access Controls "[d]efines which AI agents are permitted to run on managed endpoints, blocking unauthorized agents"; the AI Gateway "will provide a centralized control point for enterprise AI traffic". The pre-beta status of that gateway layer and its fourth-quarter 2026 general-availability target come from conference coverage of Fal.Con 2026 rather than from CrowdStrike documentation.
[^onyx]: Onyx, "AI Security" platform page, read 2026-09-16. https://onyx.security/platform/ai-security
[^neuraltrust]: [NeuralTrust — TrustGuard documentation overview](https://docs.neuraltrust.ai/trustguard/overview), read 2026-09-16, for the detector list and the evaluate-versus-enforce split; [NeuralTrust — AI agent security](https://neuraltrust.ai/ai-agent-security) for the product framing.
[^hiddenlayer]: HiddenLayer documentation, console product overview, read 2026-09-16. https://docs.hiddenlayer.ai/docs/products/console/overview
[^lakera]: Check Point, press release announcing the Lakera acquisition, 2025-09-16. Full URL not captured in the research record.
[^promptsec]: SentinelOne, press release announcing the Prompt Security acquisition, 2025-08-05. Full URL not captured in the research record.
[^repello]: Repello AI, "Argus", read 2026-09-16. https://repello.ai/argus
[^lasso]: [Lasso Security — Intent Security](https://www.lasso.security/platform/intent-security), read 2026-09-16. The page states the behavioural-baseline and semantic-decoding claims and describes no method for computing the baseline.
[^defender]: Microsoft, "Securing AI Agents at Runtime: Real-Time Protection and Threat Detection for Microsoft Agent 365", TechCommunity, 2026-07-27. https://techcommunity.microsoft.com/blog/microsoft-security-blog/securing-ai-agents-at-runtime-real-time-protection-and-threat-detection-for-micr/4541255
[^entra-ga]: Microsoft Learn, "What's new in Microsoft Entra Agent ID", ms.date 2026-05-01. https://learn.microsoft.com/en-us/entra/agent-id/whats-new-agent-id
[^entra-docs]: Microsoft Learn, Entra Agent ID authorization documentation, ms.date 2026-06-12. https://learn.microsoft.com/en-us/entra/agent-id/
[^authz-fabric]: Microsoft Security Blog, "Authorization and Governance for AI Agents: Runtime Authorization Beyond Identity", TechCommunity, 2026-04-07. https://techcommunity.microsoft.com/blog/microsoft-security-blog/authorization-and-governance-for-ai-agents-runtime-authorization-beyond-identity/4509161
[^agentcore]: Amazon Web Services, "Policy in Amazon Bedrock AgentCore is now generally available", 2026-03-03. https://aws.amazon.com/about-aws/whats-new/2026/03/policy-amazon-bedrock-agentcore-generally-available/
[^anthropic]: Anthropic, "Claude Code auto mode", 2026-03-25. https://www.anthropic.com/engineering/claude-code-auto-mode
