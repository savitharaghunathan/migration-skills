# Phase 3: Compose

Assemble the generated skill from templates and populated mapping tables.

## Input

- Populated mapping tables from Phase 2 (dependency-map.md, api-map.md, config-map.md, pattern-map.md, verify-errors.md — whichever have data)
- Prerequisites collected from Phase 2 (preconditions that must be met before starting the migration)
- Detected metadata from Phase 1 (language, source_framework, source_version, target_framework, target_version)

## Overwrite protection

Before writing, check if `skills/{{language}}/{{migration_name}}/` already exists.

- **If it exists and the repo has git history for this path:** Warn the user:
  > "Skill directory `skills/{{language}}/{{migration_name}}/` already exists. Generating will overwrite. Commit or stash any manual edits first."
  - If the user confirms → proceed, overwrite
  - If the user declines → write to `skills/{{language}}/{{migration_name}}-{{YYYYMMDD}}/` instead
- **If it exists but has no git history:** Proceed with overwrite (it's a previous uncommitted generation)
- **If it doesn't exist:** Proceed normally

## Steps

### 1. Determine the migration name

Construct the kebab-case migration name: `{{source}}-to-{{target}}`

Examples:
- `spring-boot-3-to-4`
- `django-4-to-5`
- `httpclient-4-to-5`
- `dotnet-6-to-8`

Must be under 64 characters and contain only lowercase letters, numbers, and hyphens.

### 2. Build the SKILL.md

Read `generator/references/skill-template.md` and fill in all `{{...}}` placeholders:

- `{{migration_name}}` — the kebab-case name from step 1
- `{{source_framework}}`, `{{target_framework}}` — human-readable names (e.g., "Spring Boot")
- `{{source_version}}`, `{{target_version}}` — version numbers (e.g., "3", "4")
- `{{source}}`, `{{target}}` — short identifiers for labels (e.g., "spring-boot-3", "spring-boot-4")
- `{{language}}` — detected language (e.g., "java", "go", "python", "dotnet")
- `{{build_tool}}` — detected build tool and command (e.g., "maven: mvn compile")
- `{{smoke_tool}}` — detected smoke command (e.g., "mvn quarkus:dev &; sleep 15; curl -sf http://localhost:8080/q/health; kill %1"). If no smoke command was detected, remove the `smoke_tool` metadata line entirely.
- `{{guide_urls}}` — the original guide URL(s) provided by the user
- `{{timestamp}}` — current ISO 8601 timestamp
- `{{prerequisites}}` — if prerequisites were collected in Phase 2, render a bold "Prerequisite:" block listing each precondition. If none were collected, remove the placeholder entirely (leave no blank line)
- `{{migration_summary}}` — one paragraph summarizing the migration scope based on the guide content
- `{{phase_list}}` — numbered list of phases that have data (see below)

### 3. Build the phase list

Include only phases that have corresponding data:

| Phase | Include if... |
|-------|---------------|
| Build system | `dependency-map.md` has rows |
| Code | `api-map.md` OR `pattern-map.md` has rows |
| Config | `config-map.md` has rows |
| Testing | `api-map.md` has rows with test-related `source_section` values |
| Additional | `pattern-map.md` has rows with `category` = `addition` or non-code `source_section` values |
| Cleanup | Always included (it's the verification phase) |

### 4. Customize module files from templates

For each included phase, start from the corresponding template in `generator/references/module-templates/` and **customize it with migration-specific content** drawn from the populated mapping tables. Do NOT copy templates verbatim — every module file must reference concrete artifacts from this migration.

Templates to customize: `build-system.md`, `code.md`, `config.md`, `testing.md`, `additional.md`, `cleanup.md`

For each module file:

1. **Read the template** to get the structural skeleton (headings, step numbering, build gate format)
2. **Read the relevant mapping tables** populated in Phase 2
3. **Replace generic instructions with specific ones:**

   - **build-system.md** — List the specific dependency changes from `dependency-map.md` as named steps (e.g., "Remove `old_artifact`, replace with `new_artifact`"). Include version constraints and build tool snippets where the guide provides them. Trim language/build-tool options to only the target language.

   - **code.md** — In the API replacements section, list concrete examples from `api-map.md` for each `kind` (e.g., "package: `org.old.pkg.*` → `org.new.pkg.*`", "class: `OldClass` → `NewClass`"). In the pattern changes section, call out the most impactful `pattern-map.md` rows by name with before/after summaries. Add a "Key areas to search" subsection listing the most common old identifiers.

   - **config.md** — Replace the multi-language config file list with only the target language's files. Add a "Key migration groups" subsection listing the specific property renames/removals from `config-map.md`, grouped by topic. Include any default-value changes that require explicit action.

   - **testing.md** — List specific test API changes (annotations, mock frameworks, test configuration) from the mapping tables. Call out test dependency changes. Reference specific behavioral changes that surface as test failures rather than compilation errors.

   - **additional.md** — Replace the generic category list (Database, Deployment, Build plugins, Infrastructure) with named subsections for this migration's specific non-code changes from `pattern-map.md` (category `addition` or infrastructure-related rows). Include runtime requirements, tool changes, and deployment impacts.

   - **cleanup.md** — In "Remove old package imports", list the specific old package/import patterns to search for. In "Verify no old artifacts remain", list the specific old coordinates, property names, and API names. In the behavioral changes section, enumerate the specific `behavioral` rows from `pattern-map.md` that need manual verification.

4. **Trim irrelevant content** — Remove language/framework options that don't apply (e.g., remove Go/Python/.NET references from a Java migration). Remove phases that have no data.

### 5. Copy mapping tables

Copy the populated mapping table files into the output `references/` directory.

For `verify-errors.md`, reconstruct the phase-grouped format expected by the verify/execute stage:
- Group rows by their `phase` column value
- Render each group under an H2 heading matching the phase name (e.g., `## Build System Phase`)
- Within each group, render a 3-column table: `| Error | Cause | Fix |`
- Map `error_pattern` → Error, `cause` → Cause, `fix` → Fix
- Omit the `phase` and `source_section` columns from the rendered output — they are extraction metadata, not consumed by the verify stage
- Order groups to match the phase order in SKILL.md
- Place a `## General` section at the end for rows where `phase` = `general`

### 5b. Build sources.md (provenance)

Create `references/sources.md` linking every `source_section` value in the mapping tables back to its original URL from the guide.

- Group by guide source (primary guide, supplementary guides)
- For each unique `source_section`, provide the URL with fragment anchor (e.g., `https://example.com/guide#section-name`)
- If the guide is a single-page wiki, construct fragment anchors from section headings
- If the guide spans multiple pages, link to the specific page
- Include a "Supplementary" section for any web search results or external references used during extraction

This file is bundled with the skill as provenance — it lets users and agents verify that mapping table rows trace back to authoritative sources.

### 6. Copy guide (traceability)

Copy the normalized `guide.md` into the output directory. This is a build artifact for traceability — not part of the agentskills.io skill.

### 7. Write the output

Write all files to: `skills/{{language}}/{{migration_name}}/`

```
skills/{{language}}/{{migration_name}}/
├── SKILL.md
├── guide.md              (build artifact)
├── modules/
│   ├── build-system.md   (if dependency-map has data)
│   ├── code.md           (if api-map or pattern-map has data)
│   ├── config.md         (if config-map has data)
│   ├── testing.md        (if test-related data exists)
│   ├── additional.md     (if non-code changes exist)
│   └── cleanup.md        (always)
└── references/
    ├── dependency-map.md  (if has data)
    ├── api-map.md         (if has data)
    ├── config-map.md      (if has data)
    ├── pattern-map.md     (if has data)
    ├── verify-errors.md   (if has data)
    └── sources.md         (always — provenance links)
```

## Output

The complete skill directory, ready for validation.
