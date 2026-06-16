# Phase: Code Migration

Rename all `javax.*` EE imports to `jakarta.*` throughout the codebase.

## Steps

### Part 1: API replacements

1. Read `references/api-map.md`
2. Process all package-level renames:

#### Bulk namespace rename

The core migration is a mechanical rename of all Enterprise Edition `javax.*` imports to `jakarta.*`. Apply these across the entire source tree:

- `javax.servlet.*` → `jakarta.servlet.*`
- `javax.persistence.*` → `jakarta.persistence.*`
- `javax.ws.rs.*` → `jakarta.ws.rs.*`
- `javax.enterprise.*` → `jakarta.enterprise.*`
- `javax.inject.*` → `jakarta.inject.*`
- `javax.ejb.*` → `jakarta.ejb.*`
- `javax.faces.*` → `jakarta.faces.*`
- `javax.el.*` → `jakarta.el.*`
- `javax.jms.*` → `jakarta.jms.*`
- `javax.validation.*` → `jakarta.validation.*`
- `javax.annotation.*` → `jakarta.annotation.*` (**NOT** `javax.annotation.processing.*`)
- `javax.xml.bind.*` → `jakarta.xml.bind.*`
- `javax.xml.ws.*` → `jakarta.xml.ws.*`
- `javax.xml.soap.*` → `jakarta.xml.soap.*`
- `javax.json.*` → `jakarta.json.*`
- `javax.json.bind.*` → `jakarta.json.bind.*`
- `javax.websocket.*` → `jakarta.websocket.*`
- `javax.mail.*` → `jakarta.mail.*`
- `javax.activation.*` → `jakarta.activation.*`
- `javax.transaction.*` → `jakarta.transaction.*` (**NOT** `javax.transaction.xa.*`)
- `javax.security.enterprise.*` → `jakarta.security.enterprise.*`
- `javax.security.auth.message.*` → `jakarta.security.auth.message.*`
- `javax.security.jacc.*` → `jakarta.security.jacc.*`
- `javax.interceptor.*` → `jakarta.interceptor.*`
- `javax.decorator.*` → `jakarta.decorator.*`
- `javax.batch.*` → `jakarta.batch.*`
- `javax.resource.*` → `jakarta.resource.*`
- `javax.enterprise.concurrent.*` → `jakarta.enterprise.concurrent.*`
- `javax.jws.*` → `jakarta.jws.*`

#### CRITICAL: Do NOT change these Java SE packages

These `javax.*` packages are part of Java SE and must remain unchanged:

- `javax.sql.*` — JDBC
- `javax.crypto.*` — Cryptography
- `javax.net.*` / `javax.net.ssl.*` — Networking/SSL
- `javax.naming.*` — JNDI
- `javax.management.*` — JMX
- `javax.xml.parsers.*` — DOM/SAX parsers
- `javax.xml.transform.*` — XSLT
- `javax.xml.datatype.*` — XML datatypes
- `javax.xml.validation.*` — XML Schema validation
- `javax.xml.xpath.*` — XPath
- `javax.xml.namespace.*` — XML namespaces (Java SE, NOT the EE mapping)
- `javax.security.auth.*` (base) — JAAS authentication (**only** `javax.security.auth.message.*` changes)
- `javax.annotation.processing.*` — Annotation processing
- `javax.transaction.xa.*` — XA transactions (JDBC)
- `javax.swing.*`, `javax.sound.*`, `javax.imageio.*` — Desktop APIs

#### Removed APIs

Remove usage of APIs dropped from the platform:
- `javax.enterprise.deploy.*` — Jakarta Deployment (removed)
- `javax.xml.registry.*` — Jakarta XML Registries (removed)
- `javax.xml.rpc.*` — Jakarta XML RPC (removed; migrate to JAX-WS/JAX-RS)

### Part 2: Pattern changes

1. Read `references/pattern-map.md`
2. Key areas:

**String literals and reflection:**
- Search for string references to `javax.` EE class names (e.g., in `Class.forName("javax.persistence.Entity")`)
- Update `@WebServlet`, `@WebFilter`, and other annotations that reference `javax.*` types as strings

**Bean Validation message keys:**
```java
// Before
@NotNull(message = "{javax.validation.constraints.NotNull.message}")
// After
@NotNull(message = "{jakarta.validation.constraints.NotNull.message}")
```

3. Run the build gate

## Build gate

Run `mvn compile`. Check for:
- Remaining `javax.*` EE imports (use `grep -r "import javax\." --include="*.java"` and filter out Java SE packages)
- `ClassNotFoundException` at runtime from mixed javax/jakarta
- String literals referencing old `javax.*` class names
