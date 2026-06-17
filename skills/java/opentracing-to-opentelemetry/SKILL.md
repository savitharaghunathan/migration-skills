---
name: opentracing-to-opentelemetry
description: Migrates OpenTracing applications to OpenTelemetry. Use when upgrading
  a Java project from io.opentracing APIs (including Jaeger client) to
  io.opentelemetry native APIs and SDKs.
license: Apache-2.0
metadata:
  source: opentracing
  target: opentelemetry
  language: java
  build_tool: "maven: mvn compile"
  guide_url: https://opentelemetry.io/docs/migration/opentracing/
  generated_by: migration-skills-generator
  generated_at: 2026-06-17T00:00:00Z
---

# OpenTracing to OpenTelemetry Migration

**Prerequisite:** The OpenTracing project is archived and no longer receives updates. The OpenTelemetry opentracing-shim has been formally deprecated (April 2026). All new instrumentation must use native OpenTelemetry APIs. Java 8+ is required for the OpenTelemetry SDK.

This migration replaces the OpenTracing API (`io.opentracing`) and its implementations (Jaeger client, MicroProfile OpenTracing) with native OpenTelemetry APIs (`io.opentelemetry`). The migration covers dependency replacement, API-level class and method changes, configuration property migration (Jaeger/Quarkus), propagation format updates (Jaeger → W3C TraceContext), and fundamental behavioral changes to baggage handling and error reporting.

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build system** — Replace OpenTracing/Jaeger dependencies with OpenTelemetry SDK and API artifacts. Read `modules/build-system.md`.
2. **Code** — Replace OpenTracing API calls with OpenTelemetry equivalents: Tracer, Span, Scope, Tags→Attributes, error handling, propagation. Read `modules/code.md`.
3. **Config** — Migrate Jaeger environment variables and Quarkus properties to OpenTelemetry equivalents. Read `modules/config.md`.
4. **Testing** — Update test instrumentation and tracing mocks. Read `modules/testing.md`.
5. **Additional** — Handle propagation format migration, baggage changes, and OTel SDK initialization. Read `modules/additional.md`.
6. **Cleanup** — Remove shim dependencies, verify no OpenTracing imports remain, validate tracing works end-to-end. Read `modules/cleanup.md`.

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`)
2. Run the build
3. If it fails, fix the issue before proceeding
4. If you cannot fix it, stop and report to the user
