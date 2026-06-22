# Phase: Build System Migration

Upgrade Go version and replace non-FIPS x/crypto dependencies with stdlib equivalents.

## Steps

1. **Verify Go version** — check `go.mod` for the `go` directive:
   - If `go` version is < 1.24, update to `go 1.24` or later
   - Go 1.24+ provides native FIPS 140-3 mode and `crypto/mlkem` for PQC
   - Run `go mod tidy` after updating the version

2. Read `references/dependency-map.md`

3. Open the project's `go.mod` file

4. For each row in the dependency map:
   - Search `go.mod` for `old_artifact`
   - Also search all `.go` files for imports of `old_artifact`
   - Apply the `action`:
     - `replace` → remove the old module from `go.mod`, add import of `new_artifact` in code
     - `remove` → delete the dependency and all usages
     - `rename` → update import paths (module moved to stdlib in Go 1.24+)
   - Check `notes` for FIPS compliance context
   - Run `go mod tidy` after changes

5. **Check for BoringCrypto build tags** — search for:
   - `GOEXPERIMENT=boringcrypto` in build scripts, Dockerfiles, Makefiles
   - `goexperiment.boringcrypto` build tags
   - `golang-fips/go` fork references
   - These are superseded by Go 1.24+ native FIPS mode

6. Run the build gate

## Build gate

Run `go build ./...`. Common issues:
- Missing stdlib packages (some moved to stdlib only in Go 1.24+)
- Import path conflicts between x/crypto and stdlib versions
- Build tag incompatibilities after removing BoringCrypto tags
