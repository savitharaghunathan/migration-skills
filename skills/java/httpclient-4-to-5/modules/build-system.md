# Phase: Build System Migration

Replace Apache HttpClient 4.x Maven coordinates with 5.x equivalents.

## Steps

1. Read `references/dependency-map.md`
2. Open the project's build file(s):
   - Maven: `pom.xml`
   - Gradle: `build.gradle` or `build.gradle.kts`
3. For each row in the dependency map:
   - Search the build file for `old_artifact`
   - Apply the `action`:
     - `replace` → change the artifact coordinate to `new_artifact`
     - `remove` → delete the dependency entry
     - `merge` → replace with the consolidated `new_artifact`, remove duplicates
   - Remove explicit version tags if using a BOM; otherwise set version to `5.6.x` (latest 5.x)
   - Check `notes` for gotchas
4. Key replacements:
   - `org.apache.httpcomponents:httpclient` → `org.apache.httpcomponents.client5:httpclient5`
   - `org.apache.httpcomponents:httpcore` → `org.apache.httpcomponents.core5:httpcore5`
   - `org.apache.httpcomponents:httpmime` → merged into `httpclient5` (remove separate dependency)
   - `org.apache.httpcomponents:fluent-hc` → `org.apache.httpcomponents.client5:httpclient5-fluent`
5. Run the build gate

## Build gate

Run the project build command. Expect compilation errors — the package namespace changed from `org.apache.http` to `org.apache.hc.*`, which will be resolved in the Code phase.
