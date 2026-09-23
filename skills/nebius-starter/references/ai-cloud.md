# Nebius AI Cloud

Read when the build needs cloud compute, custom model execution, storage, networking, or hosting.

## Official entry points

- [Documentation index and agent instructions](https://docs.nebius.com/llms.txt)
- [Operational Cloud MCP setup by client](https://github.com/nebius/mcp-server/blob/main/AGENT_SETUP.md)
- [Nebius infrastructure skill](https://github.com/nebius/nebius-ps-services/tree/main/skills/nebius)
- [Skills catalog and installation instructions](https://github.com/nebius/nebius-ps-services/blob/main/skills/README.md)
- [Account sign-up](https://docs.nebius.com/signup-billing/sign-up.md)
- [Promo-code redemption](https://docs.nebius.com/signup-billing/payments/promo-codes.md)
- [Serverless AI overview](https://docs.nebius.com/serverless/overview.md)
- [First VM](https://docs.nebius.com/compute/quickstart.md)

Fetch Markdown documentation as described in the index. Read the selected service's quickstart, authentication, pricing, and lifecycle guidance before configuring it.

## Choose the execution path

Match the service to the user's workload. Serverless AI offers interactive Devlabs, finite jobs, and request-serving endpoints for containerized workloads. A VM fits workloads that need direct machine control. Select the minimum hardware that meets the workload's requirements; check current GPU memory needs and available capacity. Use a cluster only when the build requires one.

The operational MCP and the CLI help the agent manage the account. Install the operational MCP when that is the chosen control path, following its active-client section and prerequisites. Preserve its documented safe-mode setting. A documentation MCP cannot verify authentication or provision a resource. Runtime SDKs are separate and only needed if the application itself calls Cloud APIs.

## Skills and account checks

Read the infrastructure skill and its matching service references. Inspect the catalog's current installation and dependency requirements. Select only the needed skill and required support; do not default to installing the full catalog. If a selected workflow requires companion skills or hooks, inspect and explain that requirement before adding them.

Check existing CLI profiles and use a working one where authorized. Verify the selected project and region through a scoped read operation, keeping credentials and full account dumps out of output. Check access, quota, and capacity separately; available quota does not prove a GPU is available.

## Verify and clean up

Use a small workload that proves the user's goal, not only an inventory command or a GPU listing. Inspect its output and save anything needed for the demo.

Record demo-owned resources and consult their current stop/delete and billing rules. Stopping compute can leave billable disks, stored objects, or other resources. Serverless deployment types have different storage lifecycles; export needed outputs before stopping or deleting a deployment. Give exact cleanup steps for what was actually created.
