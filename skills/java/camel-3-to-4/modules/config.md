# Phase: Configuration Migration

Migrate configuration properties to their new names, values, or locations.

## Steps

1. Read `references/config-map.md`
2. Identify all configuration files in the project:
   - `application.properties`, `application.yml`, `application-*.properties`, `application-*.yml`
   - `bootstrap.properties`, `bootstrap.yml`
   - Any Camel-specific configuration files
3. For each row in the config map:
   - Search all config files for `old_property`
   - If found:
     - Replace with `new_property` (or remove if `new_property` is `removed`)
     - If `default_changed` is `true`, check whether the project relies on the old default:
       - If the property is not explicitly set and the project depends on the old behavior, add it explicitly with `old_default` as the value
       - If the property is explicitly set, just rename it
   - Check `notes` for migration context

### Key configuration changes

**Health check property rename:**
- `camel.health.components-enabled` → `camel.health.producers-enabled`
- Producer health checks now disabled by default; add `camel.health.producers-enabled=true` if your app relies on producer health checks (especially AWS components and camel-kafka)

**Backlog tracing behavior change:**
- `backlogTracing=true` now auto-enables the tracer on startup
- To get the old behavior (tracer available but not auto-enabled), use `backlogTracingStandby=true` instead

**Micrometer URI tags:**
- `camel.metrics.uriTagDynamic` now defaults to `false`; set to `true` to re-enable dynamic URI tags if needed

**Component-specific default changes:**
- camel-slack: consumer delay changed from 500ms to 10000ms (10s)
- camel-spring-rabbitmq: replyTimeout changed from 5s to 30s
- camel-http: `socketTimeout` removed; use `responseTimeout`
- camel-jpa: `transactionManager` option removed; use `transactionStrategy`
- camel-azure-cosmosdb: `itemPartitionKey` type changed from `PartitionKey` to `String`

4. Run the build gate

## Build gate

Run `mvn compile` (or `gradle build`). Configuration errors often surface as startup failures rather than compilation errors — if the build passes but the application fails to start, check the config changes. Pay special attention to health check and tracing properties.
