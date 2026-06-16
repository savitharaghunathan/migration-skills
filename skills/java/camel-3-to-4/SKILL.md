---
name: camel-3-to-4
description: Migrates Apache Camel 3.x applications to Apache Camel 4.0. Use when
  upgrading a Camel project from version 3.x to 4.0.
license: Apache-2.0
metadata:
  source: camel-3
  target: camel-4
  language: java
  build_tool: "maven: mvn compile"
  guide_url: https://camel.apache.org/manual/camel-4-migration-guide.html
  generated_by: migration-skills-generator
  generated_at: 2026-06-16T00:00:00Z
---

# Apache Camel 3.x to 4.0 Migration

**Prerequisite:** Upgrade to Apache Camel 3.20 or later before starting this migration. Users on older 3.x releases should first complete the incremental 3.x upgrade steps.

Apache Camel 4.0 requires Java 17, removes 34 components (replacing most with modern alternatives), drops all JUnit 4 test support, upgrades slf4j to 2.0, and introduces significant API changes including the decoupling of ExtendedCamelContext and ExtendedExchange from their base interfaces. Component-specific changes affect camel-http (HttpComponents v5), camel-kubernetes (replace→update renames), camel-micrometer (metric name conventions), YAML DSL structure, XML DSL description syntax, and health check defaults. 123 migration items: 36 dependency, 40 API, 9 config, 38 pattern.

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build system** — Update dependencies: remove 34 deprecated components, replace with alternatives, migrate test modules to JUnit 5. Read `modules/build-system.md`.
2. **Code** — Replace renamed/moved/removed APIs and apply structural pattern changes. Read `modules/code.md`.
3. **Config** — Migrate renamed configuration properties and handle changed defaults. Read `modules/config.md`.
4. **Testing** — Migrate all test classes from JUnit 4 to JUnit 5, update CamelTestSupport usage. Read `modules/testing.md`.
5. **Additional** — Handle XML DSL changes, YAML DSL restructuring, JMX updates, and component-specific structural changes. Read `modules/additional.md`.
6. **Cleanup** — Verify no old APIs remain, check behavioral changes, final build. Read `modules/cleanup.md`.

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`)
2. Run the build
3. If it fails, fix the issue before proceeding
4. If you cannot fix it, stop and report to the user
