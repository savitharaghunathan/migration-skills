# Phase: Build System Migration

Update dependencies, starters, and build plugins before touching application code.

## Steps

1. Read `references/dependency-map.md`
2. Open the project's build file(s):
   - Maven: `pom.xml`
   - Gradle: `build.gradle` or `build.gradle.kts`
3. Update the Spring Boot parent/BOM version to 4.0.x
4. For each row in the dependency map:
   - Search the build file for `old_artifact`
   - Apply the `action`:
     - `replace` → change the artifact coordinate to `new_artifact`
     - `remove` → delete the dependency entry
     - `rename` → update the coordinate (same dependency, new name)
   - If `version_constraint` is specified, update the version accordingly
   - Check `notes` for gotchas before moving on
5. Key changes to verify:
   - `spring-boot-starter-web` → `spring-boot-starter-webmvc`
   - OAuth starters → `spring-boot-starter-security-oauth2-*`
   - `spring-boot-starter-aop` → `spring-boot-starter-aspectj`
   - Flyway/Liquibase direct dependencies → `spring-boot-starter-flyway` / `spring-boot-starter-liquibase`
   - Jackson `com.fasterxml.jackson` → `tools.jackson` group IDs
   - Undertow starters → remove (use Tomcat or Jetty)
   - Elasticsearch `elasticsearch-rest-client` → `elasticsearch-java`
   - `hibernate-jpamodelgen` → `hibernate-processor`
6. Consider using `spring-boot-starter-classic` temporarily as a migration stepping stone
7. Remove `<loaderImplementation>CLASSIC</loaderImplementation>` from build plugin config
8. Run the build gate

## Build gate

Run the project build command. If it fails, fix dependency issues before proceeding. Common issues:
- Missing starters for technologies that previously didn't need one
- Jackson 3 group ID conflicts with Jackson 2 dependencies
- Transitive dependency breakage from removed modules
- Version conflicts from Spring portfolio upgrades (Framework 7, Security 7, etc.)
