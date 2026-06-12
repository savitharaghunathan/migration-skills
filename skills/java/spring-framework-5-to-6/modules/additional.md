# Phase: Additional Changes

Handle infrastructure, runtime, and non-code changes specific to the Spring Framework 5→6 migration.

## Steps

1. Read `references/pattern-map.md` — filter for rows with category `addition` or infrastructure-related `source_section` values
2. For each relevant row:

### Java 17 runtime
- Verify the project runs on Java 17+
- Update Dockerfiles, CI/CD pipelines, and deployment configurations to use JDK 17+ base images
- Update `source`/`target` compiler settings to 17 in `pom.xml` or `build.gradle`

### Servlet container upgrade
- **Tomcat:** Upgrade to 10+ (Jakarta EE 9) or 10.1+ (Jakarta EE 10)
- **Jetty:** Upgrade to 11+ (Jakarta EE 9)
- **Undertow:** Upgrade to 2.2.19+ with `undertow-servlet-jakarta` artifact, or 2.3+ (Jakarta EE 10)
- Update container configuration files (server.xml, jetty.xml, etc.)

### Compiler configuration
- Ensure `-parameters` flag is configured in build tool (handled in build-system phase)
- Verify IDE configuration: IntelliJ, Eclipse, VS Code all need explicit configuration for `-parameters`
- For Kotlin: use `-java-parameters` flag
- For Groovy: set `groovyOptions.parameters = true` on `GroovyCompile` tasks

### Micrometer Observation
- `spring-web` module now requires `io.micrometer:micrometer-observation:1.10+` at compile time
- `RestTemplate`, `WebClient`, Spring MVC, and Spring WebFlux have built-in observation instrumentation
- If using custom Micrometer metrics, review for compatibility with the observation API

### ORM/JPA provider upgrade
- Upgrade to Hibernate ORM 5.6.x with `hibernate-core-jakarta` artifact (Jakarta EE 9), or Hibernate ORM 6.1 (native jakarta.persistence)
- Hibernate Validator 7.0+ (jakarta.validation based), or 8.0 (Jakarta EE 10)
- EclipseLink 3.0+ (Jakarta EE 9), or 4.0 (Jakarta EE 10)

3. Run the build gate

## Build gate

Run the project build command. For infrastructure changes, also verify:
- Application starts successfully on the new servlet container
- JDK 17 compatibility (no removed/deprecated JDK APIs in use)
- ORM provider initializes correctly with new Jakarta persistence API
