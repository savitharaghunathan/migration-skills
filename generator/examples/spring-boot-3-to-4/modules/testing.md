# Phase: Testing Migration

Update test annotations and testing frameworks.

## Steps

1. Read `references/api-map.md` — focus on test-related entries
2. Read `references/dependency-map.md` — focus on test-scoped dependencies
3. Update test dependencies in the build file
4. Search test source directories for old test annotations and replace
5. Run: `mvn test` (or `gradle test`)
