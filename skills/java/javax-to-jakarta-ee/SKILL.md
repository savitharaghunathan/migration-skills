---
name: javax-to-jakarta-ee
description: Migrates Java EE 8 (javax) applications to Jakarta EE 9/10 (jakarta).
  Use when upgrading any Java EE application from the javax namespace to the jakarta
  namespace, independent of application server or framework.
license: Apache-2.0
metadata:
  source: java-ee-8
  target: jakarta-ee-10
  language: java
  build_tool: "maven: mvn compile"
  guide_url: https://github.com/jakartaee/jakartaee-platform/blob/main/namespace/mappings.adoc
  generated_by: migration-skills-generator
  generated_at: 2026-06-16
---

# Java EE 8 to Jakarta EE 10 Migration

**Prerequisite:** Ensure your application compiles and runs correctly on Java EE 8 before starting. Resolve all existing compilation errors and deprecation warnings. Java 11 is the minimum JDK for Jakarta EE 10 (Java 17 recommended).

The javax-to-jakarta namespace migration is the most significant breaking change in Java enterprise development. Jakarta EE 9 renamed all Enterprise Edition packages from `javax.*` to `jakarta.*` (Java SE `javax.*` packages like `javax.sql`, `javax.crypto`, `javax.naming` are NOT affected). Jakarta EE 10 builds on this with feature upgrades (Servlet 6.0, CDI 4.0, JPA 3.1, JAX-RS 3.1, Faces 4.0). This skill covers dependency coordinate changes, all 39 package-level namespace renames, XML namespace updates for deployment descriptors, ServiceLoader file renames, and the 3 removed specifications (XML Registries, XML RPC, Deployment). 126 migration items: 35 dependency, 39 API, 23 config, 29 pattern.

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build System** — Replace all `javax.*` Maven/Gradle dependency coordinates with `jakarta.*` equivalents. See `modules/build-system.md`.
2. **Code** — Rename all `javax.*` EE imports to `jakarta.*`, preserving Java SE `javax.*` packages unchanged. See `modules/code.md`.
3. **Config** — Rename `javax.persistence.*` properties to `jakarta.persistence.*` in persistence.xml and other config files. See `modules/config.md`.
4. **Testing** — Update test code imports and test configuration files. See `modules/testing.md`.
5. **Additional** — Update XML namespace URIs in deployment descriptors, rename ServiceLoader files, handle Facelets/JSTL namespace changes. See `modules/additional.md`.
6. **Cleanup** — Verify no `javax.*` EE references remain; check for mixed javax/jakarta dependencies. See `modules/cleanup.md`.

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`)
2. Run the build
3. If it fails, fix the issue before proceeding
4. If you cannot fix it, stop and report to the user
