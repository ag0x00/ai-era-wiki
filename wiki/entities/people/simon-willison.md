---
type: entity
title: "Simon Willison"
created: 2026-04-30
updated: 2026-04-30
tags:
  - entities
  - people
  - prompt-injection
  - llm-security
status: stub
entity_type: person
role: "Independent researcher and writer; co-creator of Django; LLM-security commentator"
affiliation: "Independent (simonwillison.net); Datasette"
related:
  - "[[lethal-trifecta]]"
  - "[[indirect-prompt-injection]]"
  - "[[securing-your-agents-talk]]"
sources:
  - "[[.raw/talks/securing-your-agents-2026-04-30.md]]"
---

# Simon Willison

Independent researcher and writer at simonwillison.net, co-creator of Django, and creator of Datasette. Since 2022 has been one of the most-cited public commentators on prompt injection and LLM security. Coined the term **[[lethal-trifecta|"Lethal Trifecta for AI Agents"]]** in June 2025 — the formulation that an agent with (1) access to private data, (2) exposure to untrusted content, and (3) the ability to externally communicate can be tricked by an attacker into exfiltrating the private data.

The Lethal Trifecta has been adopted as a load-bearing framing in subsequent practitioner work, including the [[securing-your-agents-talk|Securing Your Agents]] deck (Bill McIntyre, 2026), Stripe's containment-architecture writing (see [[stripe|Stripe]]), and as a shorthand in the [[owasp-agentic-ai-top-10|OWASP Agentic Top 10]] discussion.

## Notable Contributions to the Field

- "The Lethal Trifecta for AI Agents" — simonwillison.net, June 2025
- Long-running blog coverage of prompt-injection attacks, indirect injection vectors, and tool-call exploitation across major model releases
- Public-facing analysis of agent kill chains (e.g., GeminiJack, Jules-class compromises) before they appeared in vendor disclosures

## See Also

- [[lethal-trifecta|Lethal Trifecta]] — the concept page
- [[indirect-prompt-injection|Indirect Prompt Injection]] — the attack class Willison's writing emphasizes
- [[johann-rehberger|Johann Rehberger]] — fellow public-research voice on agent security
