# GTC Berlin 2026 hackathon: participant guide

Use this guide to join your team's project, choose the services you need, and get a first working result. You can follow it yourself or use a coding agent.

## 1. Choose what you need

| What you want to do | Start here | Documentation |
| --- | --- | --- |
| Call an available hosted language or vision model | **Token Factory**: use its model API. You do not deploy that model in your team's Cloud project. | [Token Factory quickstart](https://docs.tokenfactory.nebius.com/quickstart) |
| Deploy your own model or serve an application | **AI Cloud → Serverless AI → Endpoints**. Choose CPU or GPU resources for your workload. | [Endpoints quickstart](https://docs.nebius.com/serverless/quickstart/endpoints) |
| Work in a browser notebook or development environment | **AI Cloud → Serverless AI → Devlabs**. Choose CPU-only development when your team's GPU is already serving a model. | [Devlabs quickstart](https://docs.nebius.com/serverless/quickstart/devlabs) |
| Run a finite task, such as processing data or evaluating a model | **AI Cloud → Serverless AI → Jobs**. | [Jobs quickstart](https://docs.nebius.com/serverless/quickstart/jobs) |
| Need direct control of a machine | **AI Cloud → Compute**: create a VM within your team's limits. | [Create a VM](https://docs.nebius.com/compute/virtual-machines/create) |
| Search the web or extract content from websites | **Tavily**. | [Tavily quickstart](https://docs.tavily.com/documentation/quickstart) |
| Save datasets, results, or checkpoints outside a workload's local disk | **AI Cloud → Object Storage**. | [Object Storage quickstart](https://docs.nebius.com/object-storage/quickstart) |

Token Factory and AI Cloud Serverless AI are different services, even though both can expose model endpoints. Calling a public Token Factory model uses your Token Factory account and credits. Deploying your own GPU endpoint in AI Cloud uses your team's reserved GPU and project resources.

For example, an application could call a hosted model through Token Factory, search with Tavily, and run its own embedding model on the team's GPU. Use only the services your idea needs. The shared guide does not require a particular model, database, or application architecture.

Service background: [Serverless AI](https://docs.nebius.com/serverless/overview), [Token Factory quickstart](https://docs.tokenfactory.nebius.com/quickstart), and [Tavily quickstart](https://docs.tavily.com/documentation/quickstart).

## 2. Join your assigned AI Cloud project

Each team is assigned a project in the organizer-managed tenant, with a name such as `group01-project`. Resources and limits are shared by everyone on the team.

1. Give the organizers the email addresses of your team members and obtain your assigned project name.
2. Open the Nebius invitation sent to your email and complete the sign-in and invitation-acceptance steps.
3. Open the [AI Cloud console](https://console.nebius.com/) and select the hackathon tenant and your assigned project.
4. Confirm the project region is **`uk-south2`**. Copy your project's ID from its details if you will use the CLI or an agent.
5. Confirm that you can view your project and its SecretStash entry described below.

The organizers assign team members to the corresponding `groupNN-project-editors` group. This grants project editor access, with an additional permission to read that project's credential secret. Ask an organizer if the project or secret is missing.

**Use your assigned project for the provided GPU capacity.** Creating a personal tenant or selecting a different region does not give you access to that reservation. If a Cloud sign-up screen asks you to activate personal billing, first check that you followed the event invitation and selected the intended tenant.

## 3. Know your team's limits

These limits apply to your team's project and are shared by all team members and workloads.

| Resource | Team limit |
| --- | --- |
| Reserved GPU | **1 NVIDIA RTX PRO 6000 Blackwell Server Edition** |
| GPU platform / preset | `gpu-rtx6000-a` / `1gpu-24vcpu-218gb` |
| VM count | **2 total**, including VMs managed by Serverless workloads |
| CPU capacity for non-GPU VMs | **4 vCPUs total**; this is separate from the vCPUs in the GPU preset |
| Disks | **4 total** |
| Network SSD disk capacity | **500 GiB total**, including workload container and boot disks |
| Public IPv4 addresses | **1** |
| Container registries | **1 registry**; this is not a one-image limit |
| Object Storage buckets | **1** |
| Object Storage bucket size | Set **200 GiB** when creating your Standard bucket; separate from the 500 GiB disk allowance |

### Plan around one GPU

A GPU endpoint and a GPU DevLab are separate workloads and each requests a GPU. They cannot run simultaneously within this project's one-GPU allowance.

If you want to develop while a GPU endpoint is running, choose a **CPU-only DevLab or CPU VM**, keeping its allocation within the 4-vCPU limit and the two-VM total. Select the CPU option explicitly: a default template or CLI configuration may otherwise request a GPU. The exact CPU preset and template should be checked for availability before creation.

Stopping one GPU workload and starting another lets you reuse the allowance. Wait for capacity to be released before starting the replacement. Ask your teammates what is running before creating a new workload.

The 500 GiB disk allowance is also shared: two 250 GiB disks consume the whole allowance. A stopped DevLab retains its disk. Keep room for the other workload and save important outputs outside temporary container storage.

## 4. Start a Cloud workload

You can use the console without installing agent tools.

1. In your assigned project, open **Serverless AI** and choose **Devlabs**, **Jobs**, or **Endpoints**.
2. Choose a suitable template or your container image. Use your challenge cookbook for model-specific images, commands, ports, and memory requirements.
3. For GPU workloads, select the event's RTX platform and one-GPU preset above. For CPU-only development, select **Without GPUs** and a preset within the CPU allowance.
4. Use the project's existing subnet in `uk-south2`. Check remaining GPU, VM, and disk capacity before creating the workload.
5. Configure the application's port and authentication. Protect development UIs and endpoints with the supported token authentication; store credentials as secrets.
6. Create the workload and inspect its status and logs. For an endpoint, wait until it is running and send a small real request. For a DevLab, open its UI and run a small notebook cell. For a job, inspect its exit status and output.

Endpoints and DevLabs have managed HTTPS addresses that do not require a separate public IP. Allocate a public IP only when your access method needs it, such as a direct connection to a VM. See [endpoint management](https://docs.nebius.com/serverless/endpoints/manage) and [DevLab management](https://docs.nebius.com/serverless/devlabs/manage).

For direct machine control, follow the [VM quickstart](https://docs.nebius.com/compute/quickstart), using the same assigned project, region, and limits.

### Optional: use the CLI

Install the current [Nebius CLI](https://docs.nebius.com/cli/install). Use a separate event profile and complete its browser sign-in using the identity that accepted your invitation:

```bash
nebius profile create hackathon
```

Choose your assigned tenant and project during setup. If the profile already exists, reuse it. Replace the placeholder below with the project ID shown in your console:

```bash
export NEBIUS_PROJECT_ID='YOUR_ASSIGNED_PROJECT_ID'
nebius --profile hackathon iam v2 project get --id "$NEBIUS_PROJECT_ID"
nebius --profile hackathon quotas quota-allowance list \
  --parent-id "$NEBIUS_PROJECT_ID" --all
```

Verify the returned project name before provisioning. Use `--profile hackathon` and the explicit project ID for project-scoped creation commands. If `nebius ai devlab --help` is unavailable, update the CLI using the official installation guidance or use the console.

The CLI login manages Cloud resources. It is separate from both the Object Storage access key and the Token Factory API key.

## 5. Retrieve Object Storage credentials

Each participant project has a SecretStash entry named:

```text
hackathon-object-storage
```

Open **SecretStash** in your assigned project, select this secret, and view its current payload. The project's editor group has permission to read it. It contains:

| Field | Purpose |
| --- | --- |
| `AWS_ACCESS_KEY_ID` | Access-key ID for the project's `groupNN-sa` service account |
| `AWS_SECRET_ACCESS_KEY` | Matching secret access key |
| `AWS_DEFAULT_REGION` | `uk-south2` |
| `AWS_ENDPOINT_URL` | `https://storage.uk-south2.nebius.cloud` |
| `ACCESS_KEY_EXPIRES_AT` | The exact expiry timestamp for your key |

Load the values into your application's protected runtime configuration or a local credential store. Use secret references where supported. Keep credentials out of chat, notebooks, source control, screenshots, browser-side code, and bucket contents. See [SecretStash](https://docs.nebius.com/mysterybox/overview); its CLI service name is still `mysterybox`.

### Create and use your team's bucket

Your project's editor role allows you to create a bucket. Each team shares one **Standard** bucket. Set its size limit to **200 GiB** when creating it. If a teammate has already created it, use that bucket.

To create it in the [Nebius console](https://console.nebius.com/):

1. Select your assigned project in **uk-south2**.
2. Open **Storage → Object Storage**, then select **Create resource → Bucket**.
3. Choose a name unique across the region, such as `hackathon-your-team-name-random-suffix`. Use 3–63 lowercase letters, numbers, dots, or hyphens.
4. Set **Storage class** to **Standard**, enable **Force storage class**, and set **Bucket size** to **200 GiB**. Leave versioning disabled unless your application needs it.
5. Click **Create bucket** and copy its name into your application's configuration.

See the [official bucket-creation instructions](https://docs.nebius.com/object-storage/buckets/manage#how-to-create-buckets) for console and CLI options. Configure your S3-compatible client with the credentials, region, and endpoint from SecretStash above.

For a first check, upload a small file under a unique test name, download it, compare its contents, then delete that test object. See the [Object Storage guide](https://docs.nebius.com/object-storage/overview). Save datasets, checkpoints, and results here when they must outlive a workload's local disk.

## 6. Set up Token Factory when needed

Use Token Factory's public model endpoints to call a hosted model from your application.

1. Create or reuse your account at [Token Factory](https://tokenfactory.nebius.com/).
2. Follow the organizers' instructions to obtain and redeem your individual event promo code in Token Factory, then check your credited balance.
3. Create a Token Factory API key and store it securely in your application configuration.
4. Select an available model appropriate to your challenge, then follow the [official quickstart](https://docs.tokenfactory.nebius.com/quickstart) for one small request and inspect the response.

Token Factory uses a separate API key from your Cloud Object Storage credentials. Its current OpenAI-compatible API base URL is `https://api.tokenfactory.nebius.com/v1/`; the quickstart uses `NEBIUS_API_KEY` for its application key.

Check Token Factory's available models against your challenge requirements, including any embedding model you need. If a required model is unavailable, consult the challenge cookbook about deploying it on your team's GPU. Check the challenge requirements before substituting another model.

## 7. Set up Tavily when needed

Tavily provides web search and extraction; it does not replace either your model API or Cloud compute.

1. Create or reuse an account at the [Tavily platform](https://app.tavily.com/).
2. If the organizers provide a Tavily promo code, follow their redemption instructions and check your credited balance.
3. Obtain a Tavily API key and store it securely in your application configuration.
4. Follow the [Tavily quickstart](https://docs.tavily.com/documentation/quickstart) to make a small search request. Open the returned links and check that they are relevant before using them as sources.

AI Cloud, Token Factory, and Tavily have separate access, credentials, and credits, even when you sign in with the same Google or GitHub identity. Credit redemption in one product does not fund another.

## 8. Work with a coding agent

The [Nebius Starter skill](../../skills/nebius-starter/SKILL.md) helps an agent choose and configure services. It is a general-purpose skill, so supply this event guide alongside it. Installing a skill does not grant access to your assigned project.

Open this repository with your coding agent and use this starting prompt, replacing the placeholders:

> Read `docs/hackathon/README.md`, then `skills/nebius-starter/SKILL.md` and the product references relevant to my task. I am participating in the GTC Berlin hackathon. My application is in `<application directory>` and I want to build `<goal>`. My assigned Cloud project is `<project name and ID>` in `uk-south2`. Use the event-specific invitation, reservation, limits, and SecretStash credentials described in the guide. Reuse my existing login and work only in my assigned project. Verify available capacity before provisioning; keep within the shared one-GPU, two-VM, four non-GPU-vCPU and storage limits. Use my assigned Nebius tenant and project. If you need a project ID, credit redemption instructions, or any information missing from this guide, ask me before proceeding. Ask me to complete personal sign-in steps and enter credentials securely, never in chat. Prove the first useful result and explain what remains running.

The [repository README](../../README.md#get-started) describes installation for supported agents. Give your agent this event guide alongside the installed skill so it uses your assigned project and team limits.

## 9. Save your work and release resources

Before stopping or deleting a workload, save the outputs you need and coordinate with your teammates.

- **Endpoint:** stopping or deleting it removes its container-disk data. Export results first.
- **Job:** container-disk data is removed when it finishes or is deleted. Write required results to persistent storage during the job.
- **DevLab:** the workspace survives stop/restart, but deleting the DevLab deletes its container disk. A stopped DevLab still retains storage usage.
- **VMs and attached storage:** check which disks and IPs will remain after your chosen stop/delete action. Buckets and separately mounted volumes have their own lifecycles.

After cleanup, verify the workload state and remaining usage in your project. Follow the current [Serverless lifecycle guidance](https://docs.nebius.com/serverless/overview) and the organizers' instructions for the final demo and event shutdown.

## 10. If something does not work

| Symptom | First check |
| --- | --- |
| Invitation missing or expired | Contact the organizer with your team name and invited email. |
| Project or secret is not visible | Check the signed-in identity, tenant, project, and team-group membership. |
| GPU allocation or quota error | Check for another GPU workload in your project; verify `uk-south2` and the RTX preset. A quota error can also appear after the reserved allowance is consumed. |
| VM, CPU, or disk limit reached | Count your team's combined workloads, including Serverless VMs and retained DevLab disks. |
| Object Storage authentication fails | Check the endpoint, region, matching key pair, and expiry in SecretStash. |
| Object Storage bucket missing | Select your assigned project and follow [Create and use your team's bucket](#create-and-use-your-teams-bucket). If the one-bucket limit is reached, check whether a teammate already created it. |
| Token Factory request fails | Check its own API key, credits, selected model, and response error. Cloud access does not establish Token Factory access. |

Ask the event organizers for help. Provide your project name, resource ID, timestamp, and request/trace ID when available. Redact tokens and credentials.
