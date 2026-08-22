---
type: paper
title: "AgentShield: Security Auditor for AI Agents"
address: c-000054
created: 2026-05-15
updated: 2026-08-21
tags:
  - papers
  - tool
  - agent-config-audit
  - claude-code
status: summarized
scope_axis: [sec-of-ai, ai-in-sec-offense, sec-against-ai]
year: 2026
authors:
  - "[[affaan-m|Affaan M]]"
venue: "GitHub README — `affaan-m/agentshield`"
source_url: https://github.com/affaan-m/agentshield
archived_copy: ".raw/articles/agentshield-2026-05-15.md"
key_claim: "Agentic-AI configuration surface — `.claude/` directories with hooks, MCP servers, skill manifests, subagents, and slash commands — is a first-class security audit target, and a 102-rule static scanner with provenance-aware confidence labels is a sufficient baseline control for the Claude Code class of harnesses."
methodology: "Rule-based static analysis across five categories (secrets, permissions, hooks, MCP servers, agents), graded 0–100 / A–F, with provenance-aware `runtimeConfidence` weighting that distinguishes active runtime config from project-local overrides, template catalogs, docs examples, plugin manifests, and manifest-resolved hook implementations; optional three-agent (Red Team / Blue Team / Auditor) Claude Opus 4.6 adversarial pipeline; built-in attack corpus regression gate."
related:
  - "[[openant-announcement|OpenAnt Announcement]]"
  - "[[agentshield|AgentShield product]]"
  - "[[affaan-m|Affaan M]]"
  - "[[supply-chain-security-for-agents|Supply Chain Security for Agents]]"
  - "[[ai-spm|AI Security Posture Management]]"
  - "[[mcp-security|MCP Security]]"
  - "[[prompt-injection-containment|Prompt Injection Containment]]"
  - "[[agent-sandboxing|Agent Sandboxing]]"
  - "[[lethal-trifecta|Lethal Trifecta]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
sources:
  - "[[.raw/articles/agentshield-2026-05-15.md]]"
  - https://github.com/affaan-m/agentshield
  - https://www.npmjs.com/package/ecc-agentshield
  - https://cerebralvalley.ai/e/claude-code-hackathon
aliases:
  - papers/agentshield-2026-05-15
  - agentshield-2026-05-15

---

# AgentShield — Security Auditor for AI Agent Configurations

**Source:** [GitHub — `affaan-m/agentshield`](https://github.com/affaan-m/agentshield) (README fetched 2026-05-15). Local copy: `.raw/articles/agentshield-2026-05-15.md`. NPM package `ecc-agentshield`. Built at the [Claude Code Hackathon](https://cerebralvalley.ai/e/claude-code-hackathon) (Cerebral Valley × Anthropic, Feb 2026). Part of the *Everything Claude Code* ecosystem.

## Key Claim

The agent-configuration surface — `~/.claude/` directories containing hooks, MCP servers, skill manifests, subagents, slash commands, and `CLAUDE.md` instructions — is itself a software-supply-chain artifact whose security can and should be statically audited. A 102-rule static scanner with provenance-aware confidence labels (active runtime vs. template-example vs. docs-example vs. plugin-manifest vs. manifest-resolved hook implementation) is a sufficient baseline control for the Claude Code class of harnesses; deeper analysis (taint, sandbox execution, prompt-injection probing, multi-agent adversarial review) layers on top.

## Methodology

Static analysis over the discovered config tree, intentionally skipping common generated directories (`node_modules`, build output, `.dmux` worktree mirrors) so transient copies do not duplicate findings. Rules grouped into five categories with the rule counts the repository advertises:

| Category | Rules | Severity range |
| :--- | :--- | :--- |
| Secrets | 10 (14 patterns) | Critical -- Medium |
| Permissions | 10 | Critical -- Medium |
| Hooks | 34 | Critical -- Low |
| MCP Servers | 23 | Critical -- Info |
| Agents | 25 | Critical -- Info |
| **Total** | **102 / 14 patterns** | |

Scoring is per-category 0–100 with an aggregate A–F grade. Provenance-aware weighting discounts non-secret findings whose source has lower runtime confidence: `template-example` and `docs-example` at `0.25×`, `project-local-optional` at `0.75×`, `plugin-manifest` at `0.5×`. Real secrets stay at full weight everywhere. A single template file is capped at 10 deduction points per score category so one catalog cannot dominate the grade.

Optional layered analyses:

- `--injection` — active prompt-injection probing.
- `--sandbox` — execute hooks in a sandbox and observe runtime behavior.
- `--taint` — data-flow tracking across hooks, agents, and MCP config.
- `--opus` — three-agent Claude Opus 4.6 adversarial pipeline ([[red-team-blue-team-auditor-pipeline|Red Team / Blue Team / Auditor]] pattern; requires `ANTHROPIC_API_KEY`). `--opus --stream` runs them sequentially with real-time reasoning output.
- `--corpus-gate` — fail CI if the built-in attack corpus regresses (env proxy / DNS exfiltration / runtime import mutation / env-token exfiltration / credential-store access / clipboard access).
- `--supply-chain[-online]` — MCP package provenance (npm vs. git, pinned vs. unpinned, postinstall scripts, deprecation, package age).

Output surfaces: terminal, JSON, Markdown, HTML, SARIF, evidence pack (deterministic bundle with SHA-256 digests of `manifest.json`, `agentshield-report.{json,html}`, `agentshield-results.sarif`, `policy-evaluation.json`, `baseline-comparison.json`, `supply-chain.json`, `remediation-plan.json`), and stable-fingerprint remediation-plan queue. Evidence packs redact local paths, usernames, emails, and token-shaped strings by default.

## Notable Findings

- **Five-category coverage of the Claude Code config surface** that is otherwise unaudited end-to-end. Secrets cover the obvious patterns (`sk-ant-`, `sk-proj-`, `AKIA`, `AIza`, `ghp_`, `xox[bprs]-`, JWT, bearer, postgres / mongo / mysql / redis URIs, private-key material) but also catch *env-leak forms* — `echo $SECRET` inside hooks, secrets passed through environment instead of vault references. Permissions catches wildcard allow rules (`Bash(*)`, `Write(*)`, `Edit(*)`), missing deny lists (`rm -rf`, `sudo`, `chmod 777`), and dangerous flags (`--dangerously-skip-permissions`). Hooks rules cover command injection via `${file}` interpolation, exfiltration through `curl -X POST` with variable interpolation, silent-error patterns (`2>/dev/null`, `|| true`), session-startup remote-script execution, package installs (`npm install -g`, `pip install`, `gem install`, `cargo install`), container-escape primitives (`docker --privileged`, `--pid=host`, `--network=host`), reverse shells (`/dev/tcp`, `mkfifo + nc`, Python/Perl socket shells), credential-store reads (Keychain, GNOME Keyring, `/etc/shadow`), clipboard exfil (`pbcopy`, `xclip`, `xsel`, `wl-copy`), and log tampering (`journalctl --vacuum`, `history -c`). MCP-server rules cover high-risk server types (shell, root-filesystem, database, browser-automation), supply-chain risk (`npx -y` typosquat surface), hardcoded secrets in env config, remote SSE/HTTP transports, shell metacharacters in args, sensitive-file args (`.env`, `.pem`, `credentials.json`), `0.0.0.0` binding, missing timeouts, and `autoApprove` settings. Agent rules cover unrestricted Bash, missing `allowedTools` scoping, prompt-injection surfaces, auto-run instructions in `CLAUDE.md` ("Always run", "without asking", "automatically install"), hidden instructions (zero-width Unicode, HTML comments, base64), URL-execution directives, time-bombs, data-harvesting, prompt reflection (`ignore previous instructions`, `you are now`, DAN, fake system prompts), and output-manipulation directives (`always report ok`, `remove warnings from output`).
- **Provenance-aware false-positive control is the load-bearing design choice.** The same rule emits findings at different score weights depending on `runtimeConfidence` of the source file. `active-runtime` covers `mcp.json`, `.claude/mcp.json`, `.claude.json`, and active `settings.json`. `project-local-optional` covers `settings.local.json`. `template-example` covers `mcp-configs/`, `config/mcp/`, `configs/mcp/`. `docs-example` covers `docs/`, `commands/`, `examples/`, `samples/`, `demo/`, `tutorial/`, `guide/`, `cookbook/`, `playground/`. `plugin-manifest` covers declarative `hooks/hooks.json` indirection. `hook-code` is the *manifest-resolved non-shell hook implementation* — e.g., `scripts/hooks/session-start.js`. The reading rule the README articulates: *template-example means "this repo ships a risky template", not "this is enabled right now."*
- **Three-agent Opus 4.6 adversarial pipeline** (Attacker → Defender → Auditor) chains independent perspectives into a prioritized action list. The README's worked example: the Attacker finds that `curl` hooks with `${file}` interpolation + `Bash(*)` = command-injection pivot; the Defender notes the absence of `PreToolUse` hooks to stop it; the Auditor sequences them.
- **Cross-file hook-manifest awareness** — settings-only `hooks-no-pretooluse` is now suppressed when a companion `hooks/hooks.json` manifest declares `PreToolUse` hooks. Manifest-referenced hook implementations are discovered through indirection; shell targets continue through hook rules, and non-shell `hook-code` targets emit *targeted* findings for explicit `output(...)` context injection, transcript-input access, and remote shell payloads executed via child-process wrappers.
- **MiniClaw** — a minimal sandboxed agent runtime bundled in the same npm package (`ecc-agentshield/miniclaw`). Single HTTP endpoint, isolated per-session filesystem, four independently enforced layers (rate limit + CORS + size cap → prompt sanitization stripping 12+ injection-pattern categories → tool whitelist with Safe / Guarded / Restricted tiers → sandbox FS with traversal block, symlink-escape detection, extension whitelist, 10 MB cap, 5-min timeout, no network by default). Zero external runtime dependencies — Node built-ins only. This is the product's *what-a-locked-down-agent-harness-looks-like* reference, intentionally narrower than community-plugin platforms.
- **Distribution channels and commercial posture.** Standalone CLI (`npm install -g ecc-agentshield` or `npx ecc-agentshield scan`), GitHub Action (`uses: affaan-m/agentshield@v1` with SARIF upload, baseline-drift gate, and organization-policy gate), ECC plugin into the *Everything Claude Code* ecosystem, ECC Tools GitHub App, and a paid *ECC Tools Pro* tier (Stripe, \$19 / seat / month for org-wide automated repo analysis). 102 rules, 638 stars, 135 forks, MIT license at the time of fetch.
- **Operator-grade artifacts.** Evidence packs are deterministic, redacted by default, and verifiable via `agentshield evidence-pack verify`. Remediation plans omit raw evidence and fix-before/after values so they can be attached to CI tickets without copying token-shaped strings. Organization policy presets (`oss` / `team` / `enterprise` / `regulated` / `high-risk-hooks-mcp` / `ci-enforcement`) include time-bound exception lifecycles — the exception audit surfaces total / active / expiring-within-7-days / expired exceptions with owner, ticket, scope, days-until-expiry, so temporary waivers do not become silent permanent bypasses.
- **Threat-landscape framing in the README.** Three load-bearing data points: (a) *12% of a major agent skill marketplace was malicious — 341 of 2,857 community skills*; (b) *a CVSS 8.8 CVE exposed 17,500+ internet-facing instances to one-click RCE*; (c) *the Moltbook breach compromised 1.5M API tokens across 770,000 agents.* These are the README's argument for why an automated audit step is necessary; they have not been independently corroborated in this wiki and are flagged below as a `[!gap]`.

## Strengths and Weaknesses

**Strengths.** First open-source scanner the wiki has cataloged whose unit of analysis is the *Claude Code config tree itself* — settings, hooks, MCP server manifests, subagents, slash commands, skill `.md` files, `CLAUDE.md`. Provenance-aware `runtimeConfidence` is a thoughtful answer to the dominant false-positive class in agentic-config scanners (template catalogs and docs examples are not active runtime). The three-output split — JSON, SARIF, evidence pack — slots cleanly into CI / code-scanning / audit workflows. The corpus-gate-with-prioritized-improvement-plan keeps the rule set honest. MiniClaw's *four-layer security model + zero-external-runtime-deps* is a small, copyable reference architecture for hardened single-agent harnesses.

**Weaknesses and open scope.**

- **Coverage is harness-specific.** Rules are tuned for Claude Code's `.claude/` shape. Other harnesses (OpenCode, Codex, Gemini, dmux, terminal-agent wrappers) are listed as *adapter evidence* but the rule corpus does not extend to them in a structured way yet.
- **Skill-md prompt text has narrower coverage than agent-md / CLAUDE.md.** The README admits this is a current gap — freeform `skill-md` text still bypasses most agent / injection rules.
- **Broader non-shell hook execution still needs language-aware analysis** beyond the narrow `hook-code` signals (context injection, transcript access, child-process execution).
- **No GA library API.** The npm package root export is the CLI entrypoint, not a semver-stable library module — consumers should treat JSON / SARIF report formats as the supported automation surface.
- **Threat-landscape claims in the README ("12%", "Moltbook", "17,500+ instances") have no inline citation** and have not been independently surfaced on the wiki.

## Relations

- **Supports** [[supply-chain-security-for-agents|Supply Chain Security for Agents]] because it operationalizes *skill marketplace controls*, *cognitive-file integrity*, and *MCP package provenance* as concrete static-analysis rules, with a built-in supply-chain mode that pulls npm-registry metadata for postinstall scripts, maintainers, deprecation, and package age.
- **Supports** [[ai-spm|AI Security Posture Management (AI-SPM)]] because the `.claude/` config tree is exactly the "AI asset inventory + misconfiguration detection" surface AI-SPM targets — models, prompts, indexes, connectors — restricted to a single harness instance.
- **Supports** [[mcp-security|MCP Security]] because 23 of its 102 rules are MCP-specific and the supply-chain mode is built around MCP package provenance.
- **Supports** [[prompt-injection-containment|Prompt Injection Containment]] because agent and `CLAUDE.md` rules cover the prompt-reflection / hidden-instruction / URL-execution / time-bomb classes, and `--injection` performs active probing.
- **Supports** [[agent-sandboxing|Agent Sandboxing]] in two distinct ways: (a) MiniClaw is a reference sandboxed runtime; (b) `--sandbox` executes hooks in a sandbox and observes behavior.
- **Aligns with** [[lethal-trifecta|Lethal Trifecta]] — the rule clusters around `Bash(*)` + `curl ${file}` + remote MCP transports are the structural-config form of the same untrusted-input / sensitive-action / external-communication composition.
- **Adjacent to** [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — natural evidence instrument for D3 (Control & Least-Agency via permission rules — wildcard allowlists, deny-list coverage, dangerous flags), D4 (Runtime & Guardrails via hook + agent + prompt-injection rules — `${file}` interpolation, hidden-Unicode instructions, time-bombs, prompt-reflection, and the bundled MiniClaw sandboxed runtime as worked reference), D5 (Egress & Network via MCP remote-transport rules — SSE/HTTP transports, `0.0.0.0` binding, shell-metacharacter args), D8 (Supply Chain & AI-BOM via MCP-package provenance and skill-marketplace controls — npm-vs-git, pinned-vs-unpinned, postinstall scripts, package age), and D9 (Operations & Human Factors via continuous-attestation primitives — baseline-drift gate, organization-policy presets, and time-bound policy exception lifecycle audit). The two headline instruments (corpus-gate and provenance-aware `runtimeConfidence`) are captured separately as [[control-efficacy-gate|Control-Efficacy Gate]] and [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] — both are candidate CMM revision-pass additions parked pending a peer non-Claude-Code instrument.

> [!gap] Threat-landscape claims unverified
> The README's *12% of a major agent skill marketplace was malicious (341 of 2,857)*, *CVSS 8.8 CVE exposed 17,500+ internet-facing instances to one-click RCE*, and *Moltbook breach compromised 1.5M API tokens across 770,000 agents* are stated without citation. Independent corroboration (original disclosure, CVE id, breach report) is the next ingest candidate — these are load-bearing motivating claims for the whole agent-config-audit category and should not stay anchored on a single vendor README.

**Provenance-aware static analysis is the right primitive.** AgentShield's `runtimeConfidence` discipline — labeling findings by source kind before downgrading severity — is the structural lesson independent of the rule set. Scanners that emit the same finding at the same weight regardless of whether the source file is `mcp.json`, `mcp-configs/templates/example.json`, `docs/guide/settings.json`, or a manifest-referenced hook implementation will be drowned in template-catalog noise. The reading rule generalizes: `template-example` means *"this repo ships this risky template"*, not *"this is enabled right now"* — and committed secrets stay critical regardless of source kind.

- [[openant-announcement|OpenAnt]] — a second sourced peer instrument for the same underlying discipline, arrived at from application-code scanning rather than harness-config audit. The convergence is what makes the discipline sourced rather than single-vendor.
