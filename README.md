# 2A/ArgoCD-based gitops repo proposal

This proposal covers gitops automation for creation/testing/running of 2A's ManagedCluster, their cluster addons (infra apps such as Ingress controllers and/or service mesh, logging & monitoring software etc.) and, finally, user's workload applications. The structure is flexible and can be easily adjusted to already existing ArgoCD infrastructure.

## Repo structure
```
/
├── clusters            # 2A-managed clusters manifests (infra provider and env specific)
│   ├── aws
│   │   ├── dev
│   │   └── stage
│   └── azure
│       
├ ─ (templates)         # (Optional: 2A-managed ClusterTemplates and ServiceTemplates)
│       
├── credentials         # platform credentials (encrypted)
│   ├── aws
│   │   ├── dev
│   │   └── stage
│   └── azure
│       
├── addons              # cluster's infra applications
│   ├── cert-manager
│   │   ├── dev
│   │   └── stage
│   ...    
│   ├── prometheus
│   └── sealed-secrets
│       
├── apps                # user's workload applications
│   └── kuard
│       ├── dev
│       └── stage
│       
└── argocd              # ArgoCD-related configurations (Applications etc.)
```

1. **templates** folder is optional. It might be added for teams having ClusterTemplates and ServiceTemplates editing needs.
1. **addons** - here already existing user's argocd applications might be mixed (or gradually replaced) with [2A's beach-head services](https://mirantis.github.io/project-2a-docs/glossary/#beach-head-services).
1. **credentials** are encrypted using [sealed-secretes](https://github.com/bitnami-labs/sealed-secrets) for basic use case. For large-scale use case more sophisticated secret management sysystem (such as Hashicorp Vault) should be considered.
1. **apps** contains user's workload application configurations. For the sake of simplicity it contains [kuard demo app](https://github.com/kubernetes-up-and-running/kuard).
