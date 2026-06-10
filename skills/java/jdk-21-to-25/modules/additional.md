# Phase: Additional Changes

Handle JVM options, runtime flags, tools, and infrastructure changes that fall outside standard build/code/config phases.

## Steps

1. Read `references/pattern-map.md` — filter for rows with category `removal` that relate to JVM flags, tools, or infrastructure
2. For each relevant row:
   - Read the `description` to understand what changed
   - If `before`/`after` are provided, use them as transformation guides

### JVM command-line options

Search all launch scripts, Dockerfiles, CI/CD configs, and build files for these removed/deprecated flags:

**Removed (will cause startup failure):**
- `-t`, `-tm`, `-Xfuture`, `-checksource`, `-cs`, `-noasyncgc` (removed JDK 24)
- `-Xnoagent` (removed JDK 23)
- `-XX:+RegisterFinalizersAtInit` (obsoleted JDK 23)

**Deprecated (will emit warnings):**
- `-Xdebug`, `-debug` (deprecated JDK 22)
- `-verbosegc` → use `-verbose:gc`
- `-noclassgc` → use `-XX:-ClassUnloading`
- `-verify`, `-verifyremote` → use `-Xverify`
- `-ss` → use `-Xss`
- `-ms` → use `-Xms`
- `-mx` → use `-Xmx`
- `-XX:+PreserveAllAnnotations` (deprecated JDK 23)
- `-XX:+DontYieldALot` (deprecated JDK 23)
- `-XX:+UseEmptySlotsInSupers` (deprecated JDK 23)
- `-XX:+UseNotificationThread` (deprecated JDK 23)
- `-XX:LockingMode` (deprecated JDK 24)
- `-XX:+UseCompressedClassPointers` (deprecated JDK 25)

### ZGC configuration

- Remove `-XX:-ZGenerational` if present (non-generational mode removed in JDK 24)
- Review ZGC tuning parameters — generational mode may need different tuning

### Graal JIT

- Remove `-XX:+UnlockExperimentalVMOptions -XX:+UseGraalJIT` if present (Graal JIT removed in JDK 25)

### JNI native access

- If the application uses JNI, expect runtime warnings on stderr (JEP 472, JDK 24)
- Add `--enable-native-access=<module>` to suppress warnings for known safe native access

### Tools

- `jstatd` — deprecated for removal (JDK 24); plan migration to JMX or JFR
- `jrunscript` — deprecated for removal (JDK 24); use external scripting runtime
- `jhsdb debugd` — deprecated for removal (JDK 24); use alternative debugging
- `jdeps -profile` / `-P` — removed (JDK 22)

### Certificates

- Baltimore CyberTrust Root CA and two Camerfirma Root CAs removed from default truststore in JDK 25
- If your application relies on these certificates, add them to your custom truststore

### Infrastructure

- 32-bit x86 platform support removed (JEP 503, JDK 25)
- Linux GTK2 support removed (JDK 24) — ensure GTK3 is available for Swing apps on Linux
- Dockerfiles/base images must use JDK 25-compatible 64-bit OS

3. Run the build gate

## Build gate

Run the project build command. For infrastructure changes, also verify the application starts successfully.
