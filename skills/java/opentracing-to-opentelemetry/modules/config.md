# Phase: Configuration Migration

Migrate tracing configuration from Jaeger/OpenTracing properties to OpenTelemetry equivalents.

## Steps

1. Read `references/config-map.md`
2. Identify all configuration locations in the project:
   - Environment variables (Dockerfiles, docker-compose.yml, Kubernetes manifests, .env files, CI/CD configs)
   - Java system properties (`-D` flags in startup scripts)
   - Quarkus: `application.properties`, `application.yml`, `application-*.properties`
   - Spring Boot: `application.properties`, `application.yml`
   - Programmatic configuration in Java code (Jaeger `Configuration.fromEnv()` calls)
3. For each row in the config map:
   - Search all config locations for `old_property`
   - If found, replace with `new_property` (or remove if `new_property` is `removed`)
   - Key migration groups:
     - **Service identity:** `JAEGER_SERVICE_NAME` → `OTEL_SERVICE_NAME`
     - **Endpoint:** `JAEGER_ENDPOINT` → `OTEL_EXPORTER_OTLP_ENDPOINT` (note protocol change: Thrift/HTTP → OTLP/gRPC, default port 14268 → 4317)
     - **Sampling:** `JAEGER_SAMPLER_TYPE` → `OTEL_TRACES_SAMPLER` (value mapping: `const`(1)→`always_on`, `const`(0)→`always_off`, `probabilistic`→`traceidratio`)
     - **Resource tags:** `JAEGER_TAGS` → `OTEL_RESOURCE_ATTRIBUTES`
     - **Propagation:** `JAEGER_PROPAGATION` → `OTEL_PROPAGATORS` (default changes from `jaeger` to `tracecontext,baggage`)
     - **Quarkus-specific:** All `quarkus.jaeger.*` → `quarkus.otel.*` equivalents
4. Run the build gate

## Build gate

Run `mvn compile` or `gradle build`. Configuration errors often surface as startup failures rather than compilation errors — if the build passes but the application fails to start, check:
- Endpoint URL format (OTLP uses `http://host:4317` for gRPC, `http://host:4318` for HTTP)
- Sampler type values (Jaeger `const` is not valid in OTel; use `always_on`/`always_off`)
- Missing `OTEL_SERVICE_NAME` — OTel SDK requires a service name
