# Phase: Configuration Migration

Migrate Hibernate and JPA configuration properties to their new names, values, or locations.

## Steps

1. Read `references/config-map.md`
2. Identify all configuration files in the project:
   - `persistence.xml`
   - `hibernate.cfg.xml`
   - `application.properties` / `application.yml` (Spring Boot)
   - `application-*.properties` / `application-*.yml` (profile-specific)
   - `META-INF/microprofile-config.properties` (Quarkus/MicroProfile)

### Jakarta Persistence settings

All `javax.persistence.*` property names must change to `jakarta.persistence.*`:
- `javax.persistence.jdbc.url` → `jakarta.persistence.jdbc.url`
- `javax.persistence.jdbc.user` → `jakarta.persistence.jdbc.user`
- `javax.persistence.jdbc.password` → `jakarta.persistence.jdbc.password`
- `javax.persistence.jdbc.driver` → `jakarta.persistence.jdbc.driver`
- `javax.persistence.provider` → `jakarta.persistence.provider`
- `javax.persistence.validation.mode` → `jakarta.persistence.validation.mode`

Hibernate temporarily supports both with deprecation warnings but will remove `javax.persistence.*` support in a future version.

### Property renames — `hibernate.ejb.*` prefix

All `hibernate.ejb.*` properties have been renamed:

| Old | New |
|---|---|
| `hibernate.ejb.metamodel.population` | `hibernate.jpa.metamodel.population` |
| `hibernate.ejb.cfgfile` | `hibernate.cfg_xml_file` |
| `hibernate.ejb.xml_files` | `hibernate.orm_xml_files` |
| `hibernate.hbmxml.files` | `hibernate.hbm_xml_files` |
| `hibernate.ejb.loaded.classes` | `hibernate.loaded_classes` |
| `hibernate.ejb.persistenceUnitName` | `hibernate.persistenceUnitName` |
| `hibernate.ejb.discard_pc_on_close` | `hibernate.discard_pc_on_close` |
| `hibernate.ejb.entitymanager_factory_name` | `hibernate.session_factory_name` |
| `hibernate.ejb.session_factory_observer` | `hibernate.session_factory_observer` |
| `hibernate.ejb.identifier_generator_strategy_provider` | `hibernate.identifier_generator_strategy_provider` |

Prefix renames:
- `hibernate.ejb.classcache.*` → `hibernate.classcache.*`
- `hibernate.ejb.collectioncache.*` → `hibernate.collectioncache.*`
- `hibernate.ejb.event.*` → `hibernate.event.*`

### Other property renames

- `hibernate.hql.bulk_id_strategy` → `hibernate.query.mutation_strategy`
- `hibernate.hbm2dll.create_namespaces` → `hibernate.hbm2ddl.create_namespaces` (typo fix)

### Removed properties

- `hibernate.classLoader.application` — use `hibernate.classLoaders` instead
- `hibernate.classLoader.resources` — use `hibernate.classLoaders` instead
- `hibernate.classLoader.hibernate` — use `hibernate.classLoaders` instead
- `hibernate.classLoader.environment` — use `hibernate.classLoaders` instead
- `hibernate.multiTenancy` — removed; multitenancy auto-inferred from `MultiTenantConnectionProvider` or `@TenantId`

### Default value changes (require explicit action if relying on old behavior)

**ID generation strategy (6.0):**
- `hibernate.id.db_structure_naming_strategy` — default changed from shared `hibernate_sequence` to per-entity sequences
- Set to `single` or `legacy` for backward compatibility

**Criteria query copy (6.0):**
- `hibernate.criteria.copy_tree` — default changed from `true` to `false`
- Set to `true` if mutating criteria queries after `createQuery()`

**Type mapping backward compatibility (6.0):**
- `hibernate.type.preferred_duration_jdbc_type` — set to `BIGINT` for 5.x Duration mapping
- `hibernate.type.preferred_uuid_jdbc_type` — set to `BINARY` for 5.x UUID mapping
- `hibernate.type.preferred_instant_jdbc_type` — set to `TIMESTAMP` for 5.x Instant mapping

**Timezone storage (6.2):**
- `hibernate.timezone.default_storage` — default changed from `NORMALIZE` to `DEFAULT`
- **Critical:** existing `OffsetDateTime`/`ZonedDateTime` data may load with wrong timezone if migrating from ORM 5 with non-UTC `hibernate.jdbc.time_zone`
- Set to `NORMALIZE` for backward compatibility

**CDI extensions (6.2):**
- `hibernate.cdi.extensions` — default changed from implicit `true` to `false`
- Set to `true` if using CDI-resolved Hibernate extensions

**Enhancement defaults (6.2):**
- `hibernate.enhancer.enableLazyInitialization` — default changed from `false` to `true`
- `hibernate.enhancer.enableDirtyTracking` — default changed from `false` to `true`
- `hibernate.bytecode.use_reflection_optimizer` — default changed from `false` to `true`

**Wrapper array handling (6.2):**
- `hibernate.type.wrapper_array_handling` — new, defaults to `DISALLOW`
- Set to `legacy` if using `Byte[]`/`Character[]` mapped as `VARBINARY`/`VARCHAR`

3. Run the build gate

## Build gate

Run `mvn compile`. Configuration errors often surface as startup failures rather than compilation errors — also start the application to verify:
- Hibernate SessionFactory/EntityManagerFactory initializes successfully
- No deprecation warnings for `javax.persistence.*` or `hibernate.ejb.*` properties
- ID generation works correctly (check sequence names)
- Type mappings match database schema (no schema validation errors)
