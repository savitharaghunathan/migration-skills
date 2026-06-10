# Dependency Map — Spring Boot 3 to 4

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| org.springframework.boot:spring-boot-starter-parent | org.springframework.boot:spring-boot-starter-parent | >= 4.0.0 | replace | Update version to 4.0.x; requires Java 17+, Jakarta EE 11, Servlet 6.1, Spring Framework 7.x | Before You Start |
| org.springframework.boot:spring-boot-starter-web | org.springframework.boot:spring-boot-starter-webmvc | >= 4.0.0 | rename | Deprecated; use spring-boot-starter-webmvc instead | Deprecated Starters |
| org.springframework.boot:spring-boot-starter-web-services | org.springframework.boot:spring-boot-starter-webservices | >= 4.0.0 | rename | Deprecated; renamed for consistency | Deprecated Starters |
| org.springframework.boot:spring-boot-starter-oauth2-authorization-server | org.springframework.boot:spring-boot-starter-security-oauth2-authorization-server | >= 4.0.0 | rename | Deprecated; moved under security namespace | Deprecated Starters |
| org.springframework.boot:spring-boot-starter-oauth2-client | org.springframework.boot:spring-boot-starter-security-oauth2-client | >= 4.0.0 | rename | Deprecated; moved under security namespace | Deprecated Starters |
| org.springframework.boot:spring-boot-starter-oauth2-resource-server | org.springframework.boot:spring-boot-starter-security-oauth2-resource-server | >= 4.0.0 | rename | Deprecated; moved under security namespace | Deprecated Starters |
| org.springframework.boot:spring-boot-starter-aop | org.springframework.boot:spring-boot-starter-aspectj | >= 4.0.0 | rename | Renamed to clarify scope; review if you actually need AspectJ | AOP Starter POM |
| io.undertow:undertow-core | removed | | remove | Undertow not compatible with Servlet 6.1; use Tomcat or Jetty | Undertow |
| org.springframework.boot:spring-boot-starter-undertow | removed | | remove | Undertow support dropped in SB4 | Undertow |
| org.springframework.pulsar:spring-pulsar-reactive | removed | | remove | Pulsar Reactive auto-configuration removed | Pulsar Reactive |
| org.springframework.session:spring-session-hazelcast | removed | | remove | Now under Hazelcast team leadership; add manually if needed | Spring Session Hazelcast |
| org.springframework.session:spring-session-mongodb | removed | | remove | Now under MongoDB team leadership; add manually if needed | Spring Session MongoDB |
| org.flywaydb:flyway-core | org.springframework.boot:spring-boot-starter-flyway | >= 4.0.0 | replace | Direct Flyway dependency must be replaced with starter | Module Dependencies |
| org.liquibase:liquibase-core | org.springframework.boot:spring-boot-starter-liquibase | >= 4.0.0 | replace | Direct Liquibase dependency must be replaced with starter | Module Dependencies |
| org.springframework.boot:spring-boot-starter-test | org.springframework.boot:spring-boot-starter-test | >= 4.0.0 | replace | Test starters are now per-technology; spring-boot-starter-test still exists but technology-specific test starters are preferred | Test Code |
| org.springframework.boot:spring-boot-starter-batch | org.springframework.boot:spring-boot-starter-batch | >= 4.0.0 | replace | Spring Batch now operates without DB by default; use spring-boot-starter-batch-jdbc for DB metadata storage | Spring Batch |
| org.springframework.boot:spring-boot-test-mock | removed | | remove | MockitoTestExecutionListener removed; use MockitoExtension from Mockito | Mockito Captor and Mock Annotations |
| org.springframework.boot:spring-boot-starter-tomcat | org.springframework.boot:spring-boot-starter-tomcat-runtime | >= 4.0.0 | rename | For WAR deployments only; use spring-boot-starter-tomcat-runtime | Tomcat |
| org.elasticsearch.client:elasticsearch-rest-client | co.elastic.clients:elasticsearch-java | | replace | Low-level RestClient replaced by Rest5Client in elasticsearch-java | Elasticsearch Client |
| org.elasticsearch.client:elasticsearch-rest-client-sniffer | removed | | remove | Sniffer now built into elasticsearch-java module | Elasticsearch Client |
| org.hibernate.orm:hibernate-jpamodelgen | org.hibernate.orm:hibernate-processor | | rename | Renamed in Hibernate 7 | Hibernate Dependency Management |
| org.hibernate:hibernate-proxool | removed | | remove | No longer published | Hibernate Dependency Management |
| org.hibernate:hibernate-vibur | removed | | remove | No longer published | Hibernate Dependency Management |
| org.springframework.retry:spring-retry | removed | | remove | Dependency management removed; use Spring Framework retry or add explicit version | Dependency Management for Spring Retry |
| org.springframework.security:spring-security-oauth2-authorization-server | removed | | remove | Now part of Spring Security 7; use spring-security.version to override | Dependency Management for Spring Authorization Server |
| com.fasterxml.jackson.core:jackson-databind | tools.jackson.core:jackson-databind | >= 3.0.0 | replace | Jackson 3 uses new group IDs; exception: jackson-annotations stays at com.fasterxml.jackson.core | Upgrading Jackson |
| com.fasterxml.jackson.core:jackson-core | tools.jackson.core:jackson-core | >= 3.0.0 | replace | Jackson 3 group ID change | Upgrading Jackson |
| com.fasterxml.jackson.datatype:jackson-datatype-jsr310 | tools.jackson.datatype:jackson-datatype-jsr310 | >= 3.0.0 | replace | Jackson 3 group ID change | Upgrading Jackson |
| com.fasterxml.jackson.module:jackson-module-kotlin | tools.jackson.module:jackson-module-kotlin | >= 3.0.0 | replace | Jackson 3 group ID change | Upgrading Jackson |
| com.fasterxml.jackson.dataformat:jackson-dataformat-yaml | tools.jackson.dataformat:jackson-dataformat-yaml | >= 3.0.0 | replace | Jackson 3 group ID change | Upgrading Jackson |
| com.fasterxml.jackson.dataformat:jackson-dataformat-xml | tools.jackson.dataformat:jackson-dataformat-xml | >= 3.0.0 | replace | Jackson 3 group ID change | Upgrading Jackson |
