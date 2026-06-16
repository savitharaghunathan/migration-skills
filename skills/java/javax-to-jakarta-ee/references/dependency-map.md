# Dependency Map — Java EE 8 to Jakarta EE 10

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| javax:javaee-api | jakarta.platform:jakarta.jakartaee-api | >= 9.0.0 | replace | Umbrella API; use 10.0.0 for Jakarta EE 10; requires Java 11+ | Jakarta EE Platform |
| javax:javaee-web-api | jakarta.platform:jakarta.jakartaee-web-api | >= 9.0.0 | replace | Web Profile umbrella API | Jakarta EE Platform |
| javax.servlet:javax.servlet-api | jakarta.servlet:jakarta.servlet-api | >= 5.0 (EE9) / >= 6.0 (EE10) | replace | Servlet 5.0 (EE9 namespace only) or 6.0 (EE10 with new features) | Servlet |
| javax.servlet.jsp:javax.servlet.jsp-api | jakarta.servlet.jsp:jakarta.servlet.jsp-api | >= 3.0 | replace | Jakarta Server Pages | Pages |
| javax.servlet.jsp.jstl:javax.servlet.jsp.jstl-api | jakarta.servlet.jsp.jstl:jakarta.servlet.jsp.jstl-api | >= 2.0 | replace | Jakarta Standard Tag Library | JSTL |
| javax.el:javax.el-api | jakarta.el:jakarta.el-api | >= 4.0 (EE9) / >= 5.0 (EE10) | replace | Jakarta Expression Language | Expression Language |
| javax.persistence:javax.persistence-api | jakarta.persistence:jakarta.persistence-api | >= 3.0 (EE9) / >= 3.1 (EE10) | replace | Jakarta Persistence (JPA) | Persistence |
| javax.transaction:javax.transaction-api | jakarta.transaction:jakarta.transaction-api | >= 2.0 | replace | Jakarta Transactions (JTA); `javax.transaction.xa` remains Java SE | Transactions |
| javax.validation:javax.validation-api | jakarta.validation:jakarta.validation-api | >= 3.0 | replace | Jakarta Bean Validation | Validation |
| javax.enterprise:cdi-api | jakarta.enterprise:jakarta.enterprise.cdi-api | >= 3.0 (EE9) / >= 4.0 (EE10) | replace | Jakarta CDI | CDI |
| javax.inject:javax.inject | jakarta.inject:jakarta.inject-api | >= 2.0 | replace | Jakarta Dependency Injection | Dependency Injection |
| javax.annotation:javax.annotation-api | jakarta.annotation:jakarta.annotation-api | >= 2.0 (EE9) / >= 2.1 (EE10) | replace | Jakarta Annotations; `javax.annotation.processing` remains Java SE | Annotations |
| javax.interceptor:javax.interceptor-api | jakarta.interceptor:jakarta.interceptor-api | >= 2.0 (EE9) / >= 2.1 (EE10) | replace | Jakarta Interceptors | Interceptors |
| javax.ejb:javax.ejb-api | jakarta.ejb:jakarta.ejb-api | >= 4.0 | replace | Jakarta Enterprise Beans; Entity Beans (CMP/BMP) removed in EE10 | EJB |
| javax.ws.rs:javax.ws.rs-api | jakarta.ws.rs:jakarta.ws.rs-api | >= 3.0 (EE9) / >= 3.1 (EE10) | replace | Jakarta RESTful Web Services (JAX-RS) | JAX-RS |
| javax.json:javax.json-api | jakarta.json:jakarta.json-api | >= 2.0 (EE9) / >= 2.1 (EE10) | replace | Jakarta JSON Processing | JSON Processing |
| javax.json.bind:javax.json.bind-api | jakarta.json.bind:jakarta.json.bind-api | >= 2.0 (EE9) / >= 3.0 (EE10) | replace | Jakarta JSON Binding | JSON Binding |
| javax.websocket:javax.websocket-api | jakarta.websocket:jakarta.websocket-api | >= 2.0 (EE9) / >= 2.1 (EE10) | replace | Jakarta WebSocket | WebSocket |
| javax.faces:javax.faces-api | jakarta.faces:jakarta.faces-api | >= 3.0 (EE9) / >= 4.0 (EE10) | replace | Jakarta Faces (JSF) | Faces |
| javax.mail:javax.mail-api | jakarta.mail:jakarta.mail-api | >= 2.0 (EE9) / >= 2.1 (EE10) | replace | Jakarta Mail | Mail |
| javax.activation:javax.activation-api | jakarta.activation:jakarta.activation-api | >= 2.0 (EE9) / >= 2.1 (EE10) | replace | Jakarta Activation | Activation |
| javax.xml.bind:jaxb-api | jakarta.xml.bind:jakarta.xml.bind-api | >= 3.0 (EE9) / >= 4.0 (EE10) | replace | Jakarta XML Binding (JAXB) | XML Binding |
| javax.xml.ws:jaxws-api | jakarta.xml.ws:jakarta.xml.ws-api | >= 3.0 | replace | Jakarta XML Web Services (JAX-WS) | XML Web Services |
| javax.xml.soap:javax.xml.soap-api | jakarta.xml.soap:jakarta.xml.soap-api | >= 2.0 (EE9) / >= 3.0 (EE10) | replace | Jakarta SOAP with Attachments | SOAP |
| javax.jms:javax.jms-api | jakarta.jms:jakarta.jms-api | >= 3.0 (EE9) / >= 3.1 (EE10) | replace | Jakarta Messaging (JMS) | Messaging |
| javax.resource:javax.resource-api | jakarta.resource:jakarta.resource-api | >= 2.0 (EE9) / >= 2.1 (EE10) | replace | Jakarta Connectors (JCA) | Connectors |
| javax.batch:javax.batch-api | jakarta.batch:jakarta.batch-api | >= 2.0 (EE9) / >= 2.1 (EE10) | replace | Jakarta Batch | Batch |
| javax.security.enterprise:javax.security.enterprise-api | jakarta.security:jakarta.security-api | >= 2.0 (EE9) / >= 3.0 (EE10) | replace | Jakarta Security; artifact name simplified | Security |
| javax.security.auth.message:javax.security.auth.message-api | jakarta.authentication:jakarta.authentication-api | >= 2.0 (EE9) / >= 3.0 (EE10) | replace | Jakarta Authentication (JASPIC); artifact renamed | Authentication |
| javax.security.jacc:javax.security.jacc-api | jakarta.authorization:jakarta.authorization-api | >= 2.0 (EE9) / >= 2.1 (EE10) | replace | Jakarta Authorization (JACC); artifact renamed | Authorization |
| javax.enterprise.concurrent:javax.enterprise.concurrent-api | jakarta.enterprise.concurrent:jakarta.enterprise.concurrent-api | >= 2.0 (EE9) / >= 3.0 (EE10) | replace | Jakarta Concurrency | Concurrency |
| javax.management.j2ee:javax.management.j2ee-api | jakarta.management.j2ee:jakarta.management.j2ee-api | >= 1.1.4 | replace | Jakarta Management | Management |
| javax.xml.registry:javax.xml.registry-api | removed | | remove | Jakarta XML Registries — removed from platform | XML Registries |
| javax.xml.rpc:javax.xml.rpc-api | removed | | remove | Jakarta XML RPC — removed from platform | XML RPC |
| javax.enterprise.deploy:javax.enterprise.deploy-api | removed | | remove | Jakarta Deployment — removed from platform | Deployment |
