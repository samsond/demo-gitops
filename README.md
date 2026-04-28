# Demo GitOps Repository

The GitOps repository for the example applications in the Kargo Book.

This repo is managed by **Argo CD** (for reconciliation) and **Kargo** (for promotion). When Kargo promotes a new image tag, it updates the Kustomize overlay in this repo, and Argo CD syncs the change into the cluster.

## Repository Structure

```
apps/
  payments/
    base/                     # Shared Deployment + Service manifests
    envs/
      dev/kustomization.yaml  # Dev overlay (image tag managed by Kargo)
      staging/                # Staging overlay
      prod/                   # Prod overlay (3 replicas)
  auth/
    base/
    envs/
      dev/
      staging/
      prod/

argocd/                       # Argo CD Application manifests
  payments-dev.yaml
  payments-staging.yaml
  payments-prod.yaml
  auth-dev.yaml
  auth-staging.yaml
  auth-prod.yaml

kargo/                        # Kargo resources
  project.yaml                # Kargo Project with promotion policies
  warehouse-payments.yaml     # Watches ghcr.io/kargobook/payments-api
  warehouse-auth.yaml         # Watches ghcr.io/kargobook/auth-service
  stages-payments.yaml        # dev → staging → prod pipeline
  stages-auth.yaml            # dev → staging → prod pipeline
```

## Stage Graph

```
payments-warehouse → payments-dev → payments-staging → payments-prod
auth-warehouse     → auth-dev     → auth-staging     → auth-prod
```

- **dev** and **staging**: auto-promotion enabled
- **prod**: manual promotion required

## Setup

1. Install Kargo and Argo CD in your cluster
2. Apply the Argo CD Applications: `kubectl apply -f argocd/`
3. Apply the Kargo resources: `kubectl apply -f kargo/`
4. Push a new semver tag to [payments-api](https://github.com/kargobook/payments-api) or [auth-service](https://github.com/kargobook/auth-service) and watch it promote through the stages

## Related Repos

- [kargobook/payments-api](https://github.com/kargobook/payments-api) — Payments API service
- [kargobook/auth-service](https://github.com/kargobook/auth-service) — Auth service

## License

MIT
