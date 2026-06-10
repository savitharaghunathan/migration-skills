# Phase: Cleanup and Verification

Remove compatibility shims, dead code, and verify the final build.

## Steps

1. **Remove compatibility imports:** Search for any remaining imports of old packages that should have been migrated in earlier phases:
   - `import sun.misc.Unsafe`
   - `import java.beans.beancontext.*`
   - `import java.security.Policy`
   - Any `javax.naming.Context.APPLET` references

2. **Remove Security Manager artifacts:** Search for and remove:
   - `System.setSecurityManager(...)` calls
   - `System.getSecurityManager()` calls
   - `SecurityManager` subclass definitions
   - `.policy` files that are no longer loaded
   - `@SuppressWarnings` annotations related to Security Manager deprecation

3. **Remove suppression annotations:** Search for `@SuppressWarnings("removal")` or `@SuppressWarnings("deprecation")` that referenced APIs from JDK 21 that are now fully removed.

4. **Verify no old artifacts remain:**
   - Search the entire codebase (including build files, config, launch scripts, and source) for:
     - `sun.misc.Unsafe` (except `allocateInstance` which is still available)
     - `Thread.suspend`, `Thread.resume`, `ThreadGroup.stop`
     - Removed JVM flags (`-Xnoagent`, `-Xfuture`, `-checksource`, etc.)
     - `java.locale.providers=JRE` or `COMPAT`
     - `java.security.manager` / `java.security.policy`
     - String Template syntax (`STR."..."`, `FMT."..."`)
   - Any remaining references are migration gaps — fix them

5. **Check preview feature usage:**
   - If the project used `--enable-preview` with JDK 21, review which preview features were used
   - Features finalized in JDK 25: Scoped Values (JEP 506), Flexible Constructor Bodies (JEP 513), Module Import Declarations (JEP 511), Compact Source Files (JEP 512)
   - Features still in preview: Structured Concurrency (JEP 505), Primitive Patterns (JEP 507), Stable Values (JEP 502)
   - Features withdrawn: String Templates — must be reverted

6. **Final build and test:**
   - Run the full build: compilation + tests
   - If the project has integration tests, run those too
   - Verify the application starts successfully
   - Check for runtime warnings about `sun.misc.Unsafe` usage or JNI

7. **Report to user:**
   - List all changes made across all phases
   - Flag any items that could not be automatically migrated (require manual review)
   - Note behavioral changes that the user should verify manually:
     - ZGC now generational only
     - Compact Object Headers enabled by default
     - Virtual threads no longer pin in synchronized blocks
     - JNI runtime warnings
     - Stricter jar validation
     - JFR sampling behavior changes
