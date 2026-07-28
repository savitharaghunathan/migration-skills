---
name: generate-migration-skill
description: Generates an agentskills.io migration skill from a framework migration
  guide. Use when asked to generate a migration skill, create a migration skill,
  convert a migration guide to a skill, or turn a migration guide into a skill.
license: Apache-2.0
metadata:
  author: konveyor
  version: "0.1.0"
---

# Migration Skill Generator

Reads a framework migration guide and produces an executable agentskills.io migration skill with mapping tables and phased workflow.

## What this produces

A complete skill directory at `skills/<language>/<migration-name>/` containing:
- `SKILL.md` — phased migration workflow with build gates
- `modules/` — per-phase instructions
- `references/` — mapping tables (dependency, API, config, pattern, verify-errors)

The generated skill can be installed in any project and used by any LLM agent to perform the actual migration.

## Input

The user provides one or more migration guide sources:
- URLs (GitHub wiki pages, documentation sites, HTML pages)
- Local file paths (markdown or HTML files)

Example:
```
Generate a migration skill from https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-4.0-Migration-Guide
```

## Execution

Run the four phases in order. Each phase has a module file in `modules/` with detailed instructions.

### Phase 1: Ingest
**Module:** Read `modules/ingest.md`

Fetch the guide source(s), convert to markdown, detect language and framework metadata.

### Phase 2: Extract
**Module:** Read `modules/extract.md`

Read the normalized guide section by section. For each section, identify all actionable changes and fill the appropriate mapping table. Uses templates from `references/table-schemas/` and few-shot examples from `examples/`.

**Key principle:** Extract source-side artifacts (what exists in unmigrated code), not target-only artifacts.

### Phase 3: Compose
**Module:** Read `modules/compose.md`

Assemble the output skill from templates in `references/`. Fill in the SKILL.md template, copy relevant module templates and populated mapping tables. Write to `skills/<language>/<migration-name>/`.

### Phase 4: Validate
**Module:** Read `modules/validate.md`

Verify structure (valid agentskills.io format), completeness (all guide artifacts covered), and consistency (no duplicates or contradictions). Loop back to Phase 2 if gaps are found (max 2 loops).

## After generation

Report to the user:
1. Where the skill was written: `skills/<language>/<migration-name>/`
2. How many rows in each mapping table
3. Any uncovered sections from the validation phase
4. How to install the generated skill in a target project:
   ```
   npx skills add ./skills/<language>/<migration-name>
   ```
