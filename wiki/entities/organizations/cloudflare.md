---
type: entity
entity_type: organization
org_type: vendor
title: "Cloudflare"
address: c-000090
created: 2026-05-22
updated: 2026-06-21
tags:
  - entities
  - organizations
  - glasswing
  - ai-vuln-discovery
  - ai-in-sec-defense
  - critical-infrastructure
status: stub
scope_axis:
  - ai-in-sec-defense
website: "https://www.cloudflare.com/"
homepage: "https://www.cloudflare.com"
role: "Project Glasswing partner; applied Claude Mythos Preview to its critical-path systems and reported 2,000 bugs found at a false-positive rate better than human testers"
related:
  - "[[glasswing]]"
  - "[[mythos]]"
  - "[[anthropic-glasswing-initial-update]]"
sources:
  - "[[anthropic-glasswing-initial-update]]"
  - "https://blog.cloudflare.com/cyber-frontier-models/"
---

# Cloudflare

**Sources:** [Cloudflare (homepage)](https://www.cloudflare.com) · [Cyber Frontier Models blog post](https://blog.cloudflare.com/cyber-frontier-models/)

Cloudflare is an internet-infrastructure and security company and a partner in [[glasswing|Project Glasswing]]. Its inclusion among the ~50 Glasswing partners places it in the coalition's extended membership beyond the [[anthropic-glasswing-announcement|twelve named launch partners]].

## Glasswing Result (One Month In)

Per [[anthropic-glasswing-initial-update|Anthropic's one-month update]] and Cloudflare's own [Cyber Frontier Models post](https://blog.cloudflare.com/cyber-frontier-models/), Cloudflare used [[mythos|Claude Mythos Preview]] to find **2,000 bugs** across its critical-path systems, of which **400 were high- or critical-severity**, at a **false-positive rate the Cloudflare team considers better than human testers**.

**FP rate "better than human testers".** Cloudflare's false-positive-rate claim is one of the clearest first-party defender statements that frontier-AI vulnerability discovery has crossed the precision threshold where it competes with skilled human review — not just on recall but on precision. It corroborates the [[frontier-ai-for-vuln-discovery|harness-over-model precision argument]] from a deploying enterprise rather than a model vendor.

## See Also

- [[glasswing|Project Glasswing]] — the coalition.
- [[anthropic-glasswing-initial-update|Glasswing initial update]] — source for the result above.
- [[mythos|Claude Mythos Preview]] — the model deployed.
