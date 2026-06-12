# Dependency Map — Spring Framework 5 to 6

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| org.springframework:spring-remoting | removed | | remove | RPC-style remoting removed: Hessian, HTTP Invoker, JMS Invoker, JAX-WS support all gone | Removed APIs (6.0) |
| org.springframework:spring-context (Joda-Time support) | removed | | remove | `org.springframework.format.datetime` Joda-Time package removed; migrate to `java.time` types | Removed APIs (6.0) |
| org.springframework:spring-context-support (EhCache 2) | removed | | remove | `org.springframework.cache.ehcache` package removed; use EhCache 3 via JCache API or native API | Removed APIs (6.0) |
| net.sf.ehcache:ehcache | org.ehcache:ehcache:jakarta | | replace | EhCache 2 approaching EOL; switch to EhCache 3 with `jakarta` classifier | Removed APIs (6.0) |
| io.micrometer:micrometer-observation | io.micrometer:micrometer-observation | >= 1.10 | replace | Now a compile dependency of `spring-web` module; must be on classpath | Observability (6.0) |
| io.r2dbc:r2dbc-spi | io.r2dbc:r2dbc-spi | >= 1.0 | replace | Spring Framework 6.0 upgrades to R2DBC 1.0 | Data Access and Transactions (6.0) |
| org.yaml:snakeyaml | org.yaml:snakeyaml | >= 2.0 | replace | Minimum version raised in 6.1 | Baseline Upgrades (6.1) |
| com.fasterxml.jackson.core:jackson-databind | com.fasterxml.jackson.core:jackson-databind | >= 2.14 | replace | Minimum version raised in 6.1; 2.18/2.19 recommended in 6.2 | Baseline Upgrades (6.1) |
| org.freemarker:freemarker | org.freemarker:freemarker | >= 2.3.33 | replace | Minimum version raised in 6.2 | Baseline Upgrades (6.2) |
| net.sourceforge.htmlunit:htmlunit | net.sourceforge.htmlunit:htmlunit | >= 4.2 | replace | HtmlUnit 2.x → 3.x/4.x migration required; Selenium driver coordinates changed to `org.seleniumhq.selenium:htmlunit3-driver` | Testing (6.2) |
| org.webjars:webjars-locator-core | org.webjars:webjars-locator-lite | | replace | `WebJarsResourceResolver` deprecated; use `LiteWebJarsResourceResolver` with `webjars-locator-lite` | Web Applications (6.2) |
| org.springframework:spring-connector (JCA CCI) | removed | | remove | JCA CCI support removed in 6.0 | Data Access and Transactions (6.0) |
