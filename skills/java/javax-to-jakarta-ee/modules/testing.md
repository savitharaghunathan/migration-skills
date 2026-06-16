# Phase: Testing Migration

Update test code imports, test configuration files, and test utilities.

## Steps

1. Read `references/api-map.md` — apply all package renames to test source directories
2. Read `references/config-map.md` — apply property renames to test configuration files

### Test source code

Apply the same `javax.*` → `jakarta.*` package renames to all test source files (`src/test/java/**`):
- Test entity classes and DTOs
- Integration test configurations
- Mock objects and test utilities using EE APIs
- Arquillian test classes
- Test servlets and filters

### Test configuration files

Update test-specific configuration:
- `src/test/resources/META-INF/persistence.xml` — same namespace and property changes as main
- `src/test/resources/beans.xml` — update namespace
- `src/test/resources/web.xml` — update namespace (for integration tests)

### Arquillian tests

If using Arquillian for container testing:
- Update Arquillian dependencies to Jakarta EE compatible versions
- Update container adapters to Jakarta EE compatible versions
- Update `ShrinkWrap` deployment descriptors embedded in tests

### Test framework compatibility

Verify test frameworks are Jakarta-compatible:
- **JUnit 5** — no changes needed (framework-independent)
- **Mockito** — works with Jakarta (no namespace dependency)
- **REST Assured** — version 5.x+ supports Jakarta
- **Selenium/WebDriver** — no changes needed

5. Run the full test suite

## Build gate

Run `mvn test` (or `gradle test`). Test failures may indicate:
- Remaining `javax.*` imports in test classes
- Test persistence.xml with old `javax.persistence.*` properties
- Arquillian container adapter incompatibility with Jakarta EE
- Third-party test libraries still on javax namespace
