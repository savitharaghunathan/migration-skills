# Dependency Map — Spring Boot 2 to 3

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| javax.servlet:javax.servlet-api | jakarta.servlet:jakarta.servlet-api | >= 6.0 | replace | Jakarta EE 10 migration; all javax.servlet imports must change to jakarta.servlet | Jakarta EE |
| javax.persistence:javax.persistence-api | jakarta.persistence:jakarta.persistence-api | >= 3.1 | replace | JPA namespace migration javax.persistence → jakarta.persistence | Jakarta EE |
| javax.validation:validation-api | jakarta.validation:jakarta.validation-api | >= 3.0 | replace | Bean Validation namespace migration | Jakarta EE |
| javax.annotation:javax.annotation-api | jakarta.annotation:jakarta.annotation-api | >= 2.1 | replace | Common annotations namespace migration | Jakarta EE |
| javax.mail:javax.mail-api | jakarta.mail:jakarta.mail-api | >= 2.1 | replace | Mail API namespace migration | Jakarta EE |
| javax.transaction:javax.transaction-api | jakarta.transaction:jakarta.transaction-api | >= 2.0 | replace | JTA namespace migration | Jakarta EE |
| javax.websocket:javax.websocket-api | jakarta.websocket:jakarta.websocket-api | >= 2.1 | replace | WebSocket namespace migration | Jakarta EE |
| javax.xml.bind:jaxb-api | jakarta.xml.bind:jakarta.xml.bind-api | >= 4.0 | replace | JAXB namespace migration | Jakarta EE |
| javax.inject:javax.inject | jakarta.inject:jakarta.inject-api | >= 2.0 | replace | CDI/Inject namespace migration | Jakarta EE |
| org.apache.httpcomponents:httpclient | org.apache.httpcomponents.client5:httpclient5 | >= 5.0 | replace | Package relocated from org.apache.http to org.apache.hc; RestTemplate falls back to JDK client if not on classpath | Apache HttpClient in RestTemplate |
| mysql:mysql-connector-java | com.mysql:mysql-connector-j | | rename | MySQL JDBC driver coordinates changed | MySQL JDBC Driver |
| org.hibernate:hibernate-core | org.hibernate.orm:hibernate-core | >= 6.1 | rename | Hibernate group ID changed from org.hibernate to org.hibernate.orm | Hibernate 6.1 |
| org.hibernate:hibernate-envers | org.hibernate.orm:hibernate-envers | >= 6.1 | rename | Hibernate Envers group ID changed | Hibernate 6.1 |
| org.hibernate:hibernate-jpamodelgen | org.hibernate.orm:hibernate-jpamodelgen | >= 6.1 | rename | Hibernate JPA Metamodel Generator group ID changed | Hibernate 6.1 |
| de.flapdoodle.embed:de.flapdoodle.embed.mongo | removed | | remove | Auto-configuration removed; use flapdoodle spring library or Testcontainers instead | Embedded MongoDB |
| io.r2dbc:r2dbc-bom | removed | | remove | R2DBC 1.0 no longer publishes a BOM; use individual module version properties | R2DBC 1.0 |
| org.elasticsearch.client:elasticsearch-rest-high-level-client | co.elastic.clients:elasticsearch-java | | replace | High-level REST client removed; use new Elasticsearch Java client | Elasticsearch Clients and Templates |
| org.apache.activemq:activemq-broker | removed | | remove | Apache ActiveMQ support removed in Spring Boot 3.0 | Other Removals |
| com.atomikos:transactions-jta | removed | | remove | Atomikos JTA support removed | Other Removals |
| net.sf.ehcache:ehcache | removed | | remove | EhCache 2 support removed; use Ehcache 3 with jakarta classifier | Other Removals |
| com.hazelcast:hazelcast | com.hazelcast:hazelcast | | replace | Hazelcast 3 support removed; requires Hazelcast 5.x | Other Removals |
| org.ehcache:ehcache | org.ehcache:ehcache:jakarta | | replace | Ehcache 3 now requires jakarta classifier for Jakarta EE 9+ support | Ehcache3 |
| org.ehcache:ehcache-transactions | org.ehcache:ehcache-transactions:jakarta | | replace | Ehcache transactions module now requires jakarta classifier | Ehcache3 |
| org.apache.johnzon:johnzon-core | org.eclipse:yasson | | replace | Apache Johnzon dependency management removed in favor of Eclipse Yasson | JSON-B |
| antlr:antlr | removed | | remove | ANTLR 2 dependency management removed | ANTLR 2 |
| io.reactivex:rxjava | io.reactivex.rxjava3:rxjava | | replace | RxJava 1.x dependency management removed; RxJava 3 added | RxJava |
| io.reactivex.rxjava2:rxjava | io.reactivex.rxjava3:rxjava | | replace | RxJava 2.x dependency management removed; RxJava 3 added | RxJava |
| com.hazelcast:hazelcast-hibernate53 | removed | | remove | Hazelcast Hibernate dependency management removed; consider org.hibernate.orm:hibernate-jcache | Hazelcast Hibernate Removed |
| org.apache.solr:solr-solrj | removed | | remove | Apache Solr support removed; Jetty-based client incompatible with Jetty 11 | Other Removals |
| pl.project13.maven:git-commit-id-plugin | io.github.git-commit-id:git-commit-id-maven-plugin | | rename | Plugin coordinates changed in version 5 | Git Commit ID Maven Plugin |
| org.springframework.boot:spring-boot-properties-migrator | org.springframework.boot:spring-boot-properties-migrator | | replace | Add as runtime dependency to analyze renamed/removed properties at startup; remove after migration | Configuration Properties Migration |
| org.flyway:flyway-core | org.flyway:flyway-core | >= 9.0 | replace | Flyway 9.0 default; cleanDisabled now true by default; ignoreIgnoredMigrations removed | Flyway |
