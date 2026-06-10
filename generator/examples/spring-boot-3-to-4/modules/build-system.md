# Phase: Build System Migration

Update dependencies and build plugins before touching application code.

## Steps

1. Read `references/dependency-map.md`
2. Open the project's build file (`pom.xml` or `build.gradle`/`build.gradle.kts`)
3. For each row in the dependency map:
   - Search the build file for `old_artifact`
   - Apply the `action` (replace, remove, rename, merge)
   - If `version_constraint` is specified, update the version
   - Check `notes` for gotchas
4. Run: `mvn compile` (or `gradle build`)
