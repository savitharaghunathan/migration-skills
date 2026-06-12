# Config Map — Spring Framework 4 to 5

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| allowCredentials (CORS) | allowCredentials (CORS) | true | `true` (implicit) | `false` | Java code (`@CrossOrigin`, `WebMvcConfigurer`) | Must set `allowCredentials=true` explicitly if cookies/auth needed in CORS requests | CORS Support (5.0) |
| XML namespace version declarations | unversioned schemas | | versioned XSD | latest XSD | `applicationContext.xml`, `beans.xml` | XML config namespaces streamlined to unversioned schemas; version-specific still works but resolves to latest | Core Container (5.0) |
| spring.spel.ignore | spring.spel.ignore | | not available | `false` | JVM system property | New in 5.3; set to `true` to remove SpEL support for apps not using it | Core Container (5.3) |
| spring.xml.ignore | spring.xml.ignore | | not available | `false` | JVM system property | New in 5.3; set to `true` to remove XML support including converters and codecs | General Web (5.3) |
| spring.test.constructor.autowire.mode | spring.test.constructor.autowire.mode | | not available | `all` (opt-in) | JVM system property or `spring.properties` | New in 5.2; configures autowiring mode for test constructors with JUnit Jupiter | Testing (5.2) |
| spring.test.enclosing.configuration | spring.test.enclosing.configuration | | not available | `INHERIT` | JVM system property or `spring.properties` | New in 5.3; set to `override` to restore 5.0-5.2 behavior for `@Nested` test classes | Testing (5.3) |
| Suffix pattern matching (MVC) | removed | true | enabled | disabled | Java code (`RequestMappingHandlerMapping` config) | `.*` suffix pattern matching no longer performed by default in 5.3; path extensions no longer used for content negotiation | Suffix Pattern Matching (5.3) |
| maxInMemorySize (WebFlux codecs) | maxInMemorySize (WebFlux codecs) | true | unlimited | `256K` | Java code (`CodecConfigurer`) | All `Decoder`/`HttpMessageReader` implementations limit to 256K by default in 5.2 | WebFlux (5.2) |
