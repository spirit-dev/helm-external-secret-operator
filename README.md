# external-secret-operator



<!--TOC-->

- [Installation process](#installation-process)
- [External Secrets Operator](#external-secrets-operator)

<!--TOC-->

## Installation process

The installation is entirely managed by Argocd.

A `Makefile` is present here to ease the first and one-time deployment or in case of an issue.
The installation should be done in two steps:

```shell
#> make dry-run ENV=<ENV>
#> make install ENV=<ENV>
```

## External Secrets Operator

The [External Secrets Operator](https://external-secrets.io) reconciles Kubernetes `Secret` objects from external secret management systems (Cloudflare, Vault, GitLab CI variables, ...) using `SecretStore` and `ExternalSecret` resources.

`ClusterSecretStore` definitions for this cluster will live in this chart's `helm/templates/` directory (per-environment, values-driven), once the secret backends are chosen. Until then the chart only deploys the operator itself
