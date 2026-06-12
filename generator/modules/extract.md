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
- **prerequisite** — A precondition that must be met before starting the migration (e.g., "upgrade to the latest minor version first", "resolve all deprecation warnings", "minimum runtime version required"). Do NOT route to a mapping table — instead, collect these and pass them to the compose phase to include as a prerequisite block in SKILL.md.
- **informational** — Context or background with no actionable migration change. **Skip.**

### 2. Fill the appropriate table

For each classified change, add a row to the matching table. Follow the column definitions in `generator/references/table-schemas/`.

**Critical rules:**
- **Extract source-side artifacts.** The `old_*` column should contain the thing that exists in unmigrated code — what the executing agent will search for. Do NOT extract only target-side artifacts.
- **One row per artifact.** If a section describes 5 removed classes, create 5 rows in api-map, not 1 row with a list.
- **Always fill `source_section`.** This is how the validate phase checks completeness.
- **Include before/after when the guide provides code examples.** If the guide shows a before/after pair, copy the relevant snippet into the `before`/`after` columns.
- **Use fully qualified names for `old_api`/`new_api`.** Include the full package path, not just the class name.

### 3. One section → multiple tables

A single guide section can produce rows in multiple tables. For example, "Removed Spring MVC Support" might produce:
- `dependency-map.md` rows for the removed starter artifact
- `api-map.md` rows for removed controller classes
- `config-map.md` rows for removed configuration properties

Extract ALL changes, not just the primary one.

## Output

Write four files (omit any that have no rows):
- `dependency-map.md` — markdown table with columns from the schema
- `api-map.md` — markdown table with columns from the schema
- `config-map.md` — markdown table with columns from the schema
- `pattern-map.md` — markdown table with columns from the schema

These are temporary working files — they will be copied to the output skill's `references/` directory during the compose phase.
