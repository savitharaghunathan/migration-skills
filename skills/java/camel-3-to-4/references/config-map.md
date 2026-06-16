# Config Map — Apache Camel 3.x to 4.0

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| camel.health.components-enabled | camel.health.producers-enabled | | | | application.properties | Producer health checks renamed and disabled by default | Camel Health |
| camel.health.producers-enabled | camel.health.producers-enabled | true | true | false | application.properties | Producer health checks now disabled by default; must enable explicitly | Camel Health |
| backlogTracing | backlogTracing | true | | | application.properties | Setting true now auto-enables tracer on startup; use backlogTracingStandby=true for old behavior | Backlog Tracing |
| camel.metrics.uriTagDynamic | camel.metrics.uriTagDynamic | true | true | false | application.properties | URI tags are static by default to avoid excessive tag generation; set true to re-enable dynamic URIs | camel-micrometer-starter |
| camel.component.slack.delay | camel.component.slack.delay | true | 500 | 10000 | application.properties | Default consumer delay changed from 0.5s to 10s to reduce Slack rate limiting | camel-slack |
| camel.component.spring-rabbitmq.reply-timeout | camel.component.spring-rabbitmq.reply-timeout | true | 5000 | 30000 | application.properties | Default changed from 5s to 30s to match Spring's default | camel-spring-rabbitmq |
| camel.component.http.socket-timeout | removed | | | | application.properties | Use responseTimeout instead; soTimeout config must be prefixed with httpConnection. | camel-http |
| camel.component.jpa.transaction-manager | removed | | | | application.properties | Removed; use transactionStrategy option as vendor-neutral abstraction | camel-jpa |
| camel.component.azure-cosmosdb.item-partition-key | camel.component.azure-cosmosdb.item-partition-key | | | | application.properties | Type changed from PartitionKey to String | camel-azure-cosmosdb |
