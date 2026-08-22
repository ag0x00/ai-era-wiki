---
type: paper
title: "Non-Human Identities"
created: 2026-04-30
updated: 2026-06-23
tags:
  - papers
  - non-human-identity
  - nhi
  - identity
  - vendor-blog
  - oasis-security
status: summarized
scope_axis:
  - sec-of-ai
year: 2026
authors: ["Oasis Security"]
venue: "Oasis Security blog"
source_url: "https://www.oasis.security/blog/what-are-non-human-identities"
archived_copy: ".raw/articles/oasis-what-are-non-human-identities-2026-04-30.md"
no_public_url: ""
key_claim: "NHIs are not human identities at scale — they are a different governance class. Legacy IAM (HR-driven joiner/mover/leaver) and PAM (credential storage without usage context) fail for NHIs because NHIs are decentralized, developer-created, lack ownership, scale 45–100×, and authenticate without MFA. Securing them requires six purpose-built strategies: least privilege by default, ownership and accountability, automated rotation, behavioral monitoring, dev-lifecycle integration, and Zero Trust alignment."
methodology: "Industry/vendor opinion piece grounded in Rubrik Zero Labs (45:1 NHI-to-human ratio), arxiv 2503.18255 (machine identity 50K→250K per enterprise 2021–2025, 400% growth), IBM 2025 Cost of a Data Breach Report ($4.44M global / $10.22M US average; explicit recommendation for NHI controls), and three real-world incidents (Microsoft SAS token / 38TB; CircleCI OAuth; Mercedes-Benz service accounts)."
contradicts: []
supports:
  - "[[non-human-identity]]"
  - "[[nhi-governance-for-agents]]"
  - "[[agent-identity-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[credential-proxy-pattern]]"
related:
  - "[[oasis-security]]"
  - "[[non-human-identity]]"
  - "[[nhi-governance-for-agents]]"
  - "[[identity-credential-coupling]]"
  - "[[credential-proxy-pattern]]"
  - "[[shadow-automation]]"
sources:
  - "[[.raw/articles/oasis-what-are-non-human-identities-2026-04-30.md]]"
  - "https://www.oasis.security/blog/what-are-non-human-identities"
aliases:
  - papers/oasis-what-are-non-human-identities
  - oasis-what-are-non-human-identities

---

# What Are Non-Human Identities? (Oasis Security)

**Source:** [Oasis Security — What Are Non-Human Identities?](https://www.oasis.security/blog/what-are-non-human-identities) (2026). Local copy: `.raw/articles/oasis-what-are-non-human-identities-2026-04-30.md`.

## Key Claim

NHIs are a structurally different governance class from human identities. The mismatch is not just terminological: HR-driven IAM and credential-storage-focused PAM cannot govern decentralized, developer-created, ownerless identities that scale 45–100× faster than humans. **Securing NHIs requires purpose-built strategies, not retrofits.**

## Methodology

Vendor blog (Oasis Security positioning piece) grounded in:

- **Rubrik Zero Labs** — NHI-to-human ratio 45:1 enterprise average; up to 100:1 in some orgs.
- **arxiv 2503.18255** — machine identities per enterprise grew from 50K (2021) to 250K (2025); 400% increase.
- **IBM 2025 Cost of a Data Breach Report** — \$4.44M global / \$10.22M US average breach; report explicitly recommends NHI controls.
- Three named incidents: Microsoft AI Storage / SAS token (38TB exposure), CircleCI OAuth breach, Mercedes-Benz service-account misuse.

## Notable Findings

### 1. NHI taxonomy (eleven types)

| Type | Authentication mechanism |
|---|---|
| Service Account | Username/password; service-account secret |
| Service Principal | Client secret or certificate (cloud-native) |
| System Account | OS-issued, typically high privilege |
| IAM Role | AWS-style role with temporary session credentials |
| API Key | Static key for machine-to-machine |
| Machine Identity (VM / container / serverless) | Cloud-issued cert / SVID |
| Token (OAuth, JWT) | Time-limited bearer |
| Certificate (TLS, mTLS) | Asymmetric key |
| Storage Access Key | Long-lived, broad-permission |
| Database User | Application-level |
| Personal Access Token (PAT) | Developer-generated, often long-lived |
| SAS Token (Azure) | Time-limited, granular |

### 2. Identity-credential coupling (sharper than current wiki framing)

> "Special considerations arise in scenarios where identities are inseparable from the authentication string, as seen in Storage account access keys, Shared Access Signatures (SAS) tokens, and API keys for SaaS applications like Snowflake."

Where the credential **is** the identity, rotation is identity rotation. The [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]] cannot separate what is structurally inseparable; some workflows must rotate-as-identity-rotation. See new concept: [[identity-credential-coupling|Identity-Credential Coupling]].

### 3. Why human-identity controls fail for NHIs

Eight structural differences:

| Property | Human | NHI |
|---|---|---|
| Centralization | IT-managed, single source of truth | Decentralized, created by developers, citizen-developers, IT, infrastructure-as-code |
| Ownership | Tied to individual | Shared across teams / apps; often unowned |
| Scale | Linear with org headcount | 10–100× human count, growing exponentially |
| Rate of change | HR-driven (joiner/mover/leaver) | Code-pace (per commit / per deploy) |
| Provisioning | IT-mediated | Developer-driven, often invisible to IT |
| Secret expiration | Frequent password rotation | Often never rotated; sometimes no expiration |
| Operational risk of rotation | Low (user can re-authenticate) | High (rotation can break production workflows) |
| Authentication factor diversity | Three-factor (know / are / have) + MFA + SSO | Single-factor (the secret); no MFA equivalent |

**Implication**: when an attacker gets a service-account secret, there is no MFA challenge or SSO step to stop them.

### 4. Why legacy IAM and PAM fail

- **IAM**: built around HR-driven joiner/mover/leaver lifecycle events. NHIs don't have HR events — they have code commits and deploys. Ownership assignment, certification, and deprovisioning are extremely difficult.
- **PAM**: stores secrets in a vault but lacks usage context — what does the secret connect to, is it still required, what depends on it. Without that context, rotation is operationally risky and least-privilege is unenforceable.

### 5. Six-pillar securing-NHI strategy

| Pillar | Maps to [[agentic-ai-security-cmm-2026\|Agentic AI Security Capability Maturity Model]] |
|---|---|
| Enforce least privilege by default | **D2 L3+** (already covered) |
| Establish ownership and accountability | **D1 L3** (decision-rights matrix) + **D2 L3** (per-NHI human owner field) |
| Automate credential rotation | **D2 L4** (credential proxy + rotation cadence) |
| Monitor behavior continuously | **D7 L4** (per-credential behavioral baselines for NHIs) |
| **Integrate NHI governance into the development lifecycle** | **D8 L3 + D2 L3** (CI/CD gate; new: dev-lifecycle integration as level criterion) |
| Align NHI governance with Zero Trust principles | **D2 + D5** (already covered conceptually; sharpened to "Zero Trust without NHI visibility is incomplete") |

### 6. Real-world incidents

| Incident | Vector | NHI lesson |
|---|---|---|
| Microsoft AI Storage Breach | Misconfigured SAS token | 38TB internal data exposed (passwords + private keys); SAS tokens have identity-credential coupling — rotation requires identity rotation. |
| CircleCI Breach | OAuth token compromise | Mass-rotation across thousands of customer environments — rotation infrastructure must be tested. |
| Mercedes-Benz Breach | Service accounts with excessive privileges | Long-lived, over-permissioned NHIs are persistent attacker access. |

## Gap Analysis vs Existing Framework

### Confirmations already covered

| Oasis emphasis | Where it lives |
|---|---|
| NHI scale problem | [[non-human-identity\|Non-Human Identity (NHI)]]; [[agent-identity-architecture\|AI Agent Identity Architecture]] |
| Credential Zero / SPIFFE | [[non-human-identity\|Non-Human Identity (NHI)]]; [[nhi-governance-for-agents\|NHI Governance for AI Agents]] |
| Least privilege + scope governance | [[agentic-ai-security-cmm-2026\|Agentic AI Security Capability Maturity Model]] **D2** + **D3**; [[least-agency-principle\|Least Agency Principle]] |
| Action-to-identity tracing | [[nhi-governance-for-agents\|NHI Governance for AI Agents]] §5; [[agent-observability\|Agent Observability]] |
| Automated rotation | [[nhi-governance-for-agents\|NHI Governance for AI Agents]] §6 |
| Credential sprawl as attacker target | [[credential-proxy-pattern\|Credential Proxy Pattern for AI Agents]] |

### Gaps the Oasis post surfaces

**Five sharpenings worth applying.**

1. **Identity-credential coupling** is a load-bearing concept the wiki has not named. Where the credential IS the identity (SAS tokens, storage access keys, PATs, Snowflake API keys), the credential proxy pattern cannot help — these workflows must rotate-as-identity-rotation. New concept page: [[identity-credential-coupling|Identity-Credential Coupling]].
2. **HR-driven vs code-pace lifecycle mismatch** — NHIs don't have joiner/mover/leaver. The CMM **D2 L3** should explicitly require an NHI lifecycle that does not depend on HR events; **D2 L4** should require automated provisioning gates at code-deploy time (tied to CI/CD, not to onboarding).
3. **Dependency mapping before rotation** — Oasis: "Where rotation is operationally risky, invest in dependency mapping to understand what will break before making changes." This is concrete CMM **D9 L4** evidence: the org must maintain a per-credential consumer-graph (what depends on this credential) before automated rotation is enabled.
4. **NHI scale evidence** is currently single-sourced (CyberArk 82:1). Oasis adds Rubrik 45:1 and arxiv 250K-per-enterprise — both worth surfacing in [[non-human-identity|Non-Human Identity (NHI)]] for triangulation.
5. **Eleven NHI types vs current treatment** — wiki lists service accounts and SPIFFE-flavor workload IDs but does not enumerate Service Principals, IAM Roles, PATs, SAS tokens, Storage Access Keys, Database Users. Each has a distinct rotation / ownership / detection profile. Worth enumerating in [[non-human-identity|Non-Human Identity (NHI)]].

### Framework coverage beyond Oasis

- **CMM cumulative levels with floor rule** (CMMC import).
- **ID-tagged evidence** (`ASI03` for identity findings).
- **[[lethal-trifecta|Lethal Trifecta]]**, **CFI**, **runtime AI-BOM** — broader agentic primitives.
- **Standards crosswalk** ([[agentic-ai-security-cmm-crosswalk|Agentic AI Security CMM — Standards Crosswalk Matrix]]) — Oasis name-checks Zero Trust and IBM but doesn't crosswalk.
- **Six-plane reference architecture** — Oasis content fits cleanly into D2 + D7 + D8 + D9.

## Strengths and Weaknesses

**Strengths.** Sharpest published treatment of why HR-driven IAM fails for NHIs. Clear identity-credential-coupling articulation. Concrete incident anchors. Six-pillar strategy maps cleanly to existing CMM domains.

**Weaknesses.** Vendor blog: Oasis Security is positioned as the answer; technical implementation depth is thin. No discussion of agent-specific NHI concerns (e.g., NHIs created by AI agents themselves, MCP server identities, A2A cryptographic identity). No engagement with NIST CAISI Concept Paper (Feb 2026) on AI agent identity. No mention of platform-vs-prompt enforcement.

## Relations

- Supports: [[non-human-identity|Non-Human Identity (NHI)]] — sharpened with eleven-type taxonomy, identity-credential coupling, additional scale data, three incidents.
- Supports: [[nhi-governance-for-agents|NHI Governance for AI Agents]] — sharpened with HR-vs-code-pace lifecycle mismatch and dependency-mapping-before-rotation.
- Supports: [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — five sharpenings in the gap analysis above (see [[agentic-cmm-vs-standards-validation#§9 Oasis NHI ingest sharpenings (2026-04-30, post-Knostic)|§9 Oasis NHI ingest sharpenings]]).
- Introduces: [[identity-credential-coupling|Identity-Credential Coupling]] (new concept), [[oasis-security|Oasis Security]] (org).
