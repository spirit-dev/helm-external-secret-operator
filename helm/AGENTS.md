# helm/ — External Secrets Operator chart

## Purpose

Helm chart deploying the [External Secrets Operator](https://external-secrets.io) into `kube-system` (release `external-secret-operator-turingpi`).

## Ownership

- `Chart.yaml` / `Chart.lock` / `values.yaml` — chart definition and defaults
- `values.turingpi.yaml` — environment overrides (currently the only env)
- `templates/` — rendered resources, all values-driven per environment:
  - `registry-secrets.yaml` — Vault-backed image pull secrets: one shared `ClusterSecretStore` + one `ExternalSecret` per `registryPullSecrets.secrets` entry
  - `secret-syncs.yaml` — Vault-backed plain secrets: one `ExternalSecret` per `secretSyncs.secrets` entry, body templated by ESO at sync time; `storeName` reuses
    the shared store. Two entry forms: single-key (legacy: `secretKey`/`template`, with `data[].secretKey` hardcoded to `value` so the ESO template body
    references `.value`; `property` selects the KV field, default `value` — what `vault-put.sh` in gitlab-token-updater writes) and multi-key (`keys` list
    syncing several KV fields of one record into distinct secret keys; ESO exposes provider values in the template body by secretKey, `missingkey=error`).
    `homeassistant-config-git` (ns `homeassistant`, keys `user`+`token`) is the multi-key consumer of the gitlab-token-updater-rotated
    `gitlab/tokens/homeassistant-config` record.

## Local Contracts

- `helm/templates/` is excluded from `check-yaml` in pre-commit (templated YAML is not valid YAML).
- Chart is installed via Argocd; `make dry-run|install` (from repo root) is for first-time/troubleshooting only, with `-f values.<ENV>.yaml`.
- CRDs are applied with ArgoCD Server-Side Apply (`argocd.argoproj.io/sync-options: ServerSideApply=true` via `crds.annotations`) because they exceed the 256KB annotation limit under client-side apply. Keep this annotation when upgrading the subchart.

## Verification

- `make template ENV=<ENV>` renders the chart; pre-commit `helmlint` lints it.

## Child DOX Index

None — no subdirectory is a durable boundary.
