# Phase: Code Migration

Replace renamed, moved, and removed APIs throughout the codebase.

## Steps

### Part 1: API replacements

1. Read `references/api-map.md`
2. Process rows in `kind` order to prevent conflicts:

#### Package-level namespace migration (highest priority)

All `javax.persistence.*` imports must change to `jakarta.persistence.*`:
- `javax.persistence.Entity` → `jakarta.persistence.Entity`
- `javax.persistence.Column` → `jakarta.persistence.Column`
- `javax.persistence.Table` → `jakarta.persistence.Table`
- `javax.persistence.Id` → `jakarta.persistence.Id`
- `javax.persistence.GeneratedValue` → `jakarta.persistence.GeneratedValue`
- `javax.persistence.ManyToOne` → `jakarta.persistence.ManyToOne`
- (all other javax.persistence.* imports)

#### Interface and class changes

**Type system renames:**
- `org.hibernate.type.descriptor.java.JavaTypeDescriptor` → `org.hibernate.type.descriptor.java.JavaType`
- `org.hibernate.type.descriptor.sql.SqlTypeDescriptor` → `org.hibernate.type.descriptor.jdbc.JdbcType` (package also changed from `sql` to `jdbc`)

**Legacy Criteria API removed:**
- `org.hibernate.Criteria` — removed entirely; migrate to JPA Criteria API:
  ```java
  // Before (5.x)
  Criteria criteria = session.createCriteria(Person.class);
  criteria.add(Restrictions.eq("name", "John"));
  List<Person> results = criteria.list();

  // After (6.0)
  CriteriaBuilder cb = session.getCriteriaBuilder();
  CriteriaQuery<Person> cq = cb.createQuery(Person.class);
  Root<Person> root = cq.from(Person.class);
  cq.where(cb.equal(root.get("name"), "John"));
  List<Person> results = session.createQuery(cq).getResultList();
  ```
- `org.hibernate.criterion.*` — entire package removed (Restrictions, Order, Projections, etc.)

**Callable NativeQuery → ProcedureCall:**
```java
// Before (5.x)
List<Object[]> results = session.createNativeQuery("{? = call fn_person(?)}")
    .setParameter(1, 1L).getResultList();

// After (6.0)
List<Object[]> results = session.createStoredProcedureCall("fn_person")
    .setParameter(1, 1L).getResultList();
```

**Dialect classes — version-specific → base dialect:**
- `PostgreSQL81Dialect` / `PostgreSQL91Dialect` → `PostgreSQLDialect`
- `MySQL5Dialect` / `MySQL57Dialect` / `MySQL8Dialect` → `MySQLDialect`
- `MariaDB102Dialect` / `MariaDB103Dialect` → `MariaDBDialect`
- `Oracle10gDialect` / `Oracle12cDialect` → `OracleDialect`
- `SQLServer2012Dialect` → `SQLServerDialect`
- `DerbyTenSevenDialect` → `DerbyDialect`
- `DB2390Dialect` → `DB2zDialect`
- All spatial dialect classes (PostgisPG94Dialect, MySQL56SpatialDialect, etc.) → base dialect

**Community dialect classes relocated:**
- `org.hibernate.dialect.CacheDialect` → `org.hibernate.community.dialect.CacheDialect`
- `org.hibernate.dialect.CUBRIDDialect` → `org.hibernate.community.dialect.CUBRIDDialect`
- (and FirebirdDialect, InformixDialect, IngresDialect, SQLiteDialect, TeradataDialect, TimesTenDialect)

#### Annotation changes

**@TypeDef / @TypeDefs / @AnyMetaDef / @AnyMetaDefs — removed:**
```java
// Before (5.x) — package-info.java
@TypeDef(name = "json", typeClass = JsonType.class)
package com.example;

// field
@Type(type = "json")
private Map<String, Object> metadata;

// After (6.0)
@Type(JsonType.class)
private Map<String, Object> metadata;
```

**Boolean type conversions:**
```java
// Before: @Type(type="yes_no")
@Convert(converter = YesNoConverter.class)
boolean active;

// Before: @Type(type="true_false")
@Convert(converter = TrueFalseConverter.class)
boolean enabled;

// Before: @Type(type="numeric_boolean")
@Convert(converter = NumericBooleanConverter.class)
boolean visible;
```

**@CollectionType changed from String to Class:**
```java
// Before: @CollectionType(type = "com.example.MyCollType")
@CollectionType(type = MyCollType.class)
```

**@Columns removed:**
```java
// Before: @Columns(columns = {@Column(name="CURRENCY"), @Column(name="AMOUNT")})
@AttributeOverride(name = "currency", column = @Column(name = "CURRENCY"))
@AttributeOverride(name = "value", column = @Column(name = "AMOUNT"))
```

**@NamedNativeQuery(callable=true) → @NamedStoredProcedureQuery:**
```java
// Before
@NamedNativeQuery(name = "proc", query = "{? = call fn(?)}", callable = true,
    resultSetMapping = "mapping")

// After
@NamedStoredProcedureQuery(name = "proc", procedureName = "fn",
    resultSetMappings = "mapping",
    hints = @QueryHint(name = "org.hibernate.callableFunction", value = "true"),
    parameters = @StoredProcedureParameter(type = Long.class))
```

#### Enum and field changes

- `AvailableSettings.MULTI_TENANT` — removed; multitenancy auto-inferred
- `MultiTenancyStrategy` enum — removed entirely
- `QueryHints.HINT_PASS_DISTINCT_THROUGH` — removed; DISTINCT always passed through

#### Method changes

**Query#iterate() removed:**
```java
// Before
Iterator<Person> it = query.iterate();
// After
Stream<Person> s = query.stream(); // or query.getResultList().iterator()
```

**Interceptor signatures — Serializable → Object for ID parameter:**
```java
// Before
boolean onSave(Object entity, Serializable id, Object[] state, String[] names, Type[] types)
// After
boolean onSave(Object entity, Object id, Object[] state, String[] names, Type[] types)
```
Applies to: `onSave`, `onLoad`, `onDelete`, `onFlushDirty`, `getEntity`. Always use `@Override` to catch these.

**SPI changes (6.2):**
- `EntityPersister#lock` — parameter changed from `SharedSessionContractImplementor` to `EventSource`
- `EntityPersister#multiLoad` — same parameter change
- `Executable#afterDeserialize` — same parameter change
- Use `session.asEventSource()` for the conversion

#### Package relocations (SPI/internal)

- `org.hibernate.loader.custom.*` → `org.hibernate.query.sql.*`
- `org.hibernate.loader.collection.*` → `org.hibernate.loader.ast.*`
- `org.hibernate.loader.entity.*` / `org.hibernate.loader.plan.*` → removed
- `org.hibernate.sql.ordering.*` → `org.hibernate.metamodel.mapping.ordering.ast.*`

### Part 2: Pattern changes

1. Read `references/pattern-map.md`
2. Key areas to search and transform:

**HQL query changes:**
- Remove `from` keyword in UPDATE statements: `update from Entity` → `update Entity`
- Replace collection pseudo-attributes with functions: `.size` → `size()`, `.elements` → `value()`
- Fix 0-based ordinal parameters to 1-based: `?0` → `?1`
- Wrap pass-through tokens with `sql()`: `octets` → `sql('octets')`
- Remove unnecessary `distinct` from join-fetch queries
- Replace FK-value association comparisons: `where e.assoc = 1` → `where e.assoc.id = 1`

**Type mapping changes (may require schema migration):**
- Duration: `BIGINT` → `INTERVAL_SECOND`/`numeric(21)`
- UUID: `binary(16)` → native `uuid` type
- Instant: `TIMESTAMP` → `TIMESTAMP_UTC`
- Arrays (6.1): `VARBINARY` → `ARRAY`/`JSON`/`XML`
- Enums (6.1/6.2): `TINYINT` → `SMALLINT` (or context-dependent in 6.2)

**ID generation:**
- Replace `hibernate_sequence` with per-entity sequences in `import.sql`
- Ensure database has `<entity_name>_seq` sequences

3. Run the build gate

## Build gate

Run `mvn compile`. Check for:
- Remaining `javax.persistence.*` imports
- `org.hibernate.criterion.*` references (legacy Criteria)
- `@TypeDef` / `@TypeDefs` annotations
- Version-specific or spatial-specific dialect class references
- `org.hibernate.type.descriptor.sql.SqlTypeDescriptor` references
