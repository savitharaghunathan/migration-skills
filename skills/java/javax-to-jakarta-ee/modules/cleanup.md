# Phase: Cleanup and Verification

Verify no `javax.*` EE references remain and the migration is complete.

## Steps

### 1. Remove old `javax.*` EE imports

Search for any remaining `javax.*` EE imports that should have been migrated. Use this grep pattern and manually filter out Java SE packages:

```bash
grep -rn "import javax\." --include="*.java" src/
```

**Must be migrated** (EE packages):
- `javax.servlet.*`, `javax.persistence.*`, `javax.ws.rs.*`
- `javax.enterprise.*`, `javax.inject.*`, `javax.ejb.*`
- `javax.faces.*`, `javax.el.*`, `javax.jms.*`
- `javax.validation.*`, `javax.annotation.*` (not `.processing`)
- `javax.xml.bind.*`, `javax.xml.ws.*`, `javax.xml.soap.*`
- `javax.json.*`, `javax.json.bind.*`
- `javax.websocket.*`, `javax.mail.*`, `javax.activation.*`
- `javax.transaction.*` (not `.xa.*`)
- `javax.security.enterprise.*`, `javax.security.auth.message.*`
- `javax.security.jacc.*`, `javax.interceptor.*`, `javax.decorator.*`
- `javax.batch.*`, `javax.resource.*`, `javax.jws.*`

**Must NOT be changed** (Java SE packages):
- `javax.sql.*`, `javax.crypto.*`, `javax.net.*`
- `javax.naming.*`, `javax.management.*`
- `javax.xml.parsers.*`, `javax.xml.transform.*`, `javax.xml.datatype.*`
- `javax.xml.validation.*`, `javax.xml.xpath.*`
- `javax.security.auth.*` (base — only `.message.*` moves)
- `javax.annotation.processing.*`
- `javax.transaction.xa.*`
- `javax.swing.*`, `javax.sound.*`, `javax.imageio.*`

### 2. Verify no old artifacts in build files

Search `pom.xml`/`build.gradle` for:
- `javax:javaee-api` or `javax:javaee-web-api`
- `javax.servlet:javax.servlet-api`
- `javax.persistence:javax.persistence-api`
- `javax.ws.rs:javax.ws.rs-api`
- `javax.enterprise:cdi-api`
- `javax.inject:javax.inject`
- `javax.annotation:javax.annotation-api`
- `javax.validation:javax.validation-api`
- `javax.xml.bind:jaxb-api`
- Any other `javax.*:javax.*-api` coordinate

### 3. Verify no old XML namespaces

Search XML files for old JCP namespace URIs:
```bash
grep -rn "xmlns.jcp.org" --include="*.xml" --include="*.xhtml" .
grep -rn "java.sun.com/xml/ns" --include="*.xml" .
```

Should find zero results. All should be `jakarta.ee/xml/ns/` or `jakarta.*` URNs.

### 4. Verify no old ServiceLoader files

```bash
find . -path "*/META-INF/services/javax.*" -type f
```

Should find zero results.

### 5. Verify no mixed javax/jakarta dependencies

Check the effective dependency tree for conflicts:
```bash
mvn dependency:tree | grep -E "javax\.(servlet|persistence|ws\.rs|enterprise|inject|annotation|validation|xml\.bind)"
```

Any `javax.*` EE dependencies in the tree indicate transitive dependency issues that must be resolved (exclude and replace, or apply Eclipse Transformer).

### 6. Verify no old property references

Search for `javax.persistence.*` property names in source code and config files:
```bash
grep -rn "javax\.persistence\." --include="*.xml" --include="*.properties" --include="*.yml" --include="*.java" .
```

### 7. Final build and test

- Run the full build: `mvn clean verify`
- Deploy to a Jakarta EE 10 compatible server
- Run integration tests
- Verify all EE features work: Servlet, JPA, CDI, JAX-RS, Faces, etc.

### 8. Report to user

- List all changes made across all phases
- Flag any third-party libraries that still use `javax.*` and need Eclipse Transformer
- Note any removed specifications (XML Registries, XML RPC, Deployment) that need manual migration
- Report any files that could not be automatically migrated
