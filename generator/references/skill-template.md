---
name: {{migration_name}}
description: Migrates {{source_framework}} {{source_version}} applications to
  {{target_framework}} {{target_version}}. Use when upgrading a {{framework}}
  project from version {{source_version}} to {{target_version}}.
license: Apache-2.0
metadata:
  source: {{source}}
  target: {{target}}
  language: {{language}}
  build_tool: "{{build_tool}}"
  smoke_tool: "{{smoke_tool}}"
  guide_url: {{guide_urls}}
  generated_by: migration-skills-generator
  generated_at: {{timestamp}}
---

# {{source_framework}} {{source_version}} to {{target_version}} Migration

{{prerequisites}}

{{migration_summary}}

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

{{phase_list}}

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`, `go.mod` → `go build ./...`, `package.json` → `npm run build`, `pyproject.toml` → `python -m build`, `Makefile` → `make`)
2. Run the build
3. If it fails, consult `references/verify-errors.md` for known error-fix mappings before attempting ad-hoc fixes
4. If you cannot fix it, stop and report to the user

## Smoke gate

After the final phase completes and the build gate passes:
1. Run the smoke command from metadata `smoke_tool` field above (if present)
2. The smoke command starts the application and verifies runtime wiring — dependency injection, driver loading, and framework initialization
3. If it fails, consult `references/verify-errors.md` for known error-fix mappings
4. Apply the same fix loop as the build gate (fix, re-run, max iterations)
5. If you cannot fix it, stop and report to the user
