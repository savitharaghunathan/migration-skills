# Phase: Additional Changes

Handle changes that fall outside the standard build/code/config/testing phases — database schemas, deployment configuration, build plugin configuration, infrastructure, or other non-code artifacts.

## Steps

1. Read `references/pattern-map.md` — filter for rows with category `addition` or rows whose `source_section` relates to non-code changes
2. For each relevant row:
   - Read the `description` to understand what changed
   - If `before`/`after` are provided, use them as transformation guides
   - If `before`/`after` are omitted (common for infrastructure changes), follow the guidance in `notes`
   - Common areas:
     - **Database:** Schema changes, migration scripts, ORM configuration
     - **Deployment:** Dockerfile updates, CI/CD pipeline changes, container configuration
     - **Build plugins:** Plugin version updates, new required plugins, removed plugin support
     - **Infrastructure:** Runtime requirements (JDK version, OS dependencies)
3. Run the build gate

## Build gate

Run the project build command. For infrastructure changes, also verify the application starts successfully.
