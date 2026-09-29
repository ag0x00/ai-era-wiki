---
type: entity
title: "Project Glasswing"
created: 2026-04-30
updated: 2026-09-28
tags:
  - entities
  - initiatives
  - glasswing
  - claude-mythos
  - coalition
  - ai-in-sec-defense
  - ai-vuln-discovery
  - critical-infrastructure
status: developing
scope_axis:
  - ai-in-sec-defense
  - sec-against-ai
entity_type: organization
org_type: advisory
role: "Coalition initiative led by Anthropic — twelve named partners plus 40+ additional organizations — applying Claude Mythos Preview to defensive vulnerability discovery on the world's most-critical software"
homepage: "https://www.anthropic.com/glasswing"
related:
  - "[[anthropic]]"
  - "[[mythos]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[anthropic-glasswing-initial-update]]"
  - "[[cloudflare]]"
  - "[[mozilla]]"
  - "[[microsoft]]"
  - "[[google]]"
  - "[[aws]]"
  - "[[crowdstrike]]"
  - "[[palo-alto-networks]]"
  - "[[nvidia]]"
  - "[[cisco]]"
  - "[[xbow]]"
  - "[[mdash]]"
  - "[[cybergym]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
  - "[[vvah|VVAH]]"
  - "[[visa|Visa]]"
  - "[[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]]"
  - "[[openai-daybreak]]"
sources:
  - "https://www.anthropic.com/glasswing"
  - "[[anthropic-glasswing-announcement]]"
  - "[[anthropic-glasswing-initial-update]]"
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
  - "https://openai.com/index/daybreak-securing-the-world/"
  - "https://openai.com/index/expanding-daybreak-as-the-cyber-defense-window-narrows/"
  - "[[.raw/articles/openai-daybreak-securing-the-world-2026-09-28.md]]"
  - "[[.raw/articles/openai-expanding-daybreak-2026-09-28.md]]"
verified: 2026-09-29
verified_against:
  - ".raw/articles/cybersecurity-2026-09-28.md"
  - ".raw/articles/openai-daybreak-securing-the-world-2026-09-28.md"
  - ".raw/articles/openai-expanding-daybreak-2026-09-28.md"
verified_findings: 0
verified_note: "verify2, diff-scoped to the retargeted footnotes; no findings"
---

# Project Glasswing

**Sources:** [Anthropic — Project Glasswing](https://www.anthropic.com/glasswing) · [[anthropic-glasswing-announcement|Project Glasswing announcement paper page]]

Project Glasswing is a coalition initiative announced by [[anthropic|Anthropic]] on **May 12, 2026** to apply **Claude Mythos Preview** (an unreleased frontier model) to defensive vulnerability discovery on the world's most-critical software. Twelve named partners and 40+ additional organizations participate; Anthropic has committed up to **\$100M in usage credits** plus **\$4M in direct donations** to open-source security organizations. The initiative is framed as a 90-day-report cadence preview, with explicit national-security positioning and a longer-term goal of seeding an independent third-party body to coordinate large-scale AI-augmented defensive cybersecurity.

## One-Month Update (2026-05-22)

The first progress report ([[anthropic-glasswing-initial-update|Glasswing initial update]]) records that the ~50 partners have collectively found **more than ten thousand high- or critical-severity vulnerabilities** in one month, with several reporting bug-finding rates up more than tenfold. The headline reframing: discovery is no longer the constraint; verification, disclosure, and patching are.

Named partner and evaluator results:

| Organization | Result |
|---|---|
| [[cloudflare\|Cloudflare]] | 2,000 bugs (400 high/critical) across critical-path systems; FP rate better than human testers |
| [[mozilla\|Mozilla]] | 271 vulnerabilities found and fixed in Firefox 150, >10× the Firefox 148 count under Opus 4.6 |
| [[aisi-uk\|UK AI Security Institute]] | Mythos first model to solve both AISI cyber ranges end to end |
| Palo Alto Networks | Latest release shipped >5× the usual number of patches |
| Oracle | Finding and fixing vulnerabilities multiple times faster |
| One partner bank | Mythos helped prevent a fraudulent \$1.5M wire transfer |

In parallel, Anthropic's open-source scanning program (1,000+ projects) estimated **6,202 high/critical** vulnerabilities, of which 1,752 have been assessed (90.6% true positives) and an estimated 530 disclosed to maintainers, with only **75 patched** so far,[^glasswing-update] the funnel that gives the bottleneck-inversion its empirical shape. Maintainer overload is a named constraint: some maintainers asked Anthropic to **slow down** disclosures. See [[anthropic-glasswing-initial-update|the update page]] for the full funnel and tooling releases ([[claude-code-security|Claude Security]] public beta, the Cyber Verification Program, shared skills/harness/threat-model-builder).

## Coalition Partners

### Named launch partners (12)

| Partner | Wiki page | Role / Quote attribution |
|---|---|---|
| Amazon Web Services | [[aws\|AWS]] | Amy Herzog (VP & CISO): testing Mythos in AWS security operations |
| Anthropic | [[anthropic\|Anthropic]] | Model vendor; initiative lead |
| Apple | (no wiki page yet) | (no public quote) |
| Broadcom | (no wiki page yet) | (no public quote) |
| Cisco | [[cisco\|Cisco]] | Anthony Grieco (SVP & CSTO): "AI capabilities have crossed a threshold" |
| CrowdStrike | [[crowdstrike\|CrowdStrike]] | Elia Zaitsev (CTO): "the window … has collapsed" |
| Google | [[google\|Google]] | Heather Adkins (VP Security Engineering): Mythos via Vertex AI |
| JPMorganChase | (no wiki page yet) | Pat Opet (CISO): financial-system framing |
| The Linux Foundation | (no wiki page yet) | Jim Zemlin (CEO): OSS maintainer access |
| Microsoft | [[microsoft\|Microsoft]] | Igor Tsyganskiy (EVP Cybersecurity + Microsoft Research): CTI-REALM evaluation |
| NVIDIA | [[nvidia\|NVIDIA]] | (no public quote) |
| Palo Alto Networks | [[palo-alto-networks\|Palo Alto Networks]] | Lee Klarich (CPTO): "more attacks, faster attacks, more sophisticated attacks" |

### Additional organizations

40+ further organizations have access to Mythos Preview to "scan and secure both first-party and open-source systems." The post does not enumerate them; the 90-day public report may.

Glasswing's method has one documented instance of reaching outside the coalition's published membership. [[visa|Visa]]'s `visa-vulnerability-agentic-harness` is described in Semgrep's July 2026 survey as an agentic static-analysis pipeline built on learnings from Project Glasswing, released under Apache 2.0 and closed to external contributions.[^semgrep] The description is Semgrep's LLM-generated repository summary rather than a statement by Visa or Anthropic, and it places Visa nowhere in relation to the 40+ organizations holding Mythos Preview access. The instance documents the method's reach into third-party open source rather than Visa's standing in the coalition. See [[vvah|VVAH]] for the harness itself.

## Partner terms

- **Access to Claude Mythos Preview** during the research preview period.
- **Coverage of substantial usage** via Anthropic's \$100M usage-credit commitment.
- **Post-preview pricing**: \$25 / \$125 per million input/output tokens (Glasswing-participant rates). Available on Claude API, Amazon Bedrock, Google Cloud Vertex AI, and Microsoft Foundry.
- **Information-sharing**: partners "to the extent they're able" share information and best practices with each other.
- **Public reporting** by Anthropic within 90 days on lessons learned, vulnerabilities fixed, and improvements that can be disclosed.

## Operational Focus Areas

Per the announcement, Glasswing partner work concentrates on:

1. **Local vulnerability detection**: finding bugs in partner-owned code.
2. **Black-box testing of binaries**: pentest-style coverage where source isn't accessible.
3. **Securing endpoints**: defender-side product enhancement.
4. **Penetration testing of systems**: internal adversarial use of Mythos.

## Industry-Standards Component

Anthropic commits to "collaborate with leading security organizations to produce a set of practical recommendations for how security practices should evolve in the AI era." Named candidate areas:

- Vulnerability disclosure processes
- Software update processes
- Open-source and supply-chain security
- Software development lifecycle and secure-by-design practices
- Standards for regulated industries
- Triage scaling and automation
- Patching automation

## Donations

Direct cash donations to open-source security organizations:

- **\$2.5M to Alpha-Omega and OpenSSF** (via the Linux Foundation).
- **\$1.5M to the Apache Software Foundation**.
- **Claude for Open Source** program: additional access for OSS maintainers via [claude.com/contact-sales/claude-for-oss](https://claude.com/contact-sales/claude-for-oss).

## National-Security Positioning

Anthropic frames Glasswing as a defensive imperative against state-sponsored threats (named: China, Iran, North Korea, Russia). Explicit language: *"The US and its allies must maintain a decisive lead in AI technology."* Anthropic discloses ongoing discussions with US government officials about Mythos's offensive and defensive cyber capabilities. The long-term proposed structure is an independent third-party body bringing private- and public-sector organizations together.

## Wiki Position

Glasswing is the **organizing artifact** for the wiki's `ai-in-sec-defense` axis as of May 13, 2026, and for [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] within it. It supersedes the wiki's previous treatment of defender-AI as a vendor-by-vendor productized capability with a coalition-backed industrial-scale framing. Semgrep's July 2026 market-structure finding sits at a different layer and does not contradict this: no reference open-source harness will emerge today, and many organizations will build their own "shop jigs" for vulnerability finding, so where Glasswing consolidates model access, the harness layer built around that access is diffusing rather than consolidating.[^semgrep]

Since the June 2026 expansion of [[openai-daybreak|OpenAI Daybreak]], Glasswing shares that organizing role with it. Daybreak is the programme through which OpenAI gates its own cyber-capable models, and the two distribute access differently. Glasswing admits organizations to a coalition around one preview model. Daybreak admits approved individuals and organizations to two tiers, frontier general-purpose models with production safeguards removed and purpose-trained cyber models.[^daybreak-aug] It also lets partner vendors build its models into their own products, and direct model access stays with the partners. Under the June 2026 terms the partner model was OpenAI's general model with trusted access, its primary model for most defensive workflows.[^daybreak-partners] Both labs state that the constraint has moved from discovery to what follows it: OpenAI names patching,[^daybreak-june] and Anthropic names verification, disclosure and patching.[^glasswing-update] OpenAI's Patch the Planet answers the maintainer overload recorded above with funded researchers who validate and deduplicate findings and patches before a maintainer sees them.[^daybreak-ptp]

Critical context:

- **Glasswing is not a product**; it is a coalition initiative. The product (model) is [[mythos|Claude Mythos Preview]].
- **Glasswing partners include both AI vendors and AI-adopting enterprises**: the model vendor (Anthropic), other AI vendors (Google, Microsoft), CSPs (AWS), security vendors (CrowdStrike, Palo Alto Networks), infrastructure providers (NVIDIA, Cisco, Broadcom, Apple), foundations (Linux Foundation), and financial institutions (JPMorganChase).
- **The Linux Foundation's inclusion** is structurally important: it signals OSS maintainer reach beyond commercial product customers.
- **Microsoft's [[mdash|MDASH]] is a Glasswing artifact**: Microsoft's "generally available AI models" silence in the MDASH announcement is explained by coordinated-launch constraints. Mythos is almost certainly one of MDASH's orchestrated models.

## Open Questions

- The 40+ additional organizations beyond the named 12.
- Specifics of OSS maintainer access via Claude for Open Source.
- Per-partner case studies: the 90-day public report should surface some.
- Government / standards-body engagement beyond the high-level mention.
- Whether the proposed "independent third-party body" materializes; what its charter would be.
- How responsible-disclosure timelines align across 50+ partners using the same model on the same codebases.

## See Also

- [[anthropic-glasswing-announcement|Project Glasswing announcement paper page]]: source summary.
- [[mythos|Claude Mythos Preview]]: the model.
- [[anthropic|Anthropic]]: initiative lead.
- [[xbow-mythos-evaluation|XBOW's Mythos Evaluation]]: independent (non-partner) third-party evaluation of Mythos.
- [[mdash-defense-at-ai-speed|MDASH announcement]]: Glasswing-partner defender-AI artifact.
- [[cybergym|CyberGym]]: benchmark on which both Mythos (raw, 83.1%) and MDASH (88.45%) sit.[^glasswing-ann][^mdash-blog]
- [[openai-daybreak|OpenAI Daybreak]]: OpenAI's counterpart programme, with tiered model access and a partner network.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]: wiki thesis Glasswing anchors.
- [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]: adjacent thesis directly supported by Glasswing's framing.

## Notes

[^semgrep]: [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses), July 2026 (no day-level date exposed; author not named). The market-structure argument (no reference open-source harness, "shop jigs") is human-written; the VVAH description ("built on learnings from Project Glasswing," licence, closed-to-contributions) is from Semgrep's LLM-generated repository summary. Summarized at [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].
[^glasswing-update]: Anthropic, [Project Glasswing: An initial update](https://www.anthropic.com/research/glasswing-initial-update) (2026-05-22): the open-source scanning funnel (estimated high- and critical-severity findings, those assessed with the true-positive share among them, those disclosed to maintainers, and those patched), and progress named as limited by how quickly vulnerabilities can be verified, disclosed and patched. Summarized at [[anthropic-glasswing-initial-update|Project Glasswing: Initial Update]].
[^glasswing-ann]: Anthropic, [Project Glasswing](https://www.anthropic.com/glasswing) (2026-05-12): raw Claude Mythos Preview's CyberGym score, the share of reproduction tasks solved. Summarized at [[anthropic-glasswing-announcement|Project Glasswing: Securing Critical Software]].
[^mdash-blog]: Microsoft Security Blog, [Defense at AI speed](https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/) (2026-05-12): MDASH's CyberGym score. Summarized at [[mdash-defense-at-ai-speed|MDASH: Defense at AI Speed]].
[^daybreak-aug]: [OpenAI — Expanding Daybreak as the Cyber Defense Window Narrows](https://openai.com/index/expanding-daybreak-as-the-cyber-defense-window-narrows/) (2026-08-10): the Daybreak Blue and Red access tiers. Summarized at [[openai-daybreak|OpenAI Daybreak]].
[^daybreak-partners]: [OpenAI — Daybreak: Tools for securing every organization in the world, "Working with the security ecosystem"](https://openai.com/index/daybreak-securing-the-world/#working-with-the-security-ecosystem) (2026-06-22): partners use GPT-5.5 with Trusted Access for Cyber in the security products and services they provide to customers, and direct model access stays with the partners.
[^daybreak-june]: [OpenAI — Daybreak: Tools for securing every organization in the world, "Cyber defense at an inflection point"](https://openai.com/index/daybreak-securing-the-world/#cyber-defense-at-an-inflection-point) (2026-06-22): the bottleneck named as patching now that defenders are overwhelmed by the number of vulnerabilities found. Summarized at [[openai-daybreak|OpenAI Daybreak]].
[^daybreak-ptp]: [OpenAI — Daybreak, "Patch the Planet: landing fixes in open-source"](https://openai.com/index/daybreak-securing-the-world/#patch-the-planet-landing-fixes-in-open-source) (2026-06-22): the funded-researcher engagement model, with vulnerabilities and patches validated and deduplicated before they reach maintainers. Summarized at [[openai-daybreak|OpenAI Daybreak]].
