# Phase: Testing Migration

Verify FIPS mode works and update tests that use non-FIPS algorithms.

## Steps

1. Read `references/api-map.md` — filter for rows where test files use the `old_api`
2. Read `references/dependency-map.md` — check for test-only imports of non-FIPS packages

3. **Update test cryptographic usage:**
   - Search test files (`*_test.go`) for non-FIPS algorithms identified in the code phase
   - Replace test fixtures that use DES, RC4, MD5, SHA-1 with FIPS-approved equivalents
   - Update test vectors if they depend on specific non-FIPS algorithm outputs
   - Replace `math/rand` in tests with `crypto/rand` if used for cryptographic test data

4. **Test with FIPS mode enabled:**
   - Run `GOFIPS140=latest go test ./...`
   - FIPS mode will panic if non-approved algorithms are used at runtime
   - Fix any panics — they indicate remaining non-FIPS algorithm usage

5. **Test TLS configuration:**
   - Verify TLS tests don't rely on `InsecureSkipVerify: true` for non-test servers
   - For test-internal TLS (test servers), `InsecureSkipVerify` is acceptable if documented
   - Ensure test TLS configs use TLS 1.2+ minimum

6. Run the full test suite

## Build gate

Run the test suite with FIPS mode:
- `GOFIPS140=latest go test ./...`

Test failures in FIPS mode indicate non-FIPS algorithm usage that was missed in earlier phases. Fix the underlying code, not the test.
