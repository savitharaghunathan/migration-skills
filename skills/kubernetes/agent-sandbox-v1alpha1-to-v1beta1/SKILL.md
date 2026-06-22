---
name: agent-sandbox-v1alpha1-to-v1beta1
description: Migrates Kubernetes Agent Sandbox CRDs from v1alpha1 to v1beta1 API.
  Use when upgrading an existing Agent Sandbox installation that has v1alpha1-serialized
  resources in etcd.
license: Apache-2.0
metadata:
  source: agent-sandbox-v1alpha1
  target: agent-sandbox-v1beta1
  language: kubernetes
  build_tool: "kubectl / helm"
  guide_url: https://agent-sandbox.sigs.k8s.io/docs/getting_started/api-migration-guide/
  generated_by: migration-skills-generator
  generated_at: 2026-06-22T00:00:00Z
---

# Agent Sandbox v1alpha1 to v1beta1 Migration

**Prerequisite:** This migration is only needed when upgrading existing installations with v1alpha1-serialized resources in etcd. Fresh installations with v1beta1-storage have nothing to migrate.

This migration upgrades Kubernetes Agent Sandbox CRDs (`Sandbox`, `SandboxClaim`, `SandboxTemplate`, `SandboxWarmPool`) from the v1alpha1 API to v1beta1. The key breaking change is that `SandboxClaim` is not field-compatible — `spec.sandboxTemplateRef` + `spec.warmpool` become `spec.warmPoolRef.name`. Additionally, `Sandbox.spec.replicas` becomes `Sandbox.spec.operatingMode`. A conversion webhook handles the field rewriting, but a two-phase migration script (bootstrap + migrate) is required to pre-create shadow pools and force-rewrite etcd storage.

## Phases

Execute in order. Each phase has explicit verification steps — do not proceed until they pass.

1. **Build system** — Update controller manifests and CRDs to v1beta1 versions. Read `modules/build-system.md`.
2. **Code** — Apply CRD field changes (handled by conversion webhook + migration script). Read `modules/code.md`.
3. **Additional** — Post-migration verification, shadow pool management, and operator action items. Read `modules/additional.md`.
4. **Cleanup** — Verify storage versions, prune storedVersions, clean up shadow pools. Read `modules/cleanup.md`.

## How to use

Load each phase's module when starting that phase. Each module references mapping tables in `references/` — apply every row in the relevant table to the cluster. Use the before/after examples as guides for each transformation.

## Build gate

After completing each phase:
1. Verify webhook is responsive: `kubectl get sandboxwarmpools.extensions.agents.x-k8s.io -A`
2. Verify controller is running: `kubectl rollout status deploy/agent-sandbox-controller -n agent-sandbox-system`
3. If either fails, check troubleshooting in `modules/additional.md`
4. If you cannot fix it, stop and report to the user
