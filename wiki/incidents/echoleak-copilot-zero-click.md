---
type: incident
title: "EchoLeak Zero-Click Copilot Exfiltration"
address: c-000312
created: 2026-08-20
updated: 2026-08-20
tags:
  - incidents
  - prompt-injection
  - indirect-prompt-injection
  - microsoft-copilot
  - zero-click
  - exfiltration
  - cve
status: published
scope_axis:
  - sec-of-ai
origin: aggregated
incident_class: prompt-injection
attack_with_or_on_ai: "on AI"
date_observed: 2025-01
date_disclosed: 2025-06-11
target: "Microsoft 365 Copilot"
threat_actor: "research-disclosure (Aim Security / Aim Labs)"
impact: "Remote, unauthenticated, zero-click exfiltration of OneDrive, SharePoint, Teams, and email content in Copilot's access scope"
related:
  - "[[lethal-trifecta]]"
  - "[[indirect-prompt-injection]]"
  - "[[three-retrieval-paths]]"
  - "[[geminijack-gemini-enterprise-injection]]"
  - "[[slack-ai-private-channel-exfiltration]]"
sources:
  - ".raw/articles/echoleak-arxiv-case-study-2025-09-06.md"
  - "https://arxiv.org/abs/2509.10540"
  - "https://nvd.nist.gov/vuln/detail/CVE-2025-32711"
---

# EchoLeak Zero-Click Copilot Exfiltration

## Summary

EchoLeak (CVE-2025-32711) is a critical zero-click [[indirect-prompt-injection|indirect prompt injection]] vulnerability in Microsoft 365 Copilot, discovered by Aim Security (Aim Labs) in January 2025 and publicly disclosed June 11, 2025. A single crafted email — requiring no attachment, no malware, and no action from the recipient beyond Copilot's normal operation — let a remote, unauthenticated attacker exfiltrate any data within Copilot's access scope: mailbox content, OneDrive files, SharePoint documents, and Teams messages. Aim Labs categorized the underlying failure as an **"LLM Scope Violation"**: the model was tricked into crossing its trust boundary and disclosing internal data on the strength of instructions hidden in untrusted, externally-supplied content. It is the first documented case of prompt injection weaponized into concrete data exfiltration against a shipping enterprise AI product, and the source case for the "full-text / direct retrieval" path in [[three-retrieval-paths|Three Retrieval Paths for Injection Payloads]].

## Attack Vector

EchoLeak chained four independent bypasses, each defeating one Copilot safeguard in sequence.[^arxiv]

1. **XPIA classifier bypass.** Microsoft's cross-prompt-injection-attempt (XPIA) classifier screens inbound content for injection language. The attacker's email was phrased as an ordinary business request to the *human* recipient rather than an imperative command to the AI — for example, asking that "confidential project files" be compiled "for compliance" — evading the classifier while also instructing Copilot never to cite the source email, so the victim saw no sign an external message was involved.
2. **Link-redaction bypass.** Copilot's output filter redacted standard inline Markdown links (`[text](url)`). The exploit used reference-style Markdown links instead (`[text][ref]` with a separate `[ref]: url` definition), a syntactic variant the filter did not recognize, letting a link carrying stolen data as a URL parameter reach the rendered response.
3. **Image auto-fetch for zero-click egress.** A clickable link still required victim action. Substituting a reference-style *image* tag (`![alt][ref]`) caused the Copilot client to auto-fetch the attacker's URL as soon as the response rendered — no click required — but Copilot's content security policy (CSP) blocked direct requests to unlisted external domains.
4. **CSP bypass via a Microsoft Teams proxy.** The CSP allowlisted `asyncgw.teams.microsoft.com/urlp`, a Microsoft Teams asynchronous link-preview API that fetches an arbitrary URL passed to it as a parameter. Routing the image request through this endpoint let a first-party Microsoft service retrieve the attacker's URL — secret embedded in the path — on the client's behalf, completing exfiltration with a Microsoft IP or the victim's own IP visible in the attacker's server logs, and no user interaction of any kind.

The result satisfies all three legs of the [[lethal-trifecta|Lethal Trifecta]] inside a single Copilot session: private data access (mailbox/SharePoint/OneDrive), untrusted content ingestion (the crafted email), and external communication (the Teams-proxied image fetch) — with the third leg triggered automatically rather than by any victim click.

## Timeline

- **2025-01** — Aim Labs discovers the vulnerability, builds a working proof of concept, and reports it to Microsoft's Security Response Center (MSRC).
- **2025-04** — Initial, partial remediation work begins (staged fix, prior to full closure).
- **2025-05** — Microsoft deploys a server-side fix; no customer action required.
- **2025-06-11** — CVE-2025-32711 advisory published (CVSS 9.3, critical); Aim Labs publishes its research and public disclosure begins.
- **2025-06-12** — Follow-on technical and press analysis confirms the zero-click characterization and the absence of any observed in-the-wild exploitation.

Microsoft's own account places the completed server-side fix in May 2025, roughly six weeks before public disclosure, consistent with a standard coordinated-disclosure sequence.

## Defensive Lessons

**Single-layer defenses fail in sequence, not in aggregate.** Each of Copilot's three safeguards — the XPIA classifier, link redaction, and CSP — was individually reasonable and individually insufficient. EchoLeak did not defeat any one control through brute force; it found the syntactic edge case each control's implementation had not covered (reference-style Markdown, image versus link semantics, an allowlisted first-party proxy) and chained four such edge cases into full bypass. This is the general argument for defense-in-depth over any single filter: the [[indirect-prompt-injection|Indirect Prompt Injection]] literature treats this as the expected failure mode of classifier-based mitigation, not an implementation defect specific to Copilot.

**Zero-click removes the human from the exfiltration leg, not from the trigger.** Unlike [[slack-ai-private-channel-exfiltration|the Slack AI case]], where the victim had to click a crafted "reauthenticate" link to complete exfiltration, EchoLeak's image-auto-fetch mechanism required no click once Copilot rendered a response containing the payload. The victim still had to invoke Copilot for an ordinary work query (the RAG retrieval trigger is unchanged) — but any human-in-the-loop control premised on "the user must act to send data out" is defeated once the send leg is a passive client-side resource fetch. See [[lethal-trifecta|Lethal Trifecta]] §Manifestation for how this refines the trifecta's send leg.

**Every domain on a content-security-policy allowlist is itself attack surface.** The CSP bypass did not require breaking CSP; it required finding an allowlisted domain that itself performed unrestricted server-side URL fetches on request. Any first-party proxy or preview service added to a CSP allowlist inherits the CSP's trust and must be audited for exactly this SSRF-shaped capability — a pattern also true of the JFrog Artifactory proxy in the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]].

**Aim Security's original disclosure post is no longer independently reachable**; Aim Security was acquired by Cato Networks, and `aim.security` now redirects to the Cato Networks homepage rather than the original write-up. The peer-reviewed case study cited here (Reddy and Gujral, arXiv:2509.10540) is the most complete technical record still publicly available and is used as the primary source for this page.

## Sources

See frontmatter `sources:`.

## Notes

[^arxiv]: [Reddy, P. and Gujral, A.S. — "EchoLeak: The First Real-World Zero-Click Prompt Injection Exploit in a Production LLM System," arXiv:2509.10540](https://arxiv.org/abs/2509.10540), 2025-09-06. Attack-chain steps, timeline dates, and CVSS score are drawn from this source's Table 1 and vulnerability analysis section.
