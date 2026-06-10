# Phase: Testing Migration

Update test annotations, frameworks, and testing patterns.

## Steps

1. Read `references/api-map.md` — filter for rows where `source_section` relates to testing, or where `old_api` is a test annotation/class
2. Read `references/dependency-map.md` — filter for test-scoped dependencies
3. For test dependencies:
   - Update test framework dependencies in the build file (same process as build-system phase, but for test scope)
4. For test API changes:
   - Search test source directories for `old_api`
   - Apply the same transformation rules as the code phase
   - Pay special attention to:
     - Test runner annotations (`@RunWith`, `@ExtendWith`, etc.)
     - Mock framework changes
     - Integration test configuration annotations
     - Test property/profile handling
5. Run the full test suite (not just compilation)

## Build gate

Run the project's test suite:
- Java Maven: `mvn test`
- Java Gradle: `gradle test`
- Go: `go test ./...`
- Python: `pytest` or `python -m pytest`
- .NET: `dotnet test`

Test failures may indicate migration issues in the code phase that weren't caught by compilation alone.
