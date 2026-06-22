# API Map

| old_api | new_api | kind | action | before | after | notes | source_section |
|---|---|---|---|---|---|---|---|
| golang.org/x/crypto/md4 | removed | package | remove | `import "golang.org/x/crypto/md4"` | (remove — MD4 is broken) | MD4 is cryptographically broken. Eliminate all usage | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/ripemd160 | crypto/sha256 | package | replace | `import "golang.org/x/crypto/ripemd160"` | `import "crypto/sha256"` | RIPEMD-160 not FIPS-approved | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/blowfish | crypto/aes | package | replace | `import "golang.org/x/crypto/blowfish"` | `import "crypto/aes"` | Blowfish has 64-bit block size (Sweet32 vulnerable) | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/twofish | crypto/aes | package | replace | `import "golang.org/x/crypto/twofish"` | `import "crypto/aes"` | Twofish not FIPS-approved | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/cast5 | crypto/aes | package | replace | `import "golang.org/x/crypto/cast5"` | `import "crypto/aes"` | CAST5 not FIPS-approved | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/tea | crypto/aes | package | replace | `import "golang.org/x/crypto/tea"` | `import "crypto/aes"` | TEA not FIPS-approved | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/xtea | crypto/aes | package | replace | `import "golang.org/x/crypto/xtea"` | `import "crypto/aes"` | XTEA not FIPS-approved | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/chacha20 | crypto/aes + crypto/cipher | package | replace | `import "golang.org/x/crypto/chacha20"` | `import ("crypto/aes"; "crypto/cipher")` | ChaCha20 not FIPS-approved. Use AES-GCM | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/salsa20 | crypto/aes + crypto/cipher | package | replace | `import "golang.org/x/crypto/salsa20"` | `import ("crypto/aes"; "crypto/cipher")` | Salsa20 not FIPS-approved. Use AES-GCM | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/chacha20poly1305 | crypto/aes + crypto/cipher | package | replace | `import "golang.org/x/crypto/chacha20poly1305"` | `import ("crypto/aes"; "crypto/cipher")` | Not FIPS-approved. Use AES-GCM | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/poly1305 | crypto/hmac + crypto/sha256 | package | replace | `import "golang.org/x/crypto/poly1305"` | `import ("crypto/hmac"; "crypto/sha256")` | Poly1305 not FIPS-approved. Use HMAC-SHA-256 | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/curve25519 | crypto/ecdh | package | replace | `import "golang.org/x/crypto/curve25519"` | `import "crypto/ecdh"` | Curve25519 not NIST-approved. Use P-256/P-384 | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/ed25519 | crypto/ed25519 | package | move_package | `import "golang.org/x/crypto/ed25519"` | `import "crypto/ed25519"` | Moved to stdlib. Ed25519 itself is not FIPS-approved | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/bn256 | crypto/ecdsa | package | replace | `import "golang.org/x/crypto/bn256"` | `import "crypto/ecdsa"` | BN256 not FIPS-approved. Use NIST curves | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/blake2b | crypto/sha256 | package | replace | `import "golang.org/x/crypto/blake2b"` | `import "crypto/sha256"` | BLAKE2b not FIPS-approved | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/blake2s | crypto/sha256 | package | replace | `import "golang.org/x/crypto/blake2s"` | `import "crypto/sha256"` | BLAKE2s not FIPS-approved | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/hkdf | crypto/hkdf | package | move_package | `import "golang.org/x/crypto/hkdf"` | `import "crypto/hkdf"` | Moved to stdlib in Go 1.24+. FIPS-approved (SP 800-56C) | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/sha3 | crypto/sha3 | package | move_package | `import "golang.org/x/crypto/sha3"` | `import "crypto/sha3"` | Moved to stdlib in Go 1.24+. FIPS-approved (FIPS 202) | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/argon2 | crypto/pbkdf2 | package | replace | `import "golang.org/x/crypto/argon2"` | `import "crypto/pbkdf2"` | Argon2 not FIPS-approved. PBKDF2 available in Go 1.24+ | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/scrypt | crypto/pbkdf2 | package | replace | `import "golang.org/x/crypto/scrypt"` | `import "crypto/pbkdf2"` | scrypt not FIPS-approved. Use PBKDF2 | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/xts | crypto/aes + crypto/cipher | package | replace | `import "golang.org/x/crypto/xts"` | `import ("crypto/aes"; "crypto/cipher")` | XTS not FIPS-approved for general use. Use AES-GCM | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/nacl/box | crypto/ecdh + crypto/aes + crypto/cipher | package | replace | `import "golang.org/x/crypto/nacl/box"` | `import ("crypto/ecdh"; "crypto/aes"; "crypto/cipher")` | NaCl Box: Curve25519+XSalsa20+Poly1305, none FIPS-approved | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/nacl/secretbox | crypto/aes + crypto/cipher | package | replace | `import "golang.org/x/crypto/nacl/secretbox"` | `import ("crypto/aes"; "crypto/cipher")` | NaCl Secretbox: XSalsa20+Poly1305, not FIPS-approved | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/nacl/sign | crypto/ecdsa | package | replace | `import "golang.org/x/crypto/nacl/sign"` | `import "crypto/ecdsa"` | NaCl Sign: Ed25519, not FIPS-approved. Use ECDSA | x/crypto Non-FIPS Packages |
| golang.org/x/crypto/nacl/auth | crypto/hmac + crypto/sha256 | package | replace | `import "golang.org/x/crypto/nacl/auth"` | `import ("crypto/hmac"; "crypto/sha256")` | NaCl Auth: non-standard. Use HMAC-SHA-256 | x/crypto Non-FIPS Packages |
| des.NewCipher | aes.NewCipher | method | replace | `block, err := des.NewCipher(key)` | `block, err := aes.NewCipher(key)` | DES has 56-bit key, broken. Key must be 16/24/32 bytes for AES | Weak Algorithms |
| des.NewTripleDESCipher | aes.NewCipher | method | replace | `block, err := des.NewTripleDESCipher(key)` | `block, err := aes.NewCipher(key)` | 3DES has 64-bit block, Sweet32 vulnerable. Use AES-128+ | Weak Algorithms |
| rc4.NewCipher | aes.NewCipher + cipher.NewGCM | method | replace | `c, err := rc4.NewCipher(key)` | `block, _ := aes.NewCipher(key); gcm, _ := cipher.NewGCM(block)` | RC4 prohibited (RFC 7465). Use AES-GCM for stream encryption | Weak Algorithms |
| md5.New() | sha256.New() | method | replace | `h := md5.New()` | `h := sha256.New()` | MD5 broken for collision resistance (CWE-328) | Weak Algorithms |
| md5.Sum(data) | sha256.Sum256(data) | method | replace | `sum := md5.Sum(data)` | `sum := sha256.Sum256(data)` | MD5 broken. SHA-256 is FIPS-approved | Weak Algorithms |
| sha1.New() | sha256.New() | method | replace | `h := sha1.New()` | `h := sha256.New()` | SHA-1 deprecated for digital signatures (SP 800-131A) | Weak Algorithms |
| sha1.Sum(data) | sha256.Sum256(data) | method | replace | `sum := sha1.Sum(data)` | `sum := sha256.Sum256(data)` | SHA-1 deprecated. SHA-256 is FIPS-approved | Weak Algorithms |
| rsa.GenerateKey(rand, 1024) | rsa.GenerateKey(rand, 2048) | method | replace | `key, err := rsa.GenerateKey(rand.Reader, 1024)` | `key, err := rsa.GenerateKey(rand.Reader, 2048)` | RSA < 2048 bits disallowed (SP 800-131A). Use >= 2048 | Key Strength |
| elliptic.P224() | elliptic.P256() | method | replace | `curve := elliptic.P224()` | `curve := elliptic.P256()` | P-224 deprecated by NIST (SP 800-186). Use P-256 or P-384 | Key Strength |
| argon2.IDKey | pbkdf2.Key | method | replace | `dk := argon2.IDKey(password, salt, 1, 64*1024, 4, 32)` | `dk := pbkdf2.Key(password, salt, 600000, 32, sha256.New)` | Argon2 not FIPS-approved. PBKDF2 with >= 600000 iterations | Password Hashing |
| scrypt.Key | pbkdf2.Key | method | replace | `dk, err := scrypt.Key(password, salt, 32768, 8, 1, 32)` | `dk := pbkdf2.Key(password, salt, 600000, 32, sha256.New)` | scrypt not FIPS-approved. Use PBKDF2 | Password Hashing |
| math/rand | crypto/rand | package | replace | `import "math/rand"` | `import "crypto/rand"` | math/rand is not cryptographically secure (CWE-338). Use crypto/rand for all security-sensitive randomness | Operational Security |
