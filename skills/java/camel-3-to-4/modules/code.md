# Phase: Code Migration

Replace renamed, moved, and removed APIs throughout the codebase.

## Steps

### Part 1: API replacements

1. Read `references/api-map.md`
2. Process rows in `kind` order to prevent conflicts:
   - First: `package` — apply package-level renames/moves
   - Then: `class` — update class references
   - Then: `annotation` — swap annotation imports and usages
   - Then: `interface` — update interface references
   - Then: `enum` — update enum references
   - Then: `method` — update method calls
   - Finally: `field` — update field references
3. For each row:
   - Search the codebase for `old_api` (check imports, type references, annotations, method calls)
   - Apply the `action`:
     - `replace` → substitute with `new_api`
     - `remove` → delete usage; if `after` column has a replacement pattern, use it
     - `rename` → update the name
     - `move_package` → update import statements to new package
   - Use the `before`/`after` columns as concrete examples of the transformation
   - Check `notes` for edge cases

### Key API migration areas

**CamelContext extension access (highest impact):**
- `context.adapt(ExtendedCamelContext.class)` → `context.getCamelContextExtension()`
- `context.getExtension(...)` → `context.getCamelContextExtension()`
- `exchange.adapt(ExtendedExchange.class)` → `exchange.getExchangeExtension()`
- Access plugins via: `context.getCamelContextExtension().getContextPlugin(ManagedCamelContext.class)`

**Annotation changes:**
- `@FallbackConverter` → `@Converter(fallback = true)`
- `@EndpointInject(uri = "...")` → `@EndpointInject("...")`
- `@Produce(uri = "...")` → `@Produce("...")`
- `@Consume(uri = "...")` → `@Consume("...")`

**Lifecycle interface renames:**
- `OnCamelContextStart` → `OnCamelContextStarting`
- `OnCamelContextStop` → `OnCamelContextStopping`

**Removed APIs:**
- `ExchangePattern.InOptionalOut` — use `InOut`
- `ProducerTemplate.asyncCallback()` — use `asyncSend()` or `asyncRequest()`
- `SimpleBuilder` — was mostly internal
- `ThreadPoolRejectedPolicy.Discard` and `DiscardOldest` — no replacement

**Component-specific API changes:**
- camel-kubernetes: all `replace*` operations → `update*` (replaceConfigMap→updateConfigMap, etc.)
- camel-atom: `org.apache.abdera.model.Feed` → `com.apptasticsoftware.rssreader.Item`
- camel-twitter: `twitter4j.Status` → `twitter4j.v1.Status`
- camel-openapi-java: `OasDocument` → `io.swagger.v3.oas.models.OpenAPI`
- camel-main: constants moved from `Main`/`BaseMainSupport` to `MainConstants`
- camel-aws2-sns: `queueUrl` → `queueArn`

### Part 2: Pattern changes

1. Read `references/pattern-map.md`
2. For each row:
   - Use the `before` code block to locate affected code in the project
   - Transform to match the `after` pattern
   - Pay attention to `category`:
     - `behavioral` — the code may not need changes, but behavior differs; check `notes`
     - `structural` — code reorganization required
     - `removal` — delete the code or replace with alternative from `notes`
     - `addition` — add new code/config as shown in `after`

### Key pattern changes

**camel-bean method syntax:**
```java
// Before
"bean:myBean?method=foo(com.foo.MyOrder, true)"
// After
"bean:myBean?method=foo(com.foo.MyOrder.class, true)"
```

**camel-http timeout restructuring:**
- `socketTimeout` removed; use `responseTimeout`
- `soTimeout` parameters must use `httpConnection.` prefix
- Now 4 timeout options: `connectionRequestTimeout`, `connectTimeout`, `soTimeout`, `responseTimeout`

**camel-platform-http-starter URL path change:**
```java
// Before: http://localhost:8080/camel/myservice
// After:  http://localhost:8080/myservice
// No more servlet context-path prefix
```

**camel-jpa transaction management:**
```java
// Before
from("jpa:MyEntity?transactionManager=#txManager")
// After
from("jpa:MyEntity?transactionStrategy=#txStrategy")
```

3. Run the build gate

## Build gate

Run `mvn compile` (or `gradle build`). If it fails, check for:
- Missing imports after package moves (BacklogTracerEventMessage, MainConstants)
- Incompatible method signatures (adapt→getCamelContextExtension, replace→update)
- Removed APIs still referenced in the code (InOptionalOut, asyncCallback, SimpleBuilder)
