# Phase: Cleanup and Verification

Verify no non-FIPS imports remain, run compliance scan, and report findings.

## Steps

1. **Verify no non-FIPS x/crypto imports remain:**
   - Search all `.go` files for imports of packages listed in `references/dependency-map.md` with action `replace` or `remove`
   - Any remaining references are migration gaps — go back and fix them
   - Acceptable x/crypto packages that can remain: `ssh`, `acme`, `ocsp`, `pkcs12`, `x509roots`, `cryptobyte` (protocol/format packages, not raw crypto)

2. **Verify no weak algorithms remain:**
   - Search for `des.NewCipher`, `des.NewTripleDESCipher`, `rc4.NewCipher`
   - Search for `md5.New()`, `md5.Sum()`
   - Search for `sha1.New()`, `sha1.Sum()`
   - Search for `math/rand` in cryptographic contexts
   - Any findings indicate missed remediations

3. **Verify TLS hardening:**
   - Search for `InsecureSkipVerify:\s*true` — should only appear in test files with justification
   - Search for `MinVersion:.*tls.VersionTLS10` or `tls.VersionTLS11` — must be TLS 1.2+
   - Search for `MaxVersion:.*tls.VersionTLS12` — blocks TLS 1.3 and PQC

4. **Verify FIPS mode compatibility:**
   - Run `GOFIPS140=latest go test ./...`
   - Any panics indicate remaining non-FIPS algorithm usage

5. **Run go mod tidy** to clean up unused dependencies

6. **Report to user:**
   - List all changes made across all phases
   - Summarize FIPS compliance status (all non-FIPS algorithms replaced, TLS hardened)
   - List all quantum-vulnerable algorithm usages found (PQC inventory from additional phase)
   - Flag any items that could not be automatically migrated (require manual review)
   - Note BoringCrypto migration status if applicable
   - Recommend next steps for PQC transition timeline
