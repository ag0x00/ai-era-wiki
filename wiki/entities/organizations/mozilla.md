---
type: entity
entity_type: organization
org_type: vendor
title: "Mozilla"
address: c-000091
created: 2026-05-22
updated: 2026-06-21
tags:
  - entities
  - organizations
  - glasswing
  - ai-vuln-discovery
  - ai-in-sec-defense
  - open-source
  - firefox
status: stub
scope_axis:
  - ai-in-sec-defense
homepage: "https://www.mozilla.org"
website: "https://www.mozilla.org/"
role: "Tested Claude Mythos Preview against Firefox; found and fixed 271 vulnerabilities in Firefox 150, over ten times the Firefox 148 count under Claude Opus 4.6"
related:
  - "[[glasswing]]"
  - "[[mythos]]"
  - "[[anthropic-glasswing-initial-update]]"
  - "[[zero-day-clock]]"
sources:
  - "[[anthropic-glasswing-initial-update]]"
  - "https://blog.mozilla.org/en/privacy-security/ai-security-zero-day-vulnerabilities/"
  - "https://hacks.mozilla.org/2026/05/behind-the-scenes-hardening-firefox/"
---

# Mozilla

**Sources:** [Mozilla (homepage)](https://www.mozilla.org) · [Mozilla security blog: AI-found zero-days](https://blog.mozilla.org/en/privacy-security/ai-security-zero-day-vulnerabilities/) · [Behind the scenes: hardening Firefox](https://hacks.mozilla.org/2026/05/behind-the-scenes-hardening-firefox/)

Mozilla is the open-source organization that develops the Firefox browser. It tested [[mythos|Claude Mythos Preview]] against Firefox in coordination with [[glasswing|Project Glasswing]] and published its own write-ups of the experience.

## Firefox 150 Result

Per [[anthropic-glasswing-initial-update|Anthropic's one-month Glasswing update]] and Mozilla's own [security blog](https://blog.mozilla.org/en/privacy-security/ai-security-zero-day-vulnerabilities/) and [hardening write-up](https://hacks.mozilla.org/2026/05/behind-the-scenes-hardening-firefox/), Mozilla **found and fixed 271 vulnerabilities in Firefox 150** while testing Mythos Preview — **over ten times** the number it found in Firefox 148 with [[mythos|Claude Opus 4.6]].

**Most AI-found vulnerabilities never become CVEs.** The [[mythos-ready-briefing|Mythos-ready briefing]] noted that of Mozilla's 271 disclosed vulnerabilities, only **3 warranted CVEs**. This matters for the [[zero-day-clock|Zero Day Clock]]: TTE curves built on CVE-exploit pairs undercount AI-driven discovery, because the vast majority of AI-found-and-fixed bugs are patched without ever entering the CVE system. Mozilla's Firefox 150 figure is the cleanest single illustration of the gap between *AI discovery volume* and *CVE volume*.

## See Also

- [[glasswing|Project Glasswing]] — the coalition context.
- [[anthropic-glasswing-initial-update|Glasswing initial update]] — primary source for the 271 figure.
- [[zero-day-clock|Zero Day Clock]] — where the CVE-undercount caveat is load-bearing.
- [[mythos|Claude Mythos Preview]] — the model tested.
