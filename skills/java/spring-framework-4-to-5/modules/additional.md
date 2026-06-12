# Phase: Additional Changes

Handle runtime, infrastructure, and non-code changes.

## Steps

1. Read `references/pattern-map.md` — filter for `addition` category and infrastructure-related rows

### JDK baseline upgrade
- JDK 8 is the absolute minimum for Spring Framework 5.0
- JDK 11 fully supported from 5.1 (compile with `-target 1.8` for defensive upgrade)
- CGLIB on JDK 11 uses JDK 9+ API for class definition; may log JVM warnings via fallback
- ASM upgraded to 7.0 (5.1) then 9.0 (5.3) for modern bytecode levels

### Servlet container upgrade
- Tomcat 8.5+ (was 7.0+)
- Jetty 9.4+ (was 8.x+)
- WildFly 10+ (was 8.x+)
- WebSphere 9+ (was 8.x+)
- Netty 4.1 and Undertow 1.4 added for WebFlux

### Java EE 7 baseline
- Servlet 3.1 required (4.0 supported at runtime)
- JPA 2.1 required (2.2 supported)
- Bean Validation 1.1 required (2.0 supported)
- JMS 2.0 required
- JSON Binding API 1.0 supported as Jackson/Gson alternative

### Reactive stack (Spring WebFlux)
- New `spring-webflux` module available as alternative to `spring-webmvc`
- `WebClient` replaces `AsyncRestTemplate` for non-blocking HTTP
- Reactive types (`Mono`, `Flux`) supported as MVC controller return values
- `ReactiveTransactionManager` SPI added in 5.2

### R2DBC integration (5.3)
- New `spring-r2dbc` module with core R2DBC support and `R2dbcTransactionManager`
- Native reactive database access without Spring Data dependency

### Forwarded header handling (5.1+)
- Configure `ForwardedHeaderFilter` (Servlet) or `ForwardedHeaderTransformer` (WebFlux) for proxy environments
- Forwarded/X-Forwarded-* headers no longer checked individually in framework internals
- Filter supports safe mode to check and discard untrusted headers

### Hibernate ORM integration
- Read-only transactions set `Session.setDefaultReadOnly(true)` by default (5.1) — reduces memory
- `HibernateJpaVendorAdapter` exposes `Session(Factory)` as `EntityManager(Factory)` extension interface (5.1)
- Hibernate Search must be 5.11.6+ for 5.3 compatibility

### Remoting deprecation (5.3)
- Hessian, RMI, HTTP Invoker, JMS Invoker all deprecated with no replacement
- Plan migration to REST APIs or messaging before upgrading to 6.0 (where these are removed)

### Component index (5.0)
- Optional: add `spring-context-indexer` annotation processor for faster startup
- Generates `META-INF/spring.components` at compile time as alternative to classpath scanning

2. Run the build gate

## Build gate

Run `mvn compile` or `gradle build`. Also verify the application starts successfully — runtime changes (JDK version, servlet container, Hibernate version) surface at startup.
