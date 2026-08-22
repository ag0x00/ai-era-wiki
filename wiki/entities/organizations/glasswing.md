---
type: entity
title: "Project Glasswing"
created: 2026-04-30
updated: 2026-08-21
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
  - "[[xbow]]"
  - "[[mdash]]"
  - "[[cybergym]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
sources:
  - "https://www.anthropic.com/glasswing"
  - "[[anthropic-glasswing-announcement]]"
  - "[[anthropic-glasswing-initial-update]]"
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

In parallel, Anthropic's open-source scanning program (1,000+ projects) estimated **6,202 high/critical** vulnerabilities, of which 1,752 have been assessed (90.6% true positives) and only **75 patched** so far, the funnel that gives the bottleneck-inversion its empirical shape. Maintainer overload is a named constraint: some maintainers asked Anthropic to **slow down** disclosures. See [[anthropic-glasswing-initial-update|the update page]] for the full funnel and tooling releases ([[claude-code-security|Claude Security]] public beta, the Cyber Verification Program, shared skills/harness/threat-model-builder).

## Coalition Partners

### Named launch partners (12)

| Partner | Wiki page | Role / Quote attribution |
|---|---|---|
| Amazon Web Services | [[aws\|AWS]] | Amy Herzog (VP & CISO): testing Mythos in AWS security operations |
| Anthropic | [[anthropic\|Anthropic]] | Model vendor; initiative lead |
| Apple | (no wiki page yet) | (no public quote) |
| Broadcom | (no wiki page yet) | (no public quote) |
| Cisco | (no wiki page yet) | Anthony Grieco (SVP & CSTO): "AI capabilities have crossed a threshold" |
| CrowdStrike | [[crowdstrike\|CrowdStrike]] | Elia Zaitsev (CTO): "the window … has collapsed" |
| Google | [[google\|Google]] | Heather Adkins (VP Security Engineering): Mythos via Vertex AI |
| JPMorganChase | (no wiki page yet) | Pat Opet (CISO): financial-system framing |
| The Linux Foundation | (no wiki page yet) | Jim Zemlin (CEO): OSS maintainer access |
| Microsoft | [[microsoft\|Microsoft]] | Igor Tsyganskiy (EVP Cybersecurity + Microsoft Research): CTI-REALM evaluation |
| NVIDIA | [[nvidia\|NVIDIA]] | (no public quote) |
| Palo Alto Networks | [[palo-alto-networks\|Palo Alto Networks]] | Lee Klarich (CPTO): "more attacks, faster attacks, more sophisticated attacks" |

### Additional organizations

40+ further organizations have access to Mythos Preview to "scan and secure both first-party and open-source systems." The post does not enumerate them; the 90-day public report may.

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

Glasswing is the **organizing artifact** for the wiki's `ai-in-sec-defense` axis as of May 13, 2026, and for [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] within it. It supersedes the wiki's previous treatment of defender-AI as a vendor-by-vendor productized capability with a coalition-backed industrial-scale framing.

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
- [[cybergym|CyberGym]]: benchmark on which both Mythos (raw, 83.1%) and MDASH (88.45%) sit.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]: wiki thesis Glasswing anchors.
- [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]: adjacent thesis directly supported by Glasswing's framing.
