---
name: Onboard and activate a ZEDEDA edge node
description: Register a new EVE-OS edge node in a ZedCloud project and activate it for workload deployment.
api: openapi/zededa-node_service-openapi.json
operations:
  - ResourceGroup_CreateResourceGroup
  - EdgeNodeConfiguration_CreateEdgeNode
  - EdgeNodeConfiguration_GetEdgeNodeOnboarding
  - EdgeNodeConfiguration_ActivateEdgeNode
  - EdgeNodeStatus_GetEdgeNodeStatus
---

# Onboard and activate a ZEDEDA edge node

Use the ZedCloud API (`https://zedcontrol.zededa.net/api`) to bring a new EVE-OS device under management. Authenticate every call with the `X-API-KEY` header (or an `Authorization` bearer token).

## Steps

1. **Create or choose a project.** Call `ResourceGroup_CreateResourceGroup` to make a project (resource group) that will scope the node, or query existing projects first. Capture the returned project `id`.
2. **Register the edge node.** Call `EdgeNodeConfiguration_CreateEdgeNode` with the node's serial/model and the project id. Capture the device `id`.
3. **Retrieve onboarding details.** Call `EdgeNodeConfiguration_GetEdgeNodeOnboarding` to get the onboarding/enrollment information used when flashing EVE-OS onto the hardware.
4. **Activate the node.** Once the device has checked in, call `EdgeNodeConfiguration_ActivateEdgeNode` to move it into the active admin state.
5. **Confirm health.** Poll `EdgeNodeStatus_GetEdgeNodeStatus` until the run state is running.

## Rules

- Resources are project-scoped; always pass the correct project id.
- Updates use optimistic concurrency — a `VersionMismatch` code or `412 Precondition Failed` means re-fetch and retry.
- List/query endpoints paginate via `next.pageSize` / `next.pageToken`; follow the `next` field.
- Errors return `google.rpc.Status`; inspect the granular `ZsrvError` code (see errors/zededa-error-codes.yml).
- Send an `X-Request-Id` header to correlate calls in support tickets.
