# Phase: Configuration Migration

Migrate Spring Framework system properties and internal configuration files.

## Steps

1. Read `references/config-map.md`
2. Unlike Spring Boot properties, Spring Framework configuration is spread across:
   - `META-INF/spring.factories` — bean info factory registration
   - JVM system properties (`-D` flags)
   - `spring.properties` file at classpath root
   - Code-level property settings
3. For each row in the config map:
   - **BeanInfoFactory (6.0):** Check `META-INF/spring.factories` for `org.springframework.beans.BeanInfoFactory`. If using `ExtendedBeanInfoFactory` explicitly, verify whether the new default `SimpleBeanInfoFactory` works. For full backwards compatibility, add `ExtendedBeanInfoFactory` entry.
   - **spring.cache.reactivestreams.ignore (6.1):** If using `@Cacheable` with reactive return types (`Mono`/`Flux`) and relying on synchronous caching of `Mono.cache()` results, set to `true` in `spring.properties`.
   - **spring.jdbc.getParameterType.ignore (6.1):** Default changed to `true` on PostgreSQL and SQL Server. If seeing side effects with null parameters, set to `false`.
   - **spring.test.aot.processing.failOnError (6.1):** Now `true` by default. Set to `false` in test JVM args if AOT processing should continue after errors.
   - **spring.locking.strict (6.2):** Set to `true` if background bean initialization causes issues with internal locking.
   - **spring.expression.maxOperations (6.2):** Default 10,000 since 6.2.19. Increase if complex SpEL expressions are truncated.
4. Run the build gate

## Build gate

Run the project build command. Configuration errors often surface as startup failures rather than compilation errors — if the build passes but the application fails to start, check:
- `META-INF/spring.factories` entries
- Missing `-parameters` compiler flag causing dependency injection failures
- SpEL expression evaluation hitting the new 10,000 operation limit
