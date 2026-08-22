---
type: incident
title: "Gemini CLI Workspace-Trust RCE"
address: c-000290
created: 2026-08-16
updated: 2026-08-16
tags:
  - incidents
  - agentic-coding
  - ci-cd
  - toolchain-poisoning
  - prompt-injection
  - google
  - gemini-cli
status: published
scope_axis:
  - sec-of-ai
  - sec-against-ai
origin: aggregated
incident_class: toolchain-poisoning
attack_with_or_on_ai: "on AI"
date_observed: 2026-04-16
date_disclosed: 2026-04-24
target: "Gemini CLI in headless mode; google-github-actions/run-gemini-cli CI workflows"
threat_actor: "research-disclosure"
impact: "CVSS 10.0 remote code execution on the CI runner by an unprivileged external contributor; reachable secrets, credentials, and source in the workflow environment"
related:
  - "[[gemini-cli|Gemini CLI]]"
  - "[[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[guard-canonicalization-gap|Guard Canonicalization Gap]]"
  - "[[guardfall-shell-injection-audit|GuardFall Shell-Injection Audit]]"
  - "[[agents-rule-of-two|Agents Rule of Two]]"
  - "[[indirect-prompt-injection|Indirect Prompt Injection]]"
  - "[[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency|D3 Control & Least-Agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails|D4 Runtime Guardrails]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain|D8 Supply Chain & AI-BOM]]"
  - "[[google|Google]]"
  - "[[novee-security|Novee Security]]"
  - "[[pillar-security|Pillar Security]]"
sources:
  - https://github.com/advisories/GHSA-wpqr-6v78-jr5g
  - https://github.com/google-github-actions/run-gemini-cli/security/advisories/GHSA-wpqr-6v78-jr5g
  - https://novee.security/blog/google-gemini-cli-rce-vulnerability-cvss-10-critical-security-advisory/
  - https://www.pillar.security/blog/my-agentic-trust-issues-from-prompt-injection-to-supply-chain-compromise-on-gemini-cli
  - https://www.theregister.com/2026/04/30/googles_fix_for_critical_gemini/
---

# Gemini CLI Workspace-Trust RCE

## Summary

On 2026-04-24 Google published [GHSA-wpqr-6v78-jr5g](https://github.com/advisories/GHSA-wpqr-6v78-jr5g), a **CVSS 10.0** advisory against [[gemini-cli|Gemini CLI]] and the `run-gemini-cli` GitHub Action.[^ghsa] No CVE was assigned. The advisory bundles two independently reported defects that reach the same outcome — arbitrary command execution on a CI runner, triggerable by an unprivileged outsider — through opposite sides of the model.

**Folder trust in headless mode.** Non-interactive Gemini CLI automatically trusted the current workspace folder when loading configuration and environment variables, which the advisory states permits "remote code execution via malicious environment variables in the local `.gemini/` directory."[^ghsa] Anyone who could put content into the workspace — an external fork's pull request is the worked case — could ship a `.gemini/` tree that the harness read and acted on. Reported by Elad Meged of [[novee-security|Novee Security]].

**Tool allowlisting under `--yolo`.** Running with `--yolo` caused the harness to ignore the fine-grained tool allowlist in `settings.json` altogether, so a workflow that had enumerated the commands its agent was permitted to run would execute any command the model was talked into.[^pillar] Reported by Dan Lisichkin of [[pillar-security|Pillar Security]], who chained it into repository write access on a live Google project.

Affected: `@google/gemini-cli` before **0.39.1** (and preview `0.40.0-preview.2`), and `google-github-actions/run-gemini-cli` before **0.1.22**. Patched in 0.39.1, 0.40.0-preview.3, and action 0.1.22.[^ghsa] No in-the-wild exploitation has been reported.

## Attack Vector

### Configuration trust, decided before the model runs

The folder-trust defect never involves the agent's reasoning. The harness's configuration loader reads the workspace `.gemini/` tree during startup, and in headless mode it treated the presence of that tree as authorization to load it. Novee's write-up puts the ordering plainly: the result "was direct command execution on the host running the agent, before its sandbox ever initialized," and it is "infrastructure-level execution before the AI system ever had to reason."[^novee] The Register's account attributes the same sequencing to the researchers: attacker-controlled content "was silently accepted as trusted configuration and executed before any sandbox was initialized."[^register]

Every model-layer control — instruction hardening, an injection classifier, chain-of-thought auditing — is downstream of a step that has already run. So is the sandbox. The control that would have caught this is an ordering property of the harness, not a capability in any plane of the [[agentic-ai-security-reference-architecture|AAI-S RA]] as currently written.

### Allowlist suppression, decided after the model runs

The Pillar chain is model-mediated throughout and needs no privileged access to start. The target workflow triggered on `issues: opened` with no author validation, so "any GitHub user can activate it by opening an issue," and the agent ran under `--yolo`, which auto-approves every tool call.[^pillar] From there:

1. **Injection.** A public issue body carries the instructions. This is [[indirect-prompt-injection|indirect prompt injection]] with the lowest possible barrier to delivery — no fork, no pull request, no account standing.
2. **Allowlist suppression.** The workflow's `settings.json` permitted `echo` and `gh issue edit`. Under the pre-patch `--yolo` behavior that list was not consulted, so any command ran.[^pillar]
3. **Credential extraction.** The agent read the parent process environment through `/proc/$PPID/environ`, and read Git credentials from `.git/config`, which `actions/checkout` had written to disk under `persist-credentials: true`.[^pillar]
4. **Escalation.** The triage workflow's token carried `actions:write`. That was enough to dispatch a second workflow, `smoke-test.yml`, against an attacker-controlled fork branch, and that workflow ran with `contents: write`.[^pillar]

Step 3 is the same primitive as the [[claude-code-github-action-credential-exposure|Claude Code GitHub Action finding]], which reached `/proc/self/environ` past a Bash-only sandbox boundary. Two vendors, two harnesses, independent research teams, one file. A process environment holding credentials is readable by anything inside the process, and every agentic CI harness that delivers credentials as environment variables has this shape available to it.

Step 4 is the part a per-workflow token scope does not cover on its own. The compromised token was scoped to triage and still reached repository write, because `actions:write` is transitive: the authority to start a workflow is the authority of that workflow.

Both halves land in the CI-runner variant of [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] — the second public finding in that shape, and the first where the failure sat outside every plane of the [[agentic-ai-security-reference-architecture|RA]].

## Timeline

- **2026-04-16** — Lisichkin reports to Google's OSS Vulnerability Rewards Program with a proof of concept against `google/draco`.[^pillar]
- **2026-04-17** — Google disables the affected workflows across repositories.[^pillar]
- **2026-04-20** — Full supply-chain proof of concept against `gemini-cli` itself submitted.[^pillar]
- **2026-04-24** — GHSA-wpqr-6v78-jr5g published with patches 0.39.1, 0.40.0-preview.3, and action 0.1.22.[^ghsa]
- **2026-04-30** — Novee publishes its write-up; The Register reports the advisory and the breaking change.[^register]

Eight days from first report to published patch, with mitigation on the affected repositories the day after the report.

## The Fix and Its Cost

The patch requires explicit workspace trust rather than inferring it, aligning headless behavior with interactive behavior, and enforces the tool allowlist under `--yolo`.[^ghsa] Google's advisory adds command sanitization against shell substitution and redirect injection.[^pillar] Operators must now choose per workflow: set `GEMINI_TRUST_WORKSPACE: 'true'` where the input is trusted, or leave it unset and apply the hardening guidance where it is not.[^google-advisory]

The change is breaking by design, and Google says so: pipelines relying on the previous automatic trust "will fail to load workspace-specific settings."[^google-advisory] The Register's framing is that such workflows "may fail silently."[^register] A secure default that changes behavior for every existing consumer is the expected shape of this correction, and it is also the shape most likely to be reverted in the field by an operator restoring a broken pipeline — setting `GEMINI_TRUST_WORKSPACE: 'true'` on an untrusted-input workflow reinstates the vulnerable posture with a supported flag.

## Defensive Lessons

**A control that initializes after untrusted configuration is read is not in the path.** The sandbox in this incident was correctly implemented and irrelevant. Sandbox coverage is normally assessed as a question of scope — which tools are inside the boundary, per the Bash-only limit that the [[claude-code-github-action-credential-exposure|Claude Code case]] exploited. This adds a second question with the same weight: at what point in startup does the boundary exist. Both belong in a [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] assessment, and only the first is commonly asked.

**An allowlist a flag can suppress is not a policy decision point.** The `--yolo` bypass is adjacent to the [[guard-canonicalization-gap|guard canonicalization gap]] and is not an instance of it. In the canonicalization gap the guard runs and evaluates the wrong representation; here the guard did not run. The failure is cruder and the [[agentic-ai-security-cmm-d3-control-least-agency|D3]] consequence is the same: an organization that scored its control maturity on the contents of an allowlist scored an artifact that an autonomy flag removed from the path. Verify that the guard is consulted before grading what it contains.

**Repository-shipped harness configuration is executable content.** `.gemini/` in a pull request is the attack, which is [[harness-config-as-supply-chain-artifact|harness config as supply-chain artifact]] demonstrated rather than argued, and the clearest instance so far outside the `.claude/` tree. Fork-PR exclusion policies written against *instructions* — issue bodies, comment text, commit messages — do not cover it, because the loader does not classify `.gemini/` as instructions.

**Untrusted-input exclusion has to cover the trigger, not only the payload.** The Pillar chain started at `issues: opened` with no author check. Restricting fork-PR execution, the usual recommendation, leaves that trigger open to any account.

**Token scope is bounded by what the token can start.** `actions:write` on a triage token is repository write with extra steps. Scoping tokens per workflow, the first recommendation in the [[claude-code-github-action-credential-exposure|Microsoft analysis]], holds only if the permission set excludes workflow dispatch.

## Assessment

The advisory is a vendor disclosing a maximum-severity flaw in its own product, with patches shipped before publication and an eight-day report-to-fix window that stands up against the version history. Both researchers published independent technical write-ups; Pillar's carries a full chain with named target repositories and dates, which is the stronger of the two on evidence.

Three limits. The CVSS 10.0 rating carries `S:C` (scope change) and full confidentiality, integrity, and availability impact — defensible for a CI runner holding organizational credentials, and dependent on the runner's contents rather than on anything in the CLI. Novee's public write-up does not name the `.gemini/` file or environment variable that carries execution, so the folder-trust half is documented at the level of mechanism rather than of a reproducible payload. And no in-the-wild exploitation is reported, so this is disclosure evidence, not incident evidence, notwithstanding the score.

## Notes

[^ghsa]: [GitHub Advisory Database — GHSA-wpqr-6v78-jr5g](https://github.com/advisories/GHSA-wpqr-6v78-jr5g), published 2026-04-24. Carries the CVSS 10.0 score and vector `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H`, the affected and patched version ranges, CWE-20 / CWE-77 / CWE-78 / CWE-200, and the researcher credits.
[^google-advisory]: [google-github-actions/run-gemini-cli — Update to Gemini CLI and run-gemini-cli Trust Model](https://github.com/google-github-actions/run-gemini-cli/security/advisories/GHSA-wpqr-6v78-jr5g), 2026-04-24. Google's own write-up, carrying the `GEMINI_TRUST_WORKSPACE` migration and the breaking-change statement.
[^novee]: [Novee Security — Google Gemini CLI CVSS 10.0 RCE Vulnerability](https://novee.security/blog/google-gemini-cli-rce-vulnerability-cvss-10-critical-security-advisory/), 2026-04-30, Elad Meged. Source for the pre-sandbox execution ordering on the folder-trust half.
[^pillar]: [Pillar Security — My Agentic Trust Issues: From Prompt Injection to Supply-Chain Compromise on gemini-cli](https://www.pillar.security/blog/my-agentic-trust-issues-from-prompt-injection-to-supply-chain-compromise-on-gemini-cli), Dan Lisichkin. Source for the `--yolo` allowlist bypass, the four-step chain, and the disclosure dates.
[^register]: [The Register — Google fixes CVSS 10.0 vulnerability in Gemini CLI](https://www.theregister.com/2026/04/30/googles_fix_for_critical_gemini/), 2026-04-30. Independent reporting; source for the breaking-change criticism and the researchers' sandbox-ordering statement.
