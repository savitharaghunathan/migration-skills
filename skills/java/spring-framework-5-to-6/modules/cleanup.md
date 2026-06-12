# Phase: Cleanup and Verification

Remove compatibility shims, dead code, and verify the final build.

## Steps

1. **Remove old package imports:** Search for any remaining imports from removed packages:
   - `org.springframework.remoting.*` (RPC remoting)
   - `org.springframework.format.datetime.joda.*` (Joda-Time)
   - `org.springframework.cache.ehcache.*` (EhCache 2)
   - `org.springframework.ejb.*` (EJB access)
   - `org.springframework.web.multipart.commons.*` (Commons FileUpload)
   - `org.springframework.web.servlet.view.tiles3.*` (Apache Tiles)
   - `org.springframework.util.concurrent.ListenableFuture*` (deprecated)
   - `org.springframework.http.client.reactive.ReactorResourceFactory` (moved)
   These indicate missed transformations — go back and fix them.

2. **Remove suppression annotations:** Search for `@SuppressWarnings("deprecation")` that referenced Spring Framework 5.x deprecated APIs.

3. **Verify HttpMethod usage:** Search for `EnumSet<HttpMethod>` or `switch` on `HttpMethod` — these must be updated since HttpMethod is now a class.

4. **Verify no old artifacts remain:**
   - Search build files for old EhCache 2 coordinates, Joda-Time, old servlet container versions
   - Search for `javax.servlet`, `javax.persistence`, etc. in imports (should be `jakarta.*`)
   - Search for `META-INF/spring.factories` entries referencing removed classes

5. **Review behavioral changes:** These don't require code changes but may affect runtime behavior:
   - Trailing slash matching is off by default
   - JDBC exception translation uses `SQLExceptionSubclassTranslator` (different exception types)
   - `PathPatternParser` is now the default (replaces `AntPathMatcher`)
   - `ThreadPoolTaskExecutor` stops accepting tasks during context shutdown
   - Jackson `ParameterNamesModule` is auto-registered (may change JSON behavior)
   - SpEL expressions capped at 10,000 operations (6.2.19+)
   - Property placeholder keys with `:` must be escaped (6.2)

6. **Final build and test:**
   - Run the full build: `mvn clean verify` or `gradle clean build`
   - Run integration tests if available
   - Verify the application starts successfully

7. **Report to user:**
   - List all changes made across all phases
   - Flag behavioral changes that need manual verification
   - Note any items requiring manual review (RPC remoting replacements, Tiles migration)
