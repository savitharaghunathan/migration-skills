# Phase: Cleanup and Verification

Remove remaining old imports, verify no old artifacts remain, and validate the final build.

## Steps

1. **Remove remaining old imports:** Search for any remaining imports from the old package namespace:
   - `org.apache.http.` (without `hc.`) — all should have been migrated to `org.apache.hc.*`
   - Any remaining old imports indicate missed transformations — go back and fix them

2. **Verify no old artifacts remain:**
   - Search build files for old Maven coordinates:
     - `org.apache.httpcomponents:httpclient` (should be `org.apache.httpcomponents.client5:httpclient5`)
     - `org.apache.httpcomponents:httpcore` (should be `org.apache.httpcomponents.core5:httpcore5`)
     - `org.apache.httpcomponents:httpmime` (merged into httpclient5)
     - `org.apache.httpcomponents:fluent-hc` (should be `httpclient5-fluent`)
   - Search for removed APIs:
     - `SSLConnectionSocketFactory` (should use TLS strategy builder)
     - `getStatusLine()` calls (should use `getCode()` or `new StatusLine()`)
     - `setConnectTimeout(int)` / `setSocketTimeout(int)` on `RequestConfig` (should be on `ConnectionConfig` with `Timeout`)
     - `setDefaultSocketConfig` on `HttpClients.custom()` (should be on connection manager builder)

3. **Check for deprecated APIs in 5.x:**
   - `URIBuilder.normalizeSyntax()` — deprecated since 5.3, use `optimize()` instead

4. **Final build and test:**
   - Run the full build: compilation + tests
   - If the project has integration tests that make HTTP calls, run those too
   - Verify HTTP requests work correctly (timeouts, TLS, connection pooling)

5. **Report to user:**
   - List all changes made across all phases
   - Flag any items that could not be automatically migrated:
     - Custom `HttpRequestRetryHandler` implementations (need `HttpRequestRetryStrategy` interface)
     - Custom `SSLConnectionSocketFactory` subclasses (need TLS strategy equivalent)
     - Code relying on `URIUtils.normalizeSyntax()` behavior (5.x normalizes more aggressively)
   - Note behavioral changes:
     - Zero TTL now means single-use connections (was unlimited in 4.x)
     - 5.x percent-encodes all URI components during normalization
     - `getEntity()` only available on `ClassicHttpResponse`, not base `HttpResponse`
