# Phase: Cleanup and Verification

Remove shim dependencies, verify no OpenTracing remnants remain, and validate end-to-end tracing.

## Steps

1. **Remove the OpenTracing shim** (if it was added for incremental migration):
   - Remove `io.opentelemetry:opentelemetry-opentracing-shim` from the build file
   - Remove any `OpenTracingShim.createTracerShim(...)` calls
   - Remove any `GlobalTracer.registerIfAbsent(shimTracer)` calls

2. **Remove old package imports:** Search for any remaining imports of old packages:
   - `import io.opentracing.*`
   - `import io.opentracing.tag.*`
   - `import io.opentracing.util.*`
   - `import io.opentracing.propagation.*`
   - `import io.opentracing.contrib.*`
   - `import io.jaegertracing.*`
   - `import org.eclipse.microprofile.opentracing.*`
   These indicate missed transformations — go back and fix them.

3. **Verify no old artifacts remain in build files:**
   - Search `pom.xml` / `build.gradle` for:
     - `io.opentracing`
     - `io.jaegertracing`
     - `opentracing-spring-cloud`
     - `quarkus-smallrye-opentracing`
     - `microprofile-opentracing`
   - Any remaining references are migration gaps — fix them

4. **Verify no old configuration properties remain:**
   - Search all config files, Dockerfiles, k8s manifests for:
     - `JAEGER_SERVICE_NAME`, `JAEGER_ENDPOINT`, `JAEGER_AGENT_HOST`, `JAEGER_SAMPLER_TYPE`
     - `quarkus.jaeger.*`
     - `uber-trace-id` (Jaeger propagation header)
     - `uberctx-` (Jaeger baggage header prefix)

5. **Verify behavioral changes from pattern-map.md:**
   - Baggage: confirm all baggage operations use `io.opentelemetry.api.baggage.Baggage`, not span-based baggage
   - Error handling: confirm errors use `span.setStatus(StatusCode.ERROR)` + `span.recordException(e)`, not `span.setTag("error", true)`
   - SpanKind: confirm span kinds are set via `spanBuilder.setSpanKind()`, not as tags
   - Propagation: confirm W3C TraceContext is the active propagator (or explicitly configured multi-format during transition)

6. **Final build and test:**
   - Run the full build: compilation + tests
   - Verify the application starts successfully
   - Verify trace data flows to the collector (check Jaeger UI or OTel Collector logs)
   - If the project has integration tests, run those too

7. **Report to user:**
   - List all changes made across all phases
   - Flag any items that could not be automatically migrated (require manual review):
     - Custom OpenTracing `Tracer` implementations
     - Custom `TextMap` carrier implementations
     - Complex baggage propagation patterns
     - Third-party libraries that depend on OpenTracing API internally
   - Note behavioral changes the user should verify manually:
     - Propagation format change (all communicating services must be aligned)
     - Baggage isolation (OTel baggage is independent from spans)
     - `@WithSpan` creates child spans where `@Traced` modified the current span
