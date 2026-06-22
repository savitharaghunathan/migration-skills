# Phase: Code Migration (Storage Rewrite)

Force-rewrite all Agent Sandbox resources from v1alpha1 to v1beta1 storage format in etcd.

## Steps

### Part 1: Run the migrate phase

1. Read `references/api-map.md` to understand what field changes are applied
2. Run the migration script:
   ```bash
   bash dev/tools/migrate.sh --phase=migrate --dry-run   # inspect first
   bash dev/tools/migrate.sh --phase=migrate
   ```
3. For large clusters, scope per namespace:
   ```bash
   bash dev/tools/migrate.sh --phase=migrate --namespace=team-alpha
   ```

The script patches every resource with a benign annotation (`agents.x-k8s.io/storage-migrated-at`), forcing the API server to read through the conversion webhook and rewrite to etcd in v1beta1 format.

### Part 2: Key field changes (handled by webhook)

The conversion webhook applies these automatically during the storage rewrite:

- **SandboxClaim:** `spec.sandboxTemplateRef` + `spec.warmpool` → `spec.warmPoolRef.name`
  - Specific pool name (`warmpool: my-pool`) → used verbatim
  - Empty/default, warm-started → derives pool name via `stripRandomSuffix(sandboxName)`
  - Empty/none/default, cold-start → redirects to `shadow-pool-<template-name>`
- **Sandbox:** `spec.replicas` → `spec.operatingMode`
  - `replicas: 0` → `Suspended`
  - `replicas: 1` (or unset) → `Running`
- **SandboxTemplate, SandboxWarmPool:** Structurally identical but storage rewrite ensures etcd holds v1beta1 form

### Part 3: Handle operator action items

If the bootstrap phase printed `OPERATOR ACTION REQUIRED` for claims referencing non-existent specific pools:
1. Create the missing pools manually, OR
2. Re-point claims to existing pools:
   ```bash
   kubectl patch sandboxclaim <name> -n <ns> --type=merge \
     -p '{"spec":{"warmPoolRef":{"name":"my-preferred-pool"}}}'
   ```

## Build gate

Both phases are idempotent — safe to re-run. If the migrate phase reports failures on specific resources, re-run the script. If a specific resource keeps failing, inspect it:
```bash
kubectl get <kind> <name> -n <ns> -o yaml
```
Check for conversion-webhook errors from bad field combinations.
