# GTC Berlin 2026 hackathon: participant guide

This README helps your coding agent configure itself and access your team's Nebius Cloud environment. It also contains the service instructions and shared resource limits for the hackathon.

**The guide and the skills work together:** this README explains the hackathon setup and environment for participants and agents. The **Nebius skills** are installed instructions that help your agent operate Cloud resources. You can follow the guide yourself or ask your agent to use it with the skills.

## 1. Set up your coding agent

Download this README, open or attach it in your coding agent, and ask the agent to read it and follow the setup instructions. No separate prompt, application directory, or project idea is required. You do not need to install the Nebius skills beforehand. Once setup is complete, tell the agent what you want to do.

Use an agent with terminal access. If yours already works, keep its current model provider. If you want to connect **OpenCode to Token Factory**, first follow the [OpenCode setup guide](https://dev.nebius.com/cookbook/opencode-nebius-token-factory). Complete model-provider setup before asking the agent to read this file.

### Instructions for the agent

Your task is to prepare yourself to interact with the participant's assigned Nebius Cloud environment. Follow the setup sequence below. Complete the steps your tools and permissions allow; when the participant must act, explain exactly what to do and verify the result afterward. Ask only for missing setup information, such as the assigned project or a required sign-in. Do not ask for an application directory or development goal.

The later sections are reference material for future tasks. When the participant asks you to follow this setup, install or enable the skills and perform read-only access checks. Do not deploy a workload, create a bucket, or execute every example as part of setup.

#### Step 1: Identify the agent environment

Confirm that you can read this complete README. Identify your client, operating system, shell, existing workspace, and whether your tools run on the participant's computer or in a separate environment. Install the skills where this agent will discover them. Reuse a working Nebius skills installation when available.

#### Step 2: Install the Nebius skills

If the Nebius skills are already available in this session, skip installation and verify them in Step 3. Otherwise, use the appropriate method from the [Nebius installation guide](https://github.com/nebius/skills#install). Choose one method. If a command is unsupported, check the current installation guide and client help.

**Claude Code:** run these terminal commands in sequence:

```bash
claude plugin marketplace add nebius/skills
claude plugin install nebius-cloud@nebius
```

If you cannot run them, ask the participant to enter these commands inside Claude Code, one at a time:

```text
/plugin marketplace add nebius/skills
/plugin install nebius-cloud@nebius
```

**Codex:** run:

```bash
codex plugin marketplace add nebius/skills
```

Guide the participant to restart Codex and install or enable **nebius-cloud** in the plugin browser. In a terminal client that supports it, use `/plugins`. Adding a marketplace alone does not install or enable the plugin. See [OpenAI's plugin documentation](https://developers.openai.com/plugins/build/plugins) for client-specific details.

**OpenCode or another Agent Skills-compatible client:** check that Node.js and npm/npx are available, then run:

```bash
npx skills add nebius/skills
```

Select the current agent and the Nebius Cloud skills in the installer. Use the current workspace for project-scoped installation, or the installer's supported user scope when there is no workspace. Do not require an application folder. If interactive selections need the participant, explain which options to choose. Check client compatibility and scope options in the [Agent Skills CLI documentation](https://github.com/vercel-labs/skills).

#### Step 3: Resolve installation problems and verify availability

These instructions cover installing and loading the skills. Delegate Nebius CLI setup and errors to the installed skills in Step 5.

- **Missing agent executable or installer dependency:** consult its official installation instructions, provide steps for the participant's operating system, and recheck availability in the agent's terminal.
- **Unsupported plugin command:** check client version and update guidance. Use the portable installer above only if the client supports it.
- **Permission, network, proxy, certificate, or policy failure:** report the actual error and the action needed. Preserve existing configuration and protections; do not repeatedly retry an unchanged failure.
- **Existing or partial installation:** inspect its state and complete only the missing steps. Avoid duplicate installations and changes to unrelated skills.
- **No terminal access or an interactive step you cannot perform:** explain where to act, the exact command or selection, the expected result, and what non-secret output to return. Wait for that result before continuing with dependent work.
- **Restart required:** summarize completed steps and the next check. Ask the participant to restart or open a new session, provide this same README, and ask the agent to continue setup. Do not close the active session yourself.
- **Skills not discovered:** check installation scope, enabled state, and the client's discovery rules. Verify that the current session can find and read the Nebius skills; a successful download alone is insufficient.
- **Unexpected or unresolved failure:** preserve completed work, explain the redacted error, consult the relevant official documentation, and provide a specific next step. Clearly report any remaining blocker. Do not claim successful installation or silently skip it.

Distinguish installed files from skills that are enabled and usable in this session. If a supported manual instruction-reading fallback is used, disclose it and ensure supporting references are available.

#### Step 4: Read the full environment description

After skill setup, read every remaining section of this README, including project access, limits, SecretStash, Standard buckets, and workload lifecycle. Summarize the environment constraints relevant to future cloud operations. Identify the participant's assigned tenant and project from available context; ask if the assignment is missing or ambiguous. Never infer their assignment solely from the example project name.

#### Step 5: Configure and verify Nebius Cloud access

Use **nebius-cloud-basics** for CLI installation, version requirements, profiles, authentication, scope resolution, and CLI errors. Follow its preflight and agent-identification instructions before CLI calls. It provides the participant's steps for CLI installation, updates, and browser sign-in; verify the result afterward. Check the requirements of the Serverless and Devlabs skills as well, not only the foundation skill.

Use the selected profile consistently. Confirm the authenticated identity and the assigned project and `uk-south2` region with read-only requests. Use **nebius-compute-inventory** for platform discovery and **nebius-capacity-quotas** for project quotas and reservation allowances. Check the project's Serverless listings through the relevant skills, projecting only non-secret metadata and state. A successful empty listing is valid; a failed request is not an empty environment. If a tenant-wide query is denied, report that check separately rather than changing permissions or assuming project access is broken.

Use **nebius-public-docs** when product behavior needs clarification. If a skill's command examples differ from current CLI help or public documentation, resolve the difference using those current sources while preserving the hackathon's assigned region and resource constraints.

Apply this README's environment constraints when following general skill examples. Do not create a personal tenant, generate replacement credentials, change permissions, or switch regions/projects to work around an access failure. The supplied Object Storage credentials remain in **hackathon-object-storage**. Never ask for credentials in chat or expose their values.

For subsequent tasks, use **nebius-serverless-endpoints**, **nebius-serverless-jobs**, **nebius-devlabs**, or the relevant Compute skill. Delegate job/endpoint diagnosis to **nebius-serverless-troubleshooting**, and workload storage or secrets to **nebius-serverless-data-secrets**. Token Factory and Tavily setup is described below and is optional until the participant needs those services.

#### Step 6: Report readiness and wait for the participant's task

Report the skills available, the verified Cloud profile and assigned project, the region, which read-only checks succeeded, and any unresolved setup issues. Distinguish successful setup from workload readiness: resource listings do not prove that a deployment will succeed or that a GPU is currently free. Do not describe setup as complete unless the required project-access checks succeeded. If blocked, explain the precise participant action needed to continue.

Once ready, tell the participant they can now ask you to work with Nebius Cloud. Wait for their request before creating workloads, buckets, or other cloud resources. Apply the environment limits and relevant skills when they provide that request.

Before creating a requested workload, explain how the participant will connect, whether it consumes the project's public-IP allowance, and which authentication mechanism they will use. Follow [How to connect](#how-to-connect): prefer managed HTTPS for endpoints and browser Devlabs, and configure SSH only when requested. Distinguish endpoint bearer tokens, JupyterLab login tokens, optional Devlab route protection, and SSH keys. Verify the selected template's authentication flow rather than applying endpoint settings to a Devlab.

## 2. Choose what you need

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

## 3. Join your assigned AI Cloud project

Each team is assigned a project in the organizer-managed tenant, with a name such as `group01-project`. Resources and limits are shared by everyone on the team.

1. Give the organizers the email addresses of your team members and obtain your assigned project name.
2. Open the Nebius invitation sent to your email and complete the sign-in and invitation-acceptance steps.
3. Open the [AI Cloud console](https://console.nebius.com/) and select the hackathon tenant and your assigned project.
4. Confirm the project region is **`uk-south2`**. Copy your project's ID from its details if you will use the CLI or an agent.
5. Confirm that you can view your project and its SecretStash entry described below.

The organizers assign team members to the corresponding `groupNN-project-editors` group. This grants project editor access, with an additional permission to read that project's credential secret. Ask an organizer if the project or secret is missing.

**Use your assigned project for the provided GPU capacity.** Creating a personal tenant or selecting a different region does not give you access to that reservation. If a Cloud sign-up screen asks you to activate personal billing, first check that you followed the event invitation and selected the intended tenant.

## 4. Know your team's limits

These limits apply to your team's project and are shared by all team members and workloads. The Standard bucket's 200 GiB size is a setting you apply when creating the bucket. Before launching a workload, check current project usage and reservation allowances; the table does not show remaining capacity.

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

## 5. Start a Cloud workload

You can use the console without installing agent tools.

1. In your assigned project, open **Serverless AI** and choose **Devlabs**, **Jobs**, or **Endpoints**.
2. Choose a suitable template or your container image. Use your challenge cookbook for model-specific images, commands, ports, and memory requirements.
3. For GPU workloads, select the event's RTX platform and one-GPU preset above. For CPU-only development, select **Without GPUs** and a preset within the CPU allowance.
4. Use the project's existing subnet in `uk-south2`. Check remaining GPU, VM, and disk capacity before creating the workload.
5. Configure the application's port and the authentication appropriate to the workload. Use endpoint bearer-token authentication for API access, the template's login flow for browser Devlabs, and SSH keys for SSH. Follow [How to connect](#how-to-connect) below and keep credentials secure.
6. Create the workload and inspect its status and logs. For an endpoint, wait until it is running and send a small real request. For a DevLab, open its UI and run a small notebook cell. For a job, inspect its exit status and output.

### How to connect

| What you want to access | Connection | Public IP on the workload? | Authentication |
| --- | --- | --- | --- |
| Serverless endpoint API | Managed HTTPS URL from the endpoint page | **Not needed** | Enable endpoint token authentication and send its token in the `Authorization: Bearer <token>` header. |
| JupyterLab Devlab in your browser | Managed HTTPS URL from the Devlab page | **Not needed** | Enter the **Jupyter token** in JupyterLab's login screen. Use the token configured for that template. |
| Workload through SSH | SSH to its public address, or through an established private-network path | **Needed for direct internet SSH**; a reachable private-network path can avoid it | SSH key; an endpoint or Jupyter token does not replace it. |

Managed HTTPS URLs are reachable from the internet without allocating a public IP to the workload. **No public IP does not mean private access**: configure authentication for the service behind the URL. See [endpoint access](https://docs.nebius.com/serverless/endpoints/manage#how-to-call-an-endpoint) and the [JupyterLab quickstart](https://docs.nebius.com/serverless/quickstart/devlabs).

**Jupyter login and Devlab route protection are separate.** JupyterLab's token signs you into the notebook application. Nebius also documents optional bearer-token protection for a Devlab's web route; that is an additional access layer, not the Jupyter login token or an endpoint token. Follow your template's login instructions and any route authentication you configured. For other Devlab templates or custom images, check their own authentication settings. See [Devlab management](https://docs.nebius.com/serverless/devlabs/manage).

For SSH, check the selected workload's networking requirements before enabling it. The project has one shared public-IP allowance, and SSH configuration can allocate an IP even when browser access does not need one. See [Devlab SSH](https://docs.nebius.com/cli/reference/ai/devlab/ssh) and [endpoint management](https://docs.nebius.com/serverless/endpoints/manage).

For direct machine control, follow the [VM creation guide](https://docs.nebius.com/compute/virtual-machines/create), using the same assigned project, region, and limits.

### Optional: use the CLI

Install the current [Nebius CLI](https://docs.nebius.com/cli/install). The commands below are for you to run in your terminal; your agent can guide you through them. Reuse a correctly configured event profile if one exists. Otherwise, copy your assigned project ID from the console, replace the placeholder, and create the profile:

```bash
export NEBIUS_PROJECT_ID='YOUR_ASSIGNED_PROJECT_ID'
nebius profile create --profile hackathon --parent-id "$NEBIUS_PROJECT_ID"
```

Complete browser sign-in with the identity that accepted your invitation. Check the project and quotas using the same profile:

```bash
nebius --profile hackathon iam v2 project get \
  --id "$NEBIUS_PROJECT_ID" --format json
nebius --profile hackathon quotas quota-allowance list \
  --parent-id "$NEBIUS_PROJECT_ID" --all --format json
```

If reusing a profile, set `NEBIUS_PROJECT_ID` to your assigned project ID before running these checks. Verify the returned project name and region before provisioning. Use `--profile hackathon` and the explicit project ID consistently. Your agent should follow the installed skills' CLI version requirements and setup guidance if a command is unavailable.

The CLI login manages Cloud resources. It is separate from both the Object Storage access key and the Token Factory API key.

## 6. Retrieve Object Storage credentials

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

## 7. Set up Token Factory when needed

Use Token Factory's public model endpoints to call a hosted model from your application.

1. Create or reuse your account at [Token Factory](https://tokenfactory.nebius.com/).
2. Follow the organizers' instructions to obtain and redeem your individual event promo code in Token Factory, then check your credited balance.
3. Create a Token Factory API key and store it securely in your application configuration.
4. Select an available model appropriate to your challenge, then follow the [official quickstart](https://docs.tokenfactory.nebius.com/quickstart) for one small request and inspect the response.

Token Factory uses a separate API key from your Cloud Object Storage credentials. Its current OpenAI-compatible API base URL is `https://api.tokenfactory.nebius.com/v1/`; the quickstart uses `NEBIUS_API_KEY` for its application key.

Check Token Factory's available models against your challenge requirements, including any embedding model you need. If a required model is unavailable, consult the challenge cookbook about deploying it on your team's GPU. Check the challenge requirements before substituting another model.

## 8. Set up Tavily when needed

Tavily provides web search and extraction; it does not replace either your model API or Cloud compute.

1. Create or reuse an account at the [Tavily platform](https://app.tavily.com/).
2. If the organizers provide a Tavily promo code, follow their redemption instructions and check your credited balance.
3. Obtain a Tavily API key and store it securely in your application configuration.
4. Follow the [Tavily quickstart](https://docs.tavily.com/documentation/quickstart) to make a small search request. Open the returned links and check that they are relevant before using them as sources.

AI Cloud, Token Factory, and Tavily have separate access, credentials, and credits, even when you sign in with the same Google or GitHub identity. Credit redemption in one product does not fund another.

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
