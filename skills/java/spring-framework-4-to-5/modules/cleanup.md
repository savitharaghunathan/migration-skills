# Phase: Cleanup and Verification

Remove compatibility shims, dead code, and verify the final build.

## Steps

### 1. Remove old package imports

Search for any remaining imports of removed packages:
- `org.springframework.orm.hibernate3.*`
- `org.springframework.orm.hibernate4.*`
- `org.springframework.web.servlet.view.tiles2.*`
- `org.springframework.web.servlet.view.velocity.*`
- `org.springframework.web.portlet.*`
- `org.springframework.beans.factory.access.*`
- `org.springframework.jdbc.support.nativejdbc.*`
- `org.springframework.mock.staticmock.*`
- `org.springframework.web.servlet.view.jasperreports.*`
- `org.springframework.jndi.mock.*`
- `org.springframework.cache.guava.*`

### 2. Verify no old artifacts remain

Search the entire codebase (build files, config, source) for:
- `commons-logging:commons-logging` in dependency declarations
- `jcl-over-slf4j` bridge declarations (superseded by `spring-jcl`)
- `guava` cache-related imports (`com.google.common.cache.*`)
- `javax.jdo` imports
- `FormTag` `commandName` attribute in JSP files
- `MediaType.APPLICATION_JSON_UTF8` / `APPLICATION_PROBLEM_JSON_UTF8` references
- `AsyncRestTemplate` usage
- `TransactionSynchronizationAdapter` usage
- `InstantiationAwareBeanPostProcessorAdapter` usage
- `PropertiesBeanDefinitionReader` / `JdbcBeanDefinitionReader` usage

### 3. Verify behavioral changes

These changes don't cause compilation errors but alter runtime behavior — verify manually:
- [ ] CORS `allowCredentials` is explicitly set where cookies/auth are needed
- [ ] `@RequestMapping()` without path matches only empty path (was any path in 5.1)
- [ ] Forwarded headers handled via `ForwardedHeaderFilter` or proxy config
- [ ] `DefaultUriBuilderFactory` URI encoding acceptable for existing WebClient usage
- [ ] XML config schemas resolve correctly with unversioned XSD references
- [ ] Null values handled correctly in APIs that no longer tolerate null
- [ ] `@ExceptionHandler` methods not matching unexpected nested exception causes (5.3)
- [ ] `@Nested` test classes inheriting correct annotations from enclosing classes (5.3)
- [ ] `@EventListener` method ordering via `@Order` is explicitly declared where needed (5.3)
- [ ] Read-only Hibernate transactions behave correctly without entity snapshots (5.1)
- [ ] Suffix pattern matching disabled — paths like `/person.json` no longer match `/person` controller (5.3)
- [ ] Remoting infrastructure (Hessian, RMI, HTTP Invoker) deprecated — plan replacement before 6.0

### 4. Final build and test

- Run the full build: `mvn clean verify` or `gradle clean build`
- Run integration tests if available
- Start the application and verify it runs correctly

### 5. Report to user

- List all changes made across all phases
- Flag any items requiring manual review (especially behavioral changes)
- Note remoting deprecation as a future action item for 6.0 migration
