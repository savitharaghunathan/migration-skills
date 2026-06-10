# Migration Skills Generator

Generate executable [agentskills.io](https://agentskills.io) migration skills from framework migration guides.

## What This Does

Given a migration guide (URL or local file), the generator skill reads it and produces a complete migration skill — organized mapping tables (dependency, API, config, pattern) plus phased workflow instructions — that any LLM agent can use to migrate a real codebase.

## How It Works

```
Migration Guide (URL/file)
  → generator/ meta-skill
    → Phase 1: Ingest (fetch, normalize, detect language)
    → Phase 2: Extract (fill mapping tables from guide)
    → Phase 3: Compose (assemble skill from templates)
    → Phase 4: Validate (structure, completeness, consistency)
  → skills/<language>/<migration-name>/ (output)
```

The generator uses **rigid templates** for skill structure and **table schemas** for mapping data. The LLM only fills in table rows — it doesn't design workflow or structure. This makes extraction consistent across models and runs.

## Quick Start

1. Install the generator skill:
   ```
   npx skills add <this-repo-url>
   ```

2. Ask your agent to generate a migration skill:
   ```
   Generate a migration skill from https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-4.0-Migration-Guide
   ```

3. The generated skill appears in `skills/<language>/<migration-name>/`.

4. To use a generated skill on a project, install it:
   ```
   npx skills add ./skills/java/spring-boot-3-to-4
   ```
   Then ask your agent to run the migration on your codebase.

## Available Migration Skills

| Skill | Source → Target | Rows | Link |
|-------|----------------|------|------|
| [JDK 21 to 25](skills/java/jdk-21-to-25/) | JDK 21 → JDK 25 | 88 | [SKILL.md](skills/java/jdk-21-to-25/SKILL.md) |
| [Spring Boot 3 to 4](skills/java/spring-boot-3-to-4/) | Spring Boot 3.5 → 4.0 | 122 | [SKILL.md](skills/java/spring-boot-3-to-4/SKILL.md) |

## Project Structure

- `generator/` — The meta-skill that generates migration skills
  - `SKILL.md` — Main orchestration
  - `modules/` — Phase instructions (ingest, extract, compose, validate)
  - `references/` — Templates and table schemas
  - `examples/` — Hand-curated reference skills for few-shot grounding
- `skills/` — Generated output, organized by language
  - `java/`
    - [`jdk-21-to-25/`](skills/java/jdk-21-to-25/) — JDK 21 to 25 migration (Security Manager removal, sun.misc.Unsafe deprecation, ZGC changes, JVM option removals)
    - [`spring-boot-3-to-4/`](skills/java/spring-boot-3-to-4/) — Spring Boot 3.5 to 4.0 migration (modular redesign, Jackson 3, @MockBean removal, starter renames, config property migrations)

## Generated Skill Format

Each generated skill follows the [agentskills.io](https://agentskills.io/specification) format:

```
skills/<language>/<migration-name>/
├── SKILL.md              # Phased migration workflow
├── modules/              # Per-phase instructions
│   ├── build-system.md
│   ├── code.md
│   ├── config.md
│   ├── testing.md
│   └── cleanup.md
└── references/           # Mapping tables
    ├── dependency-map.md
    ├── api-map.md
    └── config-map.md
```

## License

Apache-2.0
