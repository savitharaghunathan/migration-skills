# Phase: Build System Migration

Update dependencies and build plugins before touching application code.

## Steps

1. Read `references/dependency-map.md`
2. Open the project's build file(s):
   - Maven: `pom.xml`
   - Gradle: `build.gradle` or `build.gradle.kts`
3. **Upgrade Spring Framework BOM/dependencies to 6.x**
   - Update `spring-framework-bom` or individual Spring module versions to 6.0+
4. **Remove deprecated libraries:**
   - Remove Joda-Time dependencies (`joda-time:joda-time`) — migrate to `java.time`
   - Remove EhCache 2 (`net.sf.ehcache:ehcache`) — replace with EhCache 3 (`org.ehcache:ehcache` with `jakarta` classifier)
   - Remove Spring remoting modules if present
   - Remove JCA CCI connector support
5. **Upgrade servlet container versions:**
   - Tomcat 10+ (Jakarta EE 9) or 10.1+ (Jakarta EE 10)
   - Jetty 11+ (Jakarta EE 9)
   - Undertow 2.2.19+ with `undertow-servlet-jakarta` artifact, or Undertow 2.3+ (Jakarta EE 10)
6. **Add required new dependencies:**
   - `io.micrometer:micrometer-observation:1.10+` (compile dependency of `spring-web`)
7. **Upgrade minimum versions:**
   - SnakeYAML 2.0+, Jackson 2.14+ (2.18/2.19 recommended), FreeMarker 2.3.33+
   - HtmlUnit 4.2+ (if used in tests) — update Selenium driver to `org.seleniumhq.selenium:htmlunit3-driver`
   - Replace `org.webjars:webjars-locator-core` with `org.webjars:webjars-locator-lite`
8. **Configure -parameters compiler flag:**
   - Maven: add `<parameters>true</parameters>` to `maven-compiler-plugin` configuration
   - Gradle (Kotlin DSL): `tasks.withType<JavaCompile>() { options.compilerArgs.add("-parameters") }`
   - Gradle (Groovy DSL): `tasks.withType(JavaCompile).configureEach { options.compilerArgs.add("-parameters") }`
9. For each remaining row in the dependency map, apply the action and check notes
10. Run the build gate

## Build gate

Run the project build command (`mvn compile` or `gradle build`). If it fails, fix dependency issues before proceeding. Common issues:
- Jakarta EE 9 vs 10 version mismatches between servlet container and API jars
- Transitive dependencies still pulling in javax.* artifacts
- Missing `jakarta` classifier on EhCache 3
- Compiler plugin not configured with `-parameters` flag
