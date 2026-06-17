# Sources

## Primary Guide

- [Migrating from OpenTracing | OpenTelemetry](https://opentelemetry.io/docs/migration/opentracing/)
  - Why Migrate
  - Migration Strategy
  - Core API Mapping
  - Method Mapping
  - Baggage Changes
  - Code Examples

## Specification

- [OpenTracing Compatibility | OpenTelemetry Specification](https://opentelemetry.io/docs/specs/otel/compatibility/opentracing/)
  - OpenTracing Shim Specification Details
  - Error Event Mapping in Shim
  - Span Kind Mapping
  - Propagation Format Changes

## Jaeger Migration

- [Migration to OpenTelemetry SDK | Jaeger](https://www.jaegertracing.io/sdk-migration/)
  - Dependency Changes (Jaeger client → OTel SDK)
  - Configuration Property Changes (Jaeger env vars → OTel env vars)
  - Propagation Format Changes (uber-trace-id → traceparent)

## Quarkus Tutorial

- [Migrate from OpenTracing to OpenTelemetry tracing | Quarkus](https://quarkus.io/guides/telemetry-opentracing-to-otel-tutorial)
  - Dependency Changes (Quarkus-specific)
  - Quarkus Configuration Properties
  - Annotation Mapping (@Traced → @WithSpan)
  - Code Examples (before/after)

## OpenTelemetry Java SDK

- [opentelemetry-java/opentracing-shim | GitHub](https://github.com/open-telemetry/opentelemetry-java/tree/main/opentracing-shim)
  - Shim Usage
  - Dependency Changes (shim artifact)

## Supplementary

- [Deprecating OpenTracing Compatibility | OpenTelemetry Blog (April 2026)](https://opentelemetry.io/blog/2026/deprecating-opentracing-compatibility/)
  - Deprecation timeline and recommendations

- [OpenTelemetry Java API Documentation](https://opentelemetry.io/docs/languages/java/api/)
  - Semantic Convention Mapping
  - Method Mapping

- [konveyor/rulesets Issue #363](https://github.com/konveyor/rulesets/issues/363)
  - Original feature request for OpenTracing to OpenTelemetry migration rules
