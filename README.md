
## Roles

**Platform Admin**

- Has cluster admin access to the fleet of clusters
- Has maintainer access to the fleet Git repository
- Manages cluster wide resources (CRDs, controllers, cluster roles, etc)
- Onboards the tenant’s main `GitRepository` and `Kustomization` 
- Manages tenants by assigning namespaces, service accounts and role binding to the tenant's apps

**Tenant** 

- Has admin access to the namespaces assigned to them by the platform admin
- Has maintainer access to the tenant Git repository and apps repositories 
- Manages app deployments with `GitRepositories` and `Kustomizations`
- Manages app releases with `HelmRepositories` and `HelmReleases`

## Repository structure

The [platform admin repository](https://github.com/chenditc/multi-cluster-flux-example/tree/main) contains the following top directories:

- **cluster** dir contains the Flux configuration per cluster
- **infrastructure** dir contains common infra tools such as admission controllers, CRDs and cluster-wide polices
- **apps** dir contains namespaces, service accounts, role bindings for each app to run on a cluster

```
./
├── apps
│   ├── app1
│   │   └── base
│   │       ├── kustomization.yaml
│   │       ├── rbac.yaml
│   │       └── sync.yaml
│   └── app3
│       └── base
│           ├── kustomization.yaml
│           ├── rbac.yaml
│           └── sync.yaml
├── clusters
│   ├── store1
│   │   ├── ai
│   │   │   ├── app1
│   │   │   │   ├── cluster-patch.yaml
│   │   │   │   └── kustomization.yaml
│   │   │   ├── flux-system
│   │   │   │   ├── gotk-components.yaml
│   │   │   │   ├── gotk-sync.yaml
│   │   │   │   └── kustomization.yaml
│   │   │   └── infrastructure.yaml
│   │   └── checkout
│   │       ├── app3
│   │       │   ├── cluster-patch.yaml
│   │       │   └── kustomization.yaml
│   │       ├── flux-system
│   │       │   ├── gotk-components.yaml
│   │       │   ├── gotk-sync.yaml
│   │       │   └── kustomization.yaml
│   │       └── infrastructure.yaml
│   ├── store2
│   │   ├── ai
│   │   │   ├── app1
│   │   │   │   ├── cluster-patch.yaml
│   │   │   │   └── kustomization.yaml
│   │   │   ├── flux-system
│   │   │   │   ├── gotk-components.yaml
│   │   │   │   ├── gotk-sync.yaml
│   │   │   │   └── kustomization.yaml
│   │   │   └── infrastructure.yaml
│   │   └── checkout
│   │       ├── app3
│   │       │   ├── cluster-patch.yaml
│   │       │   └── kustomization.yaml
│   │       ├── flux-system
│   │       │   ├── gotk-components.yaml
│   │       │   ├── gotk-sync.yaml
│   │       │   └── kustomization.yaml
│   │       └── infrastructure.yaml
│   └── store3
│       ├── ai
│       │   ├── app1
│       │   │   ├── cluster-patch.yaml
│       │   │   └── kustomization.yaml
│       │   ├── flux-system
│       │   │   ├── gotk-components.yaml
│       │   │   ├── gotk-sync.yaml
│       │   │   └── kustomization.yaml
│       │   └── infrastructure.yaml
│       └── checkout
│           ├── app3
│           │   ├── cluster-patch.yaml
│           │   └── kustomization.yaml
│           ├── flux-system
│           │   ├── gotk-components.yaml
│           │   ├── gotk-sync.yaml
│           │   └── kustomization.yaml
│           └── infrastructure.yaml
├── infrastructure
│   ├── kyverno
│   │   ├── kustomization.yaml
│   │   ├── source.yaml
│   │   └── sync.yaml
│   └── kyverno-policies
│       ├── kustomization.yaml
│       ├── verify-flux-images.yaml
│       └── verify-git-repositories.yaml

```

A [tenant repository](https://github.com/chenditc/multi-cluster-flux-example/tree/dev-team) contains the following top directories:

- **base** dir contains `HelmRepository` and `HelmRelease` manifests
- **staging** dir contains `HelmRelease` Kustomize patches for deploying pre-releases on the staging cluster
- **production** dir contains `HelmRelease` Kustomize patches for deploying stable releases on the production cluster

```
├── base
│   ├── kustomization.yaml
│   ├── podinfo-release.yaml
│   └── podinfo-repository.yaml
├── production
│   ├── kustomization.yaml
│   └── podinfo-values.yaml
└── staging
    ├── kustomization.yaml
    └── podinfo-values.yaml
```
