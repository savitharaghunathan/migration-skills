# Phase: Cleanup and Verification

Prune storedVersions, clean up shadow pools, and verify final state.

## Steps

1. **Prune CRD storedVersions** — if any CRD still lists both `["v1alpha1","v1beta1"]`:
   ```bash
   for crd in \
       sandboxes.agents.x-k8s.io \
       sandboxclaims.extensions.agents.x-k8s.io \
       sandboxtemplates.extensions.agents.x-k8s.io \
       sandboxwarmpools.extensions.agents.x-k8s.io; do
     kubectl patch crd "${crd}" --subresource=status --type=merge \
       -p '{"status":{"storedVersions":["v1beta1"]}}'
   done
   ```
   Only do this after confirming every resource has `agents.x-k8s.io/storage-migrated-at`.

2. **Clean up shadow pools** — once all claims have been re-pointed to real warm pools:
   ```bash
   kubectl get sandboxwarmpools -A -o json \
     | jq -r '.items[]
         | select(.metadata.annotations["agents.x-k8s.io/migration-shadow"]=="true")
         | "\(.metadata.namespace) \(.metadata.name)"' \
     | while read ns name; do
         # Verify no claims reference this shadow pool
         refs=$(kubectl get sandboxclaims -n "$ns" -o json \
           | jq -r ".items[] | select(.spec.warmPoolRef.name==\"$name\") | .metadata.name")
         if [ -z "$refs" ]; then
           echo "Deleting unused shadow pool: $ns/$name"
           kubectl delete sandboxwarmpool "$name" -n "$ns"
         else
           echo "Shadow pool $ns/$name still referenced by: $refs — skipping"
         fi
       done
   ```

3. **Verify no v1alpha1 references remain:**
   - Search all manifests, Helm values, CI/CD pipelines, and scripts for `agents.x-k8s.io/v1alpha1`
   - Search for `spec.warmpool:` (old field name) — should now be `spec.warmPoolRef.name:`
   - Search for `spec.replicas:` on Sandbox resources — should now be `spec.operatingMode:`

4. **Verify final state:**
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
   All should show `["v1beta1"]` only.

5. **Report to user:**
   - Confirm all resources migrated successfully
   - List any shadow pools still in use (cannot be deleted yet)
   - Flag any claims that reference missing pools (operator action required)
   - Note the backup file location for future reference
   - Reference emergency rollback procedure if issues arise later

## Emergency rollback

If critical issues arise after migration, a 7-step rollback to v1alpha1 is available:
1. Disable conversion webhooks on all CRDs
2. Scale down controller deployment
3. Delete all Agent Sandbox resources
4. Delete shadow pools
5. Reset CRD storedVersions to v1alpha1
6. Revert CRD manifests and controller to old version
7. Restore data from backup

See the full procedure in `references/pattern-map.md` (row: Emergency rollback procedure).
