---
name: Deploy an edge application to a node
description: Define an edge application bundle and deploy an activated instance of it onto a ZEDEDA edge node.
api: openapi/zededa-app_service-openapi.json
operations:
  - EdgeApplicationConfiguration_CreateEdgeApplicationBundle
  - EdgeApplicationInstanceConfiguration_CreateEdgeApplicationInstance
  - EdgeApplicationInstanceConfiguration_ActivateEdgeApplicationInstance
  - EdgeApplicationInstanceConfiguration_GetEdgeApplicationInstance
  - EdgeApplicationInstanceStatus_GetEdgeApplicationInstanceStatus
---

# Deploy an edge application to a node

Use the ZedCloud Edge Application Service to publish an app and run it on a managed edge node. Authenticate with `X-API-KEY`.

## Steps

1. **Create the application bundle.** Call `EdgeApplicationConfiguration_CreateEdgeApplicationBundle` with the app manifest (image reference, resources, interfaces). Capture the app `id`.
2. **Create an app instance.** Call `EdgeApplicationInstanceConfiguration_CreateEdgeApplicationInstance`, binding the bundle `id` to a target edge node (`deviceId`) and project. Capture the instance `id`.
3. **Activate the instance.** Call `EdgeApplicationInstanceConfiguration_ActivateEdgeApplicationInstance` to start it. Use `DeActivateEdgeApplicationInstance` to stop.
4. **Verify.** Call `EdgeApplicationInstanceConfiguration_GetEdgeApplicationInstance` and `EdgeApplicationInstanceStatus_GetEdgeApplicationInstanceStatus` to confirm the run state.

## Rules

- The bundle references an image in a datastore (see data-model/zededa-data-model.yml); create the image/datastore first if needed.
- Optimistic concurrency: handle `VersionMismatch` / `412` by re-fetching before retry.
- Follow `next.*` pagination on list operations.
- Correlate with `X-Request-Id`; decode failures via the `ZsrvError` registry.
