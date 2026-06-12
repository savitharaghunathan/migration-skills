# Phase: Code Migration

Replace renamed, moved, and removed APIs throughout the codebase.

## Steps

### Part 1: API replacements

1. Read `references/api-map.md`
2. Process rows in `kind` order to prevent conflicts:
   - First: `package` — apply package-level moves (e.g., `ReactorResourceFactory` from `http.client.reactive` to `http.client`)
   - Then: `class` — update class references (e.g., `CommonsMultipartResolver` → `StandardServletMultipartResolver`, `EhCacheCacheManager` → `JCacheCacheManager`, `WebJarsResourceResolver` → `LiteWebJarsResourceResolver`)
   - Then: `annotation` — swap annotations
   - Then: `interface` — update interfaces (e.g., `ListenableFuture` → `CompletableFuture`)
   - Then: `enum` — update enums (e.g., `HttpMethod` enum → class)
   - Then: `method` — update method calls (e.g., `createBean(Class, int, boolean)` → `createBean(Class)`)
3. For each row:
   - Search the codebase for `old_api` (check imports, type references, annotations, method calls)
   - Apply the `action`:
     - `replace` → substitute with `new_api`
     - `remove` → delete usage; if `after` column has a replacement pattern, use it
     - `rename` → update the name
     - `move_package` → update import statements to new package
   - Use the `before`/`after` columns as concrete examples

### Key API removals to search for

- **RPC remoting:** `HttpInvokerServiceExporter`, `HttpInvokerProxyFactoryBean`, `HessianServiceExporter`, `HessianProxyFactoryBean`, `JaxWsPortProxyFactoryBean`, `JmsInvokerServiceExporter`, `JmsInvokerProxyFactoryBean` — migrate to REST or gRPC
- **EJB access:** `LocalStatelessSessionProxyFactoryBean`, `SimpleRemoteStatelessSessionProxyFactoryBean` — use JNDI directly
- **Joda-Time:** `JodaTimeFormatterRegistrar`, `DateTimeFormatterFactory` — use `java.time` equivalents
- **EhCache 2:** `EhCacheManagerFactoryBean`, `EhCacheCacheManager` — use `JCacheCacheManager` with EhCache 3
- **Apache Tiles:** `TilesConfigurer`, `TilesViewResolver` — migrate to FreeMarker or Thymeleaf
- **CommonsMultipartResolver** — replace with `StandardServletMultipartResolver`
- **SourceHttpMessageConverter** — no longer auto-configured; register explicitly if needed
- **ListenableFuture/ListenableFutureCallback** — replace with `CompletableFuture`

### Part 2: Pattern changes

1. Read `references/pattern-map.md`
2. For each row:
   - Use the `before` code block to locate affected code in the project
   - Transform to match the `after` pattern
   - Pay attention to `category`:
     - `behavioral` — the code may not need changes, but behavior differs; check `notes`
     - `structural` — code reorganization required
     - `removal` — delete the code or replace with alternative from `notes`
     - `addition` — add new code/config as shown in `after`

### Key pattern changes

- **HttpMethod enum → class:** Replace `EnumSet<HttpMethod>` with `Set<HttpMethod>`, replace `switch` with `if-else` chains
- **Trailing slash matching disabled:** Add explicit routes `@GetMapping("/path", "/path/")` or use `UrlHandlerFilter` (6.2+)
- **@Async return types enforced:** Must return `Future` or `void` — other return types throw exceptions
- **Controller detection:** Needs `@Controller` annotation, not just `@RequestMapping`
- **Built-in method validation (6.1):** Remove `@Validated` from controller classes; use constraint annotations directly on parameters
- **RestClient.retrieve() (6.2):** Must call terminal operation (`toBodilessEntity()`, `body()`, etc.)
- **Property placeholder escaping (6.2):** Keys with `:` must be escaped; placeholders can be escaped with `\`
- **Invalid @Configuration rejected (6.2):** No `@Bean void` methods or `@Bean @Autowired` combinations

3. Run the build gate

## Build gate

Run the project build command. If it fails, check for:
- Missing imports after package moves (`ReactorResourceFactory`)
- `HttpMethod` enum usage in `switch` statements or `EnumSet`
- Removed APIs still referenced (remoting, Tiles, EhCache 2)
- `@Async` methods with non-Future/non-void return types
- `@Bean` methods with `void` return type or `@Autowired` annotation
