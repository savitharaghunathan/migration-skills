---
name: hibernate-5-to-6
description: Migrates Hibernate ORM 5.x applications to Hibernate ORM 6.x. Use when
  upgrading a Hibernate project from version 5.x to 6.0/6.1/6.2.
license: Apache-2.0
metadata:
  source: hibernate-5
  target: hibernate-6
  language: java
  build_tool: "maven: mvn compile"
  guide_url: https://docs.hibernate.org/orm/6.0/migration-guide/migration-guide.html
  generated_by: migration-skills-generator
  generated_at: 2026-06-16
---

# Hibernate ORM 5 to 6 Migration

**Prerequisite:** Upgrade to Hibernate ORM 5.6.x (latest patch) and resolve all deprecation warnings before starting the migration. Ensure your project runs on Java 11 or later (Java 17 recommended for Hibernate 6.2).

Hibernate ORM 6 is a major release that transitions from Java Persistence (`javax.persistence`) to Jakarta Persistence (`jakarta.persistence`), overhauls the type system (removing `@TypeDef`/`@TypeDefs` and string-based type references), replaces the legacy Criteria API with JPA Criteria, introduces the Semantic Query Model (SQM) for HQL/JPQL queries, changes default ID generation from a shared `hibernate_sequence` to per-entity sequences, changes default SQL type mappings for Duration/UUID/Instant/arrays/enums, simplifies dialect configuration (version-specific and spatial-specific dialect classes deprecated), and simplifies multitenancy configuration. This skill covers ORM 6.0, 6.1, and 6.2 changes (153 migration items: 8 dependency, 62 API, 32 config, 51 pattern).

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build System** — Update Hibernate artifact coordinates (`org.hibernate` → `org.hibernate.orm`) and Jakarta Persistence dependency. See `modules/build-system.md`.
2. **Code** — Migrate `javax.persistence` → `jakarta.persistence` imports, replace removed APIs (Criteria, @TypeDef, boolean types), update HQL queries, fix dialect references. See `modules/code.md`.
3. **Config** — Rename `hibernate.ejb.*` properties, update JPA settings to `jakarta.persistence.*`, handle default value changes for ID generation, type mappings, and timezone storage. See `modules/config.md`.
4. **Testing** — Update test configurations for Jakarta Persistence, verify query result type changes, test schema validation. See `modules/testing.md`.
5. **Additional** — Handle database schema changes (new sequences, column type migrations), persistence.xml namespace updates, import.sql fixes. See `modules/additional.md`.
6. **Cleanup** — Verify no old `javax.persistence`, `org.hibernate.criterion`, or version-specific dialect references remain. See `modules/cleanup.md`.

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`)
2. Run the build
3. If it fails, fix the issue before proceeding
4. If you cannot fix it, stop and report to the user
