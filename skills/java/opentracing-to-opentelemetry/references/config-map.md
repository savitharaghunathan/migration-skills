# Config Map — OpenTracing to OpenTelemetry

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| JAEGER_SERVICE_NAME | OTEL_SERVICE_NAME | | | | environment variables | | Configuration Property Changes |
| JAEGER_ENDPOINT | OTEL_EXPORTER_OTLP_ENDPOINT | | `http://localhost:14268/api/traces` | `http://localhost:4317` | environment variables | Protocol changes from Thrift/HTTP to OTLP/gRPC | Configuration Property Changes |
| JAEGER_AGENT_HOST | OTEL_EXPORTER_OTLP_ENDPOINT | | | | environment variables | Agent concept replaced by collector endpoint | Configuration Property Changes |
| JAEGER_AGENT_PORT | OTEL_EXPORTER_OTLP_ENDPOINT | | | | environment variables | Agent concept replaced by collector endpoint | Configuration Property Changes |
| JAEGER_SAMPLER_TYPE | OTEL_TRACES_SAMPLER | | | | environment variables | Values differ: `const`→`always_on`/`always_off`, `probabilistic`→`traceidratio`, `ratelimiting`→custom sampler | Configuration Property Changes |
| JAEGER_SAMPLER_PARAM | OTEL_TRACES_SAMPLER_ARG | | | | environment variables | | Configuration Property Changes |
| JAEGER_REPORTER_LOG_SPANS | removed | | | | environment variables | Use logging exporter instead: `OTEL_TRACES_EXPORTER=logging` | Configuration Property Changes |
| JAEGER_REPORTER_MAX_QUEUE_SIZE | OTEL_BSP_MAX_QUEUE_SIZE | | | | environment variables | BSP = BatchSpanProcessor | Configuration Property Changes |
| JAEGER_REPORTER_FLUSH_INTERVAL | OTEL_BSP_SCHEDULE_DELAY | | | | environment variables | Value in milliseconds | Configuration Property Changes |
| JAEGER_TAGS | OTEL_RESOURCE_ATTRIBUTES | | | | environment variables | Format: `key1=value1,key2=value2` | Configuration Property Changes |
| JAEGER_PROPAGATION | OTEL_PROPAGATORS | true | `jaeger` | `tracecontext,baggage` | environment variables | W3C TraceContext is default in OTel | Configuration Property Changes |
| quarkus.jaeger.service-name | quarkus.application.name | | | | application.properties | Quarkus-specific | Quarkus Configuration Properties |
| quarkus.jaeger.endpoint | quarkus.otel.exporter.otlp.traces.endpoint | | | | application.properties | Quarkus-specific | Quarkus Configuration Properties |
| quarkus.jaeger.auth-token | quarkus.otel.exporter.otlp.traces.headers | | | | application.properties | Quarkus-specific | Quarkus Configuration Properties |
| quarkus.jaeger.sampler-type | quarkus.otel.traces.sampler | | | | application.properties | Quarkus-specific | Quarkus Configuration Properties |
| quarkus.jaeger.sampler-param | quarkus.otel.traces.sampler.arg | | | | application.properties | Quarkus-specific | Quarkus Configuration Properties |
| quarkus.jaeger.tags | quarkus.otel.resource.attributes | | | | application.properties | Quarkus-specific | Quarkus Configuration Properties |
| quarkus.jaeger.propagation | quarkus.otel.propagators | | | | application.properties | Quarkus-specific | Quarkus Configuration Properties |
