# Phase: Testing Migration

Update test dependencies, APIs, and testing patterns for Spring Framework 6.x.

## Steps

1. Read `references/api-map.md` — filter for rows where `source_section` relates to testing
2. Read `references/dependency-map.md` — filter for test-scoped dependencies (HtmlUnit)
3. **Servlet mock API upgrade:**
   - `MockHttpServletRequest`, `MockHttpSession` and other Servlet mocks now require Servlet 6.0 API on the test classpath
   - Production code may still compile against Servlet 5.0 — only mock-based tests need the 6.0 jar
   - Ensure `jakarta.servlet:jakarta.servlet-api:6.0+` is in test scope
4. **HtmlUnit migration (6.2):**
   - If using HtmlUnit in tests, upgrade from 2.x to 4.2+
   - Update Selenium driver from `org.seleniumhq.selenium:htmlunit-driver` to `org.seleniumhq.selenium:htmlunit3-driver:X.Y.Z`
   - Review breaking API changes: see `htmlunit.sourceforge.io/migration.html`
5. **AOT test processing (6.1):**
   - Build-time AOT processing now fails on error by default
   - If tests fail during AOT processing, set `-Dspring.test.aot.processing.failOnError=false` to allow processing to continue
   - Use `@DisabledInAotMode` to disable specific tests from AOT processing
6. **New test features (6.2):**
   - `@TestBean`, `@MockitoBean`, `@MockitoSpyBean` for bean overriding in tests (framework-level support)
   - AssertJ-based `MvcTester` for MockMvc testing
   - `DynamicPropertyRegistrar` beans for dynamic test properties
7. Run the full test suite

## Build gate

Run the project's test suite:
- Maven: `mvn test`
- Gradle: `gradle test`

Test failures may indicate:
- Servlet API version mismatch (5.0 vs 6.0 on test classpath)
- HtmlUnit API incompatibilities after major version upgrade
- AOT processing errors that were previously silently logged
- Behavioral changes in request handling (trailing slash, validation, etc.)
