# Dependency Map — Spring Boot 3 to 4

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| org.springframework.boot:spring-boot-starter-parent | org.springframework.boot:spring-boot-starter-parent | >= 4.0.0 | replace | Update version to 4.0.x; requires Java 21+ | Getting Started |
| javax.servlet:javax.servlet-api | jakarta.servlet:jakarta.servlet-api | >= 6.0 | replace | Jakarta EE 11 baseline | Jakarta EE |
| org.springframework.boot:spring-boot-starter-security | org.springframework.boot:spring-boot-starter-security | >= 4.0.0 | replace | Security filter chain defaults changed | Spring Security |
| org.springframework.boot:spring-boot-properties-migrator | removed | | remove | No longer needed; all property migrations are final in 4.0 | Configuration |
| org.springframework.boot:spring-boot-starter-logging | org.springframework.boot:spring-boot-starter-logging | >= 4.0.0 | replace | Logback 1.5+ required | Logging |
| org.apache.httpcomponents:httpclient | org.apache.httpcomponents.client5:httpclient5 | >= 5.0 | replace | Package relocated; RestTemplate uses HttpClient 5 by default | HTTP Client |
| org.springframework.boot:spring-boot-starter-test | org.springframework.boot:spring-boot-starter-test | >= 4.0.0 | replace | JUnit 5 only; JUnit 4 vintage engine removed | Testing |
| org.springframework.boot:spring-boot-starter-data-jpa | org.springframework.boot:spring-boot-starter-data-jpa | >= 4.0.0 | replace | Hibernate 7.0; Jakarta Persistence 3.2 | Data JPA |
| org.springframework.boot:spring-boot-starter-actuator | org.springframework.boot:spring-boot-starter-actuator | >= 4.0.0 | replace | All endpoints exposed by default | Actuator |
| org.springframework.boot:spring-boot-starter-web | org.springframework.boot:spring-boot-starter-web | >= 4.0.0 | replace | Embedded Tomcat 11; Servlet 6.1 | Web |
| io.micrometer:micrometer-tracing-bridge-brave | io.micrometer:micrometer-tracing-bridge-brave | >= 1.4.0 | replace | Tracing API changes; Observation API preferred | Observability |
| org.springframework.boot:spring-boot-starter-validation | org.springframework.boot:spring-boot-starter-validation | >= 4.0.0 | replace | Bean Validation 3.1 | Validation |
| com.google.code.gson:gson | com.google.code.gson:gson | | replace | HttpMessageConverter for Gson removed; add manually if needed | JSON |
| org.springframework.boot:spring-boot-starter-webflux | org.springframework.boot:spring-boot-starter-webflux | >= 4.0.0 | replace | Reactor Netty 2.0 | WebFlux |
