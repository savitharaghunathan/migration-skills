---
name: jdk-21-to-25
description: Migrates JDK 21 applications to JDK 25. Use when upgrading a Java
  project from JDK 21 to JDK 25.
license: Apache-2.0
metadata:
  source: jdk-21
  target: jdk-25
  language: java
  build_tool: "maven: mvn compile / gradle: gradle build"
  guide_url: https://docs.oracle.com/en/java/javase/25/migrate/significant-changes-jdk-release.html
  generated_by: migration-skills-generator
  generated_at: 2026-06-10T00:00:00Z
---

# JDK 21 to 25 Migration

JDK 25 is the next LTS release after JDK 21, incorporating changes from JDK 22, 23, 24, and 25. Key migration concerns include: the permanent disabling of the Security Manager (JEP 486), deprecation of `sun.misc.Unsafe` memory-access methods (JEP 471/498), removal of ZGC non-generational mode, removal of legacy thread control methods (`Thread.suspend/resume`, `ThreadGroup.stop`), removal of several JVM command-line options, Compact Object Headers enabled by default, and various security library updates including quantum-resistant algorithms. The withdrawal of String Templates (previewed in JDK 21-22) also affects projects that adopted that preview feature.

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build system** — Update dependencies and modules (`modules/build-system.md`)
2. **Code** — Replace removed, deprecated, and moved APIs (`modules/code.md`)
3. **Config** — Migrate system properties and configuration (`modules/config.md`)
4. **Additional** — Handle JVM options, runtime flags, tools, and infrastructure changes (`modules/additional.md`)
5. **Cleanup** — Verify no old artifacts remain and run full test suite (`modules/cleanup.md`)

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`, `go.mod` → `go build ./...`, `package.json` → `npm run build`, `pyproject.toml` → `python -m build`, `Makefile` → `make`)
2. Run the build
3. If it fails, fix the issue before proceeding
4. If you cannot fix it, stop and report to the user
