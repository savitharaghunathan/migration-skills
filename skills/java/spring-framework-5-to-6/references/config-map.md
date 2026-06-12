# Config Map — Spring Framework 5 to 6

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| (BeanInfoFactory in META-INF/spring.factories) | org.springframework.beans.BeanInfoFactory=org.springframework.beans.ExtendedBeanInfoFactory | true | ExtendedBeanInfoFactory | SimpleBeanInfoFactory | META-INF/spring.factories | Core container no longer uses `java.beans.Introspector` by default; add `ExtendedBeanInfoFactory` for full 5.3.x compat; use `SimpleBeanInfoFactory` on 5.3 for forward compat | Core Container (6.0) |
| spring.cache.reactivestreams.ignore | spring.cache.reactivestreams.ignore | | false | false | spring.properties | Set to `true` to revert to synchronous caching of `Mono.cache()`/`Flux.cache()` results instead of 6.1-style async caching | Core Container (6.1) |
| spring.jdbc.getParameterType.ignore | spring.jdbc.getParameterType.ignore | true | false | true | spring.properties | JDBC `setNull` bypasses `getParameterType` on PostgreSQL and MS SQL Server by default; set to `false` to restore full resolution | Data Access and Transactions (6.1) |
| spring.context.expression.maxLength | spring.context.expression.maxLength | | (unlimited) | (configurable) | spring.properties | Maximum length of SpEL expressions in ApplicationContext | SpEL (6.1) |
| spring.test.aot.processing.failOnError | spring.test.aot.processing.failOnError | true | false | true | system property | Build-time AOT processing now fails on error by default; set to `false` to allow processing to continue | Testing (6.1) |
| spring.locking.strict | spring.locking.strict | | false | false | spring.properties | Override internal locking to strict mode (restores 6.1 behavior) when background bean initialization causes issues | Core Container (6.2) |
| spring.expression.maxOperations | spring.expression.maxOperations | true | (unlimited) | 10000 | system property | Since 6.2.19, SpEL expression evaluation capped at 10,000 operations; configurable via `SpelParserConfiguration` or this property | SpEL (6.2) |
