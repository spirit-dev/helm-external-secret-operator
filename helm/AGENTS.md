# helm/ — External Secrets Operator chart

## Purpose

Helm chart deploying the [External Secrets Operator](https://external-secrets.io) into `kube-system` (release `external-secret-operator-turingpi`).

## Ownership

- `Chart.yaml` / `Chart.lock` / `values.yaml` — chart definition and defaults
- `values.turingpi.yaml` — environment overrides (currently the only env)
- `templates/` — rendered resources, all values-driven per environment:
  - `registry-secrets.yaml` — Vault-backed image pull secrets: one shared `ClusterSecretStore` + one `ExternalSecret` per `registryPullSecrets.secrets` entry
  - `secret-syncs.yaml` — Vault-backed plain secrets: one `ExternalSecret` per `secretSyncs.secrets` entry, body templated by ESO at sync time; `storeName` reuses the shared store

## Local Contracts

- `helm/templates/` is excluded from `check-yaml` in pre-commit (templated YAML is not valid YAML).
- Chart is installed via Argocd; `make dry-run|install` (from repo root) is for first-time/troubleshooting only, with `-f values.<ENV>.yaml`.

## Verification

- `make template ENV=<ENV>` renders the chart; pre-commit `helmlint` lints it.

## Child DOX Index

None — no subdirectory is a durable boundary.
