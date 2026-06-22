# Migration Skills Generator

Generate executable [agentskills.io](https://agentskills.io) migration skills from framework migration guides.

## What This Does

Given a migration guide (URL or local file), the generator skill reads it and produces a complete migration skill — organized mapping tables (dependency, API, config, pattern) plus phased workflow instructions — that any LLM agent can use to migrate a real codebase.

## How It Works

```
Migration Guide (URL/file)
  → generator/ meta-skill
    → Phase 1: Ingest (fetch, normalize, detect language)
    → Phase 2: Extract (fill mapping tables from guide)
    → Phase 3: Compose (assemble skill from templates)
    → Phase 4: Validate (structure, completeness, consistency)
  → skills/<language>/<migration-name>/ (output)
```

The generator uses **rigid templates** for skill structure and **table schemas** for mapping data. The LLM only fills in table rows — it doesn't design workflow or structure. This makes extraction consistent across models and runs.

## Quick Start

1. Install the generator skill:
   ```
   npx skills add <this-repo-url>
   ```

2. Ask your agent to generate a migration skill:
   ```
   Generate a migration skill from https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-4.0-Migration-Guide
   ```

3. The generated skill appears in `skills/<language>/<migration-name>/`.

4. To use a generated skill on a project, install it:
   ```
   npx skills add ./skills/java/spring-boot-3-to-4
   ```
   Then ask your agent to run the migration on your codebase.

## Available Migration Skills

| Skill | Source → Target | Link |
|-------|----------------|------|
| [Spring Boot 2 to 3](skills/java/spring-boot-2-to-3/) | Spring Boot 2.7 → 3.0 | [SKILL.md](skills/java/spring-boot-2-to-3/SKILL.md) |
| [Spring Boot 3 to 4](skills/java/spring-boot-3-to-4/) | Spring Boot 3.5 → 4.0 | [SKILL.md](skills/java/spring-boot-3-to-4/SKILL.md) |
| [Spring Boot to Quarkus](skills/java/spring-boot-to-quarkus/) | Spring Boot 3.x → Quarkus 3.x | [SKILL.md](skills/java/spring-boot-to-quarkus/SKILL.md) |
| [JDK 21 to 25](skills/java/jdk-21-to-25/) | JDK 21 → JDK 25 | [SKILL.md](skills/java/jdk-21-to-25/SKILL.md) |
| [Spring Framework 5 to 6](skills/java/spring-framework-5-to-6/) | Spring Framework 5.x → 6.x | [SKILL.md](skills/java/spring-framework-5-to-6/SKILL.md) |
| [Spring Framework 4 to 5](skills/java/spring-framework-4-to-5/) | Spring Framework 4.x → 5.x | [SKILL.md](skills/java/spring-framework-4-to-5/SKILL.md) |
| [Java EE to Jakarta EE](skills/java/javax-to-jakarta-ee/) | Java EE 8 (javax) → Jakarta EE 10 (jakarta) | [SKILL.md](skills/java/javax-to-jakarta-ee/SKILL.md) |
| [JBoss EAP 7 to 8](skills/java/jboss-eap-7-to-8/) | JBoss EAP 7.x → 8.0 | [SKILL.md](skills/java/jboss-eap-7-to-8/SKILL.md) |
| [Hibernate 5 to 6](skills/java/hibernate-5-to-6/) | Hibernate ORM 5.x → 6.x | [SKILL.md](skills/java/hibernate-5-to-6/SKILL.md) |
| [HttpClient 4 to 5](skills/java/httpclient-4-to-5/) | Apache HttpClient 4.x → 5.x | [SKILL.md](skills/java/httpclient-4-to-5/SKILL.md) |
| [Camel 3 to 4](skills/java/camel-3-to-4/) | Apache Camel 3.x → 4.0 | [SKILL.md](skills/java/camel-3-to-4/SKILL.md) |
| [OpenTracing to OpenTelemetry](skills/java/opentracing-to-opentelemetry/) | OpenTracing → OpenTelemetry | [SKILL.md](skills/java/opentracing-to-opentelemetry/SKILL.md) |
| [Agent Sandbox v1alpha1 to v1beta1](skills/kubernetes/agent-sandbox-v1alpha1-to-v1beta1/) | Agent Sandbox CRD v1alpha1 → v1beta1 | [SKILL.md](skills/kubernetes/agent-sandbox-v1alpha1-to-v1beta1/SKILL.md) |

## Project Structure

- `generator/` — The meta-skill that generates migration skills
  - `SKILL.md` — Main orchestration
  - `modules/` — Phase instructions (ingest, extract, compose, validate)
  - `references/` — Templates and table schemas
  - `examples/` — Hand-curated reference skills for few-shot grounding
- `skills/` — Generated output, organized by language
  - `java/`
    - [`spring-boot-2-to-3/`](skills/java/spring-boot-2-to-3/) — Spring Boot 2.7 to 3.0 migration (javax→jakarta, Hibernate 6.0/6.1, Spring Security 6.0, Flyway 9.0, 100+ config property renames, actuator/metrics restructuring)
    - [`spring-boot-3-to-4/`](skills/java/spring-boot-3-to-4/) — Spring Boot 3.5 to 4.0 migration (modular redesign, Jackson 3, @MockBean removal, starter renames, config property migrations)
    - [`spring-boot-to-quarkus/`](skills/java/spring-boot-to-quarkus/) — Spring Boot 3.x to Quarkus 3.x migration (cross-framework, DI/REST/config rewrites, dual path: pure Quarkus or Spring compat extensions)
    - [`spring-framework-4-to-5/`](skills/java/spring-framework-4-to-5/) — Spring Framework 4.x to 5.x migration (JDK 8+/Java EE 7+ baseline, Portlet/Velocity/JasperReports removal, Guava→Caffeine, spring-jcl logging, WebFlux introduction, 107 migration items across 5.0/5.1/5.2/5.3)
    - [`spring-framework-5-to-6/`](skills/java/spring-framework-5-to-6/) — Spring Framework 5.x to 6.x migration (Jakarta EE 9+ baseline, RPC remoting removal, HttpMethod enum→class, parameter name retention, property placeholder rewrite, 101 migration items across 6.0/6.1/6.2)
    - [`javax-to-jakarta-ee/`](skills/java/javax-to-jakarta-ee/) — Java EE 8 to Jakarta EE 10 namespace migration (framework-agnostic javax→jakarta, 39 package renames, 35 dependency coordinate changes, XML deployment descriptor namespace updates, ServiceLoader file renames, 3 removed specifications, Java SE javax exceptions list, 126 migration items)
    - [`jboss-eap-7-to-8/`](skills/java/jboss-eap-7-to-8/) — JBoss EAP 7.x to 8.0 migration (Jakarta EE 8→10 namespace, 25+ JBoss spec artifact renames, PicketBox/PicketLink→Elytron security, Servlet 6.0/Faces 4.0/CDI 4.0 API removals, JAXB impl relocation, RESTEasy 6.2, HornetQ→Artemis messaging, EJB client/mod_cluster/Undertow config changes, Hibernate 5→6, 225 migration items)
    - [`hibernate-5-to-6/`](skills/java/hibernate-5-to-6/) — Hibernate ORM 5.x to 6.x migration (Jakarta Persistence, type system overhaul, SQM query model, per-entity ID sequences, Duration/UUID/Instant/array/enum type mapping changes, legacy Criteria removal, dialect simplification, multitenancy simplification, 153 migration items across 6.0/6.1/6.2)
    - [`jdk-21-to-25/`](skills/java/jdk-21-to-25/) — JDK 21 to 25 migration (Security Manager removal, sun.misc.Unsafe deprecation, ZGC changes, JVM option removals)
    - [`httpclient-4-to-5/`](skills/java/httpclient-4-to-5/) — Apache HttpClient 4.x to 5.x migration (package relocation, timeout/TLS restructuring, optional async API migration)
    - [`camel-3-to-4/`](skills/java/camel-3-to-4/) — Apache Camel 3.x to 4.0 migration (Java 17 required, 34 removed components, JUnit 5 required, CamelContext/Exchange API decoupling, XML/YAML DSL changes, HttpComponents v5, health check defaults, micrometer naming, 123 migration items)
    - [`opentracing-to-opentelemetry/`](skills/java/opentracing-to-opentelemetry/) — OpenTracing to OpenTelemetry migration (io.opentracing→io.opentelemetry API replacement, Jaeger client removal, Tags→Attributes, error handling→StatusCode+recordException, @Traced→@WithSpan, baggage architecture change, Jaeger→OTLP config, W3C TraceContext propagation, 75 migration items)
  - `kubernetes/`
    - [`agent-sandbox-v1alpha1-to-v1beta1/`](skills/kubernetes/agent-sandbox-v1alpha1-to-v1beta1/) — Agent Sandbox CRD v1alpha1 to v1beta1 migration (SandboxClaim field restructure warmpool→warmPoolRef, Sandbox replicas→operatingMode, two-phase bootstrap+migrate script, shadow pool management, Helm/kubectl flows, emergency rollback, 29 migration items)

## Generated Skill Format

Each generated skill follows the [agentskills.io](https://agentskills.io/specification) format:

```
skills/<language>/<migration-name>/
├── SKILL.md              # Phased migration workflow
├── modules/              # Per-phase instructions
│   ├── build-system.md
│   ├── code.md
│   ├── config.md
│   ├── testing.md
│   └── cleanup.md
└── references/           # Mapping tables
    ├── dependency-map.md
    ├── api-map.md
    └── config-map.md
```

## License

Apache-2.0
