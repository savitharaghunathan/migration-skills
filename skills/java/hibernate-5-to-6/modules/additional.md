# Phase: Additional Changes

Handle database schema changes, deployment descriptor updates, and infrastructure changes.

## Steps

1. Read `references/pattern-map.md` — filter for `addition` category and infrastructure-related rows

### Java baseline upgrade

- Java 8 no longer supported
- Java 11 minimum required for Hibernate 6.0
- Java 17 recommended for Hibernate 6.2
- Update CI/CD pipelines, Dockerfiles, and build scripts

### persistence.xml namespace update

Update `persistence.xml` to Jakarta Persistence namespace:
```xml
<!-- Before -->
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence
             http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd"
             version="2.2">

<!-- After -->
<persistence xmlns="https://jakarta.ee/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="https://jakarta.ee/xml/ns/persistence
             https://jakarta.ee/xml/ns/persistence/persistence_3_0.xsd"
             version="3.0">
```

### orm.xml namespace update

If using `orm.xml` mapping files:
```xml
<!-- Before -->
<entity-mappings xmlns="http://xmlns.jcp.org/xml/ns/persistence/orm"
                 version="2.2">

<!-- After -->
<entity-mappings xmlns="https://jakarta.ee/xml/ns/persistence/orm"
                 version="3.0">
```

### Database schema — ID sequence migration

Hibernate 6 creates per-entity sequences instead of a shared `hibernate_sequence`:

1. Generate new sequence DDL by temporarily setting `hbm2ddl.auto=create` or running the schema migration tool
2. For each entity using `@GeneratedValue(strategy=AUTO)` or `@GeneratedValue`:
   - Create sequence `<entity_name>_seq` if it doesn't exist
   - Set the sequence value to at least `max(id) + 1 + allocationSize`:
     ```sql
     CREATE SEQUENCE person_seq START WITH 1 INCREMENT BY 50;
     ALTER SEQUENCE person_seq RESTART WITH 103;
     ```
3. Update `import.sql` files:
   - Replace `nextval('hibernate_sequence')` with `nextval('person_seq')`
   - Add sequence reset statements

### Database schema — column type migration

If using `hbm2ddl.auto=validate`, column type changes may require schema updates:

**Duration columns (6.0):**
```sql
-- Migrate from BIGINT to numeric(21)
ALTER TABLE my_table ALTER COLUMN duration_col TYPE numeric(21);
-- Or for interval second:
-- cast(cast(old as numeric(21,9) / 1000000000) as interval second(9))
```

**UUID columns (6.0):**
```sql
-- On databases with native UUID support:
ALTER TABLE my_table ALTER COLUMN uuid_col TYPE uuid USING uuid_col::uuid;
```

**Instant columns (6.0):**
```sql
-- On databases with timestamp timezone support:
ALTER TABLE my_table ALTER COLUMN instant_col TYPE timestamp with time zone;
```

**Array columns (6.1):**
- Arrays stored via Java serialization (VARBINARY) must be migrated to native ARRAY/JSON/XML types
- Requires data migration: read old serialized data, write back in new format
- Alternative: annotate with `@JdbcTypeCode(SqlTypes.VARBINARY)` per property

**Enum columns (6.1/6.2):**
```sql
-- If schema validation fails for enum columns:
ALTER TABLE my_table ALTER COLUMN enum_col TYPE smallint;
```

### hbm.xml to annotation/orm.xml migration

hbm.xml mapping format is deprecated and will be removed after 6.x:
- Plan migration to annotation-based mappings or JPA `orm.xml` format
- `<property>` with multiple `<column/>` elements → use `@AttributeOverride` with `CompositeUserType`
- `<sql-query callable="true">` → migrate to `<named-stored-procedure-query>` in orm.xml

### Callable hbm.xml migration

Callable named native queries in hbm.xml must migrate to orm.xml:
```xml
<!-- Before (hbm.xml) -->
<sql-query name="simpleScalar" callable="true">
    <return-scalar column="name" type="string"/>
    { ? = call simpleScalar(:number) }
</sql-query>

<!-- After (orm.xml) -->
<named-stored-procedure-query name="simpleScalar" procedure-name="simpleScalar">
    <parameter class="java.lang.Integer" mode="IN" name="number"/>
    <result-set-mapping>simpleScalar</result-set-mapping>
    <hint name="org.hibernate.callableFunction" value="true"/>
</named-stored-procedure-query>
```

### JMX and JACC removal

- Hibernate no longer provides built-in JMX integration — remove any JMX MBean configuration for Hibernate
- Hibernate no longer provides built-in JACC integration — remove any JACC security configuration

2. Run the build gate

## Build gate

Run `mvn compile` and start the application. Verify:
- Hibernate SessionFactory initializes without schema validation errors
- ID sequences exist and have correct values
- All entity operations (CRUD) work correctly
- No ClassNotFoundException from missing dialect classes
