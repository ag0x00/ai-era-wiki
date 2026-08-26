---
type: concept
title: "Threat Modeling for AI"
address: c-000112
created: 2026-05-24
updated: 2026-08-25
tags:
  - concepts
  - threat-modeling
  - secure-sdlc
  - agentic-ai
  - ai-security
status: developing
scope_axis: [sec-of-ai, sec-against-ai]
complexity: intermediate
domain: secure-sdlc
origin: produced
aliases:
  - Threat Modeling for AI
  - Agentic AI Threat Modeling
related:
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[owasp-llm-top-10]]"
  - "[[owasp-ai-exchange]]"
  - "[[mitre-atlas]]"
  - "[[csa-maestro]]"
  - "[[stride-ai-2026]]"
  - "[[lethal-trifecta]]"
  - "[[lethal-bifecta]]"
  - "[[microsoft-sdl|Microsoft SDL]]"
  - "[[ai-security-standards-in-q1-2026]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[dream-taiwan-multi-agent-ai-attack]]"
sources:
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
  - "https://www.microsoft.com/en-us/securityengineering/sdl/practices"
  - "https://atlas.mitre.org"
  - "https://owaspai.org/docs/ai_security_overview"
  - "https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/"
  - "https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia"
  - "https://arxiv.org/abs/2605.17163"
---

# Threat Modeling for AI

Threat modeling for AI extends classical design-time threat modeling to systems that include models and agents. The classical step asks how an attacker abuses a data flow or trust boundary; the AI extension adds boundaries and assets that traditional STRIDE-style modeling leaves unnamed: training data, model weights, the prompt and context window, retrieval corpora, tool and MCP connections, agent memory, and the agent's autonomy itself. The standard lists are awareness frameworks. They name the risks; the threat model walks a specific architecture and decides which controls it needs.

As of June 2026 three threat surfaces converge on any deployed agentic system. The **LLM-legacy** surface is the set of risks inherited from generative models (prompt injection, output handling, disclosure), cataloged in the [[owasp-llm-top-10|OWASP LLM Top 10]]. The **agentic-new** surface is the set of risks that appear only once a model plans, holds memory, and acts through tools, cataloged in the [[owasp-agentic-ai-top-10|OWASP ASI Top 10]] and its [[owasp-agentic-ai-threats-mitigations|T1–T17 reference model]]. The **supply-chain-persistent** surface (tampered models, poisoned packages, [[tool-poisoning|rug-pulled tools]], [[slopsquatting|hallucinated dependencies]]) predates AI but acquires AI-specific entry points. A useful threat model accounts for all three on the same architecture.

Document 5 of the [[owasp-ai-exchange|OWASP AI Exchange]] partitions the threats a test programme covers along a different axis, by AI paradigm: predictive, generative, agentic.[^aix-testing] Its predictive row holds evasion, model exfiltration and model poisoning, and has no counterpart among the three surfaces here, because this page scopes to the deployed agentic system.

## On this page

- [The taxonomy landscape and the selection logic](#the-taxonomy-landscape-and-the-selection-logic)
- [A method for the agentic case](#a-method-for-the-agentic-case)
- [Worked example](#worked-example--a-multi-agent-rag-assistant-with-mcp-tools)
- [Enforcement below the prompt layer](#enforcement-below-the-prompt-layer)
- [Connection to the RA and CMM](#connection-to-the-ra-and-cmm)
- [See also](#see-also)

## The taxonomy landscape and the selection logic

Seven taxonomies are in active use, plus three structural tests and the wiki's five-class expansion. They are complementary: each does a different job. The full cross-walk between them, and onto the control artifacts, is the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix; this section gives the selection logic.

- **[[stride-ai-2026|STRIDE-AI]]** is an *elicitation method*. Use it first, to walk an architecture and surface candidate threats against AI assets. It re-maps the six STRIDE categories onto training data, weights, the prompt/context window, and inference endpoints, and asks the analyst what an attacker does at each.
- **[[owasp-agentic-ai-top-10|OWASP ASI Top 10]]** (ASI01–ASI10) is the consensus *ranked risk list* for agentic systems — the Rosetta Stone the other lists cross-map to. Use it to name and rank what elicitation surfaces.
- **[[owasp-agentic-ai-threats-mitigations|OWASP T1–T17]]** is the *reference threat model* the ASI list ranks, and it carries six mitigation playbooks. Use it to move from a named risk to candidate controls.
- **[[owasp-llm-top-10|OWASP LLM Top 10]]** is the *non-agentic GenAI base*. Use it for the model-layer risks an agentic system inherits regardless of its autonomy.
- **[[mitre-atlas|MITRE ATLAS]]** is the *adversary technique catalog*, the attacker's-eye view. Use it for detection engineering and red-team planning, where the question is which techniques will be used rather than which risks exist.
- **[[csa-maestro|CSA MAESTRO]]** is a *layered decomposition* (seven layers, Foundation Models through Agent Ecosystem). Use it when the threat-modeling conversation is architectural: which layer owns a boundary, where cross-layer movement is possible.
- **[[owasp-ai-exchange|OWASP AI Exchange]]** is a *matrix and control catalogue* covering all AI, keyed on asset and impact rather than on the application ([`/go/aisecuritymatrix/`](https://owaspai.org/go/aisecuritymatrix/)). Use it for the development-time, model-layer, runtime application-security, and non-generative threats the agentic lists leave outside their scope; the [[threat-taxonomy-reconciliation|reconciliation matrix]] carries the eleven categories no ASI row anchors. It is also the entry to start from where the deliverable must map to prEN 18282 or ISO/IEC 27090, on the Exchange's own account of its liaison contribution to both.[^aix-liaison]

Three **structural tests** sit before enumeration. The [[lethal-trifecta|Lethal Trifecta]] (private data access + untrusted content exposure + external communication) is a necessary condition for exfiltration at scale; the [[lethal-bifecta|Lethal Bifecta]] (untrusted content + sensitive write capability) is the write-side analogue for damaging action; and **egress-allowlist transitivity** asks whether any allowlisted destination can itself reach further than the agent may, since an allowlist bounds reach only to the transitive closure of what it permits. They are go/no-go checks applied at design time: if the architecture satisfies the condition, the risk is structural and one leg must be removed regardless of which catalog threat is in play.

The wiki's [[agentic-ai-threat-classes-2026|five threat classes]] extend the published lists over ground the standard taxonomies under-serve: the AI-aware insider, the long-running APT campaign, agent collusion, model-version degradation, and the jurisdictional adversary with regulatory leverage. They are cross-cutting adversary models rather than per-component risks, and three of them collapse to a single control: a continuously-executed, version-pinned eval harness over every artifact.

## A method for the agentic case

A repeatable pass runs in eight steps, design-time first and revisited at each model or tool change:

1. **Apply the structural tests.** Does any agent hold the trifecta or the bifecta, and can any allowlisted egress destination reach further than the agent may? If so, record the structural risk and the leg to remove before going further — it dominates the per-threat analysis.
2. **Elicit with STRIDE-AI.** Walk the architecture asset by asset (identities, data stores, memory, prompt, tools, inter-agent channels) and surface candidate threats.
3. **Name and rank with the catalogs.** Map each candidate to an ASI category and its T-code; pull the attacker techniques from ATLAS for the ones that need detection coverage.
4. **Check the five classes.** Ask the CISO questions the lists miss: own engineers, slow adversaries, colluding agents, the next model version, regulatory cutoff.
5. **Map to controls.** Follow each named threat through the [[threat-taxonomy-reconciliation|reconciliation matrix]] to its RA plane and CMM domain, then sequence the controls by dependency (identity before egress and observability; policy decision before runtime enforcement).
6. **Treat each risk explicitly.** Every named risk resolves to mitigate, transfer, avoid, or accept, recorded with the rationale. The [[owasp-ai-exchange|AI Exchange]] states the sufficiency criterion in this form: an AI system is sufficiently secure when all identified risks can be treated, meaning transferred, avoided, or accepted ([`/go/riskanalysis/`](https://owaspai.org/go/riskanalysis/)). Mitigation is absent from that three-way list because the same source folds it into acceptance: acceptance sometimes follows directly, and sometimes requires controls that bring the risk to an acceptable level first.
7. **Allocate responsibility, then verify it.** The organization that builds and deploys owns each threat by default, and hosting, model, extension, and infrastructure providers take documented shares through a responsibility matrix. Where a provider declines to disclose how it mitigates a risk, the three available options are accept, self-mitigate, or avoid by not engaging ([`/go/riskanalysis/`](https://owaspai.org/go/riskanalysis/)). A threat model that names controls without naming who owns each one has deferred the decision.
8. **Hand the model to the test programme, and take the coverage report back.** The enumeration in steps 2 and 3 is the coverage baseline for security testing: every external input surface it names — user input, retrieved documents, tool outputs, inter-agent messages — is a surface a test either exercised or did not.[^aix-testing] A threat category the programme did not test is reported as a finding rather than omitted. The [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]] carries the form that coverage statement takes, keyed to four test layers (LLM reasoning, tool execution, infrastructure, inter-agent communication) rather than to this model's input surfaces — the two partitions do not align one-to-one, so reading the report back means mapping each surface onto the layer or layers a test on it would exercise, not matching names.

## Worked example — a multi-agent RAG assistant with MCP tools

Consider an enterprise assistant built as three coordinating agents: a **planner** that decomposes a user request, a **retrieval agent** that queries a RAG corpus of internal documents, and an **executor** that calls tools through MCP servers — a ticketing server, a database server, and an email server. Untrusted content enters through retrieved documents and through tool outputs; the corpus mixes documents with different access levels; each agent and each MCP server needs an identity.

**Structural tests.** The executor reads the private corpus (private data), processes retrieved documents and tool results an attacker can influence (untrusted content), and can send email (external communication): it holds the full [[lethal-trifecta|trifecta]], so exfiltration is structurally possible. The ticketing and database servers give it sensitive writes, so against the same untrusted content it also holds the [[lethal-bifecta|bifecta]]. The model records that the email path is the trifecta leg to constrain (route it through a [[oversight-layer|policy decision point]] or strip it from the executor) before any per-threat control is weighed.

**Elicitation and naming.** Running STRIDE-AI over the assets surfaces the threats below, and the catalogs name each one: a poisoned retrieved document that redirects the planner's objective ([[owasp-agentic-ai-top-10|ASI01]] Goal Hijack / T6, [[indirect-prompt-injection|indirect prompt injection]]); a document that writes a durable false fact into the retrieval agent's memory (ASI06 Memory & Context Poisoning / T1, [[memory-poisoning|memory poisoning]]); the executor coerced into chaining MCP calls to exfiltrate (ASI02 Tool Misuse / T2, [[tool-abuse-chains|tool-abuse chains]]); a rug-pulled or typosquatted MCP server (ASI04 Supply Chain / T17, [[tool-poisoning|tool poisoning]]); a forged planner-to-executor instruction (ASI07 Insecure Inter-Agent Communication / T16, [[mcp-security|MCP protocol abuse]]); and a fault in one agent amplifying across the mesh (ASI08 Cascading Failures / T5).

**The five classes.** The RAG curator who silently swaps a high-trust source is the Class 1 insider; a planner and executor that coordinate to stay under a per-agent monitor are Class 3 collusion; a vendor minor-version bump that regresses the executor's jailbreak resistance is Class 4. Class 5 applies if the underlying model is hosted in a jurisdiction subject to cutoff.

**Controls, sequenced by dependency.** Per-agent [[non-human-identity|identity]] for all three agents and the MCP services comes first (CMM D2), because per-agent egress mediation and behavioral baselining are capped by it. A [[oversight-layer|policy decision point]] outside the model context (D3) authorizes the executor's actions and downgrades the trifecta on the email path. An agent-aware gateway (D5, RA Egress) brokers every MCP call, fingerprints tools against rug-pull, and enforces per-agent tool scope. On the data plane (D6), answer-time entitlement enforcement keeps the retrieval agent from returning documents above the user's access level, and [[cognitive-file-integrity|cognitive file integrity]] hashes the system prompts and high-trust sources. Observability (D7) baselines each agent's tool-call sequence and wires drift to a kill switch. The dependency order runs identity, then control, then egress and data, then observability. A guardrail enforces the decisions a policy decision point makes, and per-agent monitoring resolves to the agent only where per-agent identity exists, so either one deployed ahead of its prerequisite enforces nothing.

## Enforcement below the prompt layer

The recurring architectural error, documented in [[ai-security-standards-in-q1-2026|the standards gap analysis]], places a control in the prompt — instructing the model to withhold data, to disregard injected text — where the same untrusted input that carries the attack can countermand the instruction. The controls above belong at the framework and runtime layer: a policy decision point the model cannot edit, a gateway the agent cannot bypass, identity the agent cannot forge. Prompt-level measures reduce residual risk and never form the enforcement boundary. The layer a control lives at decides whether a named threat is mitigated or only discouraged. The [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] is the production case, read from the model vendor's side: on Dream Security's account the attacker's agent framework had to defeat refusal training alone, with no framework-layer policy point described beneath it, and an "authorized penetration testing" frame countermanded that training across a four-day campaign.

## Connection to the RA and CMM

Every threat this page names lands on a plane of the [[agentic-ai-security-reference-architecture|AAI-S RA]] and a domain of the [[agentic-ai-security-cmm-2026|CMM]] through the [[threat-taxonomy-reconciliation|reconciliation matrix]]. The RA shows *where* the control sits in an architecture; the CMM grades *how mature* the program's implementation of it is. The threat model feeds both: the RA's Threat-Control Matrix answers its enumeration, and the CMM's evidence requirements are calibrated against its risk basis.

> [!gap] Where this is still thin
> The worked example is one archetype; coding-agent and closed-corpus-chatbot archetypes would exercise different leg combinations. Class 3 (collusion) no longer rests on controlled-lab evidence alone: the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]] supplies an unattributed accidental case (no threat actor — the participants were a defender's own evaluation fleet), and the [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] supplies a deliberately constructed one, though its attribution rests on linguistic evidence alone, with no named group. Neither is the narrow planner-and-executor-evading-a-monitor case this page's worked example describes; both are the wider many-agent, capability-sharing form documented on the threat-classes page. STRIDE-AI's own efficacy rests on a single self-reported case study. These limits are tracked on the [[agentic-ai-threat-classes-2026|threat-classes page]] and [[stride-ai-2026|the STRIDE-AI summary]].

## See also

- [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] — the cross-walk matrix every taxonomy maps through
- [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes]] — the five-class expansion in full
- [[owasp-agentic-ai-top-10|OWASP ASI Top 10]] · [[owasp-agentic-ai-threats-mitigations|OWASP T1–T17]] · [[mitre-atlas|MITRE ATLAS]] · [[csa-maestro|CSA MAESTRO]] · [[stride-ai-2026|STRIDE-AI]] — the catalogs and methods
- [[lethal-trifecta|Lethal Trifecta]] · [[lethal-bifecta|Lethal Bifecta]] — the structural tests; the transitivity test is derived from the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]]
- [[microsoft-sdl|Microsoft SDL]] — the classical design-stage practice this extends
- [[agentic-ai-security-reference-architecture|AAI-S RA]] · [[agentic-ai-security-cmm-2026|CMM]] — where the controls live and how maturity is graded

## Notes

[^aix-testing]: [OWASP AI Exchange — AI security testing](https://owaspai.org/go/testing/), retrieved 2026-08-19. Document 5, testing-strategies half. The threats-to-test-for taxonomy partitioned by AI paradigm, and the coverage-driven agentic methodology: threat-model the system before testing by enumerating agents, orchestrators, tools, data sources, trust boundaries and every external input surface; define minimum coverage criteria up front; report untested threat categories explicitly, since coverage gaps are findings.
[^aix-liaison]: OWASP AI Exchange, ["About the AI Exchange"](https://owaspai.org/go/about/), retrieved 2026-08-17. The Exchange states that 70 pages were contributed to prEN 18282 and 70 pages to ISO/IEC 27090 through official liaison partnership, plus contribution to ISO/IEC 27091. These are the source's own claims and are not independently verified here.

<!-- sources:auto -->
## Sources

- [owaspai.org](https://owaspai.org/docs/5_testing)
- [microsoft.com](https://www.microsoft.com/en-us/securityengineering/sdl/practices)
- [atlas.mitre.org](https://atlas.mitre.org)
- [owaspai.org](https://owaspai.org/docs/ai_security_overview)
- [genai.owasp.org](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
- [dreamgroup.com](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia)
- [arxiv.org](https://arxiv.org/abs/2605.17163)
<!-- /sources -->
