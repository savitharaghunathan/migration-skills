# Phase: Cleanup and Verification

Verify no old APIs remain, check behavioral changes, and run the final build.

## Steps

### 1. Remove old package imports

Search for any remaining imports of old Camel 3 APIs:

```bash
grep -rn "import org.apache.camel.builder.SimpleBuilder" --include="*.java" src/
grep -rn "import org.apache.camel.FallbackConverter" --include="*.java" src/
grep -rn "import org.apache.camel.spi.OnCamelContextStart\b" --include="*.java" src/
grep -rn "import org.apache.camel.spi.OnCamelContextStop\b" --include="*.java" src/
grep -rn "import org.apache.camel.api.management.mbean.BacklogTracerEventMessage" --include="*.java" src/
grep -rn "import org.apache.abdera" --include="*.java" src/
grep -rn "import twitter4j\.Status\b" --include="*.java" src/
```

Any remaining references indicate missed transformations — go back and fix them.

### 2. Verify no old artifacts in build files

Search `pom.xml`/`build.gradle` for removed component coordinates:

```bash
grep -rn "camel-directvm\|camel-vm\|camel-websocket\b\|camel-opentracing\|camel-zipkin\|camel-swagger\|camel-xstream\|camel-johnzon\|camel-rabbitmq\b\|camel-dozer\|camel-cdi\b\|camel-test\b" pom.xml build.gradle 2>/dev/null
```

### 3. Verify no old API patterns

Search for deprecated method patterns:

```bash
grep -rn "\.adapt(" --include="*.java" src/
grep -rn "\.getEndpointMap()" --include="*.java" src/
grep -rn "\.asyncCallback(" --include="*.java" src/
grep -rn "\.getExtension(" --include="*.java" src/
grep -rn "InOptionalOut" --include="*.java" src/
grep -rn "uri\s*=" --include="*.java" src/ | grep -E "@(EndpointInject|Produce|Consume)"
grep -rn "replaceConfigMap\|replacePod\|REPLACE_" --include="*.java" src/
```

### 4. Verify no old configuration properties

```bash
grep -rn "camel\.health\.components-enabled" --include="*.properties" --include="*.yml" .
grep -rn "socket-timeout\|socketTimeout" --include="*.properties" --include="*.yml" . | grep -i camel
```

### 5. Check behavioral changes requiring manual verification

The following behavioral changes from `pattern-map.md` cannot be verified automatically — flag them for the user:

- **Health checks are now readiness-only by default.** If your app depends on liveness health checks, verify `CamelContextCheck` provides both.
- **Producer health checks disabled by default.** If using AWS components or camel-kafka with producer health checks, enable explicitly.
- **Backlog tracing auto-enables on startup** when `backlogTracing=true`. If you relied on manual enablement, switch to `backlogTracingStandby=true`.
- **Backlog Tracer now traces InputStream headers.** If reading headers after tracing, streams may be at end-of-stream.
- **UseOriginalMessage/UseOriginalBody now copies body to StreamCache.** This is defensive but changes behavior if code assumed the body was not cached.
- **Suspended routes return 503 instead of 404** on platform-http and platform-http-vertx.
- **Camel shutdown order changed** — Camel now shuts down after Spring Boot's graceful shutdown completes.
- **XML serialization uses camel-xml-io instead of JAXB.** If relying on JAXB-specific behavior in XML dumps, verify output.
- **Poll Enrich endpoint URI is now an Exchange property** instead of a message header.
- **OpenAPI Maven Plugin defaults to platform-http** instead of servlet as rest component.
- **camel-file readLock=changed with readLockMinAge** restored to 3.x behavior in 4.0.2; old files on startup picked up immediately.

### 6. Final build and test

- Run the full build: `mvn clean verify`
- Run integration tests if available
- Verify the application starts successfully on Java 17
- Test key Camel routes end-to-end

### 7. Report to user

- List all changes made across all phases
- Flag any removed components that had no replacement (camel-any23, camel-atmos, camel-corda, camel-gora, camel-hbase, etc.)
- Note behavioral changes listed above for manual verification
- Report any component-specific SDK upgrades (camel-box SDK v4, camel-http HttpComponents v5, camel-google SDK v2, camel-web3j 5.0) that may require additional API adjustments beyond what the mapping tables cover
