# Phase: Code Migration

Replace renamed, moved, and removed APIs throughout the codebase.

## Steps

### Part 1: Jakarta EE namespace migration

This is the most impactful change. All `javax.*` Jakarta EE packages must change to `jakarta.*`.

1. Read `references/api-map.md` — process all `package` kind rows first
2. Search the entire codebase for `javax.servlet`, `javax.persistence`, `javax.validation`, `javax.annotation`, `javax.inject`, `javax.transaction`, `javax.mail`, `javax.websocket`, `javax.xml.bind`, `javax.activation`, `javax.el`, `javax.faces`, `javax.json`, `javax.jws`, `javax.ws.rs`
3. Replace each `javax.*` import with its `jakarta.*` equivalent
4. **Important:** Do NOT rename `javax.xml.datatype`, `javax.xml.parsers`, `javax.xml.transform`, `javax.xml.validation`, `javax.xml.xpath`, `javax.xml.namespace`, `javax.xml.stream`, `javax.crypto`, `javax.net`, `javax.security.auth`, `javax.sql` — these are Java SE packages that stayed in the `javax` namespace

### Part 2: API replacements

1. Read `references/api-map.md` — process remaining rows in `kind` order:
   - `class` → update class references
   - `annotation` → swap annotation imports and usages
   - `interface` → update interface references
   - `enum` → update enum references
   - `method` → update method calls
   - `field` → update field references
2. For each row:
   - Search the codebase for `old_api`
   - Apply the `action`:
     - `replace` → substitute with `new_api`
     - `remove` → delete usage; if `after` column has a replacement pattern, use it
     - `rename` → update the name
     - `move_package` → update import statements to new package
   - Use the `before`/`after` columns as concrete examples

### Part 3: Pattern changes

1. Read `references/pattern-map.md`
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
- Missing imports after package moves (javax→jakarta is the #1 cause)
- Incompatible method signatures (especially Hibernate type system changes)
- Removed APIs still referenced in the code
