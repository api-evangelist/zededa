---
name: Provision a ZEDEDA Kubernetes (ZKS) cluster
description: Create and manage a managed Kubernetes cluster instance on ZEDEDA edge nodes.
api: openapi/zededa-kubernetes_service-openapi.json
operations:
  - ZKSClusterInstances_CreateZKSInstance
  - ZKSClusterInstances_ListZKSInstances
  - ZKSClusterInstances_GetZKSInstance
  - ZKSClusterInstances_UpdateZKSInstance
  - ClusterGroups_GetClusterGroupManifest
---

# Provision a ZEDEDA Kubernetes (ZKS) cluster

Use the ZedCloud Kubernetes Service to stand up a managed Kubernetes (ZKS) cluster across edge nodes. Authenticate with `X-API-KEY`.

## Steps

1. **Create the ZKS instance.** Call `ZKSClusterInstances_CreateZKSInstance` with the cluster spec and target node(s)/project. Capture the cluster `id`.
2. **List / confirm.** Call `ZKSClusterInstances_ListZKSInstances` and `ZKSClusterInstances_GetZKSInstance` to confirm it registered.
3. **Retrieve the manifest.** Call `ClusterGroups_GetClusterGroupManifest` to obtain the kubeconfig/manifest needed to reach the cluster.
4. **Update as needed.** Use `ZKSClusterInstances_UpdateZKSInstance` (or `BulkUpdateZKSInstances`) to change the cluster; GitOps and Helm are managed through the KubernetesGitOps / HelmChartManagement operations in the same service.

## Rules

- Clusters are project-scoped and bind to edge nodes (`clusterId`).
- Optimistic concurrency: handle `VersionMismatch` / `412` on updates.
- Paginate list operations via `next.*`.
- Errors carry a `ZsrvError` code alongside the HTTP status.
