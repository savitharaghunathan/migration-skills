# Phase: Configuration Migration

Migrate configuration properties to their new names, values, or locations.

## Steps

1. Read `references/config-map.md`
2. Identify all configuration files in the project:
   - `application.properties`, `application.yml`
   - `application-*.properties`, `application-*.yml` (profiles)
   - `bootstrap.properties`, `bootstrap.yml`
   - `pom.xml` (for version properties like `spring-authorization-server.version`)
3. For each row in the config map:
   - Search all config files for `old_property`
   - If found:
     - Replace with `new_property` (or remove if `new_property` is `removed`)
     - If `default_changed` is `true`, check whether the project relies on the old default
   - Check `notes` for migration context
4. Key property migrations:
   - `spring.jackson.read/write.*` → `spring.jackson.json.read/write.*`
   - `spring.session.redis.*` → `spring.session.data.redis.*`
   - `spring.session.mongodb.*` → `spring.session.data.mongodb.*`
   - `spring.data.mongodb.*` → `spring.mongodb.*` (13 properties)
   - `management.health.mongo.*` → `management.health.mongodb.*`
   - `spring.dao.exceptiontranslation.enabled` → `spring.persistence.exceptiontranslation.enabled`
   - `spring.kafka.retry.topic.backoff.random` → `spring.kafka.retry.topic.backoff.jitter`
5. Consider adding the `spring-boot-properties-migrator` temporarily:
   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-properties-migrator</artifactId>
       <scope>runtime</scope>
   </dependency>
   ```
   Remove after migration is complete.
6. Run the build gate

## Build gate

Run the project build command. Configuration errors often surface as startup failures rather than compilation errors — if the build passes but the application fails to start, check the config changes.
