# Dependency Map — Agent Sandbox v1alpha1 to v1beta1

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| agent-sandbox manifest (old version) | `https://github.com/kubernetes-sigs/agent-sandbox/releases/download/v0.5.0/manifest.yaml` | >= v0.5.0 | replace | Core controller + base CRDs + webhook Service | Migration flows |
| agent-sandbox extensions (old version) | `https://github.com/kubernetes-sigs/agent-sandbox/releases/download/v0.5.0/extensions.yaml` | >= v0.5.0 | replace | Extensions API group CRDs: SandboxClaim, SandboxTemplate, SandboxWarmPool | Migration flows |
| agent-sandbox Helm chart (old version) | agent-sandbox Helm chart (new version) | >= v0.5.0 | replace | Requires `--set controller.extensions=true` if using extension resources; CRDs must be applied manually with `kubectl apply --server-side --force-conflicts` since Helm does not upgrade CRDs | Migration flows |
