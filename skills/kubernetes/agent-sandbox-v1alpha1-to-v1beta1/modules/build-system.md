# Phase: Build System Migration

Update controller manifests and CRDs from v1alpha1 to v1beta1.

## Steps

1. Read `references/dependency-map.md`
2. **Back up all resources** before making any changes:
   ```bash
   kubectl get sandboxes,sandboxclaims,sandboxtemplates,sandboxwarmpools \
     -A -o yaml > agent-sandbox-backup-$(date -u +%Y%m%dT%H%M%SZ).yaml
   ```
3. **Run bootstrap phase** (BEFORE upgrading CRDs):
   ```bash
   bash dev/tools/migrate.sh --phase=bootstrap --dry-run   # inspect first
   bash dev/tools/migrate.sh --phase=bootstrap
   ```
   This pre-creates `shadow-pool-<template>` pools for cold-start claims. It operates on v1alpha1 API — this is the last step that does.
4. **Apply updated manifests** (choose Flow A or Flow B):

   **Flow A — kubectl:**
   ```bash
   kubectl apply -f https://github.com/kubernetes-sigs/agent-sandbox/releases/download/v0.5.0/manifest.yaml
   kubectl apply -f https://github.com/kubernetes-sigs/agent-sandbox/releases/download/v0.5.0/extensions.yaml
   ```

   **Flow B — Helm:**
   ```bash
   kubectl apply --server-side --force-conflicts -f path/to/chart/crds/
   helm upgrade agent-sandbox ./helm/ \
     --namespace agent-sandbox-system \
     --reuse-values \
     --set image.tag=<new-version> \
     --set controller.extensions=true
   ```
5. **Wait for controller and webhook:**
   ```bash
   kubectl rollout status deploy/agent-sandbox-controller -n agent-sandbox-system
   kubectl wait --for=condition=Ready pods -l app=agent-sandbox-controller -n agent-sandbox-system
   until kubectl get sandboxwarmpools.extensions.agents.x-k8s.io -A >/dev/null 2>&1; do
     echo "Waiting for conversion webhook to be responsive..."
     sleep 2
   done
   ```

## Build gate

Verify:
- Controller pod is Ready
- Conversion webhook responds to `kubectl get sandboxwarmpools.extensions.agents.x-k8s.io -A`
- For GKE private clusters: ensure firewall allows ingress from master IP range to workers on TCP 9443
