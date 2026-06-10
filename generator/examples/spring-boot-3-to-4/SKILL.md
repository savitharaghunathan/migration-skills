---
name: spring-boot-3-to-4
description: Migrates Spring Boot 3.x applications to Spring Boot 4.0.
  Use when upgrading a Spring Boot project from version 3 to version 4.
license: Apache-2.0
metadata:
  source: spring-boot-3
  target: spring-boot-4
  language: java
  build_tool: "maven: mvn compile"
  guide_url: https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-4.0-Migration-Guide
  generated_by: migration-skills-generator
  generated_at: "2026-06-10T00:00:00Z"
---

# Spring Boot 3 to 4 Migration

Spring Boot 4.0 requires Java 21+, adopts Jakarta EE 11, removes several deprecated APIs, and changes several defaults. This skill guides a phased migration with build verification between each step.

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build system** — Update dependencies and plugins
2. **Code** — Replace renamed/removed APIs
3. **Config** — Migrate configuration properties
4. **Testing** — Update test annotations and frameworks
5. **Cleanup** — Remove compatibility shims, verify final build

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`)
2. Run the build
3. If it fails, fix the issue before proceeding
4. If you cannot fix it, stop and report to the user
