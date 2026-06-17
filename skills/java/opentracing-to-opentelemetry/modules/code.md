# Phase: Code Migration

Replace OpenTracing API calls with native OpenTelemetry equivalents throughout the codebase.

## Steps

### Part 1: API replacements

1. Read `references/api-map.md`
2. Process rows in `kind` order to prevent conflicts:
   - First: `interface` — replace core interfaces (`Tracer`, `Span`, `SpanContext`, `Scope`, `ScopeManager`)
   - Then: `class` — replace `GlobalTracer`, `Tags`, `TextMapAdapter`
   - Then: `annotation` — swap `@Traced` → `@WithSpan`
   - Then: `field` — replace `Tags.*` constants with `SemanticAttributes.*` or `AttributeKey`
   - Finally: `method` — update `setTag`→`setAttribute`, `log`→`addEvent`, `finish`→`end`, `buildSpan`→`spanBuilder`
3. For each row:
   - Search the codebase for `old_api` (check imports, type references, annotations, method calls)
   - Apply the `action` using the `before`/`after` columns as concrete examples
   - Key areas to search:
     - `import io.opentracing.*` — replace with `import io.opentelemetry.*`
     - `import io.opentracing.tag.Tags` — replace with semantic convention attributes
     - `import io.opentracing.util.GlobalTracer` — replace with `import io.opentelemetry.api.GlobalOpenTelemetry`
     - `import io.opentracing.propagation.*` — replace with `import io.opentelemetry.context.propagation.*`
     - `import org.eclipse.microprofile.opentracing.Traced` — replace with `import io.opentelemetry.instrumentation.annotations.WithSpan`

### Part 2: Pattern changes

1. Read `references/pattern-map.md`
2. For each row, transform the code pattern:
   - **Tracer initialization** — Replace Jaeger `Configuration.fromEnv().getTracer()` with OTel SDK autoconfigure or programmatic setup
   - **Span activation** — Replace `tracer.activateSpan(span)` / `tracer.scopeManager().activate(span)` with `span.makeCurrent()`
   - **Active span access** — Replace `tracer.activeSpan()` with `Span.current()`
   - **Error handling** — Replace `span.setTag(Tags.ERROR, true)` + `span.log(...)` with `span.setStatus(StatusCode.ERROR)` + `span.recordException(e)`
   - **SpanKind** — Move from `span.setTag(Tags.SPAN_KIND, ...)` to `tracer.spanBuilder("name").setSpanKind(SpanKind.SERVER).startSpan()`
   - **Child spans** — Replace `tracer.buildSpan("child").asChildOf(parent).start()` with `tracer.spanBuilder("child").setParent(Context.current().with(parent)).startSpan()`
   - **Span logs** — Replace `span.log(Map.of(...))` with `span.addEvent(name, Attributes.of(...))`
   - **Context injection/extraction** — Replace `tracer.inject(...)/extract(...)` with `propagator.inject(...)/extract(...)`
3. Run the build gate

## Build gate

Run `mvn compile` or `gradle build`. If it fails, check for:
- Missing imports after package moves (most common: `io.opentelemetry.context.Scope` vs `io.opentracing.Scope`)
- Incompatible method signatures (`setTag` → `setAttribute`, `finish` → `end`)
- `Tags.ERROR` usage — must be replaced with `StatusCode` enum, not an attribute
- `SpanKind` set as tag instead of on SpanBuilder
