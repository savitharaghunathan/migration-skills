# Phase: Additional Changes

Handle XML DSL changes, YAML DSL restructuring, EIP modifications, JMX updates, and component-specific structural changes.

## Steps

1. Read `references/pattern-map.md` — filter for `structural` and `addition` categories

### XML DSL description migration

Update all route XML files to use `description` as an attribute instead of a child element:

```xml
<!-- Before -->
<route id="myRoute">
  <description>Something that this route do</description>
  <from uri="kafka:cheese"/>
  ...
</route>

<!-- After -->
<route id="myRoute" description="Something that this route do">
  <from uri="kafka:cheese"/>
  ...
</route>
```

Search for `<description>` elements inside `<route>`, `<from>`, and other EIP nodes. The `lang` attribute on `<description>` has also been removed.

### YAML DSL steps restructuring

If using YAML DSL routes, `steps` must now be nested under `from`:

```yaml
# Before
- route:
    from:
      uri: "direct:info"
    steps:
    - log: "message"

# After
- route:
    from:
      uri: "direct:info"
      steps:
      - log: "message"
```

### EIP changes

**InOnly/InOut EIPs removed:**
```xml
<!-- Before -->
<inOnly uri="direct:foo"/>

<!-- After -->
<to uri="direct:foo" pattern="InOnly"/>
```
Use `SetExchangePattern` or `To` with `pattern` attribute.

**CircuitBreaker Resilience4j configuration:**
```xml
<!-- Before -->
<circuitBreaker>
    <resilience4jConfiguration>
        <timeoutEnabled>true</timeoutEnabled>
        <timeoutDuration>2000</timeoutDuration>
    </resilience4jConfiguration>
</circuitBreaker>

<!-- After -->
<circuitBreaker>
    <resilience4jConfiguration timeoutEnabled="true" timeoutDuration="2000"/>
</circuitBreaker>
```

Affected options: `bulkheadEnabled`, `bulkheadMaxConcurrentCalls`, `bulkheadMaxWaitDuration`, `timeoutEnabled`, `timeoutExecutorService`, `timeoutDuration`, `timeoutCancelRunningFuture`.

### Micrometer metric name updates

If monitoring dashboards or alerts reference Camel metric names, update them:

| Old Name | New Name |
|---|---|
| CamelExchangeEventNotifier | camel.exchange.event.notifier |
| CamelExchangesFailed | camel.exchanges.failed |
| CamelExchangesFailuresHandled | camel.exchanges.failures.handled |
| CamelExchangesInflight | camel.exchanges.external.redeliveries |
| CamelExchangesSucceeded | camel.exchanges.succeeded |
| CamelExchangesTotal | camel.exchanges.total |
| CamelMessageHistory | camel.message.history |
| CamelRoutePolicy | camel.route.policy |
| CamelRoutePolicyLongTask | camel.route.policy.long.task |
| CamelRoutesAdded | camel.routes.added |
| CamelRoutesRunning | camel.routes.running |

### JMX changes

- `ManagedChoiceMBean.choiceStatistics()` → `extendedInformation()`
- `ManagedFailoverLoadBalancerMBean.exceptionStatistics()` → `extendedInformation()`
- `dumpRouteAsXml(boolean, boolean)` two-argument overload removed from `CamelContextMBean` and `CamelRouteMBean`
- `doCatch` and `doFinally` MBeans now included in the processor MBean tree

### camel-jbang CLI changes

- `camel dependencies` → `camel dependency`
- `-dir` → `--dir` (two dashes)
- `camel stop` now stops all integrations by default (`--all` option removed)
- Placeholder syntax changed from `$name` to `#name`

### Java 17 requirement

Verify the project builds and runs on Java 17. Update:
- Maven `maven.compiler.source`/`maven.compiler.target` to `17`
- Gradle `sourceCompatibility`/`targetCompatibility` to `JavaVersion.VERSION_17`
- CI/CD pipeline Java version
- Dockerfile base image to Java 17+

2. Run the build gate

## Build gate

Run `mvn compile` and verify the application starts. Check:
- XML DSL route files parse correctly (description attribute, CircuitBreaker config)
- YAML DSL routes parse correctly (steps under from)
- Monitoring dashboards show metrics under new names
- JMX access works with renamed MBeans
