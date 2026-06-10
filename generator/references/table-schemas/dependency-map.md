# Dependency Map Schema

Each row represents a dependency (library, package, module) that changed between source and target versions.

## Columns

| Column | Required | Description |
|--------|----------|-------------|
| old_artifact | yes | Full coordinate. Java: `group:artifact` (e.g., `org.springframework.boot:spring-boot-starter-web`). Go: module path (e.g., `github.com/old/pkg`). Python: package name (e.g., `django-rest-framework`). .NET: package ID (e.g., `Microsoft.AspNetCore.Mvc`). |
| new_artifact | yes | Replacement coordinate, or `removed` if no replacement exists |
| version_constraint | no | Version where the change applies (e.g., `>= 4.0.0`) |
| action | yes | One of: `replace`, `remove`, `rename`, `merge` |
| notes | no | Brief migration context — gotchas, breaking changes, behavioral differences |
| source_section | yes | Guide section heading this row was extracted from |

## Action Definitions

- **replace** — Old artifact is replaced by a different one (different group/name)
- **remove** — Old artifact is removed with no replacement
- **rename** — Same artifact, new coordinates (group or name changed)
- **merge** — Multiple old artifacts consolidated into one new artifact

## Example Rows

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| org.springframework.boot:spring-boot-starter-web | org.springframework.boot:spring-boot-starter-web | >= 4.0.0 | replace | Jakarta EE 11 baseline; javax.* packages no longer supported | Core Changes |
| org.apache.httpcomponents:httpclient | org.apache.httpcomponents.client5:httpclient5 | >= 5.0 | replace | Package relocated from org.apache.http to org.apache.hc | Removed Libraries |
| com.example:deprecated-lib | removed | | remove | No replacement; functionality moved to core framework | Deprecations |
