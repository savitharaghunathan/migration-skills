# Phase: Build System Migration

Update dependencies and build plugins before touching application code.

## Steps

1. Read `references/dependency-map.md`
2. Open the project's build file(s): `pom.xml` (Maven) or `build.gradle`/`build.gradle.kts` (Gradle)
3. For each row in the dependency map, apply the changes:

### Dropped library removals
- Remove `spring-webmvc-portlet` — Portlet support is gone entirely
- Remove Velocity dependencies — migrate templates to FreeMarker or Thymeleaf
- Remove `jasperreports` — JasperReports integration dropped
- Remove `xmlbeans` — XMLBeans support dropped
- Remove `javax.jdo:jdo-api` — JDO support dropped

### Caching: Guava → Caffeine
- Replace `com.google.guava:guava` (if used for caching) with `com.github.ben-manes.caffeine:caffeine`
- `GuavaCacheManager` → `CaffeineCacheManager` (handled in code phase)

### Logging: commons-logging → spring-jcl
- Remove `commons-logging:commons-logging` dependency — `spring-jcl` in `spring-core` replaces it
- Remove `org.slf4j:jcl-over-slf4j` if present — `spring-jcl` auto-detects SLF4J
- Remove `<exclusion>` blocks for `commons-logging` from `spring-core` — no longer needed
- Check other libraries still pulling in `commons-logging` transitively; add explicit excludes if needed

### ORM and view technology upgrades
- Upgrade `org.apache.tiles:tiles-*` to Tiles 3+ — `web.view.tiles2` package removed
- Upgrade `org.hibernate:hibernate-core` to 5.0+ (5.2+ for 5.3 features, 5.4.x recommended)
- If using Hibernate Search, upgrade to 5.11.6+ for 5.3 JPA compatibility

### Library version bumps
- `com.fasterxml.jackson.core:jackson-databind` → 2.9+ (2.9.7+ for 5.2)
- `org.ehcache:ehcache` → 2.10+
- `com.squareup.okhttp3:okhttp` → 3.0+
- If using Reactor Kotlin extensions, add `io.projectreactor.kotlin:reactor-kotlin-extensions` (moved from reactor-core in 5.2)

### RxJava migration (if applicable)
- Replace `io.reactivex:rxjava` (RxJava 1.x) with `io.reactivex.rxjava2:rxjava` (RxJava 2.x)
- RxJava 3 also supported from 5.3

4. Run the build gate

## Build gate

Run `mvn compile` or `gradle build`. Common issues:
- Version conflicts between Hibernate 5 and legacy Hibernate 3/4 transitive dependencies
- Missing Tiles 3 configuration after Tiles 2 removal
- Classpath conflicts from both `commons-logging` and `spring-jcl` present
- Caffeine API differences from Guava cache builder
