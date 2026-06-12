# Config Map — Spring Boot 2 to 3

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| spring.redis.host | spring.data.redis.host | | | | application.properties | Redis properties moved under spring.data prefix | Redis Properties |
| spring.redis.port | spring.data.redis.port | | | | application.properties | | Redis Properties |
| spring.redis.password | spring.data.redis.password | | | | application.properties | | Redis Properties |
| spring.redis.database | spring.data.redis.database | | | | application.properties | | Redis Properties |
| spring.redis.url | spring.data.redis.url | | | | application.properties | | Redis Properties |
| spring.redis.username | spring.data.redis.username | | | | application.properties | | Redis Properties |
| spring.redis.client-name | spring.data.redis.client-name | | | | application.properties | | Redis Properties |
| spring.redis.client-type | spring.data.redis.client-type | | | | application.properties | | Redis Properties |
| spring.redis.connect-timeout | spring.data.redis.connect-timeout | | | | application.properties | | Redis Properties |
| spring.redis.timeout | spring.data.redis.timeout | | | | application.properties | | Redis Properties |
| spring.redis.ssl | spring.data.redis.ssl | | | | application.properties | | Redis Properties |
| spring.redis.sentinel.master | spring.data.redis.sentinel.master | | | | application.properties | | Redis Properties |
| spring.redis.sentinel.nodes | spring.data.redis.sentinel.nodes | | | | application.properties | | Redis Properties |
| spring.redis.sentinel.password | spring.data.redis.sentinel.password | | | | application.properties | | Redis Properties |
| spring.redis.sentinel.username | spring.data.redis.sentinel.username | | | | application.properties | | Redis Properties |
| spring.redis.cluster.max-redirects | spring.data.redis.cluster.max-redirects | | | | application.properties | | Redis Properties |
| spring.redis.cluster.nodes | spring.data.redis.cluster.nodes | | | | application.properties | | Redis Properties |
| spring.redis.lettuce.shutdown-timeout | spring.data.redis.lettuce.shutdown-timeout | | | | application.properties | | Redis Properties |
| spring.redis.lettuce.cluster.refresh.adaptive | spring.data.redis.lettuce.cluster.refresh.adaptive | | | | application.properties | | Redis Properties |
| spring.redis.lettuce.cluster.refresh.dynamic-refresh-sources | spring.data.redis.lettuce.cluster.refresh.dynamic-refresh-sources | | | | application.properties | | Redis Properties |
| spring.redis.lettuce.cluster.refresh.period | spring.data.redis.lettuce.cluster.refresh.period | | | | application.properties | | Redis Properties |
| spring.data.cassandra.compression | spring.cassandra.compression | | | | application.properties | Cassandra properties moved from spring.data.cassandra to spring.cassandra | Cassandra Properties |
| spring.data.cassandra.config | spring.cassandra.config | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.contact-points | spring.cassandra.contact-points | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.keyspace-name | spring.cassandra.keyspace-name | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.local-datacenter | spring.cassandra.local-datacenter | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.password | spring.cassandra.password | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.username | spring.cassandra.username | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.port | spring.cassandra.port | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.ssl | spring.cassandra.ssl | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.schema-action | spring.cassandra.schema-action | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.session-name | spring.cassandra.session-name | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.connection.connect-timeout | spring.cassandra.connection.connect-timeout | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.connection.init-query-timeout | spring.cassandra.connection.init-query-timeout | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.controlconnection.timeout | spring.cassandra.controlconnection.timeout | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.pool.heartbeat-interval | spring.cassandra.pool.heartbeat-interval | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.pool.idle-timeout | spring.cassandra.pool.idle-timeout | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.request.consistency | spring.cassandra.request.consistency | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.request.page-size | spring.cassandra.request.page-size | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.request.serial-consistency | spring.cassandra.request.serial-consistency | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.request.timeout | spring.cassandra.request.timeout | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.request.throttler.type | spring.cassandra.request.throttler.type | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.request.throttler.max-concurrent-requests | spring.cassandra.request.throttler.max-concurrent-requests | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.request.throttler.max-requests-per-second | spring.cassandra.request.throttler.max-requests-per-second | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.request.throttler.max-queue-size | spring.cassandra.request.throttler.max-queue-size | | | | application.properties | | Cassandra Properties |
| spring.data.cassandra.request.throttler.drain-interval | spring.cassandra.request.throttler.drain-interval | | | | application.properties | | Cassandra Properties |
| management.metrics.export.appoptics.* | management.appoptics.metrics.export.* | | | | application.properties | Metrics export properties restructured: management.metrics.export.<product> → management.<product>.metrics.export | Actuator Metrics Export Properties |
| management.metrics.export.atlas.* | management.atlas.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.datadog.* | management.datadog.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.dynatrace.* | management.dynatrace.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.elastic.* | management.elastic.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.ganglia.* | management.ganglia.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.graphite.* | management.graphite.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.humio.* | management.humio.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.influx.* | management.influx.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.jmx.* | management.jmx.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.kairos.* | management.kairos.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.newrelic.* | management.newrelic.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.prometheus.* | management.prometheus.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.signalfx.* | management.signalfx.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.simple.* | management.simple.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.stackdriver.* | management.stackdriver.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.statsd.* | management.statsd.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.export.wavefront.* | management.wavefront.metrics.export.* | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.web.client.request.metric-name | management.observations.http.client.requests.name | | | | application.properties | | Actuator Metrics Export Properties |
| management.metrics.web.server.request.metric-name | management.observations.http.server.requests.name | | | | application.properties | | Actuator Metrics Export Properties |
| server.max-http-header-size | server.max-http-request-header-size | | | | application.properties | Now applies only to request header size; use WebServerFactoryCustomizer for response headers | 'server.max-http-header-size' |
| management.endpoint.httptrace.cache.time-to-live | management.endpoint.httpexchanges.cache.time-to-live | | | | application.properties | Endpoint renamed from httptrace to httpexchanges | 'httptrace' Endpoint Renamed |
| management.endpoint.httptrace.enabled | management.endpoint.httpexchanges.enabled | | | | application.properties | | 'httptrace' Endpoint Renamed |
| management.trace.http.enabled | management.httpexchanges.recording.enabled | | | | application.properties | | 'httptrace' Endpoint Renamed |
| management.trace.http.include | management.httpexchanges.recording.include | | | | application.properties | | 'httptrace' Endpoint Renamed |
| spring.session.store-type | removed | | | | application.properties | Explicit store type no longer supported; auto-detected by fixed order or define SessionRepository bean | Spring Session Store Type |
| spring.jpa.hibernate.use-new-id-generator-mappings | removed | | | | application.properties | Hibernate no longer supports switching back to old ID generator mappings | Hibernate 6.1 |
| spring.security.saml2.relyingparty.registration.{id}.identity-provider.* | spring.security.saml2.relyingparty.registration.{id}.asserting-party.* | | | | application.properties | Property prefix renamed from identity-provider to asserting-party | SAML2 Relying Party Configuration |
| spring.flyway.ignore-ignored-migrations | spring.flyway.ignore-migration-patterns | | | | application.properties | Removed in Flyway 9.0; use ignore-migration-patterns | Flyway |
| spring.flyway.ignore-future-migrations | spring.flyway.ignore-migration-patterns | | | | application.properties | Removed in Flyway 9.0 | Flyway |
| spring.flyway.ignore-missing-migrations | spring.flyway.ignore-migration-patterns | | | | application.properties | Removed in Flyway 9.0 | Flyway |
| spring.flyway.ignore-pending-migrations | spring.flyway.ignore-migration-patterns | | | | application.properties | Removed in Flyway 9.0 | Flyway |
| spring.flyway.baseline-migration-prefix | removed | | | | application.properties | Removed in Flyway 9.0; moved to flyway-proprietary | Flyway |
| spring.flyway.oracle-kerberos-config-file | spring.flyway.kerberos-config-file | | | | application.properties | | Flyway |
| spring.banner.image.* | removed | | | | application.properties | Image banner support removed; use text-based banner.txt | Image Banner Support Removed |
| spring.batch.job.names | removed | | | | application.properties | Use spring.batch.job.name (singular) to specify single job | Multiple Batch Jobs |
| spring.config.use-legacy-processing | removed | | | | application.properties | Legacy config processing no longer available | Configuration Properties Migration |
| spring.profiles | spring.config.activate.on-profile | | | | application.properties | | Configuration Properties Migration |
| spring.activemq.* | removed | | | | application.properties | Apache ActiveMQ support removed | Other Removals |
| spring.data.solr.* | removed | | | | application.properties | Apache Solr support removed | Other Removals |
| spring.jta.atomikos.* | removed | | | | application.properties | Atomikos JTA support removed | Other Removals |
| spring.jta.log-dir | removed | | | | application.properties | JTA log dir removed with Atomikos | Other Removals |
| spring.jta.transaction-manager-id | removed | | | | application.properties | JTA transaction manager ID removed with Atomikos | Other Removals |
| spring.mongodb.embedded.* | removed | | | | application.properties | Flapdoodle embedded MongoDB auto-configuration removed | Embedded MongoDB |
| spring.cache.ehcache.config | removed | | | | application.properties | EhCache 2 support removed | Other Removals |
| spring.data.elasticsearch.client.reactive.endpoints | spring.elasticsearch.uris | | | | application.properties | Reactive Elasticsearch client properties restructured | Elasticsearch Clients and Templates |
| spring.data.elasticsearch.client.reactive.connection-timeout | spring.elasticsearch.connection-timeout | | | | application.properties | | Elasticsearch Clients and Templates |
| spring.data.elasticsearch.client.reactive.password | spring.elasticsearch.password | | | | application.properties | | Elasticsearch Clients and Templates |
| spring.data.elasticsearch.client.reactive.socket-timeout | spring.elasticsearch.socket-timeout | | | | application.properties | | Elasticsearch Clients and Templates |
| spring.data.elasticsearch.client.reactive.username | spring.elasticsearch.username | | | | application.properties | | Elasticsearch Clients and Templates |
| spring.data.elasticsearch.client.reactive.use-ssl | removed | | | | application.properties | Indicate SSL through https URI scheme instead | Elasticsearch Clients and Templates |
| spring.elasticsearch.rest.connection-timeout | spring.elasticsearch.connection-timeout | | | | application.properties | | Elasticsearch Clients and Templates |
| spring.elasticsearch.rest.password | spring.elasticsearch.password | | | | application.properties | | Elasticsearch Clients and Templates |
| spring.elasticsearch.rest.read-timeout | spring.elasticsearch.socket-timeout | | | | application.properties | | Elasticsearch Clients and Templates |
| spring.elasticsearch.rest.uris | spring.elasticsearch.uris | | | | application.properties | | Elasticsearch Clients and Templates |
| spring.elasticsearch.rest.username | spring.elasticsearch.username | | | | application.properties | | Elasticsearch Clients and Templates |
| spring.elasticsearch.rest.sniffer.delay-after-failure | spring.elasticsearch.restclient.sniffer.delay-after-failure | | | | application.properties | | Elasticsearch Clients and Templates |
| spring.elasticsearch.rest.sniffer.interval | spring.elasticsearch.restclient.sniffer.interval | | | | application.properties | | Elasticsearch Clients and Templates |
| spring.elasticsearch.webclient.max-in-memory-size | removed | | | | application.properties | Reactive Elasticsearch client no longer uses WebClient | Elasticsearch Clients and Templates |
| spring.mustache.allow-request-override | spring.mustache.servlet.allow-request-override | | | | application.properties | Mustache properties moved under servlet sub-prefix | Configuration Properties Migration |
| spring.mustache.allow-session-override | spring.mustache.servlet.allow-session-override | | | | application.properties | | Configuration Properties Migration |
| spring.mustache.cache | spring.mustache.servlet.cache | | | | application.properties | | Configuration Properties Migration |
| spring.mustache.content-type | spring.mustache.servlet.content-type | | | | application.properties | | Configuration Properties Migration |
| spring.mustache.expose-request-attributes | spring.mustache.servlet.expose-request-attributes | | | | application.properties | | Configuration Properties Migration |
| spring.mustache.expose-session-attributes | spring.mustache.servlet.expose-session-attributes | | | | application.properties | | Configuration Properties Migration |
| spring.mustache.expose-spring-macro-helpers | spring.mustache.servlet.expose-spring-macro-helpers | | | | application.properties | | Configuration Properties Migration |
| spring.liquibase.labels | spring.liquibase.label-filter | | | | application.properties | Deprecated; use label-filter | Liquibase |
| spring.security.oauth2.resourceserver.jwt.jws-algorithm | spring.security.oauth2.resourceserver.jwt.jws-algorithms | | | | application.properties | Pluralized to support multiple algorithms | Spring Security Changes |
| spring.mvc.date-format | removed | | | | application.properties | Use spring.mvc.format.date instead | Configuration Properties Migration |
| spring.mvc.contentnegotiation.favor-path-extension | removed | | | | application.properties | Path extension content negotiation removed | Configuration Properties Migration |
| spring.mvc.pathmatch.use-suffix-pattern | removed | | | | application.properties | Path extension for request mapping removed | Configuration Properties Migration |
| spring.mvc.pathmatch.use-registered-suffix-pattern | removed | | | | application.properties | Path extension for request mapping removed | Configuration Properties Migration |
| management.endpoint.configprops.keys-to-sanitize | removed | | | | application.properties | Replaced by role-based approach: management.endpoint.configprops.show-values | Actuator Endpoints Sanitization |
| management.endpoint.configprops.additional-keys-to-sanitize | removed | | | | application.properties | Replaced by role-based approach | Actuator Endpoints Sanitization |
| management.endpoint.env.keys-to-sanitize | removed | | | | application.properties | Replaced by role-based approach: management.endpoint.env.show-values | Actuator Endpoints Sanitization |
| management.endpoint.env.additional-keys-to-sanitize | removed | | | | application.properties | Replaced by role-based approach | Actuator Endpoints Sanitization |
| management.endpoint.jolokia.config | removed | | | | application.properties | Jolokia endpoint removed | Configuration Properties Migration |
| management.endpoint.jolokia.enabled | removed | | | | application.properties | | Configuration Properties Migration |
| management.health.solr.enabled | removed | | | | application.properties | Solr support removed | Other Removals |
| management.metrics.web.client.request.autotime.enabled | removed | | | | application.properties | Apply at ObservationRegistry level instead | Micrometer and Metrics Changes |
| management.metrics.web.server.request.autotime.enabled | removed | | | | application.properties | Apply at ObservationRegistry level instead | Micrometer and Metrics Changes |
| management.metrics.web.server.request.ignore-trailing-slash | removed | | | | application.properties | Not needed; direct instrumentation in Spring MVC | Micrometer and Metrics Changes |
| management.metrics.graphql.autotime.enabled | removed | | | | application.properties | Apply at ObservationRegistry level instead | Micrometer and Metrics Changes |
| spring.data.neo4j.password | spring.neo4j.authentication.password | | | | application.properties | | Configuration Properties Migration |
| spring.data.neo4j.uri | spring.neo4j.uri | | | | application.properties | | Configuration Properties Migration |
| spring.data.neo4j.username | spring.neo4j.authentication.username | | | | application.properties | | Configuration Properties Migration |
| spring.kafka.listener.only-log-record-metadata | removed | | | | application.properties | Use KafkaUtils#setConsumerRecordFormatter instead | Configuration Properties Migration |
| spring.mvc.ignore-default-model-on-redirect | removed | | | | application.properties | Deprecated for removal in Spring MVC | Configuration Properties Migration |
| spring.webflux.multipart.streaming | removed | | | | application.properties | | Configuration Properties Migration |
| spring.webflux.session.cookie.same-site | server.reactive.session.cookie.same-site | | | | application.properties | | Configuration Properties Migration |
| server.servlet.session.cookie.comment | removed | | | | application.properties | Cookie comment deprecated in Servlet spec | Configuration Properties Migration |
