---
type: talk
title: "The AI Security Larsen Effect"
address: c-000170
created: 2026-06-02
updated: 2026-06-02
tags:
  - papers
  - talks
  - vendor-evaluation
  - capability-framework
  - configure-buy-build
status: summarized
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
origin: aggregated
year: 2026
authors: ["Maxim Kovalsky"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 3, 2026 (Day 1 / Stage 2)"
key_claim: "The AI security vendor landscape produces so much marketing noise that buyer-side selection breaks down; a capability-based framework that resolves each required capability into a configure, buy, or build decision restores signal and grounds the choice in the actual deployment."
methodology: "Practitioner talk from a value-added reseller, plus a live demonstration of the speaker's vendor-evaluation tool against a sample GenAI system. Transcript and slides captured to .raw/talks/. Vendor-count figures (2,000+ claims, 62→~80 vendors, ~4 new companies/week) are the speaker's own, stated as of the talk; treat them as point-in-time."
source_url: "https://unpromptedcon.org/#"
related:
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[evaluating-ai-soc-agents|Evaluating AI SOC Agents]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]"
  - "[[red-teaming-capability-framework|Red-Teaming Capability Framework]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - https://unpromptedcon.org/#
  - .raw/talks/2026-03-03_Maxim-Kovalsky_The-AI-Security-Larsen-Effect_transcript.md
  - .raw/talks/2026-03-03_Maxim-Kovalsky_The-AI-Security-Larsen-Effect_slides.pdf
---

# The AI Security Larsen Effect

A practitioner talk by [[maxim-kovalsky|Maxim Kovalsky]] of [[consortium-networks|Consortium Networks]], a value-added reseller (VAR), at the March 2026 [[unprompted-conference-march-2026|Unprompted Conference]].

**Source:** [Conference abstracts page](https://unpromptedcon.org/abstract-march2026/) · [Conference agenda](https://unpromptedcon.org/#)

## The Argument

The Larsen effect is the audio feedback loop — the squeal when a microphone picks up its own output through a speaker. Kovalsky uses it as a metaphor for the AI security buyer market: vendor marketing noise feeds back on itself until buyers can no longer extract signal. Consortium Networks is a VAR whose role is matchmaking — clients arrive with a problem, the VAR identifies the right products in the market. Kovalsky frames his work as "bringing the VA back to the VAR," restoring the value-added analysis that the noise has eroded.

The dysfunction starts at the first call. A client says "we need AI security, who's good?" without naming use cases, risk tolerance, or required controls. Kovalsky distinguishes two risk postures that imply different solutions: being concerned about *having* an incident versus being concerned about *filing* that incident with the SEC. Buyers tend to skip that discovery and ask only for a vendor name.

Vendor datasheets compound the problem. The talk quotes representative claims as examples of the noise: "swarms of autonomous agents," "broadest and most comprehensive," "99% efficiency at sub-30ms latency," "AI-powered threat detection to automatically block all adversaries." Some claims may be legitimate; some are public relations. None is verifiable from the datasheet alone.

### Scale of the noise

The figures below are the speaker's own counts, stated as of the talk and moving fast enough that he cited two snapshots six weeks apart.

| Metric | Value |
|---|---|
| Vendor claims to make sense of | 2,000+ (December) → 3,000+ |
| Vendors analyzed | 62 at proposal time → ~80 six weeks later |
| New entrants | ~4 per week — startups out of stealth, or existing vendors bolting on "AI security" |

### The broken AI-governance loop

Kovalsky diagrams the procurement loop he sees repeatedly from the VAR seat:

1. A need is declared: "we need AI security."
2. Six vendor datasheets arrive.
3. A risk register is hand-jammed from [[owasp-llm-top-10|OWASP LLM Top 10]], [[nist-ai-rmf|NIST AI RMF]], or another framework into spreadsheets and GRC tools, because little exists that targets AI security specifically.
4. Four vendors are invited to demos.
5. Two go to parallel proofs-of-concept, usually with unclear evaluation criteria.
6. The POCs stall on objections: doesn't Bedrock already have guardrails for this? we have DLP — can it do this? there are open-source tools, so why not build it in-house?
7. Three months pass; the team decides it needs an AI security strategy and brings in a Big Four firm.
8. The process restarts. Meanwhile, the customer chatbot has already shipped — the [[shadow-ai|shipped-while-evaluating]] risk that makes the whole loop moot.

## The Tool: Evidence-Backed Vendor Capability Mapping

Kovalsky built a tool to break the loop, using Claude Code (Opus 4.5, later 4.6). It is an agentic loop orchestrated inside Claude with two roles: a research agent and a QC agent, driven by system prompts and skills files refined over several months.

The research agent searches for substantiating evidence behind each vendor claim — GitHub repositories, API documentation, user forums where people report their experience with a product. Based on the evidence found, it assigns a **confidence rating**. The top rating, 5 of 5, requires code samples from a GitHub repository showing how the capability is actually instrumented. The process is not flawless: heavy human QC manages hallucinations and context errors.

Each vendor capability is mapped to a custom **AI risk taxonomy**, published on GitHub, that synthesizes [[owasp-llm-top-10|OWASP LLM Top 10]], [[nist-ai-rmf|NIST AI RMF]], and [[mitre-atlas|MITRE ATLAS]]. Kovalsky's view is that each of the three is good but partial; the taxonomy attempts to cover the full present-day risk surface that no single framework does.

## The Configure / Buy / Build Wizard

The demonstration ran a sample system — "AdjusterIQ," an insurance claim-adjuster GenAI application — through the tool's risk-modeling wizard. The sample stack: AWS Bedrock with Sonnet 3.5, Amazon Kendra for RAG, direct API calls with tool calling, and session-scoped memory.

The wizard walks the buyer through a sequence that mirrors the discovery a VAR would do in person:

1. **Inherent risk profile** — data sensitivity (internal confidential, employee access over the internet), autonomy level, and how coding assistants are used by the dev team, since coding assistants become part of the attack surface. Each choice surfaces the risks it activates; choosing autonomous actions over read-only produces a materially different risk profile.
2. **Architecture patterns** — managed PaaS (Bedrock), RAG plus traditional databases, serverless compute, chat interface, tool calling, document analysis. Selections update the risk profile live.
3. **Existing strategic investments** — what platforms the organization already owns (Bedrock guardrails, [[crowdstrike|CrowdStrike]], Zscaler). These are examined first, before going to the market.
4. **Buy-vs-build preference** — buy, build, or no preference.
5. **Implementation-layer preference** — capabilities handed to application teams via SDK/API versus capabilities deployed and governed centrally at an infrastructure choke point (network gateway/proxy) or via a managed endpoint agent. "No preference" surfaces vendors that can do both.
6. **Platform vs. best-of-breed** — leaning toward a platform surfaces vendors that provide the most coverage.

The wizard then reports capability gaps at critical and high severity. In the demo, existing vendor relationships covered 8 capabilities, leaving 15 gaps. The buyer marks some gaps as build (for example, internal governance workflows the organization can build itself) and accepts the rest, producing a shortlist of vendors per remaining gap. For one set of 8 gaps the system suggested 5 vendors, each covering part of the set. The output is a printable summary for sourcing/procurement, engineering, or development teams — with configuration guidance for what is built in-house (for example, AWS Bedrock guardrails) alongside the commercial shortlist.

## Market Judgment

Asked about consolidating to a platform versus picking best-of-breed, Kovalsky's assessment was that the AI security landscape is highly fragmented. Even the large platforms — [[palo-alto-networks|Palo Alto Networks]] and [[crowdstrike|CrowdStrike]] — cover only about 50% of the most relevant risks. Wiz is far from that breadth, holding a small handful of relevant capabilities, though it may expand. The tool will recommend a platform when the buyer prefers one, but the consolidation-vs-best-of-breed tension remains open and deployment-specific.

## Maintenance

Two agentic workflows keep the database current via GitHub Actions. A nightly workflow (roughly 12 hours each evening) re-researches a subset of vendors so that every vendor is revisited at least once a month. A separate workflow discovers new entrants — startups leaving stealth and traditional vendors adding AI security capabilities.

## Placement

This is a **configure / buy / build** capability-based vendor-selection framework: the buyer/practitioner counterpart to [[evaluating-ai-soc-agents|Gartner's AI-SOC-agent evaluation criteria]]. Both cut through vendor marketing in a fast-moving category but produce different artifacts. Gartner publishes evaluation criteria a buyer applies to candidates already under review; Kovalsky's framework runs earlier, deciding whether a vendor purchase is even the right unit of work for a given requirement, or whether the capability should be configured from owned tools or built in-house.

The per-capability sourcing decision maps onto the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]: the CMM enumerates the capability domains an enterprise must mature; this framework supplies the configure/buy/build decision for each one. The structural cousin on the offensive side is the [[red-teaming-capability-framework|Red-Teaming Capability Framework]], which decomposes red-team work into capabilities before assigning them to people, tooling, or vendors. Each reads a vendor-described market capability-first rather than product-first. The shipped-chatbot endpoint of the broken governance loop is the [[shadow-ai|Shadow AI]] problem — deployment outrunning the evaluation meant to govern it. For the broader market backdrop, see [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]].

> [!gap] Risk taxonomy not enumerated in the source
> Kovalsky's AI risk taxonomy (the OWASP + NIST + ATLAS synthesis) is published on GitHub, but the repository URL was not stated in the talk. The full capability list and the per-branch decision criteria were demonstrated live rather than written down in the captured source.

## Status

`summarized` — based on the transcript and slide deck captured to `.raw/talks/`. Vendor-count figures are the speaker's own, stated as of the talk (March 2026).
