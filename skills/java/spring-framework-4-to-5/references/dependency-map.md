# Dependency Map — Spring Framework 4 to 5

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| org.springframework:spring-webmvc-portlet | removed | | remove | Portlet support dropped entirely in 5.0 | Dropped Support (5.0) |
| org.springframework:spring-web (Velocity support) | removed | | remove | Velocity support dropped; migrate templates to FreeMarker or Thymeleaf | Dropped Support (5.0) |
| net.sf.jasperreports:jasperreports | removed | | remove | JasperReports support dropped entirely | Dropped Support (5.0) |
| org.apache.xmlbeans:xmlbeans | removed | | remove | XMLBeans support dropped entirely | Dropped Support (5.0) |
| javax.jdo:jdo-api | removed | | remove | JDO support dropped entirely | Dropped Support (5.0) |
| com.google.guava:guava (caching) | com.github.ben-manes.caffeine:caffeine | | replace | Guava caching support replaced by Caffeine | Dropped Support (5.0) |
| commons-logging:commons-logging | org.springframework:spring-jcl | | replace | spring-jcl is a built-in bridge in spring-core; remove commons-logging excludes and jcl-over-slf4j if using SLF4J | Commons Logging Setup (5.0) |
| org.slf4j:jcl-over-slf4j | org.springframework:spring-jcl | | replace | spring-jcl auto-detects SLF4J; jcl-over-slf4j can be removed (spring-jcl supersedes it) | Commons Logging Setup (5.0) |
| org.apache.tiles:tiles-* (tiles2) | org.apache.tiles:tiles-* (tiles3) | >= 3.0 | replace | `org.springframework.web.view.tiles2` package removed; minimum Tiles 3 required | Removed Packages (5.0) |
| org.hibernate:hibernate-core (3.x/4.x) | org.hibernate:hibernate-core | >= 5.0 | replace | `orm.hibernate3` and `orm.hibernate4` packages removed; Hibernate 5.0+ required | Removed Packages (5.0) |
| com.fasterxml.jackson.core:jackson-databind | com.fasterxml.jackson.core:jackson-databind | >= 2.9 | replace | Jackson 2.9+ required (5.0), 2.9.7+ required (5.2) | Libraries (5.0/5.2) |
| org.ehcache:ehcache | org.ehcache:ehcache | >= 2.10 | replace | EhCache 2.10+ required | Libraries (5.0) |
| com.squareup.okhttp3:okhttp | com.squareup.okhttp3:okhttp | >= 3.0 | replace | OkHttp 3.0+ required | Libraries (5.0) |
| io.projectreactor.kotlin:reactor-kotlin-extensions | io.projectreactor.kotlin:reactor-kotlin-extensions | | replace | Kotlin extensions moved from reactor-core to dedicated project in Reactor 3.3 (5.2) | Libraries (5.2) |
| org.hibernate:hibernate-core | org.hibernate:hibernate-core | >= 5.2 | replace | Hibernate ORM 5.2+ baseline in 5.3; focus on 5.4.x; Hibernate Search must be 5.11.6+ | Third-Party APIs (5.3) |
| io.reactivex:rxjava | io.reactivex.rxjava2:rxjava | | replace | RxJava 1.x deprecated in 5.3; RxJava 2.x is new baseline, RxJava 3 also supported | Third-Party APIs (5.3) |
