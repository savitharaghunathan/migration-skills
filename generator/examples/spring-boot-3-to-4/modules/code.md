# Phase: Code Migration

Replace renamed, moved, and removed APIs throughout the codebase.

## Steps

### Part 1: API replacements

1. Read `references/api-map.md`
2. Process rows in `kind` order: `package` → `class` → `annotation` → `interface` → `enum` → `method` → `field`
3. For each row, search the codebase for `old_api` and apply the action using the before/after examples

### Part 2: Pattern changes

1. Read `references/pattern-map.md` (if it exists)
2. For each row, use the `before` code block to locate affected code, transform to match `after`
3. Run: `mvn compile` (or `gradle build`)
