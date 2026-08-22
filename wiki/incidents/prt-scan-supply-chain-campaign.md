---
type: incident
title: "prt-scan CI/CD Supply-Chain Campaign"
address: c-000118
created: 2026-05-25
updated: 2026-05-25
tags:
  - incidents
  - supply-chain
  - ci-cd
  - github-actions
  - sec-against-ai
status: developing
incident_class: supply-chain
attack_with_or_on_ai: "with AI"
date_observed: 2026-03-11
date_disclosed: 2026-04-02
target: "Public GitHub repositories with pull_request_target CI/CD workflows"
threat_actor: "unknown (single actor, six accounts)"
impact: "500+ malicious pull requests over ~3 weeks; <10% success rate; verified theft of AWS keys, Cloudflare API tokens, and Netlify auth tokens; two npm packages compromised across 106 versions"
scope_axis: [sec-against-ai]
related:
  - "[[sdlc-in-the-ai-attacker-era]]"
  - "[[ai-era-supply-chain-hardening]]"
  - "[[slopsquatting]]"
sources:
  - "https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign"
---

# prt-scan CI/CD Supply-Chain Campaign

## Summary

Between March 11 and April 3, 2026, a single threat actor operating six GitHub accounts ran an automated campaign against public repositories that use the `pull_request_target` workflow trigger. Across the six waves the actor opened well over 500 malicious pull requests.[^pr-count] Wiz Research published its analysis on April 2, 2026, naming the campaign after the attacker's branch-naming convention, `prt-scan-{12-hex}`.[^name] The attack used no zero-day. It exploited the documented behavior of `pull_request_target`, which runs in the context of the base repository and can reach repository secrets even when the pull request originates from a fork.[^trigger] Payloads evolved from hand-written shell scripts into AI-generated, language-aware injections that fingerprint a target's language, framework, test runner, and CI configuration before adapting the payload.[^ai-payload] The observed success rate was under 10% across more than 450 analyzed exploit attempts.[^success-rate] Verified credential theft affected AWS keys, Cloudflare API tokens, and Netlify auth tokens, and the actor compromised at least two npm packages sharing a maintainer across 106 versions.[^exfil][^npm]

## Attack Vector

The campaign targeted repositories whose CI/CD workflows use the `pull_request_target` trigger. Unlike `pull_request`, this trigger executes in the context of the base repository and grants access to repository secrets even when the pull request comes from a fork.[^trigger] The attack exploited the default `GITHUB_TOKEN` permissions in such workflows. It required no software vulnerability or zero-day.[^trigger]

The attack chain ran end to end without manual steps per target:

1. Discover candidate repositories by searching GitHub for `pull_request_target` workflows.
2. Fork the repository and create a branch matching `prt-scan-{12-hex}`.
3. Inject a payload into CI-relevant files (`conftest.py`, `package.json`, Makefile, `build.rs`).
4. Open a pull request titled "ci: update build configuration" with body "Automated build configuration update".
5. Trigger workflow execution and attempt credential exfiltration.

Wiz attributes the language-aware payload generation to AI tooling. The payloads identified the target's language and framework and emitted matching wrappers: Go test files for Go repositories, npm scripts for JavaScript projects, `conftest.py` for Python. They added obfuscation absent from the earlier shell-script variants.[^ai-payload] During the peak wave the actor sustained roughly seven pull requests per hour over 22-plus hours, opening over 475 in a 26-hour window.[^velocity] The report characterizes the operation as automation without comprehension: the actor misread GitHub's permission model, leaving payload phases (label bypass, workflow dispatch) that would rarely succeed.[^automation]

### Indicators of Compromise

- GitHub accounts: `testedbefore`, `beforetested-boop`, `420tb`, `69tf420`, `elzotebo`, `ezmtebo` (Proton Mail addresses derived from two base handles).[^accounts]
- Branch pattern: `prt-scan-` followed by 12 hex characters.[^name]
- Pull request title "ci: update build configuration"; body "Automated build configuration update"; User-Agent `python-requests/2.32.5`.[^pr-signature]
- Payload markers: `==PRT_EXFIL_START_[nonce]==`, `==PRT_RECON_START_[nonce]==`, `==PRT_HARVEST_START_[nonce]==`, and the `PRT_GIT_AUTH=` and `PRT_LABEL_BYPASS_[nonce]=` tokens.[^markers]

## Timeline

- 2026-03-11 — first wave begins; account `testedbefore` opens the initial malicious pull requests.[^accounts]
- 2026-03-13 to 2026-03-29 — additional accounts (`beforetested-boop`, `420tb`, `69tf420`) run further waves.[^accounts]
- 2026-04-02 — Wiz Research publishes its analysis; accounts `elzotebo` and `ezmtebo` run the peak wave.[^name][^accounts]
- 2026-04-02 to 2026-04-03 — peak 26-hour window, over 475 pull requests opened.[^velocity]
- Campaign span: roughly three weeks from first observation to public disclosure, with the final wave continuing through April 3.[^duration]

## Defensive Lessons

The campaign confirms that automated, AI-assisted tooling can fork, analyze, inject, and submit at machine speed against any public repository with a misconfigured CI/CD trigger. The controls are the same ones the [[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]] practice specifies for CI/CD pipelines, and the repositories that blocked the attack did so with exactly these gates.

- Set `pull_request_target` workflows to deny external fork contributors access to organization secrets, and gate first-time contributors behind manual approval. Repositories using first-time-contributor approval, actor-restricted workflows, and path-based trigger conditions blocked the attack.[^blocked]
- Set `GITHUB_TOKEN` permissions to read-only by default at the organization level, so a triggered workflow cannot write or reach secrets it does not need.
- Replace static cloud credentials in CI with short-lived OpenID Connect (OIDC) tokens bound to verified workflows. The verified theft here was of long-lived AWS keys, Cloudflare API tokens, and Netlify auth tokens — credential classes that OIDC trusted publishing removes from the pipeline entirely.[^exfil]
- Pin third-party actions to commit SHAs and enforce branch protection with CODEOWNERS review, per [[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]] control IM-1.

The npm component links to dependency-confusion risk: the actor pushed tainted versions of packages a downstream consumer might install. This is the same install-time trust gap that [[slopsquatting|Slopsquatting]] exploits through hallucinated package names, and the same lockfile and provenance controls apply. The broader threat-model shift (reconnaissance asymmetry and exploit-velocity collapse against a human-paced SDLC) is the subject of [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]].

## Sources

See frontmatter `sources:`.

## Notes

[^name]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. Campaign named for the `prt-scan-{12-hex}` branch convention; analysis published April 2, 2026.
[^pr-count]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. "Across all six waves, the attacker opened well over 500 malicious PRs."
[^trigger]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. `pull_request_target` runs in the base-repository context and reaches secrets even for fork PRs; the attack used default `GITHUB_TOKEN` permissions and no zero-day.
[^ai-payload]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. Payloads "evolved from crude bash scripts to AI-generated, language-aware payloads" that dynamically identify language, framework, test runner, and CI configuration.
[^success-rate]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. "Across over 450 analyzed exploit attempts, we have observed a <10% success rate."
[^exfil]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. "Verified credential theft was observed impacting AWS keys, Cloudflare API tokens, and Netlify auth tokens."
[^npm]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. "Successfully compromised at least two npm packages with a shared maintainer, across 106 versions."
[^velocity]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. Peak wave: over 475 PRs in 26 hours, roughly seven PRs per hour sustained over 22-plus hours.
[^automation]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. "The attack shows automation, not understanding"; dead-code phases (label bypass, workflow dispatch) reflect a misread permission model.
[^accounts]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. Six accounts attributed to one actor, active in sequence from March 11; Proton Mail addresses derived from `testedbefore` and `elzotebo` base handles.
[^pr-signature]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. PR title "ci: update build configuration", body "Automated build configuration update", User-Agent `python-requests/2.32.5`.
[^markers]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. Payload markers `==PRT_EXFIL_*==`, `==PRT_RECON_*==`, `==PRT_HARVEST_*==`, `PRT_GIT_AUTH=`, `PRT_LABEL_BYPASS_[nonce]=`.
[^duration]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. Six waves "starting on March 11, 2026 - three weeks before public disclosure"; final wave ran through April 3.
[^blocked]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. High-value targets blocked the attack via first-time-contributor approval gates, actor-restricted workflows, and path-based trigger conditions.
