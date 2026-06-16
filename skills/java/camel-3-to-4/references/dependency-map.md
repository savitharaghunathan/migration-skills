# Dependency Map — Apache Camel 3.x to 4.0

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| org.apache.camel:camel-any23 | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-atlasmap | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-atmos | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-caffeine-lrucache | org.apache.camel:camel-cache | | replace | Alternatives: camel-ignite, camel-infinispan | Removed Components |
| org.apache.camel:camel-cdi | org.apache.camel.springboot:camel-spring-boot-starter | | replace | Alternative: camel-quarkus | Removed Components |
| org.apache.camel:camel-corda | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-directvm | org.apache.camel:camel-direct | | replace | Direct component now handles cross-context | Removed Components |
| org.apache.camel:camel-dozer | org.apache.camel:camel-mapstruct | | replace | | Removed Components |
| org.apache.camel:camel-elasticsearch-rest | org.apache.camel:camel-elasticsearch | | replace | | Removed Components |
| org.apache.camel:camel-gora | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-hbase | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-hyperledger-aries | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-iota | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-ipfs | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-jbpm | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-jclouds | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-johnzon | org.apache.camel:camel-jackson | | replace | Alternatives: camel-fastjson, camel-gson | Removed Components |
| org.apache.camel:camel-microprofile-metrics | org.apache.camel:camel-micrometer | | replace | Alternative: camel-opentelemetry | Removed Components |
| org.apache.camel:camel-milo | org.apache.camel:camel-plc4x | | replace | | Removed Components |
| org.apache.camel:camel-opentracing | org.apache.camel:camel-opentelemetry | | replace | Alternative: camel-micrometer | Removed Components |
| org.apache.camel:camel-rabbitmq | org.apache.camel:camel-spring-rabbitmq | | replace | Uses Spring AMQP under the hood | Removed Components |
| org.apache.camel:camel-rest-swagger | org.apache.camel:camel-openapi-rest | | replace | | Removed Components |
| org.apache.camel:camel-restdsl-swagger-plugin | org.apache.camel:camel-restdsl-openapi-plugin | | replace | Maven/Gradle plugin | Removed Components |
| org.apache.camel:camel-resteasy | org.apache.camel:camel-cxf | | replace | Alternative: camel-rest | Removed Components |
| org.apache.camel:camel-spark | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-spring-integration | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-swagger-java | org.apache.camel:camel-openapi-java | | replace | | Removed Components |
| org.apache.camel:camel-websocket | org.apache.camel:camel-vertx-websocket | | replace | | Removed Components |
| org.apache.camel:camel-websocket-jsr356 | org.apache.camel:camel-vertx-websocket | | replace | | Removed Components |
| org.apache.camel:camel-vertx-kafka | org.apache.camel:camel-kafka | | replace | | Removed Components |
| org.apache.camel:camel-vm | org.apache.camel:camel-seda | | replace | SEDA component replaces VM for in-process | Removed Components |
| org.apache.camel:camel-weka | removed | | remove | No replacement | Removed Components |
| org.apache.camel:camel-xstream | org.apache.camel:camel-jacksonxml | | replace | | Removed Components |
| org.apache.camel:camel-zipkin | org.apache.camel:camel-opentelemetry | | replace | Alternative: camel-micrometer | Removed Components |
| org.apache.camel:camel-test | org.apache.camel:camel-test-junit5 | | replace | All JUnit 4 test modules removed | JUnit 4 |
| org.apache.camel.springboot:camel-spring-boot-starter | org.apache.camel.springboot:camel-spring-boot-starter | | replace | No longer includes camel-spring-xml; add camel-spring-boot-xml-starter if using Spring XML files | Camel Spring Boot |
