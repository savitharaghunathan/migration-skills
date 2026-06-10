# Phase: Code Migration

Replace renamed, moved, and removed APIs throughout the codebase.

## Steps

### Part 1: API replacements

1. Read `references/api-map.md`
2. Process rows in `kind` order to prevent conflicts:
   - First: `package` — apply package-level renames/moves
   - Then: `class` — update class references
   - Then: `annotation` — swap annotation imports and usages
   - Then: `interface` — update interface references
   - Then: `enum` — update enum references
   - Then: `method` — update method calls
   - Finally: `field` — update field references
3. For each row:
   - Search the codebase for `old_api` (check imports, type references, annotations, method calls)
   - Apply the `action`:
     - `replace` → substitute with `new_api`
     - `remove` → delete usage; if `after` column has a replacement pattern, use it
     - `rename` → update the name
     - `move_package` → update import statements to new package
   - Use the `before`/`after` columns as concrete examples of the transformation
   - Check `notes` for edge cases

### Part 2: Pattern changes

1. Read `references/pattern-map.md` (if it exists)
2. For each row:
   - Use the `before` code block to locate affected code in the project
   - Transform to match the `after` pattern
   - Pay attention to `category`:
     - `behavioral` — the code may not need changes, but behavior differs; check `notes`
     - `structural` — code reorganization required
     - `removal` — delete the code or replace with alternative from `notes`
     - `addition` — add new code/config as shown in `after`
3. Run the build gate

## Build gate

Run the project build command. If it fails, check for:
- Missing imports after package moves
- Incompatible method signatures
- Removed APIs still referenced in the code
