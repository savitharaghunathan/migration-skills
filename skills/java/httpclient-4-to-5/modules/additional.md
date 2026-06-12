# Phase: Additional Changes

Optional migration to HttpClient 5.x async APIs. This phase is only needed if the project wants to leverage async/non-blocking HTTP or HTTP/2.

## Steps

1. Read `references/pattern-map.md` — filter for rows with `source_section` relating to async APIs or HTTP/2

### Migration path decision

2. Choose the async migration path (if any):
   - **Stay classic** — No changes needed beyond Phase 2. Classic APIs are fully supported in 5.x.
   - **Async with simple handlers** — Intermediate step; buffers messages in memory. Good for messages of known limited size.
   - **Async with streaming handlers** — Full async with content streaming. Best for large payloads or high-concurrency scenarios.
   - **HTTP/2 only** — Optimized for HTTP/2 with multiplexed request execution. No HTTP/1.1 fallback.

### Async with simple handlers

3. If migrating to async simple APIs:
   - Replace `PoolingHttpClientConnectionManager` with `PoolingAsyncClientConnectionManager`
   - Replace `CloseableHttpClient` with `CloseableHttpAsyncClient`
   - Add `client.start()` after building the client
   - Replace `HttpPost`/`HttpGet` with `SimpleHttpRequest` via `SimpleRequestBuilder`
   - Replace synchronous `client.execute()` with async `client.execute(request, callback)`
   - Handle results via `FutureCallback<SimpleHttpResponse>`
   - Replace `client.close()` with `client.close(CloseMode.GRACEFUL)`
   - Configure HTTP version policy via `TlsConfig.custom().setVersionPolicy(HttpVersionPolicy.NEGOTIATE)`

### Async with streaming handlers

4. If migrating to full async APIs:
   - Use `BasicRequestProducer` / `BasicResponseConsumer` for custom entity handling
   - Implement custom entity producers/consumers using `AbstractBinAsyncEntityConsumer`, `AbstractCharAsyncEntityConsumer`, etc.
   - Consider `AbstractClassicEntityConsumer`/`AbstractClassicEntityProducer` as compatibility adapters for InputStream/OutputStream-based code

### HTTP/2 only

5. If migrating to HTTP/2 optimized client:
   - Replace client builder: `HttpAsyncClients.customHttp2()` instead of `HttpAsyncClients.custom()`
   - TLS settings applied directly to client (no connection manager)
   - No HTTP proxy support in HTTP/2 optimized client
   - Request/response handlers remain the same

6. Run the build gate

## Build gate

Run the project build command. For async migration, also verify the application functions correctly with async request execution.
