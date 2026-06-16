# Phase: Additional Changes

Update XML deployment descriptors, ServiceLoader files, Facelets namespaces, and handle third-party library compatibility.

## Steps

1. Read `references/pattern-map.md` — filter for `structural` and `addition` categories

### XML deployment descriptor namespaces

Update all XML deployment descriptors from JCP to Jakarta EE namespace URIs:

**persistence.xml:**
```xml
<!-- Before -->
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence
             http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd"
             version="2.2">

<!-- After -->
<persistence xmlns="https://jakarta.ee/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="https://jakarta.ee/xml/ns/persistence
             https://jakarta.ee/xml/ns/persistence/persistence_3_0.xsd"
             version="3.0">
```

**web.xml:**
```xml
<!-- Before -->
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

<!-- After -->
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
         https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
         version="6.0">
```

**beans.xml:**
```xml
<!-- Before -->
<beans xmlns="http://xmlns.jcp.org/xml/ns/javaee"
       bean-discovery-mode="all">

<!-- After -->
<beans xmlns="https://jakarta.ee/xml/ns/jakartaee"
       version="4.0"
       bean-discovery-mode="all">
```

Also update: `ejb-jar.xml`, `faces-config.xml`, `orm.xml`, `validation.xml`, `batch.xml`

### Facelets/JSF namespace URIs

If using Jakarta Faces (JSF), update XHTML template namespace URIs:

```xml
<!-- Before -->
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://xmlns.jcp.org/jsf/html"
      xmlns:f="http://xmlns.jcp.org/jsf/core"
      xmlns:ui="http://xmlns.jcp.org/jsf/facelets"
      xmlns:c="http://xmlns.jcp.org/jsp/jstl/core"
      xmlns:cc="http://xmlns.jcp.org/jsf/composite">

<!-- After -->
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="jakarta.faces.html"
      xmlns:f="jakarta.faces.core"
      xmlns:ui="jakarta.faces.facelets"
      xmlns:c="jakarta.tags.core"
      xmlns:cc="jakarta.faces.composite">
```

JSTL namespace changes:
- `http://xmlns.jcp.org/jsp/jstl/core` → `jakarta.tags.core`
- `http://xmlns.jcp.org/jsp/jstl/fmt` → `jakarta.tags.fmt`
- `http://xmlns.jcp.org/jsp/jstl/functions` → `jakarta.tags.functions`
- `http://xmlns.jcp.org/jsp/jstl/sql` → `jakarta.tags.sql`
- `http://xmlns.jcp.org/jsp/jstl/xml` → `jakarta.tags.xml`

### ServiceLoader files

Rename all `META-INF/services/` files that reference `javax.*` EE interfaces:

```bash
# Find and rename all javax.* service files
find . -path "*/META-INF/services/javax.*" -type f | while read f; do
    newname=$(echo "$f" | sed 's/javax\./jakarta\./g')
    mv "$f" "$newname"
done
```

Common files to rename:
- `META-INF/services/javax.servlet.ServletContainerInitializer` → `jakarta.servlet.ServletContainerInitializer`
- `META-INF/services/javax.enterprise.inject.spi.Extension` → `jakarta.enterprise.inject.spi.Extension`
- `META-INF/services/javax.ws.rs.ext.Providers` → `jakarta.ws.rs.ext.Providers`
- `META-INF/services/javax.persistence.spi.PersistenceProvider` → `jakarta.persistence.spi.PersistenceProvider`

Also update the contents of these files if they reference `javax.*` class names.

### Bean Validation message keys

Update constraint message keys in:
- Custom validation message files (`ValidationMessages.properties`)
- Constraint annotations with explicit message attributes
- `{javax.validation.constraints.*}` → `{jakarta.validation.constraints.*}`

### Third-party library compatibility (Eclipse Transformer)

For dependencies without Jakarta EE 10-compatible versions, use Eclipse Transformer at build time:

```xml
<!-- Maven plugin for Eclipse Transformer -->
<plugin>
    <groupId>org.eclipse.transformer</groupId>
    <artifactId>org.eclipse.transformer.maven</artifactId>
    <version>1.0.0</version>
</plugin>
```

Or use the CLI: `java -jar org.eclipse.transformer.cli.jar input.jar output.jar`

2. Run the build gate

## Build gate

Run `mvn compile` and deploy to a Jakarta EE 10 compatible server. Verify:
- Application deploys without XML parsing errors
- CDI bean discovery works (beans.xml namespace)
- JPA persistence unit initializes (persistence.xml namespace)
- JSF pages render correctly (Facelets namespace URIs)
- ServiceLoader-based extensions load correctly
