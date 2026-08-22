---
type: concept
title: "Lethal Bifecta"
created: 2026-05-02
updated: 2026-07-30
tags:
  - concepts
  - prompt-injection
  - threat-modeling
  - agentic-ai
  - human-in-the-loop
status: developing
no_public_url: "Coined by Andrew Bullen (Stripe) in a private conference talk (Unprompted 2026); no public canonical document"
aliases:
  - "Bullen Bifecta"
  - "Lethal Bifecta (sensitive-write)"
related:
  - "[[lethal-trifecta]]"
  - "[[threat-modeling-for-ai]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[indirect-prompt-injection]]"
  - "[[prompt-injection-containment]]"
  - "[[least-agency-principle]]"
  - "[[decision-rights]]"
  - "[[andrew-bullen]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[standards-review-owasp-llm-top-10-2026-Q2]]"
  - "[[agents-rule-of-two]]"
sources:
  - "[[.raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_slides.pdf]]"
  - "[[.raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_transcript.md]]"
---

# Lethal Bifecta

## Definition

Coined by [[andrew-bullen|Andrew Bullen]] (Head of AI Security, [[stripe|Stripe]]) at the [[unprompted-conference-march-2026|Unprompted Conference, March 2026]] as the **write-side analogue** of [[lethal-trifecta|Simon Willison's Lethal Trifecta]]. The trifecta describes the conditions under which an agent silently exfiltrates data; the bifecta describes the (simpler) conditions under which an agent takes a damaging *action*:

1. **Untrusted content**: the agent ingests content the attacker can influence.
2. **Sensitive action**: the agent has the capability to take a write/communication/destructive action with material consequence.

When both hold, an [[indirect-prompt-injection|indirect prompt injection]] can steer the agent into a harmful action without the attacker needing to compromise any system other than the agent's input surface.

## Basis for a separate term

The Lethal Trifecta has *three* legs because exfiltration requires distinguishing **read** (private data) from **send** (external comms): the attacker has to pull data through both stages. A damaging *write* skips the read step: the attacker is not extracting your data, they are using your agent's privileges to do something to the world. So the threat condenses to two ingredients.

This separation matters architecturally:

- **Trifecta containment** is mostly about removing the egress leg (Stripe's Guardrail 1).
- **Bifecta containment** is mostly about gating the action leg with human review (Stripe's Guardrail 2).

The two guardrails don't overlap operationally: egress controls don't catch a destructive write to your own production database, and action-review doesn't catch a quiet POST to attacker.com.

## "Sensitive" is load-bearing

Bullen (transcript): *"Sensitive is very load-bearing here. Generally, the rule of thumb is that anything that is a production write or a broad communication or sending a message are the big things that we think of as sensitive actions."*

Implication: most agent flows have many tool calls and only some of them are sensitive. The architectural lift is *classifying* writes, not gating *all* writes; hence the [[breaking-the-lethal-trifecta-talk|`ToolAnnotations` schema]] (`production_impacting_write`, `data_sensitivity`, `broadcasts_data_internally`).

## Containment patterns (from Stripe's worked example)

1. **Annotate every tool / API endpoint** with a sensitivity classification. The annotation is the policy.
2. **Force human review** on tools/endpoints whose annotation crosses a threshold. The framework injects the review step automatically.
3. **Compensate for review fatigue.** Without compensating UX, the bifecta defense degrades to rubber-stamping. Patterns: queue + batch confirmations; optimistic writes with reverts; LLM-as-second-reviewer for fast obvious-bad-action triage.
4. **Cover the deep-agent case.** Where the agent writes its own code that bypasses declared tools, the annotation has to live on the *API endpoint*, not the tool. (This is unsolved in Stripe's published architecture as of March 2026.)

## Distinguishing it from adjacent concepts

- **Lethal Bifecta vs Lethal Trifecta.** Same family, different harm. Trifecta = silent exfil. Bifecta = damaging action. An agent can be vulnerable to one and not the other.
- **Lethal Bifecta vs [[least-agency-principle|Least Agency Principle]].** Least agency is the broader governance principle ("strip every capability you can"); the bifecta is the specific structural test for the *write* side, parallel to the trifecta's structural test for the *read* side.
- **Lethal Bifecta vs [[decision-rights|Decision Rights for AI Agents]].** Decision rights are the *governance documentation* of which writes need approval; the bifecta is the *threat-model justification* for why those decision rights exist on the action axis specifically.
- **Lethal Bifecta vs [[agents-rule-of-two|Agents Rule of Two]].** The Rule of Two restates the trifecta's property set as a design constraint with a defined fallback (supervision). It does not decompose the read and write sides the way the trifecta/bifecta pair does, so the bifecta remains the sharper instrument when the harm in question is a damaging write rather than an exfiltration.

## Relationship to OWASP frameworks

- **[[owasp-llm-top-10|LLM01:2025 Prompt Injection]]**: attack vector, shared with the trifecta. Code verified against the 2025 source by [[standards-review-owasp-llm-top-10-2026-Q2|the LLM Top 10 review]].
- **[[owasp-agentic-ai-top-10|ASI02 Tool Misuse and Exploitation]]**: the agentic taxonomy's label for the bifecta's outcome — a legitimate tool or action weaponized into a damaging write.
- **[[threat-modeling-for-ai|Threat Modeling for AI]]**: the spine applies the bifecta as the write-side structural test alongside the trifecta; the [[threat-taxonomy-reconciliation|reconciliation matrix]] records both as design-time go/no-go checks.

## Provenance

Single-source-coined by Bullen at Unprompted (March 4, 2026); slide title was *"Bad Writes are even simpler…"* with the diagram showing Untrusted Content + Sensitive Actions side-by-side. The "Lethal Bifecta" name appears in the transcript only — Bullen acknowledges *"there isn't a term officially for the things you need in order to have prompt injection deal damage by taking a sensitive action, but I guess, lacking something better, I will call this the lethal bifecta."*

> [!stale] Term provenance — single-source
> "Lethal Bifecta" is currently a Bullen-only neologism. If it doesn't catch on in the OWASP / NIST / Willison-aligned vocabulary by Q4 2026, downgrade this page to a redirect-style stub pointing at [[lethal-trifecta|Lethal Trifecta]] §"write-side variant." Track adoption.
