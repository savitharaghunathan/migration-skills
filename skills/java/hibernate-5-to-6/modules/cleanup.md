# Phase: Cleanup and Verification

Remove legacy references and verify the complete migration.

## Steps

### 1. Remove old `javax.persistence.*` imports

Search for any remaining `javax.persistence.*` imports:
- `javax.persistence.Entity`
- `javax.persistence.Table`
- `javax.persistence.Column`
- `javax.persistence.Id`
- `javax.persistence.GeneratedValue`
- `javax.persistence.ManyToOne`
- `javax.persistence.OneToMany`
- `javax.persistence.ManyToMany`
- `javax.persistence.OneToOne`
- `javax.persistence.Embeddable`
- `javax.persistence.Embedded`
- `javax.persistence.Query`
- `javax.persistence.EntityManager`
- `javax.persistence.EntityManagerFactory`
- `javax.persistence.PersistenceContext`
- `javax.persistence.PersistenceUnit`
- (and all other `javax.persistence.*`)

### 2. Verify no legacy Hibernate APIs remain

Search for:
- `org.hibernate.Criteria` — legacy Criteria API (use JPA Criteria)
- `org.hibernate.criterion.*` — Restrictions, Order, Projections
- `org.hibernate.annotations.TypeDef` / `@TypeDef`
- `org.hibernate.annotations.TypeDefs` / `@TypeDefs`
- `org.hibernate.annotations.AnyMetaDef` / `@AnyMetaDef`
- `org.hibernate.annotations.AnyMetaDefs` / `@AnyMetaDefs`
- `org.hibernate.type.YesNoBooleanType` / `org.hibernate.type.TrueFalseBooleanType` / `org.hibernate.type.NumericBooleanType`
- `org.hibernate.type.descriptor.sql.SqlTypeDescriptor`
- `org.hibernate.type.descriptor.java.JavaTypeDescriptor`
- `MultiTenancyStrategy` / `AvailableSettings.MULTI_TENANT`
- `QueryHints.HINT_PASS_DISTINCT_THROUGH`

### 3. Verify no version-specific dialect references

Search build files and configuration for:
- `PostgreSQL81Dialect`, `PostgreSQL91Dialect`, `PostgreSQL95Dialect`
- `MySQL5Dialect`, `MySQL57Dialect`, `MySQL8Dialect`
- `MariaDB102Dialect`, `MariaDB103Dialect`
- `Oracle10gDialect`, `Oracle12cDialect`
- `SQLServer2012Dialect`, `SQLServer2016Dialect`
- `DerbyTenSevenDialect`
- Any `org.hibernate.spatial.dialect.*` classes
- Any `org.hibernate.dialect.*Dialect` with version numbers

### 4. Verify no old configuration properties

Search all config files for:
- `hibernate.ejb.*` properties (should be renamed)
- `javax.persistence.*` property names (should be `jakarta.persistence.*`)
- `hibernate.hql.bulk_id_strategy` (should be `hibernate.query.mutation_strategy`)
- `hibernate.classLoader.application/resources/hibernate/environment` (should be `hibernate.classLoaders`)
- `hibernate.multiTenancy` (removed)

### 5. Verify no old artifacts in build files

Search `pom.xml`/`build.gradle` for:
- `org.hibernate:hibernate-core` (should be `org.hibernate.orm:hibernate-core`)
- `org.hibernate:hibernate-envers` (should be `org.hibernate.orm:hibernate-envers`)
- `org.hibernate:hibernate-ehcache` (removed — use `hibernate-jcache`)
- `javax.persistence:javax.persistence-api` (should be `jakarta.persistence:jakarta.persistence-api`)

### 6. Verify behavioral changes

These won't cause compilation errors but alter runtime behavior:
- [ ] Per-entity ID sequences generating correct values (not shared `hibernate_sequence`)
- [ ] HQL queries returning correct result types (entity vs Object[])
- [ ] Native `count()` queries returning `Long` instead of `BigInteger`
- [ ] `query.stream()` properly closed in try-with-resources
- [ ] Duration/UUID/Instant columns validating against database schema
- [ ] `@ManyToOne(fetch=LAZY) @Fetch(JOIN)` truly lazy (not eager as in 5.x)
- [ ] Fetch join queries no longer returning duplicates (DISTINCT behavior change)
- [ ] Association comparisons using entity references or FK attributes (not raw IDs)
- [ ] Ordinal HQL parameters using 1-based indexing
- [ ] `beans.xml`-like CDI contexts work correctly with `hibernate.cdi.extensions` setting (6.2)
- [ ] Timezone/offset storage correct for `OffsetDateTime`/`ZonedDateTime` (6.2)
- [ ] `Byte[]`/`Character[]` properties handled correctly (6.2)
- [ ] Optional `@OneToOne` UNIQUE constraint doesn't cause schema validation issues (6.2)

### 7. Final build and test

- Run the full build: `mvn clean verify`
- Run integration tests
- Start the application and verify Hibernate initializes without errors
- Test CRUD operations for all entity types

### 8. Report to user

- List all changes made across all phases
- Flag HQL queries that may need manual review for behavioral changes
- Note any type mapping backward-compatibility properties that were set (these should be removed once schema migration is complete)
- Report any hbm.xml files that need manual migration to annotations/orm.xml
