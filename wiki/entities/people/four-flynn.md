---
type: entity
entity_type: person
title: "Four Flynn"
address: c-000322
created: 2026-08-24
updated: 2026-08-24
tags:
  - entities
  - people
  - google
  - deepmind
  - codemender
  - vuln-patching
  - ai-in-sec-defense
status: seed
scope_axis:
  - ai-in-sec-defense
origin: aggregated
role: "VP of Security and Privacy, Google DeepMind; leads the CodeMender programme"
affiliation: "[[google|Google DeepMind]]"
first_mentioned: "[[autonomous-code-security-google-talk]]"
related:
  - "[[google]]"
  - "[[codemender]]"
  - "[[big-sleep]]"
  - "[[heather-adkins]]"
  - "[[autonomous-code-security-google-talk]]"
  - "[[google-codemender-deepmind]]"
  - "[[llm-as-a-judge]]"
  - "[[vulnops]]"
sources:
  - "[[autonomous-code-security-google-talk|Autonomous Code Security at Google (Unprompted March 2026)]]"
  - "[[.raw/talks/2026-03-03_Heather-Adkins-and-Four-Flynn_Evaluating-Threats-Automating-Defense_transcript.md]]"
  - "https://unpromptedcon.org/abstract-march2026/"
---

# Four Flynn

VP of Security and Privacy at Google DeepMind, where he leads security for [[google|Google]]'s frontier-model lab and runs the [[codemender|CodeMender]] programme. He started CodeMender in 2025 against the **vulnpocalypse** — the volume of vulnerabilities that agentic discovery systems are about to produce — on the position that finding bugs at machine speed is worth little unless fixes arrive at the same speed.

At [[unprompted-conference-march-2026|Unprompted March 2026]] he presented [[autonomous-code-security-google-talk|Autonomous Code Security at Google]] with [[heather-adkins|Heather Adkins]], covering the patching half of Google's pipeline while Adkins covered discovery. His account put CodeMender's differentiator in the validation stack rather than in patch generation — fuzzing before and after, formal verification of functional equivalence, differential testing, and [[llm-as-a-judge|LLM judges]] — and stated the design intent as an end-to-end engine that runs completely without human intervention, naming no reviewer at submission. He gave the programme's open-source output as 178 fixes, which the deck splits into 48 patches and 130 hardening changes.

Flynn raises a criticism of his own position — that the programme verifies too heavily before releasing anything — and defends the trade-off on the grounds that the community adopts the technology only as far as it trusts the output. He names the redeployment of automatically mended code as the problem he cannot solve: "one of the hardest problems with patching is actually places in the world that struggle to actually apply patches in a timely manner… I don't know how to solve that with AI."
