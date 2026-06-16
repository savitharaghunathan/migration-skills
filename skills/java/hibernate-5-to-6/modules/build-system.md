# Phase: Build System Migration

Update Hibernate artifact coordinates and Jakarta Persistence dependency before touching application code.

## Steps

1. Read `references/dependency-map.md`
2. Open the project's build file(s): `pom.xml` or `build.gradle`/`build.gradle.kts`
3. For each row in the dependency map, search the build file for `old_artifact` and apply the action

### Hibernate ORM groupId change

The Hibernate ORM groupId changed from `org.hibernate` to `org.hibernate.orm`:

```xml
<!-- Before -->
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>5.6.15.Final</version>
</dependency>

<!-- After -->
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>6.2.x.Final</version>
</dependency>
```

Apply the same groupId change to all Hibernate modules:
- `hibernate-core`
- `hibernate-envers`
- `hibernate-spatial`
- `hibernate-testing`
- `hibernate-jcache`

### Removed modules

- **`hibernate-ehcache`** — Ehcache 2 integration removed. Replace with `hibernate-jcache` (works with Ehcache 3, Caffeine, etc.)

### Jakarta Persistence

Replace Java Persistence API with Jakarta Persistence:

```xml
<!-- Before -->
<dependency>
    <groupId>javax.persistence</groupId>
    <artifactId>javax.persistence-api</artifactId>
</dependency>

<!-- After -->
<dependency>
    <groupId>jakarta.persistence</groupId>
    <artifactId>jakarta.persistence-api</artifactId>
</dependency>
```

If using Spring Boot or Quarkus BOM, the Jakarta Persistence dependency is typically managed — just update the parent/BOM version.

### Community dialects

If using legacy or community-supported databases (Cache, CUBRID, Firebird, Informix, Ingres, MaxDB, MimerSQL, SQLite, Teradata, TimesTen), add the `hibernate-community-dialects` artifact:

```xml
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-community-dialects</artifactId>
    <version>6.2.x.Final</version>
</dependency>
```

4. Run the build gate

## Build gate

Run `mvn compile` (or `gradle build`). Common issues:
- Version conflicts between updated and non-updated dependencies
- Missing `hibernate-community-dialects` for legacy database dialects
- Jakarta Persistence API version mismatch with Hibernate 6
