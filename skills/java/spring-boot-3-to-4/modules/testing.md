# Phase: Testing Migration

Update test annotations, frameworks, and testing patterns.

## Steps

1. Read `references/api-map.md` — filter for rows where `source_section` relates to testing
2. Read `references/dependency-map.md` — filter for test-scoped dependencies

### Test annotations

3. Replace `@MockBean` → `@MockitoBean` and `@SpyBean` → `@MockitoSpyBean`:
   - Update import from `org.springframework.boot.test.mock.mockito` to `org.springframework.test.context.bean.override.mockito`
   - If `@MockBean`/`@SpyBean` were used in `@Configuration` classes, move them to test classes or use `@MockitoBean(types = {...})` on the test class
4. Replace `@Mock`/`@Captor` usage:
   - `MockitoTestExecutionListener` removed; ensure `@ExtendWith(MockitoExtension.class)` is on test classes using `@Mock`/`@Captor`
5. Update `@PropertyMapping` import:
   - `org.springframework.boot.test.autoconfigure.properties.PropertyMapping` → `org.springframework.boot.test.context.PropertyMapping`

### Test configuration

6. Add `@AutoConfigureMockMvc` to tests using MockMVC with `@SpringBootTest`
7. Update HtmlUnit config: `@AutoConfigureMockMvc(webClientEnabled=false)` → `@AutoConfigureMockMvc(htmlUnit = @HtmlUnit(webClient = false))`
8. Add `@AutoConfigureTestRestTemplate` or `@AutoConfigureRestTestClient` for tests using TestRestTemplate/WebClient with `@SpringBootTest`
9. Update `TestRestTemplate` import: `org.springframework.boot.test.web.client.TestRestTemplate` → `org.springframework.boot.resttestclient.TestRestTemplate`
10. Add `spring-boot-resttestclient` and `spring-boot-restclient` test dependencies if using TestRestTemplate

### Test dependencies

11. Add per-technology test starters (`spring-boot-starter-<technology>-test`) for each technology under test
12. Add `spring-boot-starter-security-test` if using `@WithMockUser` or `@WithUserDetails`
13. Test starters bring `spring-boot-starter-test` transitively — remove explicit `spring-boot-starter-test` if using technology-specific test starters

### Run tests

14. Run the full test suite (not just compilation)

## Build gate

Run the project's test suite:
- Maven: `mvn test`
- Gradle: `gradle test`

Test failures may indicate migration issues in the code phase that weren't caught by compilation alone.
