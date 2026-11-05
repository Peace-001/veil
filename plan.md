# Wave Program Issue Plan: Veil

## Overview & Program Strategy

The Wave Program operates on structured sprint cycles where maintainers post well-scoped, high-impact issues that external contributors claim, implement, and submit for review. For **Veil** — a passkey-powered smart wallet built on Stellar Soroban with live contracts on mainnet — our issue planning is designed to balance protocol security, client robustness, and developer adoption without compromising our high code quality standards.

Because Veil touches real user funds and cryptographic authentication on-chain, our Wave sprint issues avoid filler tasks. Every posted issue targets verified codebase improvements, test coverage gaps, edge-case bug fixes, or ecosystem extensions.

---

## Issue Scoping Criteria

In alignment with the [Drips Wave Issue Criteria](criteria.md), all issues posted across sprint cycles adhere to five strict rules:

1. **Real Impact**: Every issue addresses a tangible need — fixing a broken user path, strengthening smart contract security, increasing test coverage on zero-coverage money paths, or eliminating technical debt.
2. **Clear Context**: Issues provide the exact problem statement, user story, and architectural rationale ("why" this matters), eliminating ambiguity.
3. **Appropriate Scope**: Tasks are sized to be completed and reviewed within a single 1- to 2-week sprint cycle. Large architectural goals are broken into discrete, atomic issues.
4. **Implementation Guidelines**: Maintainers identify relevant source files, lines of interest, potential side-effects, and design constraints without micromanaging the solution.
5. **Explicit Definition of Done**: Acceptance criteria clearly specify required tests, documentation updates, typing constraints, and PR expectations.

---

## Complexity & Point Allocation Model

| Level | Points | Difficulty Tag | Typical Work Profile |
|---|---|---|---|
| **Trivial** | `100 pts` | `difficulty:easy` | Typos, minor copy changes, missing npm package metadata, small UI styling bugs, documentation clarifications. |
| **Medium** | `150 pts` | `difficulty:intermediate` | Feature enhancements, isolated bug fixes, component refactors, unit/integration test suites for specific modules, API endpoint integrations. |
| **High** | `200 pts` | `difficulty:advanced` | Complex cryptographic integrations, Soroban smart contract logic, multi-signer protocols, WebAuthn verification edge cases, full test coverage for core financial flows. |

---

## Types of Work Posted in Wave Sprints

### 1. Bug Fixes & Edge Case Handling
Bug fixes in Veil address issues discovered through audits, real-device QA, and telemetry across the web wallet, mobile app, and backend services.

- **Frontend & Navigation Flows**: Resolving broken route parameters, missing error boundaries, modal dismissal states, and unhandled loading spinners (e.g., resolving navigation targets in mobile fiat/airtime flows).
- **Transaction & Simulation Failures**: Handling Soroban RPC edge cases such as stale ledger sequences, expired resource footprints, footgun authorization entries, and Slippage tolerance errors during Soroswap aggregations.
- **Biometric & Authenticator Quirks**: Mitigating quirks in client authenticators (e.g., Android devices lacking the WebAuthn PRF extension, Safari DER-to-P1363 ECDSA signature formatting discrepancies).
- **Fee-Payer & Account Derivation**: Ensuring deterministic derivation of fee-payer keys (`G...`) from passkeys and preventing fee-payer account exhaustion or miscalculation under network congestion.

### 2. New Features & Ecosystem Expansions
Features are designed modularly so contributors can deliver complete, self-contained functional units within a sprint cycle.

- **Smart Wallet Contracts V2**: Modular policy enforcement, spend limits per time window, session key mechanisms, and batched transaction invocations on Soroban.
- **dApp Browser & WalletConnect (SEP-0043)**: Expanding the in-app dApp browser, adding support for the standard Stellar passkey wallet connector spec, and implementing approval dialogs for arbitrary contract calls.
- **Invest & Real-World Asset (RWA) Rails**: Integrating tokenized assets and regulated tokens on Stellar, displaying live asset yields, and handling trustlines seamlessly without exposing users to raw Stellar mechanics.
- **Fiat On/Off-Ramp Enhancements**: Implementing SEP-24 / SEP-6 interactive flows and regional payment rails (e.g., mobile money, bank transfers, Buy with Naira) with clean error handling and status tracking.
- **AI Agent Capabilities**: Expanding tool definitions and transaction execution safety boundaries for autonomous agent frameworks (Claude / DeepSeek) interacting with the Veil SDK.

### 3. Testing & Security Hardening ("The Money Paths")
Veil operates smart wallet contracts on Stellar mainnet. Security and test rigor are paramount.

- **Zero-Coverage Money Paths**: Writing rigorous unit and integration tests for critical business logic modules in `frontend/wallet` and `frontend/mobile` (such as `recovery.ts`, `vault.ts`, `walletConnect.ts`, `multisig.ts`, and `blend.ts`).
- **Contract Property & Fuzz Testing**: Writing property-based tests and invariant checks using `cargo-fuzz` and Soroban SDK test harnesses to ensure `__check_auth` can never be bypassed with forged WebAuthn signatures.
- **Cryptographic Assertion Hardening**: Validating clientDataJSON hashing, authenticatorData flag verification (`UP` user presence, `UV` user verification), and challenge binding under malicious inputs.
- **Mutation & Regression Testing**: Running Stryker mutation testing across `sdk/` packages to detect weak assertions, ensuring tests fail if funds or authorization paths are manipulated.

### 4. Developer Experience & Documentation
Ensuring external developers can easily integrate Veil into their decentralized applications.

- **SDK Packaging & Multi-Framework Support**: Supporting dual-module outputs (ESM + CommonJS), improving tree-shaking, maintaining accurate TypeScript declaration maps, and updating adapters for React, Vue, Svelte, Solid, and Angular.
- **Interactive Guides & Scaffolding**: Enhancing `create-veil-app` templates, building interactive Sandboxes on CodeSandbox / StackBlitz, and documenting end-to-end integration examples.
- **Verification & Deployment Runbooks**: Documenting reproducible Docker build verification steps, testnet-to-mainnet migration checklists, and contract hash verification tools.
- **Comprehensive Error Catalog**: Documenting human-readable explanations and troubleshooting steps for all Soroban host errors, contract panics, and WebAuthn rejection codes.

### 5. Infrastructure, CI/CD & Observability
Automating the health of the repository and keeping services operational.

- **Mainnet State Verification**: Automated CI workflows (such as `.github/workflows/deployed-hashes.yml`) that periodically compare on-chain bytecode against audited git commits.
- **RPC Failover & Load Balancing**: Resilient Soroban RPC connection pooling, rate-limit backoff strategies, and fallback node selection.
- **Dependency Health**: Auditing and upgrading dependencies across individual package lockfiles without breaking version alignments or introducing vulnerabilities.

---

## Standard Wave Issue Template

To ensure consistency and quality, all Wave issues follow this standardized markdown template:

```markdown
### V[ID] · [Concise, Imperative Title]

**Labels:** help wanted, Stellar Wave, area:[area], difficulty:[easy|intermediate|advanced], points:[100|150|200]

### Problem Statement
[A clear 1-2 paragraph description of the problem, the user impact, and why this work is necessary.]

### Affected Files & Context
- `path/to/file.ts#L42-L88` — [Description of current behavior]
- `path/to/contract.rs` — [Relevant structures or entry points]

### Implementation Guidelines
- [Key design constraint or edge case to handle]
- [Guidance on architecture, data structures, or security constraints]
- [What NOT to do: e.g., never weaken assertions, never elide a 56-character Stellar public key]

### Definition of Done
- [ ] Code changes implement the requested feature or fix cleanly.
- [ ] Automated unit or integration tests added with comments explaining what invariants they protect.
- [ ] Existing test suites pass (`npm test` or `cargo test`).
- [ ] No regression in lockfiles or unrelated workspace packages.
```

---

## Contributor Lifecycle & Review SLA

1. **Discovery & Claiming**: Contributors discover issues on the GitHub repository tagged with `Stellar Wave` and assign themselves or comment with their implementation approach.
2. **Community Support**: Contributors join the official contributor Telegram group for direct synchronization with maintainers on technical roadblocks.
3. **Review Turnaround**: Maintainers commit to reviewing Wave PRs within 24 to 48 hours during active sprint windows, providing constructive and detailed feedback.
4. **Verification & Merge**: Once CI passes, security invariants are verified, and reviews are approved, PRs are merged. Drips points are recorded and attributed toward the contributor's sprint allocation.
5. **Carryover Management**: Any issue uncompleted at the end of a sprint cycle is evaluated, refreshed with updated context, and carried over to the subsequent Wave cycle.
