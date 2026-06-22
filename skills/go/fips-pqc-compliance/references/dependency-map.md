# Dependency Map

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| go (< 1.24) | go (>= 1.24) | >= 1.24 | replace | Go 1.24+ required for native FIPS 140-3 mode, crypto/mlkem, crypto/hkdf, crypto/sha3, crypto/pbkdf2 | Go Version |
| golang.org/x/crypto/md4 | removed | | remove | MD4 is cryptographically broken (CWE-328). No replacement needed — eliminate usage entirely | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/ripemd160 | crypto/sha256 | | replace | RIPEMD-160 not FIPS-approved. Use SHA-256 or SHA-512 from stdlib | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/blowfish | crypto/aes | | replace | Blowfish not FIPS-approved. Use AES (128/192/256-bit) with GCM mode from crypto/cipher | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/twofish | crypto/aes | | replace | Twofish not FIPS-approved. Use AES from crypto/aes | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/cast5 | crypto/aes | | replace | CAST5 not FIPS-approved. Use AES from crypto/aes | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/tea | crypto/aes | | replace | TEA not FIPS-approved. Use AES from crypto/aes | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/xtea | crypto/aes | | replace | XTEA not FIPS-approved. Use AES from crypto/aes | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/chacha20 | crypto/aes | | replace | ChaCha20 not FIPS-approved. Use AES-GCM from crypto/cipher | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/salsa20 | crypto/aes | | replace | Salsa20 not FIPS-approved. Use AES-GCM from crypto/cipher | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/chacha20poly1305 | crypto/aes | | replace | XChaCha20-Poly1305 not FIPS-approved. Use AES-GCM from crypto/cipher | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/poly1305 | crypto/hmac | | replace | Poly1305 not FIPS-approved. Use HMAC-SHA-256 from crypto/hmac | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/curve25519 | crypto/ecdh | | replace | Curve25519 not NIST-approved. Use ECDH with P-256/P-384 from crypto/ecdh | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/ed25519 | crypto/ed25519 | | rename | Moved to stdlib. Note: Ed25519 is not FIPS-approved — consider ECDSA P-256/P-384 for compliance | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/bn256 | crypto/ecdsa | | replace | BN256 pairing curve not FIPS-approved. Use NIST curves (P-256/P-384/P-521) from crypto/ecdsa | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/blake2b | crypto/sha256 | | replace | BLAKE2b not FIPS-approved. Use SHA-256 or SHA-512 from stdlib | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/blake2s | crypto/sha256 | | replace | BLAKE2s not FIPS-approved. Use SHA-256 from stdlib | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/hkdf | crypto/hkdf | >= 1.24 | rename | Moved to stdlib in Go 1.24+. HKDF with SHA-256/SHA-512 is FIPS-approved (SP 800-56C) | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/sha3 | crypto/sha3 | >= 1.24 | rename | Moved to stdlib in Go 1.24+. SHA-3 is FIPS-approved (FIPS 202) | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/argon2 | crypto/pbkdf2 | >= 1.24 | replace | Argon2 not FIPS-approved. Use PBKDF2 (crypto/pbkdf2 in Go 1.24+) or bcrypt from x/crypto/bcrypt | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/scrypt | crypto/pbkdf2 | >= 1.24 | replace | scrypt not FIPS-approved. Use PBKDF2 from crypto/pbkdf2 (Go 1.24+) | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/xts | crypto/aes | | replace | XTS not FIPS-approved for general use. Use AES-GCM from crypto/cipher | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/openpgp | removed | | remove | Deprecated upstream. No FIPS replacement — use stdlib crypto packages directly | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/otr | removed | | remove | OTR protocol. No FIPS replacement — use TLS 1.2+ for transport security | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/nacl/box | crypto/ecdh + crypto/aes | | replace | NaCl Box (Curve25519+XSalsa20+Poly1305) — none FIPS-approved. Use ECDH P-256 + AES-GCM | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/nacl/secretbox | crypto/aes | | replace | NaCl Secretbox (XSalsa20+Poly1305) — not FIPS-approved. Use AES-GCM from crypto/cipher | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/nacl/sign | crypto/ecdsa | | replace | NaCl Sign (Ed25519) — not FIPS-approved. Use ECDSA P-256/P-384 from crypto/ecdsa | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/nacl/auth | crypto/hmac | | replace | NaCl Auth (HMAC-SHA-512-256) — non-standard. Use HMAC-SHA-256 from crypto/hmac | x/crypto Non-FIPS Packages |
| github.com/golang-fips/go | removed | | remove | BoringCrypto fork superseded by Go 1.24+ native FIPS mode. Use upstream Go toolchain | BoringCrypto Migration |
