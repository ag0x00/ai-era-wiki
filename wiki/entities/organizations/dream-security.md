---
type: entity
entity_type: organization
org_type: vendor
title: "Dream Security"
address: c-000265
created: 2026-08-15
updated: 2026-08-15
tags:
  - entities
  - organization
  - vendor
  - threat-research
status: developing
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
homepage: "https://www.dreamgroup.com"
role: "Israeli AI-native cybersecurity vendor; its Threat Research team (DREAM Lab) recovered and published the operational workspace of the [[taiwan-ai-agent-government-intrusion|Taiwan multi-agent AI intrusion]]"
related:
  - "[[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]]"
  - "[[dream-taiwan-multi-agent-ai-attack|Taiwan Multi-Agent Attack Reconstruction]]"
  - "[[hermes-agent|Hermes]]"
  - "[[openclaw|OpenClaw]]"
  - "[[offensive-agent-collective|Offensive Agent Collective]]"
sources:
  - "https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia"
---

# Dream Security

Israeli cybersecurity vendor building AI-native defensive products; its research arm, DREAM Lab, publishes threat-intelligence analysis of adversary tradecraft.

## Relevance to This Wiki

DREAM Lab's Threat Research team recovered the complete operational workspace — over 160 megabytes, 1,395 files — of a near-autonomous multi-agent attack framework running against government targets in Asia, and published a structural reconstruction of its architecture, decision logic, and confirmed impact.[^dream] The account is the primary technical source for the [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]]; Dream shared its findings first with the *Financial Times*, and Taiwan's Ministry of Digital Affairs issued a confirming statement the following day, reported by Reuters.

Dream's report is the wiki's first primary-source technical reconstruction of a threat-actor's own multi-agent orchestration internals — decision thresholds, sub-agent role assignment, self-correction logs — rather than a victim-side account of what was affected, which is the vantage point [[openai-hugging-face-incident-blackhat-2026|the OpenAI–Hugging Face reconstruction]] and [[gtg-1002-ai-orchestrated-espionage|GTG-1002]] both take.

## Notes

[^dream]: [Taiwan Multi-Agent Attack Reconstruction](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia), Dream Research Labs, 2026-08-12. Summarized at [[dream-taiwan-multi-agent-ai-attack|the source summary]].
