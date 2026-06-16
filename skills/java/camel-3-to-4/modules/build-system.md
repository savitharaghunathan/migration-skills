# Phase: Build System Migration

Update dependencies and build plugins before touching application code.

## Steps

1. Read `references/dependency-map.md`
2. Open the project's build file(s):
   - Maven: `pom.xml`
   - Gradle: `build.gradle` or `build.gradle.kts`
3. For each row in the dependency map:
   - Search the build file for `old_artifact`
   - Apply the `action`:
     - `replace` → change the artifact coordinate to `new_artifact`
     - `remove` → delete the dependency entry
     - `rename` → update the coordinate (same dependency, new name)
     - `merge` → replace with the consolidated `new_artifact`, remove duplicates
   - If `version_constraint` is specified, update the version accordingly
   - Check `notes` for gotchas before moving on

### Key dependency changes

**Removed components with no replacement** (delete these dependencies entirely):
- `camel-any23`, `camel-atlasmap`, `camel-atmos`, `camel-corda`
- `camel-gora`, `camel-hbase`, `camel-hyperledger-aries`, `camel-iota`
- `camel-ipfs`, `camel-jbpm`, `camel-jclouds`, `camel-spark`
- `camel-spring-integration`, `camel-weka`

**Swagger → OpenAPI renames:**
- `camel-swagger-java` → `camel-openapi-java`
- `camel-rest-swagger` → `camel-openapi-rest`
- `camel-restdsl-swagger-plugin` → `camel-restdsl-openapi-plugin`

**Observability consolidation:**
- `camel-opentracing` → `camel-opentelemetry` or `camel-micrometer`
- `camel-zipkin` → `camel-opentelemetry` or `camel-micrometer`
- `camel-microprofile-metrics` → `camel-micrometer` or `camel-opentelemetry`

**Communication protocol replacements:**
- `camel-websocket` and `camel-websocket-jsr356` → `camel-vertx-websocket`
- `camel-rabbitmq` → `camel-spring-rabbitmq`
- `camel-directvm` → `camel-direct`
- `camel-vm` → `camel-seda`
- `camel-vertx-kafka` → `camel-kafka`

**Serialization replacements:**
- `camel-johnzon` → `camel-jackson` (alternatives: camel-fastjson, camel-gson)
- `camel-xstream` → `camel-jacksonxml`

**Test modules:**
- All JUnit 4 `camel-test` modules → `camel-test-junit5`

**Spring Boot integration:**
- `camel-spring-boot-starter` no longer includes `camel-spring-xml`; add `camel-spring-boot-xml-starter` if using Spring XML `<beans>` files

4. Run the build gate

## Build gate

Run `mvn compile` (or `gradle build`). If it fails, fix dependency issues before proceeding. Common issues:
- Version conflicts between updated and non-updated dependencies
- Transitive dependency breakage from removed components
- Missing camel-spring-boot-xml-starter when using Spring XML files
