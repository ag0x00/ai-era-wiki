---
type: paper
title: "Agentic AI Security Scoping Matrix"
address: c-000002
created: 2026-05-07
updated: 2026-08-21
tags:
  - papers
  - aws
  - agentic-ai
  - autonomy
  - frameworks
status: summarized
publication_date: 2025-11-21
authors:
  - "[[aaron-brown|Aaron Brown]]"
  - "[[matt-saner|Matt Saner]]"
publisher: AWS Security Blog
source_url: https://aws.amazon.com/blogs/security/the-agentic-ai-security-scoping-matrix-a-framework-for-securing-autonomous-ai-systems/
related:
  - "[[aws-agentic-ai-security-scoping-matrix|AWS Agentic AI Security Scoping Matrix (framework page)]]"
  - "[[aws|AWS]]"
  - "[[least-agency-principle|Least Agency Principle]]"
sources:
  - "[[.raw/articles/aws-agentic-ai-security-scoping-matrix-2026-05-07.md]]"
---

# AWS Agentic AI Security Scoping Matrix — Source Summary

The AWS Security Blog post that introduced the [[aws-agentic-ai-security-scoping-matrix|Agentic AI Security Scoping Matrix]] on 2025-11-21. Authored by [[aaron-brown|Aaron Brown]] and [[matt-saner|Matt Saner]], AWS Security.

## Description

A four-scope ladder (No agency / Prescribed agency / Supervised agency / Full agency) for categorizing autonomous AI systems by the combination of agency (what the agent is permitted) and autonomy (how independently it decides). Each scope is mapped to required controls across six security dimensions: identity context; data, memory, and state protection; audit and logging; agent and FM controls; agency perimeters and policies; and orchestration. Five architectural patterns (progressive autonomy deployment, layered security, continuous validation, human oversight integration, graceful degradation) apply across scopes.

## Lineage

Extends AWS's earlier **Generative AI Security Scoping Matrix** to address long-running, function-calling agentic systems specifically. The matrix is positioned as the agentic counterpart to the GenAI matrix, not a replacement — both are intended to be used together for organizations whose AI portfolio spans both paradigms.

## Relevance to this corpus

The matrix's load-bearing contribution is the **explicit definitional split between agency and autonomy** — both terms had been used interchangeably across the literature before this paper formalized them. The wiki's [[least-agency-principle|Least Agency Principle]] carries the anchor citation. The four-scope ladder is also one of several convergent ladders documented in the wiki, mapped against the [[agentic-ai-security-cmm-2026|CMM]] L1–L5+, [[csa-maestro|CSA ATF]] Intern → Junior → Senior → Principal, and the [[least-agency-principle|least-agency]] auto/notify/confirm/block action-risk tiers in the [[aws-agentic-ai-security-scoping-matrix|framework page's cross-walk section]].

## Key terms (from the source)

- **Agency** — the scope of actions an AI system is permitted and enabled to take, and how much a human bounds the agent's actions or capabilities.
- **Autonomy** — the degree of independent decision-making and action the system can take without human intervention.
- **Confused deputy problem** — referenced (not coined) as a load-bearing reason that agent identity needs to be addressable as distinct from human identity.

## See also

The full structural analysis of the matrix — the four scopes, the six dimensions, the five architectural patterns, and the cross-walk to wiki ladders — lives at [[aws-agentic-ai-security-scoping-matrix|the framework page]]. This source summary is provenance; cite the framework page for substantive claims.
