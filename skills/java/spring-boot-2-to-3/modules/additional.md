# Phase: Additional Changes

Handle changes outside the standard build/code/config/testing phases.

## Steps

### 1. Hibernate schema changes

If using Hibernate with schema validation or auto-DDL:
- **Sequences:** Hibernate 6.0 creates per-entity sequences (`<entity>_seq`) instead of a single `hibernate_sequence`. Either:
  - Create new sequences: `create sequence <entity>_seq start with <max_id + 51>`
  - Or set `hibernate.id.db_structure_naming_strategy=single` for backwards compatibility
- **Type mapping changes:** Check for schema validation errors due to:
  - Duration: `BIGINT` → `interval second` / `numeric(21)` (set `hibernate.type.preferred_duration_jdbc_type=BIGINT` to revert)
  - UUID: `binary(16)` → native `uuid` (set `hibernate.type.preferred_uuid_jdbc_type=BINARY` to revert)
  - Instant: `timestamp` → `timestamp with time zone` (set `hibernate.type.preferred_instant_jdbc_type=TIMESTAMP` to revert)
  - Enum: `TINYINT` → `SMALLINT`
  - Arrays: `VARBINARY` (Java serialization) → native SQL array / JSON (use `@JdbcTypeCode(SqlTypes.VARBINARY)` to revert)

### 2. Flyway migration

- Run `flyway repair` after upgrading to fix potential checksum mismatches
- Replace `ignoreIgnoredMigrations`, `ignoreFutureMigrations`, `ignoreMissingMigrations`, `ignorePendingMigrations` with `ignoreMigrationPatterns`
- If using undo command, add `flyway-proprietary` module to classpath
- Note: `cleanDisabled` now defaults to `true`

### 3. Spring Security configuration

- If extending `WebSecurityConfigurerAdapter`, refactor to bean-based configuration with `SecurityFilterChain`
- Replace `antMatchers`/`mvcMatchers` with `requestMatchers`
- Add `@Configuration` to classes annotated with `@EnableWebSecurity` or `@EnableMethodSecurity`
- Consider upgrading to Spring Security 5.8 patterns first, then to 6.0

### 4. Auto-configuration registration

If your project or library defines custom auto-configurations:
- Create `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`
- List each auto-configuration class on its own line
- Remove the `EnableAutoConfiguration` key from `spring.factories` (other keys are unaffected)

### 5. Gradle changes

- Main class resolution simplified — configure via `springBoot { mainClass = "..." }` if needed
- Task properties use Gradle `Property` API — use `.set()` instead of direct assignment in Kotlin DSL
- Build-info exclusion: use `excludes = ['time']` instead of setting to null

### 6. Maven changes

- Remove `fork` attribute from `spring-boot:run`/`spring-boot:start` plugin configuration
- Update Git Commit ID plugin: `pl.project13.maven:git-commit-id-plugin` → `io.github.git-commit-id:git-commit-id-maven-plugin`

## Build gate

Run the project build command. For infrastructure changes, also verify the application starts successfully.
