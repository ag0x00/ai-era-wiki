---
type: incident
title: "LiteLLM Supply Chain Compromise (Google ADK Dependency)"
created: 2026-04-30
updated: 2026-04-30
tags:
  - incidents
  - supply-chain
  - dependency-chain
status: developing
incident_class: supply-chain
attack_with_or_on_ai: "on AI"
date_observed: 2026-03-24
date_disclosed: 2026-03-24
target: "Google ADK Go 1.0 dependency tree (LiteLLM transitive dependency)"
threat_actor: "unknown"
impact: "Compromised LiteLLM package detected in Google ADK Go 1.0 dependencies. Demonstrates supply-chain risk in even hyperscaler-built AI toolchains."
related:
  - "[[google]]"
  - "[[clawhavoc]]"
  - "[[sandworm-mode-npm-worm]]"
  - "[[ai-security-standards-in-q1-2026]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# LiteLLM Supply Chain Compromise

## Summary

On March 24, 2026 — coincident with the launch of **Google ADK Go 1.0** (March 31, 2026) — a compromised version of the LiteLLM library was detected in the ADK's dependency tree. The incident is significant primarily because it confirms that hyperscaler-built AI toolchains are not immune to the supply-chain risks active across the broader Q1 2026 incident set.

## Attack Vector

LiteLLM is widely used as a model-router shim in agentic stacks. A compromised release in its dependency chain means downstream consumers (including Google's own ADK) inherited the vulnerability simply by pinning to current versions.

## Defensive Lessons

- **AI-BOM (AI Bill of Materials) adoption is lagging.** The [[ai-security-standards-in-q1-2026|AI Security Standards Q1 2026]] paper notes ML-BOM adoption lags **48% behind** SBOM requirements. This incident is a concrete cost of that gap.
- **Reproducible builds and version pinning** would have constrained but not prevented this — pinning to the wrong version is still wrong.
- The pairing of [[clawhavoc|ClawHavoc]] (skill marketplace) + [[sandworm-mode-npm-worm|SANDWORM_MODE]] (npm registry) + this LiteLLM compromise (transitive dependency) describes **three distinct supply-chain attack surfaces** in the agentic AI stack within a single quarter. Treat them as a class, not three isolated events.

## Sources
- See frontmatter.
