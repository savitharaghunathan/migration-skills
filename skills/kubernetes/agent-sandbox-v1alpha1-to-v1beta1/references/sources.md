# Sources

## Primary Guide

- [v1alpha1 to v1beta1 Migration Guide | Agent Sandbox](https://agent-sandbox.sigs.k8s.io/docs/getting_started/api-migration-guide/)
  - What changes between versions
  - SandboxClaim is not field-compatible
  - Sandbox.spec.replicas becomes Sandbox.spec.operatingMode
  - Two phases
  - Before you start: back up your data
  - Migration flows (Flow A — kubectl, Flow B — Helm)
  - Dry-runs
  - After migration completes
  - Verifying the migration worked
  - Troubleshooting
  - Emergency Rollback Procedure
  - Recovery from Backup

## Project

- [Agent Sandbox GitHub](https://github.com/kubernetes-sigs/agent-sandbox)
- [Agent Sandbox Documentation](https://agent-sandbox.sigs.k8s.io/docs/getting_started/)
