# Phase: Build System Migration

Update dependencies to replace OpenTracing and Jaeger artifacts with OpenTelemetry equivalents.

## Steps

1. Read `references/dependency-map.md`
2. Open the project's build file(s):
   - Maven: `pom.xml`
   - Gradle: `build.gradle` or `build.gradle.kts`
3. **Add the OpenTelemetry BOM first** to `dependencyManagement`:
   - `io.opentelemetry:opentelemetry-bom` (manages core artifact versions)
   - `io.opentelemetry.instrumentation:opentelemetry-instrumentation-bom` (manages instrumentation artifact versions)
4. For each row in the dependency map:
   - Search the build file for `old_artifact`
   - Apply the `action`:
     - `replace` → change the artifact coordinate to `new_artifact`
     - `remove` → delete the dependency entry
   - Check `notes` for gotchas before moving on
5. Key dependency replacements to watch for:
   - `io.opentracing:opentracing-api` → `io.opentelemetry:opentelemetry-api`
   - `io.opentracing:opentracing-util` → `io.opentelemetry:opentelemetry-api` (GlobalTracer absorbed)
   - `io.jaegertracing:jaeger-client` → `io.opentelemetry:opentelemetry-sdk` + `io.opentelemetry:opentelemetry-exporter-otlp`
   - `io.jaegertracing:jaeger-thrift` → `io.opentelemetry:opentelemetry-exporter-otlp`
   - `org.eclipse.microprofile.opentracing:microprofile-opentracing-api` → `io.opentelemetry.instrumentation:opentelemetry-instrumentation-annotations`
   - `io.quarkus:quarkus-smallrye-opentracing` → `io.quarkus:quarkus-opentelemetry`
   - `io.opentracing.contrib:opentracing-spring-cloud-starter` → `io.opentelemetry.instrumentation:opentelemetry-spring-boot-starter`
6. If using the shim for incremental migration, temporarily add `io.opentelemetry:opentelemetry-opentracing-shim` — but plan to remove it
7. Run the build gate

## Build gate

Run `mvn compile` or `gradle build`. Expect compilation failures from removed OpenTracing API imports — these will be fixed in the Code phase. The build file itself should be valid with no unresolvable artifacts.

Common issues:
- Version conflicts between OpenTelemetry BOM-managed versions and explicit version declarations
- Transitive dependency on `io.opentracing:opentracing-api` pulled in by other libraries — check dependency tree with `mvn dependency:tree` or `gradle dependencies`
- Missing `opentelemetry-bom` in dependencyManagement causes version resolution failures
