---
name: fips-pqc-compliance
description: Migrates Go applications to FIPS 140-3 compliant cryptography and
  prepares for Post-Quantum Cryptography (PQC). Use when hardening a Go project's
  cryptographic posture for federal compliance or quantum readiness.
license: Apache-2.0
metadata:
  source: non-compliant-crypto
  target: fips-140-3-compliant-pqc-ready
  language: go
  build_tool: "go: go build ./..."
  guide_url: https://github.com/savitharaghunathan/fips-pqc-rules
  generated_by: migration-skills-generator
  generated_at: "2026-06-22T00:00:00Z"
---

# FIPS 140-3 & Post-Quantum Cryptography Compliance for Go

Requires Go 1.24+ for native FIPS 140-3 mode and ML-KEM support. This skill remediates non-FIPS cryptographic usage, replaces weak algorithms, hardens TLS configuration, and inventories quantum-vulnerable algorithms for PQC migration planning.

Based on 112 Konveyor/kantra rules from [savitharaghunathan/fips-pqc-rules](https://github.com/savitharaghunathan/fips-pqc-rules).

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build system** — Upgrade to Go 1.24+, replace non-FIPS x/crypto dependencies with stdlib equivalents
2. **Code** — Replace non-FIPS APIs, fix weak algorithms, remediate operational security issues
3. **Config** — Harden TLS configuration, set FIPS environment variables, fix insecure transport settings
4. **Testing** — Verify FIPS mode works, update tests that use non-FIPS algorithms
5. **Additional** — PQC inventory, JWT/signature migration planning, BoringCrypto migration
6. **Cleanup** — Verify no non-FIPS imports remain, run compliance scan

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Run `go build ./...`
2. If it fails, fix the issue before proceeding
3. If you cannot fix it, stop and report to the user
4. After all phases, run with `GOFIPS140=latest go test ./...` to verify FIPS mode
