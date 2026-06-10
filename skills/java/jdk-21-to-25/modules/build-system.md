# Phase: Build System Migration

Update dependencies and modules before touching application code.

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
4. Update the JDK source/target version:
   - Maven: set `<maven.compiler.source>25</maven.compiler.source>` and `<maven.compiler.target>25</maven.compiler.target>`, or `<maven.compiler.release>25</maven.compiler.release>`
   - Gradle: set `sourceCompatibility = JavaVersion.VERSION_25` and `targetCompatibility = JavaVersion.VERSION_25`
5. If the project uses `--enable-preview` features from JDK 21 (e.g., String Templates), review whether those features are still available in JDK 25
6. Run the build gate

## Build gate

Run the project build command. If it fails, fix dependency issues before proceeding to the next phase. Common issues:
- Version conflicts between updated and non-updated dependencies
- Transitive dependency breakage
- Missing repository declarations for relocated artifacts
- Removed modules (e.g., `jdk.random`, `jdk.jsobject`) still referenced
