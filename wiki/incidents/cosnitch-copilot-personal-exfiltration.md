---
type: incident
title: "CoSnitch: Copilot Personal Data Exfiltration"
address: c-000317
created: 2026-08-20
updated: 2026-08-20
tags:
  - incidents
  - prompt-injection
  - indirect-prompt-injection
  - memory-poisoning
  - microsoft-copilot
  - exfiltration
  - cve
  - one-click
status: published
scope_axis:
  - sec-of-ai
origin: aggregated
incident_class: prompt-injection
attack_with_or_on_ai: "on AI"
date_observed: 2025-12
date_disclosed: 2026-08-18
target: "Microsoft Copilot Personal"
threat_actor: "research-disclosure (Varonis Threat Labs)"
impact: "One-click exfiltration of connected-app data (Gmail, Drive, Calendar) via an undocumented URL auto-run parameter, plus memory poisoning the vendor describes as surviving password change, session revocation, and device re-enrollment"
related:
  - "[[echoleak-copilot-zero-click|EchoLeak Zero-Click Copilot Exfiltration]]"
  - "[[cve-2025-62453-copilot-vscode-prompt-injection|CVE-2025-62453 Copilot Prompt Injection]]"
  - "[[indirect-prompt-injection|Indirect Prompt Injection]]"
  - "[[memory-poisoning|Memory Poisoning (Agentic AI)]]"
  - "[[lethal-trifecta|Lethal Trifecta]]"
  - "[[varonis|Varonis]]"
sources:
  - ".raw/articles/oecd-aim-copilot-cosnitch-incident-2026-08-20.md"
  - "https://oecd.ai/en/incidents/2026-08-18-da47"
  - "https://www.varonis.com/blog/cosnitch"
  - "https://www.varonis.com/blog/searchleak"
  - "https://www.varonis.com/blog/reprompt"
  - "https://thehackernews.com/2026/08/microsoft-copilot-personal-flaws-could.html"
  - "https://www.computerworld.com/article/4211325/microsoft-finally-patches-critical-one-click-copilot-vulnerability-more-than-eight-months-after-learning-of-it.html"
---

# CoSnitch: Copilot Personal Data Exfiltration

## Summary

CoSnitch (CVE-2026-24301) is a three-stage vulnerability chain in Microsoft Copilot Personal, disclosed to Microsoft by [[varonis|Varonis]] Threat Labs in December 2025 and patched August 18, 2026.[^varonis] An undocumented URL parameter let an attacker's prompt execute in a victim's authenticated Copilot session the instant a crafted link loaded, with no click past the link itself and no confirmation step. From that foothold, the chain reached the victim's connected apps (Gmail, Drive, Calendar) and exfiltrated data through Copilot's own URL-fetch capability, and separately planted instructions in Copilot's persistent memory that the vendor states survive a password change, a session revocation, and device re-enrollment. Microsoft credits the finding as affecting Copilot Personal only, not Microsoft 365 Copilot Enterprise.[^cw] Varonis's discovery method — interrogating Copilot about its own architecture until the model disclosed the undocumented parameter mid-refusal — is itself the notable part of the disclosure.

## Attack Vector

The chain has three stages, and only the third is [[indirect-prompt-injection|indirect prompt injection]] in the classic retrieval sense; the first is closer to a reflected, URL-carried form of [[prompt-injection|prompt injection]] than to content the model retrieves and misreads.

**1. Auto-run URL parameter.** `https://copilot.microsoft.com/?q=<malicious_prompt>&autorun=1` combined a documented parameter (`q`, which pre-fills the chat input) with an undocumented one (`autorun`, which submits that input without user confirmation). Both had to be present: `q` alone still needs the victim to press Enter, which is the shape of Varonis's earlier **Reprompt** finding against Copilot Personal.[^varonis] With `autorun` added, the attacker's prompt ran inside the victim's authenticated session — full access to connected apps and to Copilot's persistent memory — the moment the link loaded. CoSnitch is the third Copilot flaw Varonis reports finding in 2026, after Reprompt and **SearchLeak** (CVE-2026-42824, against Microsoft 365 Copilot *Enterprise*, chaining the same `q`-parameter injection with an HTML race condition and an SSRF through Bing's CSP-allowlisted image-search endpoint).[^varonis] Varonis states the pattern common to all three is that one click on a legitimate-looking link is enough; the mechanism after that click differs each time.

**2. Exfiltration via connected-app access and Copilot's own URL-fetch.** The auto-run prompt directed Copilot to locate sensitive content across its connected sources (email bodies and metadata, Drive file names and metadata, calendar entries, and Copilot's own chat history), concatenate and base64-encode the result to evade content filters, then encode that payload into a URL path and fetch it — a request Copilot's own URL-fetch tool issued, which registers as ordinary web-summarization traffic rather than as an anomalous outbound call.[^varonis] The receiving end was an attacker-controlled webhook.

**3. Memory poisoning via a second, separate injection.** Asking Copilot to summarize an external webpage carrying hidden instructions caused those instructions to be written into Copilot's persistent memory as though legitimate — the [[indirect-prompt-injection|indirect prompt injection]] stage proper, and the same mechanism [[memory-poisoning|memory poisoning]] describes as stored injection. Varonis states the resulting entries are permanent: no automatic expiration, active across all future sessions until the user manually deletes them, and unaffected by a password change, a session revocation, or device re-enrollment — properties that outlast every credential-based containment a defender would normally reach for first. Varonis also states the write leaves no forensic footprint a conventional security tool would flag (no process, file, or network-log signature).[^varonis]

The three legs of the [[lethal-trifecta|Lethal Trifecta]] are present but split unevenly across the two injection paths: stage 1 supplies the private-data-access leg directly through the victim's own authenticated session, without needing untrusted content at all; stage 3 supplies the untrusted-content-ingestion leg through the summarized webpage; and both use Copilot's own connectors and fetch tooling for the external-communication leg. Where [[echoleak-copilot-zero-click|EchoLeak]] chained four bypasses around a single retrieval path to reach zero-click exfiltration, CoSnitch reaches a comparable outcome by combining a URL-parameter defect that needs no retrieved content at all with a second, conventional indirect-injection stage for persistence — two different vulnerability classes in the same product, not one class exploited twice.

## Discovery Method

Varonis researchers asked Copilot to explain why silent auto-execution should be impossible, then reframed each refusal as a follow-up question. Copilot's refusals carried their own technical justifications, which narrowed the researchers' search with each answer; the model eventually disclosed the undocumented `autorun` parameter unprompted, mid-refusal, along with its historical behavior and the protections Microsoft had put in place against it.[^varonis] The vulnerability chain was assembled from information the target system volunteered under interrogation, not from source access, binary analysis, or conventional fuzzing.

## Timeline

- **2025-12** — Varonis reports CoSnitch to Microsoft (Computerworld dates the report to December 31, 2025; Varonis and TheHackerNews both give December 2025).[^cw][^thn]
- **2026-02-01** — Microsoft ships a partial fix addressing the auto-execution parameter only.[^cw]
- **2026-08-18** — Microsoft ships the complete fix and CVE-2026-24301 is published; the OECD AI Incidents Monitor registers the incident the same day.[^oecd]

Computerworld and CSO Online both characterize the gap between report and complete fix as "almost eight months."[^cw]

## Defensive Lessons

**A single-purpose auto-execution flag is a reflected-injection surface, independent of any content-filtering control.** Every defense in the [[indirect-prompt-injection|indirect prompt injection]] literature — classifier screening, provenance tagging, retrieval-side filtering — assumes the malicious instruction arrives as retrieved content the model must be tricked into treating as trusted. Stage 1 of CoSnitch bypassed that entire control family by placing the attacker's prompt directly in a URL parameter the client executed without ever routing it through a retrieval or classification path. A URL-parameter or query-string field that can pre-fill and submit an AI assistant's input is attack surface equivalent to a reflected web vulnerability; review it inside conventional web-application security review, alongside any other unauthenticated GET-triggered action, in addition to prompt-injection threat modeling.

**Stated memory-poisoning persistence exceeds what [[memory-poisoning|the concept page]]'s existing production example documents.** Microsoft Defender for Cloud Apps' March 2026 finding of 50+ memory-injection cases described persistence across sessions; Varonis's CoSnitch claim — survival across password change, session revocation, and device re-enrollment, with no forensic footprint — describes persistence across every credential-based control a defender would reach for first. If accurate, incident response for a suspected Copilot memory compromise cannot rely on session or credential invalidation and must instead target the memory store directly, which argues for the state-rollback and partitioned-memory controls the concept page already catalogs over any identity-layer response.

**A vendor's product-scope exemption is a claim that a converging product line can outrun.** Microsoft states Microsoft 365 Copilot Enterprise is unaffected by CoSnitch specifically; Computerworld disputes the practical force of that statement, citing Microsoft's announced "Copilot Fusion" plan to merge the personal and enterprise Copilot lines, and noting that mixed environments running both today already blur the boundary the exemption assumes.[^cw] TheHackerNews independently notes that the underlying Varonis research does not itself state whether CoSnitch's specific behavior reaches Microsoft 365 Copilot; the enterprise-safe claim for *this* finding originates with Microsoft alone.[^thn] The product line's track record argues against treating any exemption as durable: Varonis's own prior finding, SearchLeak, targeted Microsoft 365 Copilot Enterprise directly and was already patched by the time CoSnitch was disclosed.[^varonis] A defender relying on a vendor's affected-product statement should confirm what the *research* establishes versus what the *vendor* asserts, particularly where the same vendor's own disclosure history already includes an enterprise-scoped finding.

**The disclosure method is itself a red-team technique worth generalizing.** Extracting an undocumented parameter by interrogating a production system's refusals is a reconnaissance technique targeting the model as a component rather than its outputs, closer to social-engineering a support system than to a conventional prompt-injection payload. No wiki page currently catalogs "model self-disclosure under adversarial questioning" as a named technique; the closest existing concept, [[recursive-prompt-injection|Recursive Prompt Injection]], covers a model being turned into an unwitting evaluator of injected content rather than being talked into revealing its own architecture. This gap is unaddressed here and is a candidate for future research.

## Sources

See frontmatter `sources:`.

## Notes

[^varonis]: [Varonis Threat Labs — "CoSnitch: When Your AI Assistant Becomes Its Own Whistleblower"](https://www.varonis.com/blog/cosnitch), 2026-08. Attack-chain mechanism, the `q`/`autorun` parameter pair, the three-stage exfiltration description, the memory-poisoning persistence claims, the discovery-method account, and the "third finding in 2026" framing are drawn from this primary source, which also links directly to [Reprompt](https://www.varonis.com/blog/reprompt) (last updated 2026-06-16) and [SearchLeak](https://www.varonis.com/blog/searchleak) (last updated 2026-06-15), used here for those findings' own CVE and mechanism detail.
[^cw]: [Computerworld — "Microsoft finally patches critical one-click Copilot vulnerability, almost eight months after learning of it"](https://www.computerworld.com/article/4211325/microsoft-finally-patches-critical-one-click-copilot-vulnerability-more-than-eight-months-after-learning-of-it.html), 2026-08. Timeline dates (report December 31, 2025; partial patch February 1, 2026; complete patch August 18, 2026), Microsoft's quoted statement, and the Copilot Fusion enterprise-scope dispute are drawn from this source (cross-published at CSO Online).
[^thn]: [The Hacker News — "Microsoft Copilot Personal Flaws Could Let One Click Exfiltrate Data From Connected Apps"](https://thehackernews.com/2026/08/microsoft-copilot-personal-flaws-could.html), 2026-08. Confirms CVE-2026-24301 and the December 2025 report date; states the underlying research does not itself establish Microsoft 365 Copilot's exposure.
[^oecd]: [OECD AI Incidents Monitor — incident 2026-08-18-da47](https://oecd.ai/en/incidents/2026-08-18-da47), retrieved 2026-08-20. Registry entry and taxonomy classification (harm type, AI principles, industries, stakeholders); no CVSS score or technical mechanism given in the registry entry itself.
