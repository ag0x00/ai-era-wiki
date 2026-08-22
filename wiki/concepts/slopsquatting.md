---
type: concept
title: "Slopsquatting"
address: c-000104
created: 2026-05-24
updated: 2026-06-23
tags:
  - concepts
  - supply-chain
  - ai-hallucination
  - sec-against-ai
status: developing
scope_axis:
  - sec-against-ai
coined_by: "Academic researchers (UT San Antonio, Virginia Tech, University of Oklahoma)"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[supply-chain-security-for-agents]]"
  - "[[ai-era-supply-chain-hardening]]"
  - "[[nsa-ai-ml-supply-chain-guidance-2026]]"
  - "[[litellm-supply-chain-compromise]]"
  - "[[citizen-coders]]"
sources:
  - https://arxiv.org/pdf/2406.10279
  - https://www.csoonline.com/article/3961304/ai-hallucinations-lead-to-new-cyber-threat-slopsquatting.html
  - https://snyk.io/articles/package-hallucinations/
---

# Slopsquatting

**Slopsquatting** is a supply chain attack technique in which an adversary registers packages with names that AI coding assistants consistently hallucinate, then waits for developers who trust the AI's output to install the malicious package.

## Mechanism

AI code generators hallucinate non-existent package names at scale. A study of 16 production LLMs across 576,000 generated code samples found that 440,445 references (19.7%) pointed to packages that do not exist, and 43% of hallucinated package names were regenerated in all ten repeated queries (Spracklen et al., UT San Antonio, Virginia Tech, and University of Oklahoma).[^pkg-hallucination] Stable, repeatable hallucinations let an attacker identify reliable targets by querying a given model and then pre-registering those names in public registries.

The attack chain is:

1. Attacker queries target LLM for code in a specific domain.
2. Attacker identifies packages the LLM consistently recommends that do not exist in public registries.
3. Attacker publishes malicious packages under those names.
4. Developers who copy, run, or are served AI-generated code install the attacker's package without any explicit trust decision.

Unlike classical typosquatting — which relies on keyboard-proximity errors — slopsquatting exploits the LLM's own output and is invisible to developers who do not verify AI recommendations against authoritative package registries.

## Scale

A proof-of-concept package seeded under a commonly hallucinated name accumulated over 30,000 downloads in three months.[^slop-poc] Malicious package uploads to public registries continue to climb: JFrog reported a 451% year-over-year surge in malicious npm packages, reaching 171,592 unique instances driven by three hijack campaigns, with slopsquatting a contributing driver alongside traditional typosquatting and dependency confusion.[^jfrog]

## Defenses

**Lockfile enforcement.** Require `npm ci` or the equivalent in every build and CI pipeline. Lockfiles record exact hashes of every installed package; a slopsquatted package will fail the hash check if it was not present when the lockfile was generated. This is the single most reliable control.

**Dependency allowlisting.** Maintain an approved-package list in a private registry mirror. Block resolution to public registries for unapproved packages. This eliminates the attack surface entirely for the allowlisted scope.

**Pre-install scanning.** Run SCA tooling against AI-generated dependency lists before installing any package. Some SCA tools now flag packages with no prior install history that match LLM hallucination patterns.

**LLM self-verification.** Ask the same model that suggested a package whether the package actually exists before installing. Models catch a meaningful fraction of their own hallucinations on re-examination; the gate is imperfect but useful as a secondary check.

**Code review policy for AI-generated dependencies.** Treat AI-suggested package names as unverified until cross-checked against the registry's published maintainer list, installation count, and age. Packages with zero history that appear in AI-generated code warrant the same scrutiny as an unknown executable.

## Relationship to Adjacent Threats

Slopsquatting is structurally similar to [[litellm-supply-chain-compromise|the LiteLLM supply chain compromise]] — both exploit developer trust in AI tooling — but differs in mechanism: LiteLLM was a direct package compromise, while slopsquatting exploits the hallucination pathway.

The [[citizen-coders|Citizen Coders]] dynamic amplifies slopsquatting risk. Non-developer users who generate code via AI assistants lack the instinct to verify package recommendations, and they are less likely to have lockfile enforcement in their workflows.

> [!gap] Registry-side detection
> No major package registry currently detects or flags packages that match known LLM hallucination patterns at publish time. This is an open gap in the ecosystem defense stack.

## Notes

[^pkg-hallucination]: [arXiv 2406.10279 — We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs](https://arxiv.org/abs/2406.10279), 2024 (Spracklen et al., UT San Antonio, Virginia Tech, University of Oklahoma). 16 LLMs across 576,000 generated code samples; 440,445 (19.7%) of package references were hallucinations; 43% of hallucinated packages were regenerated in all ten repeated queries.

[^slop-poc]: [SD Times — Hallucinated code, real threat: How slopsquatting targets AI-assisted development](https://sdtimes.com/coding-assistants/hallucinated-code-real-threat-how-slopsquatting-targets-ai-assisted-development/), 2025. Bar Lanyado (Lasso Security) registered a commonly hallucinated package name as an empty PyPI package; it received over 30,000 downloads in three months.

[^jfrog]: [JFrog — 2026 Software Supply Chain Security State of the Union (announcement)](https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs), 2026, report p.5. Malicious npm packages rose 451% to 171,592 unique instances, driven by three hijack campaigns producing more than two million compromised downloads. See [[jfrog-ssc-state-of-union-2026|JFrog 2026 SSC State of the Union]].

<!-- sources:auto -->
## Sources

- [arxiv.org](https://arxiv.org/pdf/2406.10279)
- [csoonline.com](https://www.csoonline.com/article/3961304/ai-hallucinations-lead-to-new-cyber-threat-slopsquatting.html)
- [snyk.io](https://snyk.io/articles/package-hallucinations/)
<!-- /sources -->
