# Phase: Configuration Migration

Migrate configuration properties to their new names, values, or locations.

## Steps

1. Read `references/config-map.md`
2. Identify all configuration files in the project:
   - Java: `application.properties`, `application.yml`, `application-*.properties`, `application-*.yml`, `bootstrap.properties`, `bootstrap.yml`
   - Go: config files referenced in code (varies by framework)
   - Python: `settings.py`, `.env`, `config.py`, `pyproject.toml`
   - .NET: `appsettings.json`, `appsettings.*.json`
3. For each row in the config map:
   - Search all config files for `old_property`
   - If found:
     - Replace with `new_property` (or remove if `new_property` is `removed`)
     - If `default_changed` is `true`, check whether the project relies on the old default:
       - If the property is not explicitly set and the project depends on the old behavior, add it explicitly with `old_default` as the value
       - If the property is explicitly set, just rename it
   - Check `notes` for migration context
4. Run the build gate

## Build gate

Run the project build command. Configuration errors often surface as startup failures rather than compilation errors — if the build passes but the application fails to start, check the config changes.
