# Dependency Map — OpenTracing to OpenTelemetry

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| io.opentracing:opentracing-api | io.opentelemetry:opentelemetry-api | | replace | Core tracing API replacement | Dependency Changes |
| io.opentracing:opentracing-util | io.opentelemetry:opentelemetry-api | | replace | GlobalTracer replaced by GlobalOpenTelemetry; util functionality absorbed into OTel API | Dependency Changes |
| io.opentracing:opentracing-noop | removed | | remove | OTel SDK handles no-op behavior internally | Dependency Changes |
| io.opentracing:opentracing-mock | removed | | remove | Use `io.opentelemetry:opentelemetry-sdk-testing` for test support | Dependency Changes |
| io.jaegertracing:jaeger-client | io.opentelemetry:opentelemetry-sdk | | replace | Jaeger clients retired in 2022; use OTel SDK with OTLP exporter | Dependency Changes |
| io.jaegertracing:jaeger-core | io.opentelemetry:opentelemetry-sdk | | replace | Core tracing functionality replaced by OTel SDK | Dependency Changes |
| io.jaegertracing:jaeger-thrift | io.opentelemetry:opentelemetry-exporter-otlp | | replace | Thrift exporter replaced by OTLP exporter; Jaeger backend supports OTLP since v1.35 | Dependency Changes |
| io.opentracing.contrib:opentracing-spring-cloud-starter | io.opentelemetry.instrumentation:opentelemetry-spring-boot-starter | | replace | Spring Cloud OpenTracing integration replaced by OTel Spring Boot starter | Dependency Changes |
| io.quarkus:quarkus-smallrye-opentracing | io.quarkus:quarkus-opentelemetry | | replace | Quarkus OpenTracing extension replaced by OpenTelemetry extension | Dependency Changes |
| org.eclipse.microprofile.opentracing:microprofile-opentracing-api | io.opentelemetry.instrumentation:opentelemetry-instrumentation-annotations | | replace | MicroProfile @Traced replaced by @WithSpan | Dependency Changes |
| io.opentelemetry:opentelemetry-opentracing-shim | removed | | remove | Temporary shim for incremental migration; remove after full migration. Shim is deprecated as of 2026. | Dependency Changes |
