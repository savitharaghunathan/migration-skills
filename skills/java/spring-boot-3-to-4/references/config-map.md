# Config Map — Spring Boot 3 to 4

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| spring.devtools.livereload.enabled | spring.devtools.livereload.enabled | true | true | false | application.properties | Live reload now disabled by default; set to true to re-enable | DevTools Live Reload Support |
| spring.jackson.read.* | spring.jackson.json.read.* | | | | application.properties | JSON-specific read features moved under json namespace | Upgrading Jackson |
| spring.jackson.write.* | spring.jackson.json.write.* | | | | application.properties | JSON-specific write features moved under json namespace | Upgrading Jackson |
| spring.jackson.parser.* | spring.jackson.json.read.* | | | | application.properties | Parser features mapped to json.read equivalents where Jackson 3 JsonReadFeature exists; others require programmatic config via JsonMapperBuilderCustomizer | Upgrading Jackson |
| spring.session.redis.* | spring.session.data.redis.* | | | | application.properties | Renamed to reflect Spring Data Redis dependency | Spring Session |
| spring.session.mongodb.* | spring.session.data.mongodb.* | | | | application.properties | Renamed to reflect Spring Data MongoDB dependency | Spring Session |
| spring.dao.exceptiontranslation.enabled | spring.persistence.exceptiontranslation.enabled | | | | application.properties | Moved to persistence namespace | Persistence Modules |
| spring.data.mongodb.additional-hosts | spring.mongodb.additional-hosts | | | | application.properties | Properties that don't require Spring Data moved from spring.data.mongodb to spring.mongodb | MongoDB |
| spring.data.mongodb.authentication-database | spring.mongodb.authentication-database | | | | application.properties | | MongoDB |
| spring.data.mongodb.database | spring.mongodb.database | | | | application.properties | | MongoDB |
| spring.data.mongodb.host | spring.mongodb.host | | | | application.properties | | MongoDB |
| spring.data.mongodb.password | spring.mongodb.password | | | | application.properties | | MongoDB |
| spring.data.mongodb.port | spring.mongodb.port | | | | application.properties | | MongoDB |
| spring.data.mongodb.protocol | spring.mongodb.protocol | | | | application.properties | | MongoDB |
| spring.data.mongodb.replica-set-name | spring.mongodb.replica-set-name | | | | application.properties | | MongoDB |
| spring.data.mongodb.uri | spring.mongodb.uri | | | | application.properties | | MongoDB |
| spring.data.mongodb.username | spring.mongodb.username | | | | application.properties | | MongoDB |
| spring.data.mongodb.ssl.bundle | spring.mongodb.ssl.bundle | | | | application.properties | | MongoDB |
| spring.data.mongodb.ssl.enabled | spring.mongodb.ssl.enabled | | | | application.properties | | MongoDB |
| spring.data.mongodb.representation.uuid | spring.mongodb.representation.uuid | | | | application.properties | Explicit config now required; no default representation for UUID | MongoDB UUID and BigDecimal Representations |
| management.health.mongo.enabled | management.health.mongodb.enabled | | | | application.properties | Renamed mongo → mongodb | MongoDB |
| management.metrics.mongo.command.enabled | management.metrics.mongodb.command.enabled | | | | application.properties | Renamed mongo → mongodb | MongoDB |
| management.metrics.mongo.connectionpool.enabled | management.metrics.mongodb.connectionpool.enabled | | | | application.properties | Renamed mongo → mongodb | MongoDB |
| spring.kafka.retry.topic.backoff.random | spring.kafka.retry.topic.backoff.jitter | | | | application.properties | Property renamed; jitter provides more flexibility than random | Spring Kafka Retry Features |
| management.endpoint.health.probes.enabled | management.endpoint.health.probes.enabled | true | false | true | application.properties | Liveness and readiness probes now enabled by default | Liveness and Readiness Probes |
| spring-authorization-server.version | removed | | | | pom.xml | Use spring-security.version instead; Authorization Server now part of Spring Security | Dependency Management for Spring Authorization Server |
