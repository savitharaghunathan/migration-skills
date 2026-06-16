# Phase: Build System Migration

Replace all Java EE dependency coordinates with Jakarta EE equivalents.

## Steps

1. Read `references/dependency-map.md`
2. Open the project's build file(s): `pom.xml` or `build.gradle`/`build.gradle.kts`
3. For each row in the dependency map, search the build file for `old_artifact` and apply the action

### Umbrella API

If using the Java EE umbrella API, replace it:

```xml
<!-- Before -->
<dependency>
    <groupId>javax</groupId>
    <artifactId>javaee-api</artifactId>
    <version>8.0</version>
    <scope>provided</scope>
</dependency>

<!-- After -->
<dependency>
    <groupId>jakarta.platform</groupId>
    <artifactId>jakarta.jakartaee-api</artifactId>
    <version>10.0.0</version>
    <scope>provided</scope>
</dependency>
```

For Web Profile only:

```xml
<!-- Before -->
<dependency>
    <groupId>javax</groupId>
    <artifactId>javaee-web-api</artifactId>
    <version>8.0</version>
    <scope>provided</scope>
</dependency>

<!-- After -->
<dependency>
    <groupId>jakarta.platform</groupId>
    <artifactId>jakarta.jakartaee-web-api</artifactId>
    <version>10.0.0</version>
    <scope>provided</scope>
</dependency>
```

### Individual API dependencies

If using individual API JARs instead of the umbrella, replace each one. Key examples:

| Old | New |
|---|---|
| `javax.servlet:javax.servlet-api` | `jakarta.servlet:jakarta.servlet-api:6.0.0` |
| `javax.persistence:javax.persistence-api` | `jakarta.persistence:jakarta.persistence-api:3.1.0` |
| `javax.ws.rs:javax.ws.rs-api` | `jakarta.ws.rs:jakarta.ws.rs-api:3.1.0` |
| `javax.enterprise:cdi-api` | `jakarta.enterprise:jakarta.enterprise.cdi-api:4.0.1` |
| `javax.inject:javax.inject` | `jakarta.inject:jakarta.inject-api:2.0.1` |
| `javax.annotation:javax.annotation-api` | `jakarta.annotation:jakarta.annotation-api:2.1.1` |
| `javax.validation:javax.validation-api` | `jakarta.validation:jakarta.validation-api:3.0.2` |
| `javax.xml.bind:jaxb-api` | `jakarta.xml.bind:jakarta.xml.bind-api:4.0.0` |

See `references/dependency-map.md` for the complete list of 35 coordinate changes.

### Removed specifications

Remove dependencies for APIs dropped from the platform:
- `javax.xml.registry:javax.xml.registry-api` — XML Registries (JAXR)
- `javax.xml.rpc:javax.xml.rpc-api` — XML RPC
- `javax.enterprise.deploy:javax.enterprise.deploy-api` — Deployment

### Third-party libraries

Check if third-party libraries have Jakarta EE compatible versions:
- **Hibernate** — 6.x uses `jakarta.persistence`
- **Jersey** — 3.x uses `jakarta.ws.rs`
- **Mojarra** — 4.x uses `jakarta.faces`
- **EclipseLink** — 4.x uses `jakarta.persistence`
- **Weld** — 5.x uses `jakarta.enterprise`

For libraries without Jakarta-compatible versions, consider using Eclipse Transformer at build time.

4. Run the build gate

## Build gate

Run `mvn compile` (or `gradle build`). Common issues:
- Version conflicts between javax and jakarta dependencies (do not mix)
- Transitive dependencies pulling in old javax API JARs
- Missing Jakarta-compatible versions of third-party libraries
