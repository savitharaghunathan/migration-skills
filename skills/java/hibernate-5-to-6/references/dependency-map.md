# Dependency Map — Hibernate 5 to 6

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| org.hibernate:hibernate-core | org.hibernate.orm:hibernate-core | >= 6.0 | rename | groupId changed from `org.hibernate` to `org.hibernate.orm`; requires Java 11+ | Java 11 |
| org.hibernate:hibernate-envers | org.hibernate.orm:hibernate-envers | >= 6.0 | rename | groupId changed from `org.hibernate` to `org.hibernate.orm` | Java 11 |
| org.hibernate:hibernate-spatial | org.hibernate.orm:hibernate-spatial | >= 6.0 | rename | groupId changed; spatial-specific dialect classes removed, spatial support auto-detected | Dialects |
| org.hibernate:hibernate-testing | org.hibernate.orm:hibernate-testing | >= 6.0 | rename | groupId changed from `org.hibernate` to `org.hibernate.orm` | Java 11 |
| org.hibernate:hibernate-jcache | org.hibernate.orm:hibernate-jcache | >= 6.0 | rename | groupId changed from `org.hibernate` to `org.hibernate.orm` | Java 11 |
| org.hibernate:hibernate-ehcache | removed | >= 6.0 | remove | Ehcache 2 integration removed; use `hibernate-jcache` with Ehcache 3 | Removals |
| javax.persistence:javax.persistence-api | jakarta.persistence:jakarta.persistence-api | >= 3.0 | replace | Jakarta Persistence replaces Java Persistence; all `javax.persistence.*` → `jakarta.persistence.*` | Jakarta Persistence |
| org.hibernate:hibernate-core (community dialects) | org.hibernate.orm:hibernate-community-dialects | >= 6.0 | replace | Legacy/community dialect classes moved to separate artifact; add if using CacheDialect, CUBRIDDialect, FirebirdDialect, InformixDialect, IngresDialect, MaxDBDialect, MimerSQLDialect, SQLiteDialect, TeradataDialect, TimesTenDialect | Dialects |
