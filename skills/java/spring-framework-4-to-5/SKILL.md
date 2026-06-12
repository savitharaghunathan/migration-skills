---
name: spring-framework-4-to-5
description: Migrates Spring Framework 4.x applications to Spring Framework 5.x.
  Use when upgrading a Spring Framework project from version 4 to 5.
license: Apache-2.0
metadata:
  source: spring-framework-4
  target: spring-framework-5
  language: java
  build_tool: "maven: mvn compile"
  guide_url:
    - https://github.com/spring-projects/spring-framework/wiki/Upgrading-to-Spring-Framework-5.x
    - https://github.com/spring-projects/spring-framework/wiki/Spring-Framework-5.0-Release-Notes
    - https://github.com/spring-projects/spring-framework/wiki/Spring-Framework-5.1-Release-Notes
    - https://github.com/spring-projects/spring-framework/wiki/Spring-Framework-5.2-Release-Notes
    - https://github.com/spring-projects/spring-framework/wiki/Spring-Framework-5.3-Release-Notes
  generated_by: migration-skills-generator
  generated_at: 2026-06-12T00:00:00Z
---

# Spring Framework 4 to 5 Migration

**Prerequisite:** Upgrade to the latest Spring Framework 4.3.x release and resolve all deprecation warnings before starting this migration. Ensure your application compiles and runs on JDK 8+.

**Prerequisite:** Upgrade Hibernate to 5.0+ (if using ORM) — the `orm.hibernate3` and `orm.hibernate4` packages are removed in Spring 5.0.

**Prerequisite:** Upgrade servlet container to Tomcat 8.5+, Jetty 9.4+, WildFly 10+, or WebSphere 9+.

Spring Framework 5.x is a major upgrade establishing a JDK 8+ and Java EE 7+ baseline. Key changes include: removal of Portlet, Velocity, JasperReports, XMLBeans, and JDO support; replacement of Guava caching with Caffeine; the new `spring-jcl` commons logging bridge; introduction of the reactive `spring-webflux` module and `WebClient`; CORS behavioral changes; JUnit 5 Jupiter support; and progressive refinements across 5.1 (JDK 11 support, forwarded header handling), 5.2 (annotation retrieval revision, `@Configuration` lite mode, suffix pattern deprecation), and 5.3 (R2DBC, remoting deprecation, `@Nested` test inheritance). This skill covers 107 migration items across all four minor releases (5.0, 5.1, 5.2, 5.3).

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build system** — Update dependencies: remove dropped libraries (Portlet, Velocity, JasperReports, XMLBeans, JDO, Guava caching), upgrade Hibernate/Tiles/Jackson, replace commons-logging with spring-jcl. Module: `modules/build-system.md`, Reference: `references/dependency-map.md`

2. **Code** — Replace removed and relocated APIs: Hibernate 3/4→5 packages, Tiles 2→3, Velocity→FreeMarker, Guava→Caffeine caching, AsyncRestTemplate→WebClient, FormTag commandName→modelAttribute. Module: `modules/code.md`, References: `references/api-map.md`, `references/pattern-map.md`

3. **Config** — Migrate configuration: CORS allowCredentials, XML namespace versions, suffix pattern matching, WebFlux maxInMemorySize, test properties. Module: `modules/config.md`, Reference: `references/config-map.md`

4. **Testing** — Update test infrastructure: JUnit 5 SpringExtension, mock JNDI replacement, `@Nested` test annotation inheritance, MockMvc Kotlin DSL changes. Module: `modules/testing.md`

5. **Additional** — Runtime and infrastructure: JDK 8+/11 baseline, servlet container upgrades, WebFlux reactive stack, R2DBC module, reactive transactions. Module: `modules/additional.md`

6. **Cleanup** — Remove old imports, verify no legacy artifacts remain, validate behavioral changes. Module: `modules/cleanup.md`

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`)
2. Run the build
3. If it fails, fix the issue before proceeding
4. If you cannot fix it, stop and report to the user
