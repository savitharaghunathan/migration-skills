---
name: spring-boot-3-to-4
description: Migrates Spring Boot 3.x applications to Spring Boot 4.0. Use when
  upgrading a Spring Boot project from version 3.5 to 4.0.
license: Apache-2.0
metadata:
  source: spring-boot-3
  target: spring-boot-4
  language: java
  build_tool: "maven: mvn compile / gradle: gradle build"
  guide_url: https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-4.0-Migration-Guide
  generated_by: migration-skills-generator
  generated_at: 2026-06-10T00:00:00Z
---

# Spring Boot 3 to 4 Migration

Spring Boot 4.0 is a major release requiring Java 17+, Jakarta EE 11, Servlet 6.1, and Spring Framework 7.x. Key migration concerns include: a complete modular redesign with new starter naming conventions, Jackson 3 adoption (group ID change from `com.fasterxml.jackson` to `tools.jackson`), removal of `@MockBean`/`@SpyBean` in favor of `@MockitoBean`/`@MockitoSpyBean`, Undertow support dropped, numerous configuration property renames (MongoDB, Session, Jackson), Elasticsearch Rest5Client migration, JSpecify nullability annotations replacing Spring's, and test infrastructure restructuring requiring per-technology test starters.

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build system** — Update dependencies, starters, and modules (`modules/build-system.md`)
2. **Code** — Replace renamed, moved, and removed APIs (`modules/code.md`)
3. **Config** — Migrate configuration properties (`modules/config.md`)
4. **Testing** — Update test annotations, frameworks, and patterns (`modules/testing.md`)
5. **Additional** — Handle Jackson 3 migration, infrastructure, and non-code changes (`modules/additional.md`)
6. **Cleanup** — Verify no old artifacts remain and run full test suite (`modules/cleanup.md`)

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`)
2. Run the build
3. If it fails, fix the issue before proceeding
4. If you cannot fix it, stop and report to the user
