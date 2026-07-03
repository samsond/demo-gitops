# demo-gitops

GitOps configuration repository for **_Kargo in Action_** (Manning). When Kargo promotes a new image tag, it updates the Kustomize overlay in this repo, and Argo CD syncs the change into the cluster.

This repo backs two parts of the book.

## Chapter 3 — Getting started (the `demo-app` walkthrough)

A single service, `demo-app`, promoted through `dev → staging → prod`. This is the content you fork/clone in chapter 3.

```
base/                       # demo-app base manifests
envs/{dev,staging,prod}/    # per-environment Kustomize overlays (Kargo writes the image tag here)
setup/                      # Kargo Project, Warehouse, Stages + the Argo CD Applications
configure.sh                # helper to point the setup at your fork
```

Kargo's `kustomize-set-image` step writes to `envs/<env>/`, and each Argo CD Application watches one of those overlays.

## Chapter 7 — Scaling across services (`payments` + `auth`)

Two coordinated services demonstrating the multi-service patterns.

```
apps/{payments,auth}/base + envs/{dev,staging,prod}/
argocd/                     # one Argo CD Application per service/environment
kargo/                      # Kargo Project, Warehouses, and Stages for both services
```

- **dev** and **staging**: auto-promotion enabled
- **prod**: manual promotion required

## Related repositories

- [demo-app](https://github.com/kargobook/demo-app) — the `demo-app` sample image
- [payments-api](https://github.com/kargobook/payments-api), [auth-service](https://github.com/kargobook/auth-service) — chapter 7 sample services
- [kargo-in-action](https://github.com/kargobook/kargo-in-action) — the book's chapter-by-chapter example listings

## License

MIT
