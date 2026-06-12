# Phase: Cleanup and Verification

Remove compatibility shims, dead code, and verify the final build.

## Steps

1. **Remove javax imports:** Search for any remaining `javax.servlet`, `javax.persistence`, `javax.validation`, `javax.annotation` (Jakarta EE variants), `javax.inject`, `javax.transaction`, `javax.mail`, `javax.websocket`, `javax.xml.bind`, `javax.activation` imports. These indicate missed transformations — go back and fix them.
   - **Exception:** `javax.xml.datatype`, `javax.xml.parsers`, `javax.xml.transform`, `javax.crypto`, `javax.net`, `javax.sql`, `javax.security.auth` — these are Java SE packages and should NOT be changed.

2. **Remove spring-boot-properties-migrator:** If added during the build system phase, remove it from the build file. It was only needed temporarily to diagnose property changes.

3. **Check for old Hibernate types:** Search for `@TypeDef`, `@TypeDefs`, `@AnyMetaDef`, `@AnyMetaDefs`, `hibernate_sequence` references, `org.hibernate.type.descriptor.sql.SqlTypeDescriptor`, `org.hibernate.type.descriptor.java.JavaTypeDescriptor`. Any remaining references are migration gaps.

4. **Check for old Spring Security patterns:** Search for `WebSecurityConfigurerAdapter`, `antMatchers(`, `mvcMatchers(`, `regexMatchers(`, `@EnableGlobalMethodSecurity`. Any remaining references need migration.

5. **Check for old actuator references:** Search for `httptrace`, `HttpTraceRepository`, `management.metrics.export.` (old prefix pattern).

6. **Verify no old artifacts remain:**
   - Search build files for old artifact coordinates from `references/dependency-map.md`
   - Search config files for old property names from `references/config-map.md`

7. **Final build and test:**
   - Run the full build: compilation + tests
   - If the project has integration tests, run those too
   - Verify the application starts successfully

8. **Report to user:**
   - List all changes made across all phases
   - Flag any items that could not be automatically migrated (require manual review)
   - Note behavioral changes from `references/pattern-map.md` that the user should verify manually, especially:
     - Trailing slash URL matching behavior
     - Hibernate query result type changes
     - Spring Security dispatch type authorization
     - Actuator endpoint sanitization (all values now masked by default)
