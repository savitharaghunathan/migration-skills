# Verify Errors Schema

Each row represents a common build, deployment, or runtime error encountered during migration, along with its cause and prescribed fix.

## Columns

| Column | Required | Description |
|--------|----------|-------------|
| error_pattern | yes | Distinctive substring of the error message, backtick-wrapped, matchable against build or smoke output (e.g., `` `package javax.ejb does not exist` ``, `` `Unsatisfied dependency for type` ``) |
| cause | yes | Why this error occurs during migration — what was changed or left unchanged that triggers it |
| fix | yes | The corrective action. If the error is expected and will be resolved by a later phase, use the format: `Expected — will fix in [Phase Name] phase` |
| phase | yes | Which migration phase this error most likely surfaces in. Must match a phase name from the generated SKILL.md, or `general` for errors that can appear in any phase |
| source_section | recommended | Guide section heading this error relates to, or `experience` if the error comes from real-world migration runs rather than the guide text |

## Phase Values

The `phase` column ties each error to when it surfaces during execution. Use the phase names from the generated SKILL.md (e.g., `Build system`, `Code`, `Config`, `Cleanup`). Use `general` for errors not tied to a specific phase.

Some errors in early phases are expected and will self-resolve in later phases. Mark these with `Expected — will fix in [Phase Name] phase` in the `fix` column so the fix loop skips them rather than attempting a premature fix.

## Example Rows

| error_pattern | cause | fix | phase | source_section |
|---|---|---|---|---|
| `package javax.ejb does not exist` | Java EE dependency removed but EJB code not yet migrated | Expected — will fix in Code phase. If blocking, add the CDI extension temporarily | Build system | Dependency Changes |
| `Ambiguous dependencies for type EntityManager` | Manual `@Produces EntityManager` producer conflicts with framework-managed synthetic bean | Remove the `@Produces` method and its enclosing class — the target framework injects `EntityManager` directly via `@Inject` | Code | experience |
| `Unsatisfied dependency for type X` (bean uses `@SessionScoped`) | `@SessionScoped` requires a servlet container; target framework does not support it without an explicit extension | Replace `@SessionScoped` with `@ApplicationScoped` or `@RequestScoped` | Code | experience |
