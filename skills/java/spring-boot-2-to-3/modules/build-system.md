# Phase: Build System Migration

Update dependencies and build plugins before touching application code.

## Steps

1. Read `references/dependency-map.md`
2. Open the project's build file(s):
   - Java Maven: `pom.xml`
   - Java Gradle: `build.gradle` or `build.gradle.kts`
3. For each row in the dependency map:
   - Search the build file for `old_artifact`
   - Apply the `action`:
     - `replace` → change the artifact coordinate to `new_artifact`
     - `remove` → delete the dependency entry
     - `rename` → update the coordinate (same dependency, new name)
     - `merge` → replace with the consolidated `new_artifact`, remove duplicates
   - If `version_constraint` is specified, update the version accordingly
   - Check `notes` for gotchas before moving on
4. Update the Spring Boot parent/BOM version to 3.0.x
5. Add `spring-boot-properties-migrator` as a runtime dependency (temporary — remove after migration)
6. Run the build gate

## Build gate

Run the project build command. If it fails, fix dependency issues before proceeding to the next phase. Common issues:
- Version conflicts between updated and non-updated dependencies
- Transitive dependency breakage (especially javax→jakarta conflicts)
- Missing repository declarations for relocated artifacts
- Ehcache 3 needing `jakarta` classifier
