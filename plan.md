# Wave Program Issue Plan: Veil

## Planned Issues Strategy & Sprint Model
The Wave Program operates on sprint cycles where maintainers post clearly scoped issues for contributors. For **Veil**—a passkey-powered smart wallet on Stellar Soroban with contracts live on mainnet—every issue directly impacts production security, user experience, or developer adoption.

We strictly adhere to Wave criteria:
- **Real Impact:** No filler work; each issue addresses verified codebase needs.
- **Clear Context:** Complete problem statements and architectural rationale ("why").
- **Appropriate Scope:** Sized for completion within a single sprint (1–2 weeks).
- **Implementation Guidance:** Points to specific files, constraints, and edge cases.
- **Explicit Definition of Done:** Clear acceptance criteria, tests, and review expectations.

### Complexity & Points
- **Trivial (100 pts):** Docs fixes, copy updates, metadata corrections, minor UI bug fixes.
- **Medium (150 pts):** Standalone features, component refactors, API integrations, targeted tests.
- **High (200 pts):** Cryptographic verification, Soroban contract logic, core money-path test suites.

---

## Types of Work We Post

### 1. Bug Fixes & Edge Cases
- **Navigation & Mobile Flows:** Fixing route transitions, broken screens (e.g., mobile airtime/KYC flows), and unhandled errors in the Expo app and Next.js PWA.
- **Transaction & Simulation Failures:** Handling Soroban RPC edge cases, footprint expiration, simulation errors, and slippage protection on DEX trades.
- **WebAuthn Quirks:** Resolving authenticator discrepancies (e.g., devices lacking WebAuthn PRF, Safari DER-to-P1363 signature conversions).

### 2. New Features & Rails
- **Smart Wallet Contracts V2:** Implementing time-bound spend limits, modular policy checks, session keys, and batched invocations on Soroban.
- **dApp Browser & WalletConnect (SEP-0043):** Integrating passkey connection standards and in-app approval dialogs.
- **Invest & Fiat Rails:** Tokenized real-world assets (RWAs), yield displays, and SEP-24 / SEP-6 on-off ramp workflows.

### 3. Testing & Security Hardening ("The Money Paths")
Because Veil manages real funds on mainnet, testing critical paths is our top priority:
- **Zero-Coverage Modules:** Writing thorough tests for critical logic (`recovery.ts`, `vault.ts`, `walletConnect.ts`, `multisig.ts`, `blend.ts`).
- **Contract Fuzzing & Property Tests:** Invariant testing with `cargo-fuzz` and Soroban test harnesses to verify `__check_auth` signature validation.
- **Mutation Testing:** Running Stryker suites in `sdk/` to ensure tests defend against logic mutations.

### 4. Documentation & Developer Experience
- **SDK Multi-Framework Support:** Supporting dual-module outputs (ESM + CJS) and adapters for React, Vue, Svelte, Solid, and Angular.
- **Interactive Guides & Scaffolding:** Expanding `create-veil-app` templates and testnet sandboxes.
- **Error Catalog & Runbooks:** Documenting contract panics, WebAuthn rejections, and reproducible Docker build verifications.

---

## Contributor Lifecycle
Issues are tagged with `Stellar Wave` and complexity labels. Contributors coordinate via our Telegram group, maintainers commit to 24–48h PR review SLAs, and unresolved issues roll over cleanly into subsequent cycles.
