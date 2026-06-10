# Phase: Additional Changes

Handle Jackson 3 migration, infrastructure, and non-code changes.

## Steps

### Jackson 3 Migration

1. Read `references/pattern-map.md` — filter for rows where `source_section` relates to Jackson
2. Update all Jackson imports:
   - `com.fasterxml.jackson.databind.*` → `tools.jackson.databind.*`
   - `com.fasterxml.jackson.core.*` → `tools.jackson.core.*`
   - `com.fasterxml.jackson.datatype.*` → `tools.jackson.datatype.*`
   - Exception: `com.fasterxml.jackson.annotation` package stays unchanged
3. If unable to migrate to Jackson 3 immediately:
   - Add `spring-boot-jackson2` dependency as stop-gap
   - Configure via `spring.jackson2.*` properties
   - Plan to migrate eventually (module is deprecated)
4. For Jersey applications: add `spring-boot-jackson2` (Jersey 4.0 does not support Jackson 3)

### Spring Batch

5. If using Spring Batch with database metadata storage:
   - Switch from `spring-boot-starter-batch` to `spring-boot-starter-batch-jdbc`
   - Without this change, Batch will use in-memory mode

### WAR Deployments

6. If deploying WAR to Tomcat:
   - Change `spring-boot-starter-tomcat` to `spring-boot-starter-tomcat-runtime`
7. If using `server.forward-headers-strategy=framework` in WAR deployment:
   - Define `ForwardedHeaderFilter` bean manually

### Build Plugin Configuration

8. Remove embedded launch script configuration (`<executable>true</executable>`)
9. Remove classic loader configuration (`<loaderImplementation>CLASSIC</loaderImplementation>`)
10. Update CycloneDX Gradle plugin to >= 3.0.0 if used
11. If using Maven: optional dependencies are no longer in uber jars; add `<includeOptional>true</includeOptional>` if needed

### Elasticsearch

12. Update Elasticsearch client customization from `RestClientBuilderCustomizer` to `Rest5ClientBuilderCustomizer`
13. Remove `elasticsearch-rest-client` and `elasticsearch-rest-client-sniffer` dependencies

### Related Spring Projects

14. Review release notes for related projects your application uses:
    - Spring Framework 7.0, Spring Security 7.0, Spring Data 2025.1
    - Spring Batch 6.0, Spring AMQP 4.0, Spring Kafka 4.0
    - Spring GraphQL 2.0, Spring Integration 7.0, Spring Session 4.0

15. Run the build gate

## Build gate

Run the project build command. For infrastructure changes, also verify the application starts successfully.
