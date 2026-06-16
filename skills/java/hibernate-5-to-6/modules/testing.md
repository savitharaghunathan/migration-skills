# Phase: Testing Migration

Update test configurations and verify behavioral changes that surface as test failures.

## Steps

1. Read `references/api-map.md` — filter for test-related changes
2. Read `references/pattern-map.md` — filter for behavioral changes that affect test assertions

### Test configuration updates

**persistence.xml in test resources:**
- Update namespace: `xmlns="http://xmlns.jcp.org/xml/ns/persistence"` → `xmlns="https://jakarta.ee/xml/ns/persistence"`
- Update version: `version="2.2"` → `version="3.0"`
- Update all `javax.persistence.*` property names to `jakarta.persistence.*`

**Test imports:**
- All `javax.persistence.*` imports in test code must change to `jakarta.persistence.*`
- This includes test entity classes, test configurations, and JPA-related test utilities

### Query result type changes

Tests asserting on query result types may fail:
- **BIGINT/count() results:** Previously returned `BigInteger`, now returns `Long`
  ```java
  // Before
  BigInteger count = (BigInteger) session.createNativeQuery("select count(*) from Person").getSingleResult();
  // After
  Long count = (Long) session.createNativeQuery("select count(*) from Person").getSingleResult();
  ```

- **Join queries without select:** Previously returned `List<Object[]>`, now returns entity list
  ```java
  // Before
  List<Object[]> results = session.createQuery("from Person p join p.address").list();
  // After
  List<Person> results = session.createQuery("from Person p join p.address", Person.class).list();
  ```

### Stream resource management

Tests using `query.stream()` must explicitly close the stream:
```java
// Before
query.stream().forEach(e -> ...);

// After
try (Stream<MyEntity> s = query.stream()) {
    s.forEach(e -> ...);
}
```

### Schema validation in tests

If tests use `hbm2ddl.auto=validate`, they may fail due to changed default type mappings:
- Duration: BIGINT → INTERVAL_SECOND
- UUID: binary(16) → uuid
- Instant: timestamp → timestamp with time zone
- Arrays (6.1): VARBINARY → ARRAY
- Enums (6.1): TINYINT → SMALLINT

Either update test schemas or set backward-compatibility properties in test configuration.

### ID generation in tests

Tests with `import.sql` or programmatic data setup:
- Replace `hibernate_sequence` references with `<entity_name>_seq`
- Account for allocation size of 50 when resetting sequences

### Fetch behavior changes

Tests that assert on SQL query counts may see different numbers due to:
- Width-first fetch circularity (more joins, fewer selects for self-referencing entities)
- Association laziness now properly respected with `@Fetch(FetchMode.JOIN)`
- Batch fetching skipped when LockMode > READ (6.2)

5. Run the full test suite

## Build gate

Run `mvn test` (or `gradle test`). Test failures may indicate:
- Query result type mismatches (BigInteger vs Long)
- Schema validation errors from changed type mappings
- Stream resource leaks (unclosed streams)
- HQL syntax changes (pass-through tokens, FROM in UPDATE, association comparisons)
- ID sequence name mismatches
