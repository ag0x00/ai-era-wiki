---
type: entity
entity_type: organization
org_type: advisory
title: "Apollo Research"
created: 2026-05-02
updated: 2026-05-23
tags:
  - entities
  - organizations
  - alignment-research
  - safety-evals
status: stub
website: "https://www.apolloresearch.ai/"
focus: "AI safety evaluations, deception detection, multi-agent collusion"
related:
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[anthropic]]"
  - "[[openai]]"
  - "[[aisi-uk]]"
sources:
  - "https://arxiv.org/abs/2402.07510"
  - "https://www.apolloresearch.ai/research/deception-probes"
---

# Apollo Research

UK-based AI safety evaluation organization focused on **deception detection, scheming evaluations, and multi-agent collusion**. The load-bearing source for the wiki's coverage of agent-agent collusion as a threat class. See [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes — 2026 Expansion]] §Class 3.

## Notable contributions

- **Secret Collusion among AI Agents: Multi-Agent Deception via Steganography** (NeurIPS 2024, [arXiv:2402.07510](https://arxiv.org/abs/2402.07510)): formal threat model for steganographic agent-to-agent collusion that bypasses output-monitoring oversight.
- **Detecting Strategic Deception Using Linear Probes** (2025, [apolloresearch.ai/research/deception-probes](https://www.apolloresearch.ai/research/deception-probes)): detection technique for scheming/deception in single agents via residual-stream probes.
- **Detecting and Reducing AI Scheming** (2025, with OpenAI): pre-deployment evals for scheming.

## Relationship to other safety bodies

Independent of (but cooperative with) [[aisi-uk|UK AISI]], [[anthropic|Anthropic]], and [[openai|OpenAI]]. Apollo's work is frequently cited in frontier-vendor pre-deployment system cards and in [[aisi-uk|AISI]] joint evaluations.

## See Also

- [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes — 2026 Expansion]], primary citation
- [[aisi-uk|UK AI Safety Institute]], peer organization
