# Phase: Configuration Migration

Harden TLS configuration, set FIPS environment variables, and fix insecure transport settings.

## Steps

1. Read `references/config-map.md`

2. Identify all configuration points in the project:
   - Go source files creating `tls.Config` structs
   - Dockerfiles, Makefiles, CI/CD pipelines with Go build/run commands
   - Environment variable files (`.env`, `docker-compose.yml`)
   - gRPC client/server setup code
   - Kubernetes client configuration
   - Database connection setup code
   - HTTP client/server configuration

3. For each row in the config map:
   - Search all relevant files for `old_property`
   - If found:
     - Apply the replacement from `new_property`
     - If `new_property` is `removed`, delete the setting
     - If `default_changed` is `true`, verify the application works with the new default
   - Check `notes` for security implications

4. **TLS hardening checklist:**
   - Remove all `InsecureSkipVerify: true` settings (or document exception with justification)
   - Ensure `MinVersion` is at least `tls.VersionTLS12`
   - Do not set `MaxVersion` below `tls.VersionTLS13` (blocks PQC key exchange)
   - Do not hardcode `CipherSuites` — let Go's default selection handle FIPS compliance
   - Do not restrict `CurvePreferences` to exclude ML-KEM curves
   - Ensure `ServerName` is set on all client TLS configs

5. Run the build gate

## Build gate

Run `go build ./...`. TLS configuration errors often surface at runtime — after the build passes, verify the application starts and can establish TLS connections.
