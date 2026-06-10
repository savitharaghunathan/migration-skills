# API Map Schema

Each row represents an API element (class, annotation, method, etc.) that was renamed, moved, or removed.

## Columns

| Column | Required | Description |
|--------|----------|-------------|
| old_api | yes | Fully qualified name (e.g., `org.springframework.lang.Nullable`) |
| new_api | yes | Replacement FQN, or `removed` if no replacement exists |
| kind | yes | One of: `package`, `class`, `annotation`, `interface`, `enum`, `method`, `field` |
| action | yes | One of: `replace`, `remove`, `rename`, `move_package` |
| before | recommended | Short code snippet showing old usage (1-3 lines) |
| after | recommended | Short code snippet showing new usage (1-3 lines) |
| notes | no | Context, gotchas, edge cases |
| source_section | yes | Guide section heading |

## Processing Order

When the generated migration skill is executed, api-map rows are applied in `kind` order to prevent conflicts:

`package` → `class` → `annotation` → `interface` → `enum` → `method` → `field`

Package-level renames are applied before individual class references. Class-level changes before method-level ones.

## Action Definitions

- **replace** — Old API replaced by a different one (different name or signature)
- **remove** — Old API removed with no replacement
- **rename** — Same API, new name (simple rename within same package)
- **move_package** — Same API name, moved to a different package

## Example Rows

| old_api | new_api | kind | action | before | after | notes | source_section |
|---|---|---|---|---|---|---|---|
| org.springframework.lang.Nullable | org.jspecify.annotations.Nullable | annotation | replace | `@Nullable String param` | `@Nullable String param` | Import change only; usage identical | Nullable Annotations |
| javax.servlet.http.HttpServletRequest | jakarta.servlet.http.HttpServletRequest | class | move_package | `import javax.servlet.http.HttpServletRequest;` | `import jakarta.servlet.http.HttpServletRequest;` | Part of Jakarta EE migration | Servlet API |
| org.springframework.boot.web.server.LocalServerPort | removed | annotation | remove | `@LocalServerPort int port;` | `@Value("${local.server.port}") int port;` | Use @Value injection instead | Removed Annotations |
