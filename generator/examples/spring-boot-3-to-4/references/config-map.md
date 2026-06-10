# Config Map — Spring Boot 3 to 4

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| spring.resources.static-locations | spring.web.resources.static-locations | | | | application.properties | Property prefix changed | Static Resources |
| spring.mvc.throw-exception-if-no-handler-found | removed | | false | | application.properties | Exceptions always thrown in SB4; property removed | Error Handling |
| management.endpoints.web.exposure.include | removed | true | (none exposed) | (all exposed) | application.properties | All actuator endpoints exposed by default in SB4 | Actuator |
| management.endpoint.health.probes.enabled | removed | true | false | true | application.properties | Health probes enabled by default | Health Probes |
| spring.jpa.open-in-view | spring.jpa.open-in-view | true | true | false | application.yml | OSIV disabled by default; set to true if your app relies on lazy loading in views | JPA |
| server.servlet.session.timeout | server.servlet.session.timeout | true | 30m | 15m | application.properties | Default session timeout reduced | Session |
| spring.main.banner-mode | spring.main.banner-mode | true | console | off | application.properties | Banner disabled by default | Startup |
| spring.jackson.default-property-inclusion | spring.jackson.default-property-inclusion | true | always | non_null | application.yml | Null values excluded from JSON by default | JSON |
| spring.datasource.url | spring.datasource.url | | | | application.properties | No change; but driver class auto-detection improved | Data Source |
| spring.flyway.enabled | spring.flyway.enabled | true | true | (auto) | application.properties | Auto-detection based on classpath; explicit true/false still works | Flyway |
| logging.level.root | logging.level.root | | | | application.properties | No change; but structured logging available via logging.structured.* | Logging |
| spring.threads.virtual.enabled | spring.threads.virtual.enabled | true | false | true | application.properties | Virtual threads enabled by default on Java 21+ | Virtual Threads |
