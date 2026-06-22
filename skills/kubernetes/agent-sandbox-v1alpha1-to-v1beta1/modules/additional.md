# Phase: Additional Changes

Post-migration verification, shadow pool management, and troubleshooting.

## Steps

1. Read `references/pattern-map.md` — filter for rows with category `addition` or `behavioral`

### Verify migration succeeded

Every resource should have the `agents.x-k8s.io/storage-migrated-at` annotation:
```bash
kubectl get sandboxes,sandboxclaims,sandboxtemplates,sandboxwarmpools -A -o json \
  | jq -r '.items[]
      | "\(.kind) \(.metadata.namespace)/\(.metadata.name) -> \(.metadata.annotations["agents.x-k8s.io/storage-migrated-at"] // "<missing>")"'
```

Verify CRD storedVersions:
```bash
for crd in \
    sandboxes.agents.x-k8s.io \
    sandboxclaims.extensions.agents.x-k8s.io \
    sandboxtemplates.extensions.agents.x-k8s.io \
    sandboxwarmpools.extensions.agents.x-k8s.io; do
  printf '%s: ' "${crd}"
  kubectl get crd "${crd}" -o jsonpath='{.status.storedVersions}'
  printf '\n'
done
```

### Shadow pool management

Bootstrap created `shadow-pool-<template>` pools marked with:
- `agents.x-k8s.io/migration-shadow: "true"`
- `agents.x-k8s.io/migration-source-template: <template-name>`

List them:
```bash
kubectl get sandboxwarmpools -A -o json \
  | jq -r '.items[]
      | select(.metadata.annotations["agents.x-k8s.io/migration-shadow"]=="true")
      | "\(.metadata.namespace)/\(.metadata.name) (for template: \(.metadata.annotations["agents.x-k8s.io/migration-source-template"]))"'
```

**Do not** delete shadow pools while any v1beta1 SandboxClaim still references them via `warmPoolRef`.

### Re-point claims to preferred pools

The `warmPoolRef.name` is editable on v1beta1 claims:
```bash
kubectl patch sandboxclaim <name> -n <ns> --type=merge \
  -p '{"spec":{"warmPoolRef":{"name":"my-preferred-pool"}}}'
```

### Troubleshooting

- **Migrate phase reports failures:** Re-run — both phases are idempotent. If a resource keeps failing, inspect with `kubectl get -o yaml`.
- **OPERATOR ACTION REQUIRED from bootstrap:** Claims reference specific pools that don't exist. Create pools manually or re-point claims.
- **Webhook timeouts in GKE private clusters:** Control plane VPC can't reach webhook port 9443. Create a firewall rule allowing ingress from GKE master IP range to workers on TCP 9443.

## Build gate

All resources should have `storage-migrated-at` annotation. All CRD storedVersions should include `v1beta1`.
