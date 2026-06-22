# Sources

## NIST Publications

- [FIPS 140-3: Security Requirements for Cryptographic Modules](https://csrc.nist.gov/publications/detail/fips/140/3/final)
  - Defines cryptographic module security requirements
  - Specifies approved algorithms and modes of operation

- [SP 800-131A Rev 2: Transitioning the Use of Cryptographic Algorithms and Key Lengths](https://csrc.nist.gov/publications/detail/sp/800-131a/rev-2/final)
  - Algorithm and key length transition guidance
  - Deprecation timeline for weak algorithms

- [SP 800-52 Rev 2: Guidelines for TLS Implementations](https://csrc.nist.gov/publications/detail/sp/800-52/rev-2/final)
  - TLS version and cipher suite requirements
  - Server and client configuration guidance

- [SP 800-56A Rev 3: Recommendation for Pair-Wise Key-Establishment Schemes Using Discrete Logarithm Cryptography](https://csrc.nist.gov/publications/detail/sp/800-56a/rev-3/final)
  - Approved key exchange mechanisms
  - ECDH parameter requirements

- [SP 800-56C Rev 2: Recommendation for Key-Derivation Methods in Key-Establishment Schemes](https://csrc.nist.gov/publications/detail/sp/800-56c/rev-2/final)
  - HKDF and other key derivation guidance

- [SP 800-57 Part 1 Rev 5: Recommendation for Key Management](https://csrc.nist.gov/publications/detail/sp/800-57-part-1/rev-5/final)
  - Key length requirements (RSA >= 2048, AES >= 128)
  - Algorithm lifecycle and transition planning

- [SP 800-186: Recommendations for Discrete Logarithm-based Cryptography — Elliptic Curve Domain Parameters](https://csrc.nist.gov/publications/detail/sp/800-186/final)
  - Approved NIST curves (P-256, P-384, P-521)
  - Deprecation of P-224

- [FIPS 202: SHA-3 Standard](https://csrc.nist.gov/publications/detail/fips/202/final)
  - SHA-3 hash function family specification

## Post-Quantum Cryptography

- [NIST Post-Quantum Cryptography Project](https://csrc.nist.gov/projects/post-quantum-cryptography)
  - Standardization timeline and selected algorithms

- [FIPS 203: Module-Lattice-Based Key-Encapsulation Mechanism Standard (ML-KEM)](https://csrc.nist.gov/pubs/fips/203/final)
  - ML-KEM (formerly Kyber) specification
  - Supported in Go 1.24+ via crypto/mlkem

- [FIPS 204: Module-Lattice-Based Digital Signature Standard (ML-DSA)](https://csrc.nist.gov/pubs/fips/204/final)
  - ML-DSA (formerly Dilithium) specification

- [FIPS 205: Stateless Hash-Based Digital Signature Standard (SLH-DSA)](https://csrc.nist.gov/pubs/fips/205/final)
  - SLH-DSA (formerly SPHINCS+) specification

## Go Documentation

- [Go FIPS 140 Documentation](https://go.dev/doc/security/fips140)
  - Native FIPS 140-3 mode in Go 1.24+
  - GOFIPS140 environment variable usage
  - Certified cryptographic module details

- [Go 1.24 Release Notes](https://go.dev/doc/go1.24)
  - crypto/mlkem, crypto/hkdf, crypto/sha3, crypto/pbkdf2 additions
  - FIPS 140-3 mode support

- [Go FIPS 140 Blog Post](https://go.dev/blog/fips140)
  - Design rationale and implementation details

- [crypto package — Go standard library](https://pkg.go.dev/crypto)
  - FIPS-approved crypto primitives

- [crypto/mlkem package — Go standard library](https://pkg.go.dev/crypto/mlkem)
  - ML-KEM-768 and ML-KEM-1024 key encapsulation

- [golang.org/x/crypto](https://pkg.go.dev/golang.org/x/crypto)
  - Extended crypto packages (some FIPS-approved, some not)

## Third-Party PQC Libraries

- [Cloudflare CIRCL](https://github.com/cloudflare/circl)
  - Post-quantum algorithms (Kyber, Dilithium, SPHINCS+)
  - Hybrid key exchange implementations

- [golang-fips/go](https://github.com/golang-fips/go)
  - BoringCrypto-based FIPS fork (superseded by Go 1.24+ native FIPS)

## Security RFCs

- [RFC 7465: Prohibiting RC4 Cipher Suites](https://datatracker.ietf.org/doc/html/rfc7465)
- [RFC 7568: Deprecating Secure Sockets Layer Version 3.0](https://datatracker.ietf.org/doc/html/rfc7568)
- [RFC 8446: The Transport Layer Security (TLS) Protocol Version 1.3](https://datatracker.ietf.org/doc/html/rfc8446)
- [RFC 8996: Deprecating TLS 1.0 and TLS 1.1](https://datatracker.ietf.org/doc/html/rfc8996)

## Security References

- [Sweet32: Birthday attacks on 64-bit block ciphers in TLS](https://sweet32.info/)
  - Why DES/3DES/Blowfish (64-bit block) must be removed

- [Lucky13: TLS CBC padding oracle attack](https://www.isg.rhul.ac.uk/tls/Lucky13.html)
  - Why CBC mode without authentication is risky

- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
  - Password hashing algorithm recommendations

## Tools

- [Konveyor](https://www.konveyor.io/)
  - Application modernization and migration analysis

- [Kantra](https://github.com/konveyor/kantra)
  - CLI tool for running Konveyor rules
