# Phase: Additional Changes

Handle non-code changes: propagation format migration, baggage architecture changes, SDK initialization, and infrastructure updates.

## Steps

1. Read `references/pattern-map.md` — filter for rows with category `behavioral` or `addition`

### Propagation Format Migration

- The Jaeger wire format (`uber-trace-id` header) is deprecated in OpenTelemetry
- W3C Trace-Context (`traceparent` header) is the default in OTel
- If your system has multiple services communicating via trace context headers:
  - All services must be migrated together, OR
  - Configure OTel to support both formats during transition: `OTEL_PROPAGATORS=tracecontext,baggage,jaeger` (add `jaeger` propagator temporarily)
- After all services are migrated, remove the `jaeger` propagator
- Update any reverse proxies, API gateways, or load balancers that inspect or forward trace headers

### Baggage Migration

- In OpenTracing, baggage is carried by the SpanContext within a Span (mutable)
- In OpenTelemetry, baggage is an independent signal propagated via Context (immutable)
- Baggage set via OpenTracing API is NOT accessible via OpenTelemetry API
- Migrate ALL baggage-related API calls at the same time to avoid data loss
- Update header format: `uberctx-{key}` → W3C `baggage` header

### SDK Initialization

- Replace Jaeger client initialization (`Configuration.fromEnv().getTracer()`) with OTel SDK setup
- Recommended: use `opentelemetry-sdk-extension-autoconfigure` for environment-variable-based configuration
- Ensure `OTEL_SERVICE_NAME` is set (required by OTel SDK)
- Configure appropriate exporter (OTLP is default; Jaeger backend supports OTLP since v1.35)

### Infrastructure Updates

- Update Jaeger collector/agent configuration to accept OTLP (port 4317 for gRPC, 4318 for HTTP)
- Update Kubernetes manifests, Docker Compose files, and CI/CD pipelines to use `OTEL_*` environment variables
- Update monitoring dashboards and alerts to consume OpenTelemetry-formatted data
- If using the OTel Java Agent (`-javaagent:opentelemetry-javaagent.jar`), no code changes are needed for auto-instrumented libraries

## Build gate

Run `mvn compile` or `gradle build`. For infrastructure changes, also verify the application starts successfully and trace data reaches the collector.
