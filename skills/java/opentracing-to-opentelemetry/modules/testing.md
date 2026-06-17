# Phase: Testing Migration

Update test instrumentation, tracing mocks, and test configuration.

## Steps

1. Read `references/api-map.md` — filter for rows where tests are affected
2. Read `references/dependency-map.md` — note test-scoped dependency changes:
   - `io.opentracing:opentracing-mock` → use `io.opentelemetry:opentelemetry-sdk-testing` instead
3. For test dependencies:
   - Add `io.opentelemetry:opentelemetry-sdk-testing` as a test dependency (provides `InMemorySpanExporter` and assertion helpers)
   - Remove `io.opentracing:opentracing-mock` if present
4. For test code changes:
   - Replace OpenTracing `MockTracer` usage with OTel `InMemorySpanExporter`:
     - Before: `MockTracer mockTracer = new MockTracer();` then `mockTracer.finishedSpans()`
     - After: `InMemorySpanExporter exporter = InMemorySpanExporter.create();` then `exporter.getFinishedSpanItems()`
   - Update span assertions:
     - `mockSpan.operationName()` → `spanData.getName()`
     - `mockSpan.tags()` → `spanData.getAttributes()`
     - `mockSpan.logEntries()` → `spanData.getEvents()`
   - Replace `@Traced` annotations in test classes with `@WithSpan`
   - Update any test configuration that sets `JAEGER_*` env vars to use `OTEL_*` equivalents
5. Run the full test suite

## Build gate

Run the project's test suite:
- Maven: `mvn test`
- Gradle: `gradle test`

Test failures may indicate:
- Span name assertions that need updating (operation name → span name)
- Tag assertions that need updating (tags → attributes, different key names per semantic conventions)
- Log assertions that need updating (logs → events)
- Timing-sensitive tests affected by different default BatchSpanProcessor behavior
