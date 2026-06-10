# Phase: Code Migration

Replace renamed, moved, and removed APIs throughout the codebase.

## Steps

### Part 1: API replacements

1. Read `references/api-map.md`
2. Process rows in `kind` order to prevent conflicts:
   - First: `package` — apply package-level moves (e.g., `java.beans.beancontext`)
   - Then: `class` — update class references (e.g., `BootstrapRegistry`, `JsonObjectSerializer`, `TestRestTemplate`)
   - Then: `annotation` — swap annotations (e.g., `@Nullable`, `@JsonComponent` → `@JacksonComponent`, `@MockBean` → `@MockitoBean`)
   - Then: `interface` — update interfaces (e.g., `EnvironmentPostProcessor`, `StreamBuilderFactoryBeanCustomizer`)
   - Then: `method` — update method calls (e.g., `PropertyMapper.alwaysApplyingWhenNonNull()`)
   - Finally: `enum` — update enums (e.g., `PropertyMapping.Skip`)
3. For each row:
   - Search the codebase for `old_api` (check imports, type references, annotations, method calls)
   - Apply the `action`
   - Use the `before`/`after` columns as concrete examples
   - Check `notes` for edge cases

### Part 2: Pattern changes

1. Read `references/pattern-map.md`
2. For each row with category `structural` or `behavioral`:
   - Use the `before` code block to locate affected code
   - Transform to match the `after` pattern
3. Key areas to search:
   - JSpecify nullability: `org.springframework.lang.Nullable` → `org.jspecify.annotations.Nullable`
   - Jackson: `com.fasterxml.jackson` imports → `tools.jackson` imports
   - HttpMessageConverters: custom converter beans → customizer pattern
   - PropertyMapper: `.alwaysApplyingWhenNonNull()` → remove (default) or `.always()`
   - Elasticsearch: `RestClientBuilder` → `Rest5ClientBuilder`
   - Package reorganization: `org.springframework.boot.autoconfigure.*` → `org.springframework.boot.<module>.*`
4. Run the build gate

## Build gate

Run the project build command. If it fails, check for:
- Missing imports after package moves
- Incompatible method signatures
- Removed APIs still referenced
- Jackson 3 API incompatibilities
