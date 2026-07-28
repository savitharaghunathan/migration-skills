# Phase 4: Validate

Verify the generated skill is structurally valid, complete, and internally consistent.

## Input

- The generated skill directory from Phase 3
- The original normalized guide from Phase 1

## Check 1: Structure

Verify the skill conforms to the agentskills.io format:

- [ ] SKILL.md exists and has YAML frontmatter with `name` and `description` fields
- [ ] `name` in frontmatter matches the directory name
- [ ] `name` is kebab-case, contains only lowercase letters/numbers/hyphens, under 64 characters
- [ ] Every module file listed in SKILL.md's phase list exists in `modules/`
- [ ] Every `references/*.md` file referenced by module files exists in `references/`
- [ ] `references/sources.md` exists and links every unique `source_section` to its original URL
- [ ] All mapping table rows have required columns filled:
  - dependency-map: `old_artifact`, `new_artifact`, `action`, `source_section`
  - api-map: `old_api`, `new_api`, `kind`, `action`, `source_section`
  - config-map: `old_property`, `new_property`, `source_section`
  - pattern-map: `description`, `category`, `source_section`
  - verify-errors: `error_pattern`, `cause`, `fix`, `phase`
- [ ] No empty required cells in any table row
- [ ] Module files are customized, not generic template copies:
  - Each module references specific APIs, classes, properties, or patterns from this migration's mapping tables
  - Generic multi-language options are trimmed to the target language only
  - Build gate sections list migration-specific failure modes, not just generic categories

**If any check fails:** Fix the issue and re-run Check 1. If the fix requires regenerating content, go back to Phase 3 (compose).

## Check 2: Completeness

Re-read the original guide and verify extraction coverage:

### Step 1: Identify artifacts in the guide

Scan the guide for named artifacts:
- Code in backticks (`` `ClassName` ``, `` `methodName()` ``, `` `config.property` ``)
- Fully qualified dotted names (`com.example.ClassName`)
- Dependency coordinates (`group:artifact`, `github.com/...`, package names)
- Table cells containing identifiers
- Code blocks showing API usage

### Step 2: Filter out noise

Remove from the artifact list:
- **Target-only artifacts** — things that only exist in the NEW version (new APIs, new config properties that didn't exist before). These are the `new_api`/`new_artifact`/`new_property` side, not something to detect in source code.
- **Informational references** — version numbers, release names, dates, URLs, links to external specs
- **Duplicate mentions** — the same artifact mentioned in multiple sections (count it once)

### Step 3: Check coverage

For each remaining source-side artifact, check whether it appears in at least one mapping table (`old_artifact`, `old_api`, `old_property`, or in a `before` code block in pattern-map).

Report uncovered artifacts with their guide section. For each, determine:
- Is this an actionable migration change that was missed? → Flag for re-extraction
- Is this informational context that doesn't require a mapping? → Skip

**If actionable artifacts are uncovered:** Go back to Phase 2 (extract) with ONLY the uncovered sections. Append new rows to existing tables. Do not re-extract already-covered sections.

## Check 3: Consistency

- [ ] No duplicate source entries: the same `old_artifact`/`old_api`/`old_property` should not appear in multiple rows (unless mapped to different targets in genuinely different contexts — e.g., different `kind` values)
- [ ] No contradictions: the same source artifact should not map to two different targets
- [ ] Before/after coherence: where `before`/`after` code snippets exist, verify that `old_api` appears in `before` and `new_api` appears in `after`
- [ ] Source section validity: every `source_section` value in every table corresponds to an actual heading in the guide (exception: `experience` values in verify-errors are valid without a guide heading)
- [ ] Phase validity: every `phase` value in `verify-errors.md` corresponds to an actual phase name in SKILL.md, or is `general`

**If consistency issues found:** Flag the specific rows and reconcile — fix the duplicate/contradiction, don't delete both entries.

## Loop limits

- Maximum **2 re-extraction loops** (Phase 2 → Phase 4 → Phase 2 → Phase 4)
- After 2 loops, report remaining gaps to the user and proceed
- Most gaps after 2 loops are informational content that genuinely has no actionable migration

## Output

A validated skill directory, or a gap report listing uncovered sections for the user to review.
