# Phase: Code Migration

Replace non-FIPS APIs, fix weak algorithm usage, and remediate operational security issues.

## Steps

### Part 1: API replacements

1. Read `references/api-map.md`
2. Process rows in `kind` order:
   - First: `package` — replace x/crypto package imports with stdlib equivalents
   - Then: `class` — update type references (cipher types, hash types)
   - Then: `method` — update function calls (NewCipher, New, Sum, etc.)
3. For each row:
   - Search all `.go` files for `old_api`
   - Apply the `action`:
     - `replace` → substitute with `new_api`, update imports
     - `remove` → delete usage; use `after` column for replacement pattern
     - `move_package` → update import path (same API, new package location)
   - Use `before`/`after` columns as transformation examples
   - Check `notes` for FIPS compliance context

### Part 2: Weak algorithm remediation

1. Read `references/pattern-map.md` — filter for `category` = `removal` or `behavioral`
2. For each weak algorithm pattern:
   - **DES/3DES** → replace with AES-128/256
   - **RC4** → replace with AES-GCM
   - **MD5** → replace with SHA-256 (for hashing) or HMAC-SHA-256 (for MACs)
   - **SHA-1** → replace with SHA-256 or SHA-512
   - **CBC/CFB/OFB without authentication** → replace with AES-GCM (authenticated encryption)

### Part 3: Operational security fixes

1. Read `references/pattern-map.md` — filter for `source_section` = `Operational Security`
2. For each operational issue:
   - **Hardcoded keys/IVs** → use `crypto/rand.Read()` for key generation
   - **math/rand for crypto** → replace with `crypto/rand`
   - **Static salts** → generate unique salts with `crypto/rand`
   - **Nonce reuse risk** → ensure unique nonces per encryption operation

3. Run the build gate

## Build gate

Run `go build ./...`. Check for:
- Missing imports after package moves
- Type mismatches between old and new cipher/hash interfaces
- Unused imports from removed algorithms
