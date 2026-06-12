# Phase: Configuration Migration

Migrate configuration properties and settings to their new defaults and locations.

## Steps

1. Read `references/config-map.md`
2. Identify all configuration sources:
   - `application.properties`, `application.yml`, `application-*.properties`, `application-*.yml`
   - `applicationContext.xml`, `beans.xml`, other Spring XML config files
   - Java-based `@Configuration` classes with programmatic config
   - JVM system properties (`-D` flags)

### CORS configuration
- Check all `@CrossOrigin` annotations and `WebMvcConfigurer#addCorsMappings` calls
- If using cookies or authentication in CORS requests, set `allowCredentials = "true"` explicitly (default changed from implicit `true` to `false` in 5.0)
- In 5.3: if `allowCredentials=true`, replace `allowedOrigins("*")` with `allowedOriginPatterns("*")` — wildcard origins not allowed with credentials

### XML schema versioning
- Search all Spring XML files for versioned schema locations (e.g., `spring-beans-4.3.xsd`)
- Update to unversioned schemas (e.g., `spring-beans.xsd`) — they resolve to the latest XSD automatically
- Version-specific declarations still work but deprecated features are removed from latest schema

### Suffix pattern matching (5.3)
- If `RequestMappingHandlerMapping` is configured with `setUseSuffixPatternMatch(true)`, remove it — disabled by default
- If using path extensions for content negotiation, switch to `Accept` header or query parameter

### WebFlux codec limits (5.2+)
- If using WebFlux, `maxInMemorySize` now defaults to 256K for all `Decoder`/`HttpMessageReader` implementations
- Configure higher limits if processing large payloads: `CodecConfigurer.defaultCodecs().maxInMemorySize(512 * 1024)`

### New system properties (5.3)
- `spring.spel.ignore=true` — opt-in to remove SpEL support for apps not using it
- `spring.xml.ignore=true` — opt-in to remove XML support including converters and codecs

### Test properties
- `spring.test.constructor.autowire.mode` — configure autowiring for test constructors (5.2)
- `spring.test.enclosing.configuration=override` — restore pre-5.3 behavior for `@Nested` test classes (5.3)

3. Run the build gate

## Build gate

Run `mvn compile` or `gradle build`. Configuration errors often surface as startup failures rather than compilation errors — start the application to verify CORS and XML config changes.
