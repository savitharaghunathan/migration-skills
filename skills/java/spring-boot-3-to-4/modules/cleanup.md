# Phase: Cleanup and Verification

Remove compatibility shims, dead code, and verify the final build.

## Steps

1. **Remove classic starters:** If you added `spring-boot-starter-classic` or `spring-boot-starter-test-classic` as a migration stepping stone, remove them now and add the specific starters you need.

2. **Remove properties migrator:** Remove the `spring-boot-properties-migrator` dependency.

3. **Remove compatibility imports:** Search for any remaining imports of old packages:
   - `org.springframework.boot.autoconfigure.domain.EntityScan` (should be `org.springframework.boot.persistence.autoconfigure.EntityScan`)
   - `org.springframework.boot.test.mock.mockito.MockBean` (should be `@MockitoBean`)
   - `org.springframework.boot.test.mock.mockito.SpyBean` (should be `@MockitoSpyBean`)
   - `org.springframework.boot.env.EnvironmentPostProcessor` (should be `org.springframework.boot.EnvironmentPostProcessor`)
   - `org.springframework.boot.BootstrapRegistry` (should be `org.springframework.boot.bootstrap.BootstrapRegistry`)
   - `org.springframework.lang.Nullable` (should be `org.jspecify.annotations.Nullable`)
   - `com.fasterxml.jackson.databind` (should be `tools.jackson.databind` except annotations)

4. **Verify no old artifacts remain:**
   - Search build files for deprecated starter names (`spring-boot-starter-web`, `spring-boot-starter-oauth2-*`, `spring-boot-starter-aop`)
   - Search for removed dependencies (`undertow`, `spring-pulsar-reactive`, `elasticsearch-rest-client`)
   - Search for old Jackson group IDs (`com.fasterxml.jackson.core:jackson-core`, `com.fasterxml.jackson.core:jackson-databind`)
   - Search for old config properties (`spring.data.mongodb.host`, `spring.session.redis.*`, `management.health.mongo.*`)

5. **Final build and test:**
   - Run the full build: compilation + tests
   - If the project has integration tests, run those too
   - Verify the application starts successfully

6. **Report to user:**
   - List all changes made across all phases
   - Flag any items that could not be automatically migrated (require manual review)
   - Note behavioral changes that the user should verify manually:
     - Liveness/readiness probes now enabled by default
     - PropertyMapper null behavior changed
     - Optional dependencies excluded from uber jars
     - Jackson auto-module detection enabled
     - Logback charset now UTF-8
     - Spring Batch in-memory by default
     - `/fonts/**` added to static resource locations
     - MongoDB UUID/BigDecimal representations require explicit config
