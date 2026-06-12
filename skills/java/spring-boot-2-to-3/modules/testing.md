# Phase: Testing Migration

Update test annotations, frameworks, and testing patterns.

## Steps

1. Read `references/api-map.md` — filter for rows where `source_section` relates to testing or Spring Security
2. Read `references/dependency-map.md` — filter for test-scoped dependencies
3. For test dependencies:
   - Update test framework dependencies in the build file
4. For test API changes:
   - Search test source directories (`src/test/`) for old APIs
   - Key transformations:
     - **Jakarta namespace:** All `javax.*` imports in tests must change to `jakarta.*`
     - **Spring Security:** `WebSecurityConfigurerAdapter` removal — tests extending it need refactoring to use `SecurityFilterChain` beans
     - **Spring Security matchers:** `antMatchers` → `requestMatchers` in test security configurations
     - **Servlet mocks:** Test classpath must have Servlet 6.0 compatible mocks
     - **Hibernate queries:** Test HQL/Criteria queries must use 1-based ordinal parameters, no collection pseudo-attributes
5. Run the full test suite (not just compilation)

## Build gate

Run the project's test suite:
- Java Maven: `mvn test`
- Java Gradle: `gradle test`

Test failures may indicate migration issues in the code phase that weren't caught by compilation alone. Common test-specific issues:
- Hibernate query changes (result row type, DISTINCT behavior, association comparisons)
- Spring Security filter chain configuration differences
- Actuator endpoint name changes in integration tests (httptrace → httpexchanges)
- Flyway checksum mismatches in test migrations — run `flyway repair`
