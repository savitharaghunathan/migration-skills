# Phase: Cleanup and Verification

Remove compatibility shims and verify the final build.

## Steps

1. Search for any remaining imports of old packages (`org.springframework.lang.Nullable`, etc.)
2. Remove suppression annotations added for old API workarounds
3. Search entire codebase for old artifact names, property names, and API names from the mapping tables
4. Run full build and test suite: `mvn verify` (or `gradle build test`)
5. Report all changes made and any items requiring manual review
