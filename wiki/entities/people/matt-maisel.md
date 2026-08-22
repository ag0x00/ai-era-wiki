---
type: entity
entity_type: person
title: "Matt Maisel"
created: 2026-05-03
updated: 2026-05-03
tags:
  - people
  - cedar
  - coding-agents
  - policy-engine
  - reference-monitor
  - unprompted-2026
status: seed
related:
  - "[[sondera]]"
  - "[[hooking-coding-agents-with-cedar-talk]]"
  - "[[cedar]]"
  - "[[unprompted-conference-march-2026]]"
sources:
  - .raw/talks/2026-03-03_Matt-Maisel_Hooking-Coding-Agents-with-Cedar_transcript.md
---

# Matt Maisel

CTO and Co-Founder of [[sondera|Sondera]]. 15 years at the intersection of machine learning, security, and software engineering.

Presented "[[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar]]" at Unprompted Conference, Stage 2, March 3, 2026.

> [!stale] Earlier name confusion (corrected 2026-05-04)
> The session-6 hot cache used "Sondera Maisel" as if the org name were part of the person's name. The auto-generated transcript also misspelled the org as "Sendera." The correct spelling, confirmed by the slide deck and the company homepage at [sondera.ai](https://www.sondera.ai), is **Sondera**. See [[sondera|Sondera]] for the org page.

## Contributions to this wiki

- Introduced the **trajectory event model** (action / observation / control / state) as the structural unit for coding-agent policy enforcement.
- Built the **hook-based Cedar harness** — open-source reference implementation that wires coding-agent lifecycle hooks (Cursor, Claude Code, Gemini CLI) to a Cedar PDP with YARA signatures, information-flow control, and a safety model.
- Proposed the **policy agent pattern** — using an LLM to generate Cedar policies from informal intent (CLAUDE.md, Cursor Rules), then verifying them with Cedar's own formal tools exposed over MCP.

## See also

- [[sondera|Sondera]] — the company he co-founded
- [[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar (talk summary)]]
- [[cedar|Cedar]] — the policy language at the center of his architecture
