---
name: spring-boot-2-to-3
description: Migrates Spring Boot 2.x applications to Spring Boot 3.0. Use when
  upgrading a Spring Boot project from version 2.7 to 3.0.
license: Apache-2.0
metadata:
  source: spring-boot-2
  target: spring-boot-3
  language: java
  build_tool: "maven: mvn compile / gradle: gradle build"
  guide_url:
    - https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Migration-Guide
    - https://docs.hibernate.org/orm/6.0/migration-guide/migration-guide.html
    - https://docs.hibernate.org/orm/6.1/migration-guide/migration-guide.html
    - https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Configuration-Changelog
  generated_by: migration-skills-generator
  generated_at: 2026-06-12T00:00:00Z
---

# Spring Boot 2 to 3 Migration

**Prerequisite:** Before migrating to Spring Boot 3.0, upgrade your project to the latest Spring Boot 2.7.x maintenance release first. Resolve all deprecation warnings on 2.7 — classes, methods, and properties deprecated in 2.x are removed in 3.0. This skill assumes you are starting from Spring Boot 2.7.

**Prerequisite:** Spring Boot 3.0 requires Java 17 or later. Ensure your project compiles and runs on Java 17+ before starting this migration.

Spring Boot 3.0 is a major release requiring Java 17+, Jakarta EE 10, Spring Framework 6.0, and Spring Security 6.0. Key migration concerns include: the javax-to-jakarta namespace migration across all Jakarta EE APIs (Servlet 6.0, JPA 3.1, Bean Validation 3.0), Hibernate ORM 6.0/6.1 (new type system, sequence-per-entity, HQL/Criteria changes, type descriptor renames), Spring Security 6.0 (WebSecurityConfigurerAdapter removal, requestMatchers replacing antMatchers/mvcMatchers, authorization on all dispatch types), Micrometer 1.10 observation-based metrics replacing filter-based instrumentation, actuator endpoint renames (httptrace → httpexchanges), 100+ configuration property renames/removals (Redis, Cassandra, Elasticsearch, metrics export restructuring), Flyway 9.0, auto-configuration registration file change, MySQL JDBC driver coordinate change, and removal of support for ActiveMQ, Atomikos, EhCache 2, Solr, and image banners.

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build system** — Update dependencies, starters, and build plugins (`modules/build-system.md`)
2. **Code** — Replace renamed, moved, and removed APIs; apply structural pattern changes (`modules/code.md`)
3. **Config** — Migrate configuration properties (`modules/config.md`)
4. **Testing** — Update test annotations, frameworks, and patterns (`modules/testing.md`)
5. **Additional** — Handle Hibernate schema changes, Flyway migration, Gradle/Maven changes (`modules/additional.md`)
6. **Cleanup** — Verify no old artifacts remain and run full test suite (`modules/cleanup.md`)

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`)
2. Run the build
3. If it fails, fix the issue before proceeding
4. If you cannot fix it, stop and report to the user
