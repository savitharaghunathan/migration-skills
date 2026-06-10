# Phase: Configuration Migration

Migrate configuration properties to their new names and values.

## Steps

1. Read `references/config-map.md`
2. Search all config files: `application.properties`, `application.yml`, `application-*.properties`, `application-*.yml`
3. For each row, find `old_property` and replace with `new_property` (or remove if `removed`)
4. If `default_changed` is `true`, check whether the project relies on the old default
5. Run: `mvn compile` (or `gradle build`)
