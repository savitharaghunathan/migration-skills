# Phase: Code Migration

Replace renamed, moved, and removed APIs throughout the codebase.

## Steps

### Part 1: API replacements

1. Read `references/api-map.md`
2. Process rows in `kind` order to prevent conflicts:

#### Package-level moves
- `org.springframework.orm.hibernate3.*` → `org.springframework.orm.hibernate5.*`
- `org.springframework.orm.hibernate4.*` → `org.springframework.orm.hibernate5.*`
- `org.springframework.web.servlet.view.tiles2.*` → `org.springframework.web.servlet.view.tiles3.*`

#### Class removals (no replacement — remove usage)
- `BeanFactoryLocator`, `BeanFactoryReference` — use CDI or direct `ApplicationContext` bootstrap
- `SpringBeanAutowiringInterceptor` — integrate Spring via CDI instead
- `NativeJdbcExtractor` — use JDBC 4 `Connection.unwrap()`
- `AnnotationDrivenStaticEntityMockingControl` — use Mockito
- `HibernateTemplate` (hibernate3) — use `SessionFactory.getCurrentSession()` directly
- `VelocityView`, `VelocityConfigurer`, `VelocityViewResolver` — migrate to FreeMarker or Thymeleaf
- `JasperReportsViewResolver` — remove JasperReports integration
- `PropertiesBeanDefinitionReader`, `JdbcBeanDefinitionReader` — use XML/annotation config
- `ResourceBundleViewResolver` — use `InternalResourceViewResolver`
- `InstantiationAwareBeanPostProcessorAdapter` — implement `SmartInstantiationAwareBeanPostProcessor` directly
- `TransactionSynchronizationAdapter` — implement `TransactionSynchronization` (extends `Ordered`)
- `SimpleNamingContext`, `SimpleNamingContextBuilder` — use Simple-JNDI

#### Class replacements
- `AsyncRestTemplate` → `WebClient` (reactive HTTP client)
- `GuavaCacheManager` → `CaffeineCacheManager`
- `GuavaCache` → `CaffeineCache`
- `SpringRunner` → `SpringExtension` (JUnit 5)

#### Field replacements
- `FormTag` attribute `commandName` → `modelAttribute` in JSP forms
- `MediaType.APPLICATION_JSON_UTF8` → `MediaType.APPLICATION_JSON`
- `MediaType.APPLICATION_PROBLEM_JSON_UTF8` → `MediaType.APPLICATION_PROBLEM_JSON`

### Part 2: Pattern changes

1. Read `references/pattern-map.md`
2. Key areas to search and transform:

- **commons-logging exclusions**: Remove `<exclusion>` blocks for `commons-logging` in POMs
- **CORS configuration**: Add explicit `allowCredentials = "true"` where cookies/auth needed
- **XML schema versions**: Update `xsi:schemaLocation` to unversioned XSD references
- **Null handling**: Check calls to `StringUtils` and other APIs that no longer accept null
- **`@Configuration` lite mode**: Consider `proxyBeanMethods=false` for non-bean-method-calling configs (5.2+)
- **Suffix pattern matching**: Remove `setUseSuffixPatternMatch(true)` config; use `Accept` header
- **Custom `Encoder` implementations**: Implement `encodeValue` method
- **Remoting deprecation**: Plan migration away from Hessian, RMI, HTTP Invoker, JMS Invoker (deprecated 5.3, removed 6.0)

3. Run the build gate

## Build gate

Run `mvn compile` or `gradle build`. Check for:
- Missing imports after `hibernate3/4` → `hibernate5` package moves
- Missing imports after `tiles2` → `tiles3` moves
- Compilation errors from removed Velocity/JasperReports classes
- `GuavaCache`/`GuavaCacheManager` references after Caffeine migration
