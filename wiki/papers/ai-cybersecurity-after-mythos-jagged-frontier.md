---
type: paper
title: "AI Cybersecurity After Mythos: The Jagged Frontier"
address: c-000154
created: 2026-05-26
updated: 2026-05-29
tags:
  - papers
  - vulnerability-discovery
  - model-capability
  - open-weights
  - ai-vuln-discovery
status: summarized
scope_axis:
  - ai-in-sec-defense
year: 2026
authors: ["Stanislav Fort"]
publisher: "AISLE"
key_claim: "AI cybersecurity capability is jagged — it does not scale smoothly with model size, generation, or price — and the discovery-and-analysis layer is broadly accessible with small, cheap, open-weights models once a good scaffold isolates the relevant code. The defensible advantage (the moat) is the system that targets, validates, triages, and earns maintainer trust, not the single frontier model."
methodology: "Opinion-and-evidence blog post by AISLE's founder. Scoped single-call probes: the showcase vulnerabilities from Anthropic's Mythos announcement were isolated to their vulnerable functions and run through ~25 models (small/cheap/open-weights and frontier) for detection, exploitability reasoning, and false-positive discrimination. Not end-to-end autonomous discovery. Full transcripts published on GitHub."
source_url: "https://aisle.com/blog/ai-cybersecurity-after-mythos-the-jagged-frontier"
related:
  - "[[jagged-frontier]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[mythos]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[aisle]]"
  - "[[stanislav-fort]]"
  - "[[aisle-openssl-12-of-12]]"
sources:
  - ".raw/articles/ai-cybersecurity-after-mythos-the-jagged-frontier-2026-05-26.md"
  - "https://aisle.com/blog/ai-cybersecurity-after-mythos-the-jagged-frontier"
---

# AI Cybersecurity After Mythos: The Jagged Frontier

A blog post by [[stanislav-fort|Stanislav Fort]] (Founder and Chief Scientist, [[aisle|AISLE]]) responding to Anthropic's [[anthropic-glasswing-announcement|Claude Mythos / Project Glasswing announcement]]. It accepts that the Mythos results are real and the mission shared, but argues the framing is overstated: the work does not depend on a restricted frontier model.

> [!contradiction] Announcement date — resolved toward May 12
> This post's body says Anthropic announced Mythos and Glasswing "on April 7" and carries an "April 9" update. That conflicts with the **May 12, 2026** date the rest of the wiki uses, which is the better-sourced one: Anthropic's own [[anthropic-glasswing-announcement|Glasswing announcement]] reads "Today we're announcing Project Glasswing" (captured May 13), [[xbow-mythos-evaluation|XBOW's evaluation]] is dated "May 12, 2026," and Microsoft's MDASH launch is described as same-day. A rebuttal cannot predate the announcement it answers, so the April dates in this post are treated as a source error; **May 12 stands.**

## The argument

The post decomposes AI cybersecurity into a modular pipeline whose stages scale very differently: broad-spectrum scanning, vulnerability detection, triage and verification, patch generation, and (sometimes) exploit construction. The Mythos narrative blends these into one integrated capability, which suggests all of them need frontier-scale intelligence. Fort's claim is that they do not. The detection-and-analysis stages are broadly accessible with small, cheap, open-weights models, while only the creative exploit-engineering step still separates frontier-class systems.

The conclusion stated up front: **the moat in AI cybersecurity is the system, not the model.** The moat is the targeting, iterative deepening, validation, triage, and maintainer trust around the model, which AISLE reports building with multiple model families rather than one frontier API.

## The evidence — capability is [[jagged-frontier|jagged]]

Fort isolated the vulnerable functions from Mythos's showcase findings and ran them through small/cheap/open-weights models as single zero-shot API calls (no tools, no agentic loop).[^post]

- **FreeBSD NFS overflow (CVE-2026-4747)** — Mythos's flagship "fully autonomous" remote-root find. Detected by 8 of 8 models tested, including a 3.6B-active model at ~\$0.11 per million tokens; most computed the remaining buffer space and graded it critical/RCE. On exploitability reasoning, models independently identified that an `int32_t` array defeats `-fstack-protector` and that disabled KASLR fixes gadget addresses, and sketched workable ROP strategies. None reproduced Mythos's specific multi-round delivery (splitting the >1000-byte chain across 15 RPC requests of 32 bytes each), but several proposed different valid ways around the 304-byte payload limit.
- **OpenBSD TCP SACK bug (27 years old)** — the subtlest find, requiring reasoning about signed-integer overflow in `SEQ_LT`/`SEQ_GT` and a NULL-deref append path. A 5.1B-active open model (GPT-OSS-120b) recovered the full public chain and proposed essentially the actual patch; a 32B dense model declared the code "robust." Rankings reshuffled completely versus the FreeBSD task.
- **OWASP false-positive task** — a Java servlet that looks like SQL injection but discards the user input via a list operation. Small open models (GPT-OSS-20b, DeepSeek R1) traced it correctly, while many larger frontier models (through Opus 4.5) reported a confident false positive. Smaller models outperformed larger ones, the inverse of the expected scaling.

## Sensitivity versus specificity

A post-publication update tested the *patched* FreeBSD function. Detection (sensitivity) stayed near-perfect, but specificity was weak: most models that find the bug also false-positive on the fix, fabricating an incorrect signed-integer bypass (the field is `u_int`). Only GPT-OSS-120b was reliable in both directions across reruns.[^post] Fort frames this as the central argument for the scaffold. A model that false-positives on patched code would drown maintainers in noise, so the triage and validation layer is what makes the system usable.

## Production context

AISLE reports operating a discovery-and-remediation system against live targets since mid-2025: 15 CVEs in OpenSSL (including [[aisle-openssl-12-of-12|12 of 12 in a single release]], some 25+ years old, one CVSS 9.8), 5 in curl, and over 180 externally validated CVEs across 30+ projects. The metric Fort emphasizes is maintainer acceptance, not benchmark score.

**Defensive implication.** For the defensive use case Project Glasswing targets, reliable discovery, triage, and patching matter more than full exploit construction — and those capabilities are accessible with cheap models now. Treating the capability as exclusive to one restricted frontier model risks discouraging adopters and concentrating a critical defensive capability behind a single API. The bottleneck Fort names is security expertise and engineering, not model access.

## Notes

[^post]: Stanislav Fort (AISLE), "AI Cybersecurity After Mythos: The Jagged Frontier" (2026). [aisle.com/blog/ai-cybersecurity-after-mythos-the-jagged-frontier](https://aisle.com/blog/ai-cybersecurity-after-mythos-the-jagged-frontier). Detection/exploitation/false-positive transcripts are published at [github.com/stanislavfort/mythos-jagged-frontier](https://github.com/stanislavfort/mythos-jagged-frontier). All results are scoped single-call probes on isolated functions, stated by the author as an upper bound on fully autonomous performance, not end-to-end discovery.
