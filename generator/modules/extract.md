# Phase 2: Extract

Read the normalized guide and fill mapping tables.

## Input

The normalized guide from Phase 1 and its detected metadata.

## Before you start

Read the few-shot example tables to calibrate your extraction:
- Read `generator/examples/spring-boot-3-to-4/references/dependency-map.md`
- Read `generator/examples/spring-boot-3-to-4/references/api-map.md`
- Read `generator/examples/spring-boot-3-to-4/references/config-map.md`

If multiple examples exist and one matches the target language, prefer that one.

Also read the table schemas for column definitions:
- Read `generator/references/table-schemas/dependency-map.md`
- Read `generator/references/table-schemas/api-map.md`
- Read `generator/references/table-schemas/config-map.md`
- Read `generator/references/table-schemas/pattern-map.md`
- Read `generator/references/table-schemas/verify-errors.md`

## Chunking strategy

If the guide is over ~200 lines, process it in batches of 5-10 top-level sections (`##` headings). Each batch appends rows to the same accumulating tables.

**Important:** When a section explicitly references an adjacent section (e.g., "Removed X" immediately followed by "X Replacement Options"), include both sections in the same batch. Breaking related sections across batches loses relationship context.

## Steps

For each section (or batch of sections) in the guide:

### 1. Identify all actionable changes

A single section may contain multiple change types. For each artifact or change described, classify it:

- **dependency** — An artifact (library, module, package) is added, removed, renamed, or version-constrained. Route to `dependency-map.md`.
- **api** — A class, annotation, method, interface, or package is renamed, moved, or removed. Route to `api-map.md`.
- **config** — A configuration property is renamed, removed, or has a changed default value. Route to `config-map.md`.
- **pattern** — A code pattern change that isn't a simple rename: behavioral change, structural refactor, new idiom. Route to `pattern-map.md`.
- **additional** — Changes outside standard categories: database schemas, deployment config, build plugin config, infrastructure. Route to `pattern-map.md` with appropriate `category` value.
- **error** — A troubleshooting tip, known issue, common failure mode, or "if you see X, do Y" guidance. Route to `verify-errors.md`. Look for these in "Troubleshooting", "Known Issues", "Common Errors", "FAQ", and "Migration Tips" sections. Also extract **implicit errors**: when the guide says "X is now provided automatically", "do not create your own X", or "the framework manages X", the old manual pattern causes a conflict at build or startup time — create a verify-errors row describing the resulting error, its cause (redundancy/conflict with framework-managed resource), and the fix (remove the old manual pattern).
- **prerequisite** — A precondition that must be met before starting the migration (e.g., "upgrade to the latest minor version first", "resolve all deprecation warnings", "minimum runtime version required"). Do NOT route to a mapping table — instead, collect these and pass them to the compose phase to include as a prerequisite block in SKILL.md.
- **informational** — Context or background with no actionable migration change. **Skip.**

### 2. Fill the appropriate table

For each classified change, add a row to the matching table. Follow the column definitions in `generator/references/table-schemas/`.

**Critical rules:**
- **Extract source-side artifacts.** The `old_*` column should contain the thing that exists in unmigrated code — what the executing agent will search for. Do NOT extract only target-side artifacts.
- **One row per artifact.** If a section describes 5 removed classes, create 5 rows in api-map, not 1 row with a list.
- **Always fill `source_section`.** This is how the validate phase checks completeness.
- **Capture ALL code examples from the guide.** When the guide provides any code snippet — before/after pairs, standalone examples, configuration snippets, command-line flags, annotations, method signatures — copy the relevant code into the `before`/`after` columns of the appropriate table row. Use backtick-wrapped inline code in table cells. If a code example doesn't map to an existing row, create a new `pattern-map` row for it. Do not skip code examples — they are the most valuable part of the skill for agents performing migrations.
- **Use fully qualified names for `old_api`/`new_api`.** Include the full package path, not just the class name.

### 3. One section → multiple tables

A single guide section can produce rows in multiple tables. For example, "Removed Spring MVC Support" might produce:
- `dependency-map.md` rows for the removed starter artifact
- `api-map.md` rows for removed controller classes
- `config-map.md` rows for removed configuration properties

Extract ALL changes, not just the primary one.

### 4. Fill detect_pattern for pattern-map rows

For each pattern-map row that has a `before` code block, extract a grep-friendly `detect_pattern` value:
- Identify the most distinctive identifier, annotation, or method call in the `before` block
- Write it as a literal string or simple regex suitable for `grep -rn`
- If the `before` block spans multiple lines, pick the single most identifying line or annotation
- For `addition` rows with no source-side code, leave `detect_pattern` empty

### 5. Extract implicit removals as verify-errors

When the guide says a feature "is now provided automatically", "is managed by the framework", or "should not be created manually":
- Create a `verify-errors.md` row where `error_pattern` describes the conflict error that results from keeping the old artifact (e.g., `Ambiguous dependencies for type EntityManager`)
- Set `cause` to explain the redundancy (e.g., "Manual producer conflicts with framework-managed bean")
- Set `fix` to the removal action (e.g., "Delete the `@Produces` method — the framework injects `EntityManager` directly")
- Set `phase` to the migration phase where this error surfaces
- Set `source_section` to the guide section, or `experience` if inferred rather than explicit

## Output

Write five files (omit any that have no rows):
- `dependency-map.md` — markdown table with columns from the schema
- `api-map.md` — markdown table with columns from the schema
- `config-map.md` — markdown table with columns from the schema
- `pattern-map.md` — markdown table with columns from the schema
- `verify-errors.md` — markdown table with columns from the schema

These are temporary working files — they will be copied to the output skill's `references/` directory during the compose phase.
