---
type: talk
title: "Breaking the Lethal Trifecta (Without Ruining Your Agents)"
created: 2026-05-02
updated: 2026-08-21
tags:
  - papers
  - talks
  - prompt-injection
  - lethal-trifecta
  - egress-control
  - mcp-security
  - human-in-the-loop
  - tool-annotations
  - stripe
  - unprompted-2026
status: summarized
scope_axis:
  - sec-of-ai
year: 2026
authors: ["Andrew Bullen"]
venue: "Unprompted Conference, San Francisco — Stage 1 Lecture 04, Wednesday March 4, 2026, 11:20"
license: "Conference talk; slides via Stripe; transcript via attendee Google Drive share"
key_claim: "Prompt injection is unsolved at the model layer (1.5–6.7% attack-success rate even on top models), so containment must be architectural. Stripe breaks the Lethal Trifecta by removing the one leg that's actually feasible to control — egress — and treats sensitive writes as a separate axis (\"Lethal Bifecta\") gated by human-in-the-loop. The hard part is not the architecture; it's making it adoptable: safe-search proxies, SaaS-MCP proxying via Toolshed, queued/batched/optimistic confirmations, and CI-time tool-annotation enforcement so the policy survives across many agent frameworks."
methodology: "Practitioner talk by Stripe's Head of AI Security, framed as a 'security leadership talk' rather than a research disclosure. Argues containment > prevention because 'we have to assume prompt injection will happen.' Case-grounded in four 2025 incidents (CVE-2025-62453, Claude→Stripe-coupon jailbreak, Cursor npm credential-stealing backdoor, Slack AI exfil) and an arXiv attack-rate study."
contradicts: []
supports:
  - "[[lethal-trifecta]]"
  - "[[prompt-injection-containment]]"
  - "[[least-agency-principle]]"
  - "[[mcp-security]]"
  - "[[agent-observability]]"
  - "[[oversight-layer]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[capability-based-authorization|Capability-Based Authorization]]"
  - "[[cmm-calibration-stress-test-2026|CMM Calibration Stress Test]]"
  - "[[lethal-bifecta]]"
  - "[[andrew-bullen]]"
  - "[[stripe]]"
  - "[[toolshed]]"
  - "[[smokescreen]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[indirect-prompt-injection]]"
sources:
  - .raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_slides.pdf
  - .raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_transcript.md
source_url: https://drive.google.com/file/d/1RmyvzZtiJTAvtTAQCjheDSjAGKUgYjzV/view
aliases:
  - papers/breaking-the-lethal-trifecta-bullen-talk
  - breaking-the-lethal-trifecta-bullen-talk

---

# Breaking the Lethal Trifecta (Without Ruining Your Agents)

> Unprompted Conference, March 4, 2026 — Stage 1, 11:20. Andrew Bullen, Head of AI Security at [[stripe|Stripe]].
> Subtitle: *Architectural Defenses against Prompt Injection*.

This page combines two source artifacts: the **slides** (13 frames; data + diagrams + the actual `ToolAnnotations` API) and the **speaker transcript** (the war stories, caveats, and Q&A that don't appear in the deck). Where the two sources tell different parts of the story, the section noting which input each claim comes from is preserved.

## TL;DR

- The model layer will not save you — even Claude 3.7 Sonnet:Thinking sees a 1.5% attack-success rate on the published competition (slide 3). For security, "1% is too high."
- Treat prompt injection as **inevitable**. Containment is two architectural rules:
  - **Guardrail 1 (egress):** break the [[lethal-trifecta|Lethal Trifecta]] by removing the External-Communication leg in any agent that touches private data + untrusted content.
  - **Guardrail 2 (sensitive writes):** for the *write*-side analogue (untrusted-content + sensitive-action — Bullen's [[lethal-bifecta|"Lethal Bifecta"]]), require human review of sensitive actions.
- Both guardrails kill agent UX unless you do compensating UX work: safe search, SaaS-MCP proxy, queued/batched confirmations, optimistic writes with reverts.
- Both guardrails decay unless you do compensating enforcement work: tag agentic services + CI-time egress checks (Stripe uses [[smokescreen|Smokescreen]]); tool annotations evaluated automatically (Stripe's [[toolshed|Toolshed]] central MCP); proxying connections out of "deep" agent sandboxes — work-in-progress.

## The threat baseline (from slides only)

Slide 3 cites *"Security Challenges in AI Agent Deployment: Insights from a Large Scale Public Competition"* (arXiv) — attack-success rate on prompt-injection challenges, by model:

| Model | ASR | Provider |
|---|---|---|
| Llama 3.3 70B | 6.7% | Meta |
| Pixtral Large | 6.1% | Mistral |
| Llama 3.1 405B | 5.9% | Meta |
| o3 Mini:High | 4.9% | OpenAI |
| Nova Lite v1 | 4.5% | Amazon |
| Nova Micro v1 | 4.4% | Amazon |
| Grok 2 | 4.4% | xAI |
| o3 Mini | 4.3% | OpenAI |
| Nova Pro v1 | 3.9% | Amazon |
| Command-R | 3.8% | Cohere |
| o3 | 2.9% | OpenAI |
| GPT 4.5 | 2.7% | OpenAI |
| o1 | 2.7% | OpenAI |
| GPT 4o | 2.5% | OpenAI |
| 3.5 Haiku | 2.4% | Anthropic |
| 3.5 Sonnet | 1.9% | Anthropic |
| 3.7 Sonnet | 1.7% | Anthropic |
| 3.7 Sonnet:Thinking | 1.5% | Anthropic |

Slide 4 grounds the abstract numbers in four incidents (headlines verbatim from the deck). Each now has a wiki incident page (ingested 2026-05-03 from primary sources):

- **CVE-2025-62453** — security feature bypass in GitHub Copilot / VS Code (NVD published 2025-11-11). See [[cve-2025-62453-copilot-vscode-prompt-injection|CVE-2025-62453 — Copilot/VS Code AI output validation bypass]].
- **"Claude Jailbroken to Mint Unlimited Stripe Coupons"** (2025-07-16) — direct hit on a Stripe-relevant surface. See [[claude-stripe-coupons-imessage-injection|Claude → Stripe coupons via iMessage metadata spoofing]].
- **"Malicious npm Packages Infect 3,200+ Cursor Users with a Credential-Stealing Backdoor"** (Socket disclosure 2025-05-07; the May 11 date comes from a downstream Medium repost). See [[cursor-npm-credential-stealer|Cursor npm credential stealer — sw-cur, sw-cur1, aiide-cur]].
- **"Data Exfiltration from Slack AI via Indirect Prompt Injection"** (PromptArmor 2024-08-20 — predates Bullen's other three by a year and is the canonical Lethal Trifecta demonstration). See [[slack-ai-private-channel-exfiltration|Slack AI private-channel exfiltration]].

Bullen's framing (transcript): "even a 0.1% failure chance on attack is not enough ... we need something better than just relying on the models." He believes prevalence in consumer settings will get worse before better.

## The two guardrails

### Guardrail 1: Prevent egress

The Lethal Trifecta is **Private Data + Untrusted Content + External Communication**. Bullen's argument (transcript) is feasibility:

| Trifecta leg | Can Stripe remove it across the agent estate? |
|---|---|
| Untrusted Content | **No.** "A big part of why LLMs are useful is that you can throw a bunch of stuff at them"; attackers are creative about smuggling injection into content surfaces. |
| Private Data | **No.** "At Stripe we have a lot — support tickets, emails, etc. Many agents need it." |
| External Communication (egress) | **Yes** — the only feasible leg. |

So Stripe's first guardrail collapses to: **break egress** for any agent that has private data + untrusted content. Two surfaces to control:

1. **General web requests** (the obvious case).
2. **Third-party SaaS connections** — "a very easy way to exfiltrate data would be writing it to a public Google Doc with a Google Doc tool." Less obvious; just-as-effective.

### Guardrail 2: Human review of sensitive actions (the "Lethal Bifecta")

Slide 8 / transcript: data exfil isn't the only outcome of prompt injection. **Bad writes** need only **two** ingredients: untrusted content + the agent's ability to take a sensitive action. Bullen calls this the [[lethal-bifecta|Lethal Bifecta]] — a Bullen coinage on the same naming pattern as Willison's trifecta, addressing the *write* side rather than the *read/exfil* side.

Same feasibility argument: untrusted content can't be removed, so you control the other leg — **route every sensitive action through human review**. "Sensitive is very load-bearing here. Generally the rule of thumb: anything that is a production write or broad communication or sending a message."

## The compensating UX work (without which the guardrails are unadoptable)

Bullen's framing (transcript, repeated in his concluding remark): *"Step one threat model, step two mitigation, step three figure out how to put in place the mitigation in a way that the business still can achieve its goal."* The talk is mostly step three. Slide 9 shows the failure mode he's trying to avoid — the Claude Code permission dialog interrupting `rm -r foo` over and over, until users just enable `dangerously-skip-permissions`.

### Egress-side UX

| Pain point | Stripe's solution |
|---|---|
| "We need public Internet data" | **Safe Search** — built on OpenAI Web Search with `external_web_access: false`, so the agent can request a search and receive cached results without itself triggering egress. Bullen's honest caveat (transcript only): *"That's not really making anything any safer. … you prevent the data from being egressed by your agent, and instead it just gets egressed by OpenAI."* The trust is shifted, not eliminated. |
| "I want to use this SaaS MCP" | **Proxy third-party MCPs through [[toolshed\|Toolshed]]** — Stripe's central MCP proxy. Rules can be written at the proxy: e.g. don't allow connections to non-Stripe tenants when writing to Google Docs / Figma / etc. Side benefit: users connect once to one MCP server, not N. |

### Sensitive-write-side UX

Slide 11 enumerates three pain points; each has a concrete countermeasure:

| Pain point | Countermeasure |
|---|---|
| Confirmations interrupt the agent | **Queue and batch** non-blocking confirmations — let the agent keep working, surface them later. Slide 11 shows a UI mockup of a "Pending Actions (3 queued)" approve/reject panel. (Slide labels: "this is a mockup btw.") |
| Review fatigue | **Optimistic writes with reverts** — execute reversible writes immediately and offer revert; keeps the agent moving. |
| Rubber-stamping | **LLM-as-second-reviewer** (transcript only — not on the slides) — when policy gets sophisticated, an automated reviewer can quickly tell the agent "you're trying to do something bad" before the human gets pinged. |

## Enforcement (slide 12 + transcript)

Bullen treats enforcement as the third design problem. Two surfaces:

### Egress enforcement

Stripe pre-existed the AI agent era with a strong egress program. The mechanism:

1. **Tag every service that's an agent.** Easy in practice — to be an agent, the service has to talk to a foundation model, and Stripe routes those through a known proxy. (Transcript: *"You could use whether your service talks to Bedrock or whatever as a way of doing this."*) This is a generalizable pattern beyond Stripe.
2. **CI-time check** — if the service is tagged-as-agent, you can't configure egress without an escalated review process.

The OSS implementation Stripe leans on for the network-side egress proxy is [[smokescreen|Smokescreen]] (open-source).

### Sensitive-write enforcement: tool annotations

Slide 12 shows the actual API. Inline Ruby example from the deck:

```ruby
class SampleGoogleTool
  def self.annotations
    ToolAnnotations.new(
      production_impacting_write: false,
      data_sensitivity: :medium,
      broadcasts_data_internally: false,
    )
  end
end
```

Two policy surfaces:

- Tools authored **inline in agent frameworks** carry the annotations directly.
- Tools exposed via Stripe's central **MCP service ([[toolshed|Toolshed]])** carry the same annotations at the registration boundary.

The framework reads annotations and decides whether human review applies. Centralizing it gives one UX surface to improve over time.

### The hard problem (work-in-progress, transcript only)

> "Increasingly, agents don't need special tools — they'll just write their own code and hit random APIs on your existing services. So what do we do here? This is, like, work that is in progress right now, but the approach we're looking at is essentially proxying the connections coming out of agents, out of their sandboxes, and then using that as a choke point where you can similarly have annotations on the API endpoints that the agents are talking to."

This is the unsolved piece on the slide too — slide 12 ends with the question "What about Claude Code style 'Deep' Agents?" but no answer. **Important caveat** for any downstream wiki claim: Stripe's tool-annotation enforcement, as of Unprompted March 2026, does NOT yet cover deep / code-writing agents that bypass declared tools.

## Q&A (transcript only)

**Two questions worth quoting verbatim.**

**Q1** (group/team aggregation of human-in-the-loop reviews): *"We're early in the UX experimentation process … in general, trying to find ways to make it so that people need to stop less, fewer checks, fewer reviews need to be made, while ensuring that human judgment is applied at the right time is going to be sort of the North Star."*

**Q2** (still doing threat modeling for prompt injection?): *"100% there's a place for detective and other types of controls that aren't guarantees. Especially for customer-facing products. But ultimately, because we're not at the point where we can fully trust those, we really want to lean on these more deterministic, architectural controls."*

The Q&A confirms a **methodological hierarchy** Stripe applies: deterministic architectural controls dominate; behavioral / detective controls are supplementary, especially for surfaces (consumer-facing) where the architectural lever is weaker.

## Placement in this wiki

| Wiki artifact | Update from this talk |
|---|---|
| [[lethal-trifecta\|Lethal Trifecta]] | Existing concept page; this talk supplies the canonical practitioner worked example. The concept page already pointed at "the Stripe Lethal Trifecta containment architecture (pending elevation)" — this page partially fulfills that promise. |
| [[lethal-bifecta\|Lethal Bifecta]] | **NEW** concept — Bullen's coining. Pairs with the trifecta on the write side. |
| [[prompt-injection-containment\|Prompt Injection Containment for Agentic Systems]] | Three new concrete patterns: agent-tag + CI egress check, SaaS-MCP proxying, ToolAnnotations declarative schema. |
| [[mcp-security\|MCP Security]] | "Toolshed" central MCP is a vendor-named instance of the proxy-MCP-for-policy pattern. |
| [[oversight-layer\|Oversight Layer (PDP + PEP for Agentic AI)]] | The Toolshed proxy + tool annotations are a concrete PDP/PEP implementation (PDP = annotation policy; PEP = MCP proxy). |
| [[agentic-ai-security-reference-architecture\|Agentic AI Security Reference Architecture]] | Direct evidence for Egress plane (Smokescreen + agent-tag CI), Control plane (annotations), Identity plane (per-tenant rules at Toolshed). |
| [[agentic-ai-security-cmm-2026\|Agentic AI Security Capability Maturity Model]] | L3/L4 evidence for D3 (decision rights via annotations), D4 (ToolAnnotations review policy), D5 (Egress & Network, via tagged services + CI). |
| [[stripe\|Stripe]] | Promotes the entity from stub to real page (Toolshed, Smokescreen, Bullen, Andrew's containment architecture). |
| [[toolshed\|Toolshed (Stripe)]] | **NEW** product stub. |
| [[smokescreen\|Smokescreen (Stripe)]] | **NEW** product stub. |
| [[andrew-bullen\|Andrew Bullen]] | **NEW** people stub. |
| [[unprompted-conference-march-2026\|Unprompted Conference — AI Security Practitioner Conference (March 3–4, 2026)]] | The "Tier 1 #1" row now resolves to this summary. |

## Slides-only vs transcript-only — what each input added

This was the first ingest of the wiki using **paired slides + transcript**. The split below is so future readers can see what each input contributed (and so we know what we'd have lost by ingesting only one).

**Slides only:**
- Hard ASR numbers per model (1.5–6.7%) and the exact arXiv title.
- The four named CVE / incident headlines (CVE-2025-62453; Claude→Stripe coupons; Cursor npm 3,200+; Slack AI exfil).
- The actual `ToolAnnotations` Ruby API with field names (`production_impacting_write`, `data_sensitivity`, `broadcasts_data_internally`).
- The "Pending Actions" UI mockup design.
- The trifecta + bifecta diagrams as pictures.

**Transcript only:**
- Names of the implementations: **Toolshed** (central MCP) and **Smokescreen** (egress proxy).
- The honest caveat that Safe Search via OpenAI shifts trust, doesn't eliminate it.
- The "Lethal Bifecta" coinage (slide just says "Bad Writes are even simpler…").
- LLM-as-second-reviewer as a future direction.
- Q&A (group-level review aggregation, threat-modeling-vs-architectural-controls hierarchy).
- The "deep agents" work-in-progress: proxying connections out of agent sandboxes as the next chokepoint.
- The agent-tag heuristic: "to be an agent, you need to talk to foundation models" — generalizable to any org with a model proxy.

If we'd ingested only the slides we'd have the architecture but not the names (Toolshed/Smokescreen) or the limits (deep agents). If we'd ingested only the transcript we'd have the names and limits but no exact API field names, no model ASR numbers, and no incident citations.

## See also

- [[lethal-trifecta|Lethal Trifecta]] · [[lethal-bifecta|Lethal Bifecta]] · [[indirect-prompt-injection|Indirect Prompt Injection]] · [[mcp-security|MCP Security]]
- [[unprompted-conference-march-2026|Unprompted Conference — AI Security Practitioner Conference (March 3–4, 2026)]]
- Companion talk (Day 1, same Stripe org): [[guardrails-beyond-vibes-talk|Guardrails Beyond Vibes: Shipping Security Agents in Production]] by [[jeffrey-zhang|Jeffrey Zhang]] + [[sid-shah|Siddh Shah]] — covers how Stripe *builds and evaluates* production security agents (threat modeling + routing agents, LLM-as-a-Judge eval pipeline, AlphaEvolve failure). This talk covers how to *contain* them. **Both are now fully ingested.**

- [[capability-based-authorization|Capability-Based Authorization]] and [[capability-based-authorization-talk|Niyikiza's talk]] — the same containment posture reached from the authorization side. The two stack rather than compete: this talk contains via egress and tool annotation, Niyikiza via delegation-aware capability attenuation.
- [[cmm-calibration-stress-test-2026|CMM Calibration Stress Test]] — the `[!contradiction]` callout on the CMM's D7 scoring was added during this talk's follow-up, and records that architectural containment of this kind can rationally score lower on observability while remaining defensible.
