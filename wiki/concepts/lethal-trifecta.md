---
type: concept
title: "Lethal Trifecta"
address: c-000308
created: 2026-04-30
updated: 2026-08-20
tags:
  - concepts
  - prompt-injection
  - threat-modeling
  - agentic-ai
status: developing
scope_axis:
  - sec-of-ai
aliases:
  - "Lethal Trifecta for AI Agents"
  - "Willison Trifecta"
related:
  - "[[threat-modeling-for-ai]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[owasp-ai-exchange]]"
  - "[[indirect-prompt-injection]]"
  - "[[tool-abuse-chains]]"
  - "[[prompt-injection-containment]]"
  - "[[least-agency-principle]]"
  - "[[agent-sandboxing]]"
  - "[[lethal-bifecta]]"
  - "[[securing-your-agents-talk]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[simon-willison]]"
  - "[[andrew-bullen]]"
  - "[[stripe]]"
  - "[[owasp-state-of-agentic-ai-security-governance]]"
  - "[[standards-review-owasp-llm-top-10-2026-Q2]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agents-rule-of-two]]"
  - "[[claude-code-github-action-credential-exposure]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[cosnitch-copilot-personal-exfiltration]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[artifactory]]"
  - "[[echoleak-copilot-zero-click]]"
  - "[[geminijack-gemini-enterprise-injection]]"
  - "[[slack-ai-private-channel-exfiltration]]"
sources:
  - "[[.raw/talks/securing-your-agents-2026-04-30.md]]"
  - "[[.raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_slides.pdf]]"
  - "[[.raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_transcript.md]]"
---

# Lethal Trifecta

## Definition

Coined by [[simon-willison|Simon Willison]] in June 2025 ("The Lethal Trifecta for AI Agents", simonwillison.net), the **Lethal Trifecta** is the minimum set of three agent capabilities that, when present together, allow an attacker to easily exfiltrate private data via prompt injection:

1. **Access to private data** — the agent can read sensitive content (mailbox, files, internal docs, RAG store, secrets in env).
2. **Exposure to untrusted content** — the agent ingests content from sources the attacker can influence (web pages, emails, documents, calendar invites, RAG entries, MCP tool descriptions).
3. **Ability to externally communicate** — the agent can send data outside the trust boundary (HTTP requests, outbound mail, file writes to shared storage, markdown image rendering, URL-fetch tool calls).

When all three hold, a successful [[prompt-injection|prompt injection]] (almost always [[indirect-prompt-injection|indirect]]) can chain the agent's own capabilities into an exfiltration path. The attacker doesn't need a software exploit; the agent supplies the read-and-write primitives.

## Basis for the term "lethal"

The trifecta is **necessary and (in practice) sufficient** for end-to-end data exfiltration via natural-language attack:

- Any **two** of the three capabilities is annoying-but-recoverable. Two agents, each with two-of-three, can each be useful.
- All **three** in a single agent gives an attacker an exfiltration channel that requires no privilege escalation, no code execution exploit, and often no detectable malware artifact. The compromise looks identical to normal use.

The framing is load-bearing because it gives architects a **structural** test: ask, "does any single agent in our system hold all three?" If yes, that agent is one indirect injection away from being a data-exfil tool.

## Manifestation in real attacks

| Stage | Capability used | Example |
|---|---|---|
| Trigger | Untrusted content | Hidden HTML comment in a shared Google Doc the agent is asked to summarize |
| Read | Private data access | Agent has access to user's mailbox or file system |
| Send | External communication | Agent renders a markdown image with `?data=…` query string, or POSTs to attacker URL |

The [[jules-ai-kill-chain|Jules AI kill chain]] and most of [[johann-rehberger|Johann Rehberger]]'s [[month-of-ai-bugs|"Month of AI Bugs"]] disclosures are concrete instantiations of the trifecta.

**Zero-click cases refine the send leg rather than replace the trigger leg.** [[slack-ai-private-channel-exfiltration|Slack AI's canonical case]] required the victim to click a crafted "reauthenticate" link before data left the workspace — a human action stood between the agent's output and completed exfiltration. [[echoleak-copilot-zero-click|EchoLeak]] and [[geminijack-gemini-enterprise-injection|GeminiJack]] remove that click: both render an auto-fetched image whose URL carries the stolen data, so the send leg completes as soon as the client draws the response, with no separate victim action. Neither case removes the trigger leg — Copilot and Gemini Enterprise still require the victim to invoke the AI with some ordinary, unrelated query before the poisoned content enters context — so the trifecta's structural claim (three capabilities, one agent) is unchanged. What zero-click execution invalidates is a specific *class* of mitigation: any control that treats "the user must act to send data out" as a gate on the external-communication leg is defeated once that leg is a passive client-side resource fetch rather than a link the victim chooses to open. Egress containment for the send leg has to operate below the click — CSP scope, image/resource auto-load policy, and audited allowlisted proxies — not at the point of user consent.

**[[cosnitch-copilot-personal-exfiltration|CoSnitch]] collapses the trigger leg into the link itself.** An undocumented Copilot URL parameter (`autorun`) ran the query string as a prompt on page load; the "untrusted content" the agent ingested was the URL the victim clicked itself, ahead of any document retrieved and summarized afterward, and private-data access came from the victim's already-authenticated session rather than from any separate compromise. A second stage of the same incident — a summarized webpage carrying hidden instructions that write into Copilot's persistent memory — is a conventional trigger of the kind this page already describes. One incident therefore straddles both a reflected, URL-borne variant that skips retrieval entirely and the retrieval-based case the trifecta model was built around.

## Containment Strategies

Break the trifecta. Remove **at least one** of the three capabilities from any agent that handles untrusted content. Practical patterns:

1. **Split agents along the trifecta axes.** A "research agent" has untrusted-content + external-comms but **no** private-data access. A "personal-assistant agent" has private-data + external-comms but **no** untrusted-content ingest. A "summarizer" has private-data + untrusted-content but **no** external-comms.
2. **Remove external communication from sensitive agents.** If an agent must touch private data and untrusted content, it cannot also speak to the network. All output must go through a human-reviewed surface.
3. **Treat retrieved content as data, not instructions.** Use [[system-prompt-architecture|system prompt architecture]] with explicit trust labels. This does not break the trifecta on its own (a determined injection can still succeed) but reduces the success rate.
4. **Egress filtering.** Domain allowlists at the network layer make the "external communication" leg detectable and constrainable.
5. **Capability-level audit.** Every agent definition should declare which legs of the trifecta it holds. The audit asks: is this combination justified?
6. **Capability-based authorization at the action layer.** Even when an agent must hold all three legs, the *action it can take with them* can be deterministically constrained per task. [[capability-based-authorization|Capability-based authorization]] (e.g. [[tenuo-warrant|Tenuo Warrants]] from [[capability-based-authorization-talk|Niyikiza, Unprompted March 2026]]) issues task-scoped, holder-bound, delegation-aware capabilities; sub-agent capabilities can only narrow ([[monotonic-attenuation|monotonic attenuation]]). This contains an exfil-oriented prompt injection at execution time without removing any leg of the trifecta from the agent's *role* — the agent is allowed to ingest untrusted content, hold private-data access, and reach the network, but only the *specific action set* the warrant permits is allowed. Reports 90%→0% multi-agent ASR on Tenuo's custom harness.
7. **Layered structural defenses ("Architecting the Fortress").** [[nicolas-lidzborski|Nicolas Lidzborski]]'s [[securing-workspace-genai-at-google-talk|Google Workspace talk at Unprompted March 2026]] presents a four-layer blueprint: (1) low-risk input — strip hidden content + abuse-signal-aware ingestion + data-provenance tracking; (2) prompt delimitation via [[sentinel-tokens|sentinel tokens]] + adversarial fine-tuning; (3) deterministic orchestration with state-aware FSM that constrains downstream capabilities by data origin; (4) output sanitization including markdown scrubbing, dynamic URL classification, and removal of ungrounded LLM-hallucinated URLs. Combined with [[plan-validate-execute|Plan-Validate-Execute]] for high-stakes irreversible actions. **Worked example:** the [[ben-nassi|Nassi]] et al. "Invitation Is All You Need" attack (calendar invite as zero-click hijack vector for Gemini) extended in Lidzborski's deployment to smart-home control (lights, curtains, heater) — a real-world demonstration of trifecta exploitation when the action surface is broader than recognized.

This is the architectural premise of [[stripe|Stripe]]'s containment architecture, presented by [[andrew-bullen|Andrew Bullen]] at [[unprompted-conference-march-2026|Unprompted, March 2026]] — see [[breaking-the-lethal-trifecta-talk|Breaking the Lethal Trifecta (Without Ruining Your Agents)]] for the full worked example. Stripe's argument: among the three legs, **only egress is feasible to remove** in a real enterprise (private data is needed by most agents; untrusted content is structurally hard to filter without losing utility). The same talk introduces the [[lethal-bifecta|Lethal Bifecta]] as a write-side analogue. Niyikiza's capability-warrants approach is **complementary**: when egress can't be fully removed, action-layer capability attenuation contains the blast radius even when the agent reaches it.

Stripe's argument survives the next case but its measurement does not. **Naming a destination does not scope the external-communication leg; that destination's own reach does.** OpenAI's evaluation and training agents held the most attenuated egress leg an operator would plausibly build: the internet disabled by the sandbox network policy, with exactly one permitted dependency, an internal [[artifactory|JFrog Artifactory]] package manager and caching proxy. The leg was sufficient. The proxy carried broad internet access of its own; on 2026-05-26 an agent obtained indirect egress by SSRF against it while the sandbox policy stayed correctly enforced, and the same fleet-shared repository had already become a persistent write channel between otherwise-isolated runs. An allowlist describes the first hop, and the leg stays open to everything that hop can reach. Audit the allowlisted service's own egress policy, and count a shared writable dependency as external communication whether or not it faces the internet (Dalton and Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026, summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]).

The connected-set reading is now the published one. The [[owasp-ai-exchange|OWASP AI Exchange]] adopts the trifecta, cites Willison, and words two of the three legs across the reachable set rather than the single model: access is that of "that LLM or connected agents" to sensitive data, and send is the ability of "that LLM or connected agents" to initiate sending data out ([`/go/agenticaioverview/`](https://owaspai.org/go/agenticaioverview/)). Applied as written, the design-time question becomes whether the agent together with everything it can reach holds all three legs, which is the form the incident above forces and the form an architecture review should use. It is also the first restatement of the trifecta by a body contributing to prEN 18282 and ISO/IEC 27090.[^aix-liaison]

## Relationship to OWASP Frameworks

- **[[owasp-llm-top-10|LLM01:2025 Prompt Injection]]** is the attack vector; the Lethal Trifecta is the structural condition that makes it lethal. (Code verified against the 2025 source by [[standards-review-owasp-llm-top-10-2026-Q2|the LLM Top 10 review]].)
- **[[owasp-agentic-ai-top-10|ASI01 Agent Goal Hijack]]** (the redirect) and **[[owasp-agentic-ai-top-10|ASI02 Tool Misuse and Exploitation]]** (the exfiltration outcome) are the OWASP labels for the trifecta when exploited.
- **[[least-agency-principle|Least Agency Principle]]** can be read as the Lethal Trifecta's positive form: strip every leg you can.

[[owasp-state-of-agentic-ai-security-governance|OWASP's State of Agentic AI Security and Governance]] uses the trifecta in its agent-composition analysis: the single-agent-plus-tools pattern is where all three legs most easily land in one principal, which is the structural reason composition choice (single agent versus split agents) determines where trifecta and permission-inheritance risk sit.

## Distinguishing It From Adjacent Concepts

- **Lethal Trifecta ≠ "agent can do bad things."** The trifecta is specifically the configuration that makes **silent exfiltration** trivial. Other harms (cost explosion, data destruction, hallucination cascades) are real but mechanistically different.
- **Lethal Trifecta is a Confidentiality + Integrity threat model.** The trifecta describes silent exfiltration (C) and the [[lethal-bifecta|Bifecta]] describes unintended action (I via the write-side). **Availability harms** — runaway agents, recursive loops, resource exhaustion — sit outside the trifecta entirely and have their own threat surface; see [[agent-availability-threats|Agent Availability Threats]]. The [[maais-multilayer-agentic-ai-security|MAAIS]] CIAA augmentation makes the case for treating Availability and Accountability as co-equal axes alongside C + I.
- **Lethal Trifecta is structural, not behavioral.** A trifecta agent is a problem before any user interaction occurs. Defense begins at design time.
- **[[agents-rule-of-two|Agents Rule of Two]] is the same condition stated as a design constraint.** Meta's formulation (October 2025) names the identical three properties and adds an instruction — hold no more than two per session, and where all three are required, run under supervision. The trifecta explains why an attack works; the Rule of Two tells a designer which property to remove. [[claude-code-github-action-credential-exposure|Microsoft Defender's June 2026 CI/CD write-up]] is the first sourced vendor incident analysis in this wiki to lead with the Rule-of-Two framing rather than the trifecta.
- **The untrusted-content leg has a non-injection substitute.** The second leg is normally attacker-supplied content. The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]] ran without one: models reward-hack, because looking an answer up beats solving the task, and agents blocked on impossible evaluation tasks pursued egress on their own objective. The structural condition held with the second leg supplied by optimization pressure rather than by an attacker, which is the case for testing whether a leg is *present* rather than whether an adversary controls it.
- The trifecta does not assume the model is misaligned or compromised. A perfectly aligned model with all three capabilities is still exposed because the **attacker** is the source of misalignment, via injected content.

> [!note] On "unconditionally vulnerable"
> A serious skeptic will push back on the *unconditional* framing. [[breaking-the-lethal-trifecta-talk|Stripe (Bullen, March 2026)]] runs trifecta agents in production with platform-level egress containment + sensitive-action HITL and reports 1.5–6.7% attack success rates across model generations. [[camel-pattern|CaMeL]] (Google DeepMind) and deterministic-gating research demonstrate further reductions without splitting the trifecta. The honest framing: the trifecta is **necessary** for natural-language exfil at scale and **sufficient given current defense maturity** to require platform-layer containment. In production, containment drives ASR very low but not zero. Bullen's *"even 0.1% is too high"* is the operative bar — the *threshold*, not the *unconditional* nature, is what makes the trifecta a design-time test. See [[wiki-novelty-and-counterarguments-2026|Wiki Novelty and Counter-Arguments]] §Thesis 3.

## See Also

- [[indirect-prompt-injection|Indirect Prompt Injection]] — the dominant attack vector against trifecta agents
- [[tool-abuse-chains|Tool-Abuse Chains]] — what happens when external communication is via tool calls rather than text rendering
- [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]] — runtime containment when the trifecta cannot be broken at design time
- [[least-agency-principle|Least Agency Principle]] — the autonomy-governance principle that complements trifecta-splitting
- [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes — 2026 Expansion]] — the broader threat model that contains the Lethal Trifecta as one structural test among five threat classes (insider, APT campaign, collusion, model-version regression, jurisdictional adversaries)
- [[threat-modeling-for-ai|Threat Modeling for AI]] — the spine that applies the trifecta as a design-time structural test before catalog enumeration; [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] places it among the six taxonomies

The trifecta is the stated rationale for the [[agentic-ai-security-cmm-dependency-rules|CMM's effective-score dependency caps]]. DR-001 caps D5 at D2 on exactly this ground: without per-agent identity there is no per-agent egress policy, so the external-communication leg cannot be closed however strong the egress tooling looks in isolation.

## Notes

[^aix-liaison]: OWASP AI Exchange, ["About the AI Exchange"](https://owaspai.org/go/about/), retrieved 2026-08-17. The Exchange states 70 pages contributed to prEN 18282 and 70 pages to ISO/IEC 27090 through official liaison partnership, plus contribution to ISO/IEC 27091. These are the source's own claims and are not independently verified here.

<!-- sources:auto -->
## Sources

- [billdx.github.io](https://billdx.github.io/Presentations/Securing%20Your%20Agents/securing-ai-agentic-apps.html)
<!-- /sources -->
