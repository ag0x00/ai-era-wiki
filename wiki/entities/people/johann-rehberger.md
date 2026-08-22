---
type: entity
title: "Johann Rehberger"
created: 2026-04-30
updated: 2026-08-20
tags:
  - entities
  - people
  - red-teaming
  - prompt-injection
  - agentic-security
status: developing
entity_type: person
role: "Red-team researcher; AI offensive security"
affiliation: "Independent (Embrace The Red); Microsoft Red Team alum"
related:
  - "[[month-of-ai-bugs]]"
  - "[[jules-ai-kill-chain]]"
  - "[[indirect-prompt-injection]]"
  - "[[tool-abuse-chains]]"
  - "[[securing-your-agents-talk]]"
  - "[[your-agent-works-for-me-now-talk]]"
  - "[[promptware]]"
  - "[[delayed-tool-invocation]]"
  - "[[agent-commander-prompt-c2]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[geminijack-gemini-enterprise-injection]]"
sources:
  - ".raw/talks/securing-your-agents-2026-04-30.md"
  - ".raw/talks/2026-03-04_Johann-Rehberger_Your-Agent-Works-for-Me-Now_transcript.md"
---

# Johann Rehberger

Independent red-team researcher widely credited with the most prolific public corpus of responsibly-disclosed prompt-injection and agent-compromise vulnerabilities. Publishes at the **Embrace The Red** blog. Originated and led the **[[month-of-ai-bugs|"Month of AI Bugs"]]** initiative in August 2025, in which he cataloged dozens of successful attacks against every frontier model and every major agentic development kit during a single calendar month, a deliberate echo of the Month of Browser Bugs / Month of Bugs tradition from earlier-era offensive security.

At Unprompted (March 2026) he was the headline offensive track speaker on Stage 2 and received an extended standing introduction from Gadi Evron. The talk, [[your-agent-works-for-me-now-talk|"Your Agent Works for Me Now"]], disclosed two previously unreported attack techniques ([[delayed-tool-invocation|Delayed Tool Invocation]] and [[agent-commander-prompt-c2|Agent Commander]]) and introduced the [[promptware|Promptware]] framing.

## Research Contributions

### Framing Contributions

- **[[promptware|Promptware]]** (coined/popularized, 2026): the reframing of agentic AI attacks from single-turn injection events to multi-stage malware with kill chain structure. Echoes Ben Nassi's concurrent *Promptware Kill Chain* paper.
- **"Offensive context engineers"**: Rehberger's characterization of red-teamers working in the LLM era, reflecting the shift from technical exploit craft to natural-language prompt engineering.
- **Normalization of deviance**: Rehberger's application of the systems-safety concept to AI adoption. The term names a pattern in which operational familiarity with AI in test contexts breeds unwarranted trust in production contexts.

### Technique Contributions

- **[[delayed-tool-invocation|Delayed Tool Invocation]]** (disclosed March 2026): bypasses platform-level tool deactivation controls by embedding a conditional trigger that fires the tool in a subsequent conversation turn; demonstrated against Gemini Workspace, Microsoft Copilot, ChatGPT, and Google Home.
- **[[agent-commander-prompt-c2|Agent Commander — Prompt-Level C2]]** (disclosed March 2026): a prompt-native command-and-control tool that enrolls agents into a C2 server via natural-language promptware; demonstrated zero-click enrollment via OpenClaw's Gmail PubSub feature; cross-platform (OpenClaw + KimiCloud).
- **ASCII / Unicode tag character steganography**: exploitation of invisible Unicode tag characters to embed injection payloads in files and issue tickets; first public Xcode injection demonstration.
- **Spyber** (prior, Black Hat): continuous exfiltration via memory-poisoned ChatGPT agent; every user keystroke forwarded to attacker after initial enrollment.

### Disclosure History (Selected)

- **[[jules-ai-kill-chain|Jules AI compromise]]** (Aug 2025): full five-stage kill chain on Google's Jules coding agent, from a hidden GitHub-issue prompt injection through persistence in project files to remote command-and-control.
- **[[month-of-ai-bugs|Month of AI Bugs]]** (Aug 2025): 30+ days of daily coordinated disclosures; all coordinated to release simultaneously; attacks documented against every frontier model and major agentic dev kit.
- **Gemini long-term memory corruption** (disclosed Feb 2025): [[delayed-tool-invocation|delayed tool invocation]] via a document summarization request implanted false persistent memories, triggered later by common conversational words ("yes," "no," "sure"). Distinct from [[geminijack-gemini-enterprise-injection|GeminiJack]], an unrelated Gemini Enterprise vulnerability disclosed by Noma Labs in December 2025 — the wiki previously conflated the two under a "GeminiJack-class" label; that label belongs to Noma's finding alone.
- **ChatGPT memory persistence attacks**: zero-click long-term memory poisoning via shared content.
- **Markdown-image and URL-fetch exfiltration patterns**, documented across multiple frontier-model deployments.
- **Microsoft Enterprise Copilot memory implant** (disclosed Dec patch; Unprompted 2026 case study): injection via file summarization writes persistent attacker-controlled memories to enterprise Copilot.
- **Apple Xcode prompt injection** (March 2026): first public demonstration of injection via Xcode's AI code review + RunSnippet tool.
- **Gemini + Google Home physical actuator control** (March 2026): delayed tool invocation causes Google Home speaker to play attacker-specified audio via document title as intent signal.

## Relevance to the Wiki

Rehberger's work is the primary public empirical evidence that the threat model of **indirect injection → tool abuse → exfiltration → persistence → C2** is not theoretical. The [[securing-your-agents-talk|Securing Your Agents]] deck cites his Month of AI Bugs as the proof-by-existence basis for treating agent-tool integrations as adversarial-by-default. His Unprompted 2026 talk extends that evidence base with two new structural techniques and the promptware malware framing, moving the conversation from "injection as a class" to "injection as initial access in a multi-stage campaign."

## See Also

- [[your-agent-works-for-me-now-talk|"Your Agent Works for Me Now" — Unprompted March 2026]]: the primary talk page
- [[month-of-ai-bugs|Month of AI Bugs (August 2025)]]: the August 2025 disclosure series
- [[jules-ai-kill-chain|Jules AI Kill Chain — Indirect Injection to Full Remote Control]]: the canonical case study from that series
- [[promptware|Promptware]]: the malware framing he introduced
- [[delayed-tool-invocation|Delayed Tool Invocation]]: the tool-reactivation bypass technique
- [[agent-commander-prompt-c2|Agent Commander — Prompt-Level Command and Control]]: the C2 infrastructure tool
- [[simon-willison|Simon Willison]]: companion public-research voice on prompt injection
- [[unit-42-prompt-injection-observations|Unit 42 In-the-Wild Prompt Injection Observations]]: Palo Alto's production-telemetry counterpart
