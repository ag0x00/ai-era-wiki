---
type: incident
title: "Cursor npm Credential Stealer"
created: 2026-05-03
updated: 2026-05-03
tags:
  - incidents
  - supply-chain
  - npm
  - cursor
  - ide
  - credential-theft
  - persistence
status: published
incident_class: supply-chain
attack_with_or_on_ai: "on AI"
date_observed: before-2025-05-07
date_disclosed: 2025-05-07
target: "macOS Cursor IDE users"
threat_actor: "gtr2018 / aiide (npm aliases)"
impact: "3,200+ macOS Cursor users compromised; credentials stolen, main.js replaced, auto-update disabled for persistence"
related:
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[sandworm-mode-npm-worm]]"
  - "[[litellm-supply-chain-compromise]]"
sources:
  - ".raw/articles/socket-cursor-npm-credential-stealer-2025-05-07.md"
  - "https://socket.dev/blog/malicious-npm-packages-hijack-cursor-editor-on-macos"
aliases:
  - incidents/cursor-npm-credential-stealer-2025
  - cursor-npm-credential-stealer-2025

---

# Cursor npm Credential Stealer (May 2025)

## Summary

In May 2025, the Socket Threat Research Team discovered three malicious npm packages — `sw-cur` (2,771 downloads), `sw-cur1` (307), and `aiide-cur` (163) — totaling over 3,200 downloads and targeting macOS users of the [[cursor-ide|Cursor]] AI code editor. Published by threat actors using the npm aliases `gtr2018` and `aiide` (registered under `404228858@qq.com` and `touzi_xiansheng@outlook.com`), the packages were marketed as "the cheapest Cursor API service on the entire internet." Upon execution, each package harvested the user's Cursor credentials, fetched an AES-encrypted secondary payload from attacker-controlled infrastructure, overwrote Cursor's internal `main.js` with attacker logic, and — in the case of `sw-cur` — disabled Cursor's auto-update mechanism to maintain persistence. The packages established C2 channels capable of executing cd, upload, and arbitrary shell commands inside the trojanized IDE. This incident is one of four supply-chain attacks Andrew Bullen cited in the March 2026 [[breaking-the-lethal-trifecta-talk|"Breaking the Lethal Trifecta"]] talk at Unprompted to illustrate real-world toolchain poisoning risk.

## Attack Vector

### The Three Packages

| Package | Downloads | Publisher alias | C2 domain |
|---|---|---|---|
| `sw-cur` | 2,771 | gtr2018 | `cursor[.]sw2031[.]com` / `t[.]sw2031[.]com` |
| `sw-cur1` | 307 | gtr2018 | `cursor[.]sw2031[.]com` |
| `aiide-cur` | 163 | aiide | `aiide[.]xyz` |

All three shared identical credential-exfiltration, AES-decryption, and `main.js`-patch logic, differing only in hardcoded C2 domain and (in `sw-cur`) the additional `disableAutoUpdate()` call.

### Social Engineering Lure

The packages exploited a genuine pain point in Cursor's pricing model: premium LLM calls (e.g., \$0.30 per o3 invocation) lead some developers to seek cheaper or unofficial integrations. The package description (`sw-cur` in Chinese: "Providing the cheapest Cursor API service on the entire internet — Shengwei Technology") was specifically crafted to attract cost-conscious developers willing to install an unofficial helper package.

### Execution Flow

1. **Credential harvest.** On install, the script URL-encodes the user's Cursor credentials and exfiltrates them via a GET request to `/api/login` on the C2 server. The C2 server presents a generic ElementAdmin login page to appear benign.
2. **Encrypted payload fetch.** The script downloads an AES-encrypted, gzip-compressed JavaScript loader from the C2.
3. **Decrypt and decompress.** All three variants use the identical hardcoded 32-byte AES key `a8f2e9c4b7d6m3k5n1p0q9r8s7t6u5v4`.
4. **`main.js` replacement.** The legitimate Cursor file at `/Applications/Cursor.app/Contents/Resources/app/extensions/cursor-always-local/dist/main.js` is backed up (`.bak`), then overwritten with the attacker-controlled payload, which injects the stolen credentials and establishes persistent backdoor logic.
5. **Persistence.** `sw-cur` calls `disableAutoUpdate()` and kills `chrome_crashpad_handler` and all Cursor processes, ensuring the trojaned binary loads on next launch without being replaced by an auto-update. `sw-cur1` and `aiide-cur` skip the kill step but prompt the user to restart, achieving the same activation.
6. **C2 command execution.** The backdoored IDE receives and executes cd, upload, and shell commands from the attacker's server.

### Indicators of Compromise (IoCs)

**Malicious packages:** `sw-cur`, `sw-cur1`, `aiide-cur`

**Threat actor identifiers:**
- npm alias: `gtr2018`
- npm alias: `aiide`
- Email: `404228858@qq[.]com`
- Email: `touzi_xiansheng@outlook[.]com`

**C2 domains:**
- `cursor[.]sw2031[.]com`
- `cursor[.]sw2031[.]com/api/login`
- `t[.]sw2031[.]com`
- `aiide[.]xyz`
- `aiide[.]xyz/api/login`

**AES decryption key:** `a8f2e9c4b7d6m3k5n1p0q9r8s7t6u5v4`

**Modified file path:** `/Applications/Cursor.app/Contents/Resources/app/extensions/cursor-always-local/dist/main.js`

### MITRE ATT&CK Coverage

- T1195.002 — Supply Chain Compromise: Compromise Software Supply Chain
- T1059.007 — Command & Scripting Interpreter: JavaScript
- T1608.001 — Stage Capabilities: Upload Malware
- T1204.002 — User Execution: Malicious File
- T1027.013 — Obfuscated Files or Information: Encrypted/Encoded File
- T1574 — Hijack Execution Flow
- T1071.001 — Application Layer Protocol: Web Protocols
- T1041 — Exfiltration Over C2 Channel

## Timeline

- **Before 2025-05-07** — packages `sw-cur`, `sw-cur1`, and `aiide-cur` published to npm; 3,200+ downloads accumulate.
- **2025-05-07** — Socket Threat Research discloses the campaign publicly and petitions npm for removal; packages remain live at time of writing.
- **Remediation note** — Socket recommends restoring Cursor from a verified installer (downgrade to v2.0.82 is cited as safe baseline), rotating all affected credentials, and auditing source control and CI/CD artifacts. Package uninstall alone does NOT remove the malware; the trojanized `main.js` persists until the application is reinstalled from a clean source.

## Defensive Lessons

**Uninstalling the malicious npm package does not remediate the compromise.** The attacker's payload is written into the IDE binary itself. Standard "remove the bad package" remediation is insufficient; full application reinstall and credential rotation are required.

**1. npm supply-chain risk is now explicitly targeting AI IDE users.** The Cursor-specific targeting is deliberate: AI-first IDEs like Cursor operate with elevated developer trust (access to credentials, source code, CI/CD pipelines). Attackers are moving up the trust hierarchy — from compromising libraries to compromising the toolchain itself. Compare [[sandworm-mode-npm-worm|SANDWORM_MODE]] (toolchain poisoning via MCP injection) and [[litellm-supply-chain-compromise|LiteLLM supply chain compromise]] for parallel 2025 patterns.

**2. "Cheaper API" social engineering is a recurring AI-specific lure.** Cost sensitivity around LLM API fees creates a new attack surface that did not exist in the pre-LLM era. Developers evaluating "discount" or "unofficial" AI integrations need the same skepticism applied to any unvetted package. Organizations should consider allowlisting approved AI-integration packages and flagging post-install scripts that access application directories.

**3. Persistence via tampered IDE binary defeats package-manager remediation.** The attack's design specifically anticipates the standard response (remove the package) and survives it. This is analogous to a rootkit pattern applied to developer tooling. Detection requires filesystem integrity monitoring of IDE installation paths, not just registry/package-manager audit logs.

**4. Elevated IDE trust is an underappreciated attack surface.** AI IDEs increasingly act as agents — they have filesystem access, network access, and run with the developer's credentials. The trust model that makes them productive (wide ambient access) also makes them high-value targets. The [[supply-chain-security-for-agents|Supply Chain Security for Agents]] practice page covers the control cluster (cognitive file integrity, Brain Git rollback, skill marketplace vetting) that addresses this pattern in the agentic context.

**5. Socket's behavioral-analysis approach (vs. static signatures) is the right detection primitive.** The malicious packages were caught because they exhibited suspicious behavioral patterns: post-install scripts writing to `/Applications/` paths, credentials being passed in GET request URLs, outbound requests to novel domains. Signature-only scanning would miss first-appearance packages. This aligns with the [[ai-bom|AI-BOM]] behavioral-baseline philosophy applied to the dependency layer.

## Sources

See frontmatter `sources:`.
