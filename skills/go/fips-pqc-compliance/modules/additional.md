# Phase: Additional Changes

PQC inventory, JWT/signature migration planning, and BoringCrypto migration.

## Steps

1. Read `references/pattern-map.md` — filter for rows with category `addition` or `source_section` relating to PQC or BoringCrypto

### Post-Quantum Cryptography inventory

2. **Inventory quantum-vulnerable algorithms** — search the codebase for:
   - `crypto/rsa` — RSA key generation, signing, encryption (quantum-vulnerable)
   - `crypto/ecdsa` — ECDSA signatures (quantum-vulnerable)
   - `crypto/ecdh` — ECDH key exchange (quantum-vulnerable)
   - `crypto/ed25519` — Ed25519 signatures (quantum-vulnerable)
   - `crypto/elliptic` — direct elliptic curve operations (quantum-vulnerable)
   - `crypto/dsa` — DSA signatures (quantum-vulnerable, also deprecated)
   - `math/big` with `DH` or `Diffie` — Diffie-Hellman key exchange (quantum-vulnerable)

3. **Document PQC exposure** — for each quantum-vulnerable usage found:
   - Note the file, function, and purpose (signing, key exchange, encryption)
   - Classify risk: key exchange (highest, migrate first) > signatures > symmetric (lowest)
   - Note: AES-128/256 and SHA-256/SHA-512 are quantum-resistant for their intended security levels (Grover's algorithm halves the effective key size)

### JWT and signature migration planning

4. **Inventory JWT/token usage** — search for:
   - JWT libraries (`github.com/golang-jwt/jwt`, `github.com/lestrrat-go/jwx`, etc.)
   - Signing method constants: `RS256`, `RS384`, `RS512`, `PS256`, `ES256`, `ES384`, `EdDSA`
   - `HS256`, `HS384`, `HS512` are HMAC-based and quantum-resistant
   - X.509 certificate generation/signing
   - SSH key generation

5. **Plan hybrid transition** — for key exchange:
   - Go 1.24+ supports `X25519MLKEM768` hybrid key exchange in TLS by default
   - Do not set `CurvePreferences` that would exclude ML-KEM curves
   - Do not set `GODEBUG=tlsmlkem=0` (disables PQC in TLS)

### BoringCrypto migration (if applicable)

6. **Migrate from BoringCrypto to Go 1.24+ native FIPS:**
   - Remove `GOEXPERIMENT=boringcrypto` from build scripts
   - Remove `golang-fips/go` fork dependency — switch to upstream Go 1.24+
   - Remove `GOFIPS=1` environment variable — replace with `GOFIPS140=latest`
   - Update CI/CD to use standard Go 1.24+ toolchain
   - Go 1.24+ native FIPS mode uses a NIST-certified module, replacing BoringCrypto

## Build gate

Run `go build ./...`. This phase is primarily planning — the build should pass. Document all quantum-vulnerable algorithm usages found for future PQC migration.
