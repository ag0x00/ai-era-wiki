---
type: concept
title: "Prompt Injection"
address: c-000114
created: 2026-05-24
updated: 2026-08-25
origin: aggregated
tags:
  - concepts
  - prompt-injection
  - llm-security
  - owasp-llm
status: developing
scope_axis: [sec-of-ai]
complexity: basic
domain: llm-security
aliases:
  - Prompt Injection
related:
  - "[[threat-modeling-for-ai]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[indirect-prompt-injection|Indirect Prompt Injection]]"
  - "[[recursive-prompt-injection|Recursive Prompt Injection]]"
  - "[[network-layer-prompt-injection-containment|Network-Layer Prompt Injection Containment]]"
  - "[[prompt-injection-containment|Prompt Injection Containment]]"
  - "[[lethal-trifecta|Lethal Trifecta]]"
  - "[[syara-semantic-detection-talk]]"
  - "[[nist-ai-600-1]]"
  - "[[mitre-atlas]]"
  - "[[standards-review-owasp-llm-top-10-2026-Q2]]"
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
  - "[[memory-poisoning|Memory Poisoning]]"
  - "[[orchestration-hijacking|Orchestration Hijacking]]"
  - "[[camel-pattern|CaMeL Pattern]]"
  - "[[sentinel-tokens|Sentinel Tokens]]"
  - "[[agent-message-structure-manipulation|Agent Message Structure Manipulation]]"
sources:
  - https://genai.owasp.org/llmrisk/llm01-prompt-injection/
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
---

# Prompt Injection

## On this page

- [Definition](#definition)
- [Variants on the wiki](#variants-on-the-wiki)
- [Basis for its load-bearing status](#basis-for-its-load-bearing-status)
- [Testing resistance](#testing-resistance)
- [See also](#see-also)

## Definition

Prompt injection is an attack that supplies adversarial input so a language model treats attacker-controlled text as trusted instructions. It is ranked [[owasp-llm-top-10|`LLM01:2025`]] in the OWASP Top 10 for LLM Applications, the #1 risk verified against the 2025 source by [[standards-review-owasp-llm-top-10-2026-Q2|the LLM Top 10 standards review]]. The model has no reliable boundary between developer instructions, user input, and content it retrieves, so text in any of those channels can redirect its behavior.

The class is recognized across the standards bodies the wiki tracks: [[mitre-atlas|MITRE ATLAS]] catalogs prompt injection as an adversarial technique against language-model systems (`AML.T0051`, LLM Prompt Injection), and the [[nist-ai-600-1|NIST AI 600-1]] GenAI Profile names prompt injection (both direct and indirect) under its information-security risk category, where its Suggested Action `MS-2.7-007` directs deployers to red-team resilience against it.

## Variants on the wiki

- [[indirect-prompt-injection|Indirect prompt injection]] — the payload arrives through content the model retrieves (a web page, document, or tool output) rather than the user's direct prompt.
- [[recursive-prompt-injection|Recursive prompt injection]] — injected instructions that propagate through agent-to-agent or multi-step chains.
- [[network-layer-prompt-injection-containment|Network-layer containment]] and [[prompt-injection-containment|prompt injection containment]] — the defensive practices.
- [[memory-poisoning|Memory poisoning]] — a planted payload that persists in a store and fires on later retrievals.
- [[orchestration-hijacking|Orchestration hijacking]] — injected content redirecting a planner or a delegation chain.
- [[agent-message-structure-manipulation|Agent message structure manipulation]] — the same objective reached through protocol fields rather than through text content.
- [[camel-pattern|CaMeL]] and [[sentinel-tokens|sentinel tokens]] — the containment and delimitation techniques.
- [[lethal-trifecta|Lethal trifecta]] — the exposure test that decides which deployments carry the highest injection risk.

The [[owasp-ai-exchange|OWASP AI Exchange]] names three further subtypes that appear when the model runs as an agent.[^aix-pi] **In-context manipulation** injects adversarial content into the active context window during a session; effects accumulate across turns until the context is reset, where a single-turn injection acts once. **Stored injection** persists the payload in a data store — a retrieval index, a shared document, a database — for retrieval in later sessions, and is the mechanism the wiki records as [[memory-poisoning|memory poisoning]]. **Multi-agent propagation** tricks a low-privileged agent into asking a higher-privileged agent to act on its behalf, which is a confused-deputy escalation rather than a second injection.

## Basis for its load-bearing status

Prompt injection is one leg of the [[lethal-trifecta|lethal trifecta]] (untrusted input, private data access, exfiltration channel). The Exchange states the position as a rule: prompt injection is not solvable at the model layer alone, and static or model-only defenses evaluated against a fixed set of example attacks provide no security guarantee against an adaptive adversary.[^aix-pi] The reason is structural. User data and system commands occupy one context plane, and the model has no parameterized-query equivalent that would let an application declare which span is data.[^aix-pi] In agentic deployments this shifts the dominant risk from safety violations in generated text to integrity compromises, where the adversary hijacks the agent's actions through its tools and their side effects.[^aix-pi] It is the recurring entry vector across the [[threat-modeling-for-ai|threat-modeling spine]]'s worked example: ASI01 goal hijack, ASI06 memory poisoning, and ASI02 tool misuse all begin with injected content. The [[threat-taxonomy-reconciliation|reconciliation matrix]] maps it to the Runtime plane (`LLM01`, `AML.T0051`), where detection is capped by the policy decision that follows.

### Testing resistance

The Exchange's own test procedure for prompt injection answers the "fixed set of example attacks" problem in one step and bounds what that step buys. Step 5 adds variation algorithms to the attack corpus — synonym substitution, encoding, reformatting — which the procedure itself describes as an evasion attack against the detection mechanisms in place, whether those sit in the model's own training and system prompts or in detectors external to it.[^aix-testing] The Exchange then states the limit: variation-tested robustness has limited impact on estimated risk, because widely available tooling means some robustness only defers an attacker who invests no effort, and for attacks with severe impact the argument does not apply at all — a system is expected to resist an attacker who does invest effort. Two further constraints on the test carry the same weight. Attack inputs are presented to the system API rather than to the model, so the production filtering and detection stack is in the path. And the test is run repeatedly, because a model's non-determinism makes a single pass or fail an unreliable reading.

## See also

- [[syara-semantic-detection-talk|SYARA Semantic Detection]] — a detection-side response: semantic rules that catch injection intent at scale.

> [!gap] Remaining depth
> This is the parent concept for the wiki's injection-variant pages. Detection limits are now carried on [[prompt-injection-containment|Prompt Injection Containment]] and the capability-constraint approach on [[camel-pattern|CaMeL]]. Still to add: the spotlighting research, and a treatment of multimodal injection, which the Exchange states is a live instruction channel because models fuse visual and textual, and sometimes audio, embeddings into a shared latent space.[^aix-dpi]

## Notes

[^aix-pi]: [OWASP AI Exchange — Prompt injection](https://owaspai.org/go/promptinjection/), retrieved 2026-08-18. The threat group, the shared context plane and the absent parameterized-query equivalent, the agentic risk shift toward integrity compromise, the three agentic subtypes, and the model-layer statement.
[^aix-dpi]: [OWASP AI Exchange — Direct prompt injection](https://owaspai.org/go/directpromptinjection/), retrieved 2026-08-18. Multimodal input as an instruction channel, on the stated basis that models fuse visual and textual, and sometimes audio, embeddings into a shared latent space.
[^aix-testing]: [OWASP AI Exchange — Testing against Prompt injection](https://owaspai.org/go/testingpromptinjection/), retrieved 2026-08-19. The variation-algorithm step, its framing as an evasion attack against the detection mechanism, the stated limited impact on estimated risk, the production-API presentation requirement, and the repeated-run requirement against model non-determinism.

<!-- sources:auto -->
## Sources

- [genai.owasp.org](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
<!-- /sources -->
