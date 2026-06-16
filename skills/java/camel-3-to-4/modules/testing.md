# Phase: Testing Migration

Migrate all test classes from JUnit 4 to JUnit 5 and update Camel test support.

## Steps

1. Read `references/api-map.md` — filter for rows where `source_section` relates to testing
2. Read `references/dependency-map.md` — filter for test-scoped dependencies

### Test dependency migration

Update test framework dependencies in the build file:
- `org.apache.camel:camel-test` → `org.apache.camel:camel-test-junit5`
- All JUnit 4 `camel-test-*` modules have JUnit 5 equivalents
- Ensure JUnit 5 dependencies are present (`org.junit.jupiter:junit-jupiter`)

### Test class migration

Search test source directories (`src/test/java/`) for JUnit 4 patterns and migrate:

**Import changes:**
```java
// Before
import org.apache.camel.test.junit4.CamelTestSupport;
import org.junit.Test;
import org.junit.Before;

// After
import org.apache.camel.test.junit5.CamelTestSupport;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.BeforeEach;
```

**Annotation changes:**
- `@org.junit.Test` → `@org.junit.jupiter.api.Test`
- `@org.junit.Before` → `@org.junit.jupiter.api.BeforeEach`
- `@org.junit.After` → `@org.junit.jupiter.api.AfterEach`
- `@org.junit.BeforeClass` → `@org.junit.jupiter.api.BeforeAll`
- `@org.junit.AfterClass` → `@org.junit.jupiter.api.AfterAll`
- `@org.junit.Ignore` → `@org.junit.jupiter.api.Disabled`
- `@org.junit.runner.RunWith` → `@org.junit.jupiter.api.extension.ExtendWith`

**Assertion changes:**
- `org.junit.Assert.*` → `org.junit.jupiter.api.Assertions.*`
- Note: assertion parameter order differs in JUnit 5 (message is last, not first)

### CamelTestSupport changes

Verify that test classes extending `CamelTestSupport` work with the JUnit 5 version. The API is largely the same but lifecycle hooks differ.

3. Run the full test suite

## Build gate

Run `mvn test` (or `gradle test`). Test failures may indicate:
- Missed JUnit 4 → 5 annotation migrations
- Assertion parameter order differences
- CamelTestSupport lifecycle changes
- Behavioral changes in components under test (check pattern-map behavioral entries)
