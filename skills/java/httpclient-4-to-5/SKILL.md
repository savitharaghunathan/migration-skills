---
name: httpclient-4-to-5
description: Migrates Apache HttpClient 4.x applications to HttpClient 5.x.
  Use when upgrading from the org.apache.httpcomponents:httpclient artifact to
  org.apache.httpcomponents.client5:httpclient5.
license: Apache-2.0
metadata:
  source: httpclient-4
  target: httpclient-5
  language: java
  build_tool: "maven: mvn compile"
  guide_url: https://hc.apache.org/httpcomponents-client-5.6.x/migration-guide/index.html
  generated_by: migration-skills-generator
  generated_at: 2026-06-12T00:00:00Z
---

# Apache HttpClient 4.x to 5.x Migration

Migrates Apache HttpClient 4.x to HttpClient 5.x. The 5.x release changes Maven coordinates (org.apache.httpcomponents → org.apache.httpcomponents.client5), relocates all classes from the org.apache.http package to org.apache.hc.client5.http and org.apache.hc.core5.http, replaces integer timeout parameters with Timeout/TimeValue objects, moves SSL/TLS configuration from SSLConnectionSocketFactory to TLS strategy builders, moves connect/socket timeouts from RequestConfig to ConnectionConfig, removes the status line from HTTP responses, and introduces async APIs with Future-based execution. The two versions can coexist on the same classpath for gradual migration.

## Phases

Execute in order. After each phase, run the project build and stop if it fails.

1. **Build system** — `modules/build-system.md` — Replace HttpClient 4.x Maven coordinates with 5.x equivalents
2. **Code** — `modules/code.md` — Update package imports, replace renamed/moved APIs, update timeout and TLS configuration patterns
3. **Additional** — `modules/additional.md` — Optional migration to async APIs (simple handlers, streaming handlers, HTTP/2)
4. **Cleanup** — `modules/cleanup.md` — Remove remaining org.apache.http imports, verify no old artifacts remain, final build

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the codebase. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Detect the project's build tool (check metadata `build_tool` field above, or detect from project files: `pom.xml` → `mvn compile`, `build.gradle` → `gradle build`, `go.mod` → `go build ./...`, `package.json` → `npm run build`, `pyproject.toml` → `python -m build`, `Makefile` → `make`)
2. Run the build
3. If it fails, fix the issue before proceeding
4. If you cannot fix it, stop and report to the user
