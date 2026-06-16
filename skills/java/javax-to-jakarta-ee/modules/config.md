# Phase: Configuration Migration

Rename `javax.persistence.*` properties and other EE configuration properties to `jakarta.*`.

## Steps

1. Read `references/config-map.md`
2. Identify all configuration files in the project:
   - `persistence.xml` / `META-INF/persistence.xml`
   - `application.properties` / `application.yml` (Spring Boot)
   - `application-*.properties` / `application-*.yml` (profile-specific)
   - `microprofile-config.properties` (MicroProfile/Quarkus)
   - Custom property files referencing JPA properties

### JPA persistence properties

All `javax.persistence.*` properties must change to `jakarta.persistence.*`:

```xml
<!-- Before (persistence.xml) -->
<properties>
    <property name="javax.persistence.jdbc.url" value="jdbc:h2:mem:test"/>
    <property name="javax.persistence.jdbc.user" value="sa"/>
    <property name="javax.persistence.jdbc.password" value=""/>
    <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
    <property name="javax.persistence.schema-generation.database.action" value="create"/>
</properties>

<!-- After -->
<properties>
    <property name="jakarta.persistence.jdbc.url" value="jdbc:h2:mem:test"/>
    <property name="jakarta.persistence.jdbc.user" value="sa"/>
    <property name="jakarta.persistence.jdbc.password" value=""/>
    <property name="jakarta.persistence.jdbc.driver" value="org.h2.Driver"/>
    <property name="jakarta.persistence.schema-generation.database.action" value="create"/>
</properties>
```

### Spring Boot properties

If using Spring Boot, JPA properties set in `application.properties` / `application.yml` also need updating:

```properties
# Before
spring.jpa.properties.javax.persistence.lock.timeout=5000

# After
spring.jpa.properties.jakarta.persistence.lock.timeout=5000
```

### Programmatic property references

Search source code for string literals referencing `javax.persistence.*` property names:
```java
// Before
properties.put("javax.persistence.jdbc.url", url);
// After
properties.put("jakarta.persistence.jdbc.url", url);
```

3. Run the build gate

## Build gate

Run `mvn compile`. Configuration errors surface as startup failures — also start the application to verify:
- JPA EntityManagerFactory initializes correctly
- Database connections established via persistence.xml properties
- Schema generation actions execute as expected
