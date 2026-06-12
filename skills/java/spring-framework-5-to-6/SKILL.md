---
name: spring-framework-5-to-6
description: Migrates Spring Framework 5.x applications to Spring Framework 6.x.
  Use when upgrading a Spring Framework project from version 5 to 6 (covering 6.0,
  6.1, and 6.2 release changes).
license: Apache-2.0
metadata:
  source: spring-framework-5
  target: spring-framework-6
  language: java
  build_tool: "maven: mvn compile"
  guide_url: https://github.com/spring-projects/spring-framework/wiki/Upgrading-to-Spring-Framework-6.x
  generated_by: migration-skills-generator
  generated_at: 2026-06-12T00:00:00Z
---

# Spring Framework 5 to 6 Migration

**Prerequisite:** Upgrade to the latest Spring Framework 5.3.x maintenance release and resolve all deprecation warnings before migrating to 6.0. Ensure your project compiles and tests pass on 5.3.x first.

**Prerequisite:** Java 17 or later is required. Spring Framework 6.0 raises the minimum to Java 17 and Jakarta EE 9+.

**Prerequisite:** Compile Java sources with the `-parameters` flag. Spring Framework 6.1 removes `LocalVariableTableParameterNameDiscoverer`; parameter names must come from reflection, not bytecode debugging info.

Spring Framework 6.x is a major generational upgrade introducing the Jakarta EE 9+ baseline (javax → jakarta namespace migration), Java 17 minimum, built-in Micrometer Observation instrumentation, AOT/GraalVM native image support, and numerous API removals including RPC-style remoting, Joda-Time, and EhCache 2. This skill covers breaking changes across 6.0, 6.1, and 6.2 releases, focusing on framework-level changes not already covered in the Spring Boot 2-to-3 migration skill. Key areas include core container changes (bean property determination, parameter name discovery, autowiring algorithm), web framework changes (trailing slash matching, HttpMethod class, controller validation), data access changes (JDBC exception translation, transaction behavior), and the complete property placeholder parser rewrite in 6.2.

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build System** — Update dependencies: remove deprecated libraries (Joda-Time, EhCache 2, RPC remoting), upgrade servlet containers, add Micrometer Observation dependency. See `modules/build-system.md`.
2. **Code** — Replace removed APIs (remoting, Tiles, CommonsMultipartResolver), migrate ListenableFuture to CompletableFuture, adapt HttpMethod enum→class usage, apply pattern changes (trailing slash, parameter names, validation). See `modules/code.md`.
3. **Config** — Migrate Spring system properties and META-INF/spring.factories entries. See `modules/config.md`.
4. **Testing** — Update Servlet mock API requirements, HtmlUnit migration, AOT test processing. See `modules/testing.md`.
5. **Additional** — Handle infrastructure changes: compiler flags, IDE configuration, servlet container upgrades. See `modules/additional.md`.
6. **Cleanup** — Remove compatibility shims, verify no old APIs remain, run final build and tests. See `modules/cleanup.md`.

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`, `go.mod` → `go build ./...`, `package.json` → `npm run build`, `pyproject.toml` → `python -m build`, `Makefile` → `make`)
2. Run the build
3. If it fails, fix the issue before proceeding
4. If you cannot fix it, stop and report to the user
