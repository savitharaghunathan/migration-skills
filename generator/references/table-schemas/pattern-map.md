# Pattern Map Schema

Each row represents a code pattern change that is NOT a simple API rename — behavioral changes, structural refactors, new idioms, or non-code changes (database, deployment, infrastructure).

Use this table for changes that don't fit dependency-map, api-map, or config-map.

## Columns

| Column | Required | Description |
|--------|----------|-------------|
| description | yes | What changed, in one sentence |
| category | yes | One of: `behavioral`, `structural`, `removal`, `addition` |
| detect_pattern | recommended | A grep-friendly search string for locating this pattern in source code. Unlike `before` (which shows illustrative code), this is a literal or regex string suitable for `grep -rn` (e.g., `@Produces.*EntityManager`, `@MessageDriven`, `InitialContext`). Distills the `before` block to its searchable essence. May be omitted for `addition` rows with no source-side artifact. |
| before | recommended | Code block showing old pattern. Required for `structural` and `behavioral` categories. May be omitted for `addition` or when the change has no code-level before state (e.g., database schema changes, Dockerfile updates). |
| after | recommended | Code block showing new pattern, or `N/A` if removal with no replacement. Same rules as `before`. |
| notes | no | Edge cases, gotchas |
| source_section | yes | Guide section heading |

## Category Definitions

- **behavioral** — Same API surface but different runtime behavior (default changed, ordering changed, timing changed)
- **structural** — Code must be reorganized (new base class, different initialization pattern, moved entry point)
- **removal** — Feature or capability removed entirely
- **addition** — New requirement introduced (new required dependency, new config file, new build step)

## Example Rows

| description | category | detect_pattern | before | after | notes | source_section |
|---|---|---|---|---|---|---|
| Health probes enabled by default, no longer need explicit opt-in | behavioral | `management.endpoint.health.probes.enabled` | `management.endpoint.health.probes.enabled=true` | (remove the property — it's the default now) | Only affects apps that explicitly set this property | Health Probes |
| Thread pool executor replaced with virtual threads by default | behavioral | `ThreadPoolTaskExecutor` | `@Bean TaskExecutor taskExecutor() { return new ThreadPoolTaskExecutor(); }` | (remove — virtual threads used automatically) | Add `spring.threads.virtual.enabled=false` to opt out | Virtual Threads |
| Dockerfile must use Java 21+ base image | addition | | | `FROM eclipse-temurin:21-jre` | Java 17 base images will fail at startup | Runtime Requirements |
