# Phase: Configuration Migration

Migrate configuration properties to their new names, values, or locations.

## Steps

1. Read `references/config-map.md`
2. Identify all configuration files in the project:
   - `application.properties`, `application.yml`
   - `application-*.properties`, `application-*.yml` (profile-specific)
   - `bootstrap.properties`, `bootstrap.yml`
3. For each row in the config map:
   - Search all config files for `old_property`
   - If found:
     - Replace with `new_property` (or remove if `new_property` is `removed`)
     - If `default_changed` is `true`, check whether the project relies on the old default:
       - If the property is not explicitly set and the project depends on the old behavior, add it explicitly with `old_default` as the value
       - If the property is explicitly set, just rename it
   - Check `notes` for migration context
4. For wildcard entries (e.g., `management.metrics.export.prometheus.*` → `management.prometheus.metrics.export.*`):
   - Search for any property matching the old prefix pattern
   - Restructure to the new prefix pattern
5. Run the build gate

## Key migration groups

- **Redis:** `spring.redis.*` → `spring.data.redis.*`
- **Cassandra:** `spring.data.cassandra.*` → `spring.cassandra.*`
- **Metrics export:** `management.metrics.export.<product>.*` → `management.<product>.metrics.export.*`
- **Elasticsearch:** `spring.data.elasticsearch.client.reactive.*` → `spring.elasticsearch.*`
- **Flyway:** Several properties removed in Flyway 9.0; use `ignore-migration-patterns`
- **Actuator:** httptrace → httpexchanges; sanitization key-based → role-based

## Build gate

Run the project build command. Configuration errors often surface as startup failures rather than compilation errors — if the build passes but the application fails to start, check the config changes.
