# Phase: Testing Migration

Update test annotations, frameworks, and testing patterns.

## Steps

1. Read `references/api-map.md` — filter for test-related rows
2. Read `references/pattern-map.md` — filter for test-related rows

### JUnit 5 Jupiter migration (5.0)
- Replace `@RunWith(SpringRunner.class)` with `@ExtendWith(SpringExtension.class)`
- Or use composed annotations: `@SpringJUnitConfig` (combines `@ExtendWith` + `@ContextConfiguration`)
- Use `@SpringJUnitWebConfig` for web tests (adds `@WebAppConfiguration`)
- New conditional annotations: `@EnabledIf` / `@DisabledIf` with SpEL expressions
- JUnit 4 still works via `SpringRunner`, but JUnit 5 is recommended

### Mock JNDI replacement (5.2)
- `SimpleNamingContext` and `SimpleNamingContextBuilder` are deprecated
- Replace with Simple-JNDI (h-thurow/Simple-JNDI) or other third-party JNDI provider

### `@Nested` test class annotation inheritance (5.3)
- Test annotations from enclosing classes are now **inherited by default** (breaking change)
- If tests break, apply `@NestedTestConfiguration(OVERRIDE)` on enclosing classes
- For project-wide rollback: set `spring.test.enclosing.configuration=override` in `spring.properties`

### MockMvc Kotlin DSL (5.3)
- Replace property syntax `isOk` with method call `isOk()` in MockMvc Kotlin DSL assertions
- Other Kotlin DSL variations possible due to major DSL revision

### Content type assertion updates (5.2)
- `MediaType.APPLICATION_JSON_UTF8` deprecated → use `MediaType.APPLICATION_JSON`
- Integration tests asserting `content().contentType(APPLICATION_JSON_UTF8)` must update

### Additional test improvements
- `WebTestClient` can now test against `MockMvc` (5.3) — unifies reactive and servlet test APIs
- `@TestConstructor` autowiring mode configurable via `spring.test.constructor.autowire.mode` (5.2)
- `@TestPropertySource` is repeatable (5.2)
- `@Sql` class-level and method-level declarations can be merged (5.2)

3. Run the full test suite

## Build gate

Run `mvn test` or `gradle test`. Test failures may indicate:
- Annotation inheritance changing test context in `@Nested` classes
- MockMvc Kotlin DSL syntax changes
- Content type assertion mismatches (`APPLICATION_JSON_UTF8` → `APPLICATION_JSON`)
- JNDI mock setup failures from deprecated `SimpleNamingContextBuilder`
