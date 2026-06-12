# Phase: Code Migration

Update package imports, replace renamed/moved APIs, and update timeout and TLS configuration patterns.

## Steps

### Part 1: Package-level import replacement

1. Read `references/api-map.md`
2. Replace all `org.apache.http` imports with their 5.x equivalents. The general mapping is:
   - `org.apache.http.impl.client.*` → `org.apache.hc.client5.http.impl.classic.*`
   - `org.apache.http.client.methods.*` → `org.apache.hc.client5.http.classic.methods.*`
   - `org.apache.http.client.config.*` → `org.apache.hc.client5.http.config.*`
   - `org.apache.http.client.protocol.*` → `org.apache.hc.client5.http.protocol.*`
   - `org.apache.http.client.entity.*` → `org.apache.hc.client5.http.entity.*`
   - `org.apache.http.client.utils.*` → `org.apache.hc.core5.net.*`
   - `org.apache.http.entity.*` → `org.apache.hc.core5.http.io.entity.*`
   - `org.apache.http.util.*` → `org.apache.hc.core5.http.io.entity.*`
   - `org.apache.http.message.*` → `org.apache.hc.core5.http.message.*`
   - `org.apache.http.config.*` → `org.apache.hc.core5.http.io.*`
   - `org.apache.http.protocol.*` → `org.apache.hc.core5.http.protocol.*`
   - `org.apache.http.conn.ssl.*` → `org.apache.hc.client5.http.ssl.*`
   - `org.apache.http.*` (core types) → `org.apache.hc.core5.http.*`
3. Process rows in `kind` order: `package` first, then `class`, then `interface`, then `method`

### Part 2: API replacements

4. For renamed/replaced APIs, apply the transformations from the api-map:
   - `HttpRequestBase` → `HttpUriRequestBase`
   - `HttpEntityEnclosingRequest` → `HttpEntityContainer`
   - `BasicHttpContext` → `HttpCoreContext` / `HttpClientContext`
   - `SSLConnectionSocketFactory` → use `ClientTlsStrategyBuilder`
   - `getAllHeaders()` → `getHeaders()`
   - `getStatusLine().getStatusCode()` → `getCode()`
   - `getStatusLine()` → `new StatusLine(response)`
   - `getRequestLine()` → `new RequestLine(request)`
   - `setRetryHandler()` → `setRetryStrategy()`
   - `closeExpiredConnections()` → `closeExpired()`
   - `closeIdleConnections()` → `closeIdle()`

### Part 3: Pattern changes

5. Read `references/pattern-map.md`
6. Apply structural changes:
   - **Timeouts:** Replace `int` millisecond values with `Timeout` objects
   - **Time values:** Replace `long, TimeUnit` pairs with `TimeValue` objects
   - **Connection manager:** Use `PoolingHttpClientConnectionManagerBuilder` instead of constructor
   - **TLS/SSL:** Replace `SSLConnectionSocketFactory` with `ClientTlsStrategyBuilder`
   - **Timeout location:** Move `connectTimeout` and `socketTimeout` from `RequestConfig` to `ConnectionConfig`
   - **Socket config:** Move from `HttpClients.custom()` to connection manager builder
   - **Status line:** Replace `response.getStatusLine().getStatusCode()` with `response.getCode()`
   - **Entity creation:** Use `HttpEntities.create()` factory methods
   - **Request building:** Consider `ClassicRequestBuilder` for entity-carrying requests

7. Run the build gate

## Build gate

Run the project build command. If it fails, check for:
- Missing imports after package moves
- `int` timeout parameters that need `Timeout` wrapping
- `RequestConfig` containing `connectTimeout`/`socketTimeout` (move to `ConnectionConfig`)
- `SSLConnectionSocketFactory` usage (replace with TLS strategy)
