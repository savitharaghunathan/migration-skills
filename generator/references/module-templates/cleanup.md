# Phase: Cleanup and Verification

Remove compatibility shims, dead code, and verify the final build.

## Steps

1. **Remove compatibility imports:** Search for any remaining imports of old packages that should have been migrated in earlier phases. These indicate missed transformations — go back and fix them rather than leaving them.

2. **Remove suppression annotations:** Search for `@SuppressWarnings`, `// NOPMD`, `// NOSONAR`, or similar suppressions that were added to work around old API issues.

3. **Clean up deprecated usage:** Search for `@Deprecated` or `@SuppressWarnings("deprecation")` that referenced APIs from the source version.

4. **Verify no old artifacts remain:**
   - Search the entire codebase (including build files, config, and source) for old package names, artifact coordinates, and property names from the mapping tables
   - Any remaining references are migration gaps — fix them

5. **Final build and test:**
   - Run the full build: compilation + tests
   - If the build fails, consult `references/verify-errors.md` for known error-fix mappings before attempting ad-hoc fixes
   - If the project has integration tests, run those too

6. **Smoke test:**
   - Run the smoke command from the skill metadata `smoke_tool` (if present)
   - The smoke command starts the app, waits for readiness, probes the health endpoint, and tears down — this catches runtime wiring errors (dependency injection, missing drivers, framework initialization) that the build gate cannot detect
   - If the smoke fails, consult `references/verify-errors.md` and apply the same fix loop as the build gate

7. **Report to user:**
   - List all changes made across all phases
   - Flag any items that could not be automatically migrated (require manual review)
   - Note any behavioral changes from `pattern-map.md` that the user should verify manually
