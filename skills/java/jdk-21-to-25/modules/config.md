# Phase: Configuration Migration

Migrate system properties, JVM options, and configuration to their new values or remove deprecated ones.

## Steps

1. Read `references/config-map.md`
2. Identify all configuration sources in the project:
   - JVM launch scripts (shell scripts, batch files, systemd units, Dockerfiles)
   - `JAVA_OPTS` or `JAVA_TOOL_OPTIONS` environment variables
   - Application server configuration
   - Maven/Gradle JVM args (`jvmArgs`, `argLine`)
   - CI/CD pipeline definitions
   - `java.security` files
   - JNDI configuration files (`jndi.properties`)
3. For each row in the config map:
   - Search all configuration sources for `old_property`
   - If found:
     - Replace with `new_property` (or remove if `new_property` is `removed`)
     - If `default_changed` is `true`, check whether the project relies on the old default:
       - If the property is not explicitly set and the project depends on the old behavior, add it explicitly with `old_default` as the value
       - If the property is explicitly set, just rename it
   - Check `notes` for migration context
4. Pay special attention to:
   - `java.locale.providers` — if set to `JRE` or `COMPAT`, change to `CLDR`
   - `java.security.policy` and `java.security.manager` — remove entirely (Security Manager disabled)
   - `java.naming.rmi.security.manager` — remove (JNDI property removed)
5. Run the build gate

## Build gate

Run the project build command. Configuration errors often surface as startup failures rather than compilation errors — if the build passes but the application fails to start, check the config changes.
