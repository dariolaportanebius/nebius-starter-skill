# GTC Berlin 2026 hackathon: participant guide

Use this guide to access your team's Nebius environment, choose services, and run your workloads. You can complete setup and use the Cloud console yourself. A coding agent is optional.

## 1. Start here

For setup in your browser, have your hackathon invitation email ready and follow this path:

1. [Join your assigned AI Cloud project](#2-join-your-assigned-ai-cloud-project). Accept the invitation, select your team's project in **uk-south2**, and check that you can see its SecretStash entry.
2. [Choose the services you need](#3-choose-what-you-need) and read [your team's limits](#4-know-your-teams-limits). Coordinate GPU and storage use with your teammates.
3. If your application needs persistent files, [retrieve the Object Storage credentials and create or reuse your team's bucket](#6-retrieve-object-storage-credentials).
4. If you need hosted models or web search, configure [Token Factory](#7-set-up-token-factory-when-needed) or [Tavily](#8-set-up-tavily-when-needed). These services have their own accounts and API keys.
5. When you are ready to run something, follow [Start a Cloud workload](#5-start-a-cloud-workload). The steps use the console; [command-line setup](#optional-use-the-cli) is available if you prefer a terminal.

Your basic Cloud access is ready when you can open the assigned project in the correct region, view its workload pages, and access the team's SecretStash entry. An empty workload list is normal for a new project. Create only the resources your team needs; setup alone does not require a running GPU workload.

**Using a coding agent?** Download or attach this complete README and ask the agent to read it and follow [Optional: set up a coding agent](#11-optional-set-up-a-coding-agent). That section contains its installation and access-check instructions. You can add an agent later; it should reuse any setup you have already completed.

## 2. Join your assigned AI Cloud project

Each team is assigned a project in the organizer-managed tenant, with a name such as `group01-project`. Resources and limits are shared by everyone on the team.

You should have received an invitation from Nebius on the morning of the hackathon, at the email address you registered with. The organizers already have your email address. Look for a message like this:

> **Hello,**
>
> You were invited to join the **`<HACKATHON_TENANT_NAME>`** Nebius tenant. Use the button below to set up your account and get started!
>
> **Accept the invitation**
>
> If you have any questions, email Nebius [support](mailto:support@nebius.com) — we'll be glad to help. Great to have you with us!

`<HACKATHON_TENANT_NAME>` is a placeholder; your email contains the actual tenant name. Use the button in your own invitation email.

1. **Open your invitation.** Check your inbox and spam folder, then click **Accept the invitation** in the email. If the email is missing or the link has expired, ask an organizer to resend it.
2. **Sign in to Nebius.** Follow the sign-in screen opened by the invitation. If you already have a Nebius account, use your usual sign-in method. Otherwise, choose one of the sign-in options offered and follow the account setup prompts. If asked for contact details, enter your name and the email address that received the invitation, complete any requested verification, and review the terms before accepting them. If the browser is signed in to a different account, switch to the account you intend to use for the hackathon before continuing.
3. **Complete invitation acceptance.** Check that the tenant name matches the invitation email and confirm joining it if prompted. Continue until you reach the console. If signing in leaves you outside the invitation flow, reopen the button in your email while signed in to the same account.
4. **Select your team's project.** In the [AI Cloud console](https://console.nebius.com/), select the tenant named in your invitation, then your assigned project, such as `group01-project`. Ask an organizer if you do not know your team's project name.
5. **Check the region and project ID.** Confirm the project region is **`uk-south2`**. Copy your project's ID from its details if you will use the CLI or an agent.
6. **Check access to your team's secret.** Open SecretStash in that project and confirm that you can see `hackathon-object-storage`, described in [Object Storage credentials](#6-retrieve-object-storage-credentials).

For help signing in, see [How to authenticate in Nebius AI Cloud](https://docs.nebius.com/iam/log-in).

The organizers assign team members to the corresponding `groupNN-project-editors` group. This grants project editor access, with an additional permission to read that project's credential secret. Ask an organizer if the project or secret is missing.

**Use your assigned project for the provided GPU capacity.** Creating a personal tenant or selecting a different region does not give you access to that reservation. If a Cloud sign-up screen asks you to activate personal billing, first check that you followed the event invitation and selected the intended tenant.

## 3. Choose what you need

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

## 4. Know your team's limits

These limits apply to your team's project and are shared by all team members and workloads. The Standard bucket's 200 GiB size is a setting you apply when creating the bucket. Before launching a workload, check current project usage and reservation allowances; the table does not show remaining capacity.

| Resource | Team limit |
| --- | --- |
| Reserved GPU | **1 NVIDIA RTX PRO 6000 Blackwell Server Edition** |
| GPU platform / preset | `gpu-rtx6000-a` / `1gpu-24vcpu-218gb` |
| VM count | **3 total**, including VMs managed by Serverless workloads |
| CPU capacity for non-GPU VMs | **20 vCPUs total**; this is separate from the vCPUs in the GPU preset |
| Disks | **4 total** |
| Network SSD disk capacity | **500 GiB total**, including workload container and boot disks |
| Public IPv4 addresses | **1** |
| Container registries | **1 registry**; this is not a one-image limit |
| Object Storage buckets | **1** |
| Object Storage bucket size | Set **200 GiB** when creating your Standard bucket; separate from the 500 GiB disk allowance |

### Plan around one GPU

A GPU endpoint and a GPU DevLab are separate workloads and each requests a GPU. They cannot run simultaneously within this project's one-GPU allowance.

If you want to develop while a GPU endpoint is running, choose a **CPU-only DevLab or CPU VM**. Your CPU-only workloads share a **20-vCPU total**, and all workloads together must stay within the **three-VM total**, including the GPU endpoint's VM. Select the CPU option explicitly: a default template or CLI configuration may otherwise request a GPU. The exact CPU preset and template should be checked for availability before creation.

Stopping one GPU workload and starting another lets you reuse the allowance. Wait for capacity to be released before starting the replacement. Ask your teammates what is running before creating a new workload.

The 500 GiB disk allowance is also shared: two 250 GiB disks consume the whole allowance. A stopped DevLab retains its disk. Keep room for the other workload and save important outputs outside temporary container storage.

## 5. Start a Cloud workload

Follow these steps in the [Cloud console](https://console.nebius.com/) after joining your project and checking your team's limits. Choose the workload type from section 3; you only need to create the one your task requires.

1. In your assigned project, open **Serverless AI** and choose **Devlabs**, **Jobs**, or **Endpoints**.
2. Choose a suitable template or your container image. Use your challenge cookbook for model-specific images, commands, ports, and memory requirements.
3. For GPU workloads, select the event's RTX platform and one-GPU preset above. For CPU-only development, select **Without GPUs** and a preset within the CPU allowance.
4. Use the project's existing subnet in `uk-south2`. Check remaining GPU, VM, and disk capacity before creating the workload.
5. Configure the application's port and the authentication appropriate to the workload. Use endpoint bearer-token authentication for API access, the template's login flow for browser Devlabs, and SSH keys for SSH. Follow [How to connect](#how-to-connect) below and keep credentials secure.
6. Create the workload and inspect its status and logs. For an endpoint, wait until it is running and send a small real request. For a DevLab, open its UI and run a small notebook cell. For a job, inspect its exit status and output.

### Allow time for startup and model loading

The first start can take several minutes: the platform must provision resources, pull the container image, and your application may download model weights and load them onto the GPU. Large images and models take longer. A running resource does not necessarily mean the model is ready to answer requests.

If an endpoint returns **503** during startup, check its status and logs for download or model-loading progress. Retry a small request after a short wait while progress continues. If logs show a crash, missing configuration, an incorrect port, or no continuing progress, investigate that error; do not keep waiting or create duplicate endpoints. Verify a real inference request before relying on the endpoint for your demo.

**Plan for another cold start after stopping an endpoint.** Its container disk is removed on stop or deletion. Weights cached only there must be downloaded again on the next start or replacement deployment. Store durable model files and results in your team's bucket and configure the application to use them; a bucket does not automatically become the model's cache. Loading weights into GPU memory still takes time. A Devlab's workspace, by contrast, survives stop/restart; place caches under its configured workspace path if you want to retain them. See [storage lifecycle](https://docs.nebius.com/serverless/overview#devlabs-endpoints-and-jobs).

### How to connect

| What you want to access | Connection | Public IP on the workload? | Authentication |
| --- | --- | --- | --- |
| Serverless endpoint API | Managed HTTPS URL from the endpoint page | **Not needed** | Enable **Bearer token authentication** and create/select its token secret. Send that token in the `Authorization: Bearer <token>` header. |
| JupyterLab Devlab in your browser | Managed HTTPS URL from the Devlab page | **Not needed** | Leave the platform **Bearer token authentication** option off. Keep Jupyter authentication enabled and enter the template's **Jupyter token** in its login screen. |
| Workload through SSH | SSH to its public address, or through an established private-network path | **Needed for direct internet SSH**; a reachable private-network path can avoid it | SSH key; an endpoint or Jupyter token does not replace it. |

**For endpoint APIs:** use the managed HTTPS URL with bearer authentication enabled. The URL is internet-accessible even without a VM public IP. Use a dedicated endpoint token, not the Object Storage access key. Keep the token private and supply it from your API client; opening the URL in a normal browser tab does not add an Authorization header.

**For JupyterLab:** open the managed HTTPS URL and enter the Jupyter token generated or configured for the template. For this hackathon, leave the platform's separate bearer-token option off for browser Devlabs. A normal browser navigation does not supply that header. Keep the application's own login enabled. For other browser templates, verify their login protection before exposing the UI; ask an organizer if the template has no login. See the [JupyterLab quickstart](https://docs.nebius.com/serverless/quickstart/devlabs).

**Endpoint authentication and application authentication are separate.** The endpoint token protects the managed URL at the platform gateway. Configuring it does not configure a token environment variable inside your container. If your application also requires a token, supply the variable or setting that application expects. In the console, use **Secret environment variables**; in the CLI, use `--env-secret APP_TOKEN=SECRET_SELECTOR`, replacing `APP_TOKEN` with the application's actual variable name and selecting a secret with that payload key. Use `--env` for non-secret settings. See [endpoint settings](https://docs.nebius.com/serverless/endpoints/manage).

Check how the application expects clients to authenticate. If both layers expect the same bearer header, their configured token values must agree; do not assume that the platform supplies an application credential. A **401** can come from either authentication layer. A **503** can indicate a backend that is still starting or has failed. Use application logs and the response to identify the cause instead of disabling authentication.

For SSH, check the selected workload's networking requirements before enabling it. The project has one shared public-IP allowance, and SSH configuration can allocate an IP even when browser access does not need one. See [Devlab SSH](https://docs.nebius.com/cli/reference/ai/devlab/ssh) and [endpoint management](https://docs.nebius.com/serverless/endpoints/manage).

For direct machine control, follow the [VM creation guide](https://docs.nebius.com/compute/virtual-machines/create), using the same assigned project, region, and limits.

### Optional: use the CLI

The CLI lets you manage Cloud resources from a terminal. It is optional for console users and does not require a coding agent. These examples use Bash or Zsh.

**Install and check the CLI.** On macOS or Ubuntu, follow the [official installation instructions](https://docs.nebius.com/cli/install). The documented installer command is:

```bash
curl -sSL https://artifacts.nebius.cloud/cli/install.sh | bash
```

Open a new terminal after installation, then check that the CLI responds:

```bash
nebius version
nebius profile list
```

If the CLI is already installed, update to the latest release and check the version again:

```bash
nebius update
nebius version
```

Use [CLI update help](https://docs.nebius.com/cli/reference/update) if updating fails. If your operating system is not covered by the installation guide, you can continue using the web console.

**Connect to your assigned project.** Copy your assigned project ID from the console, replace the placeholder, and run:

```bash
export NEBIUS_PROJECT_ID='YOUR_ASSIGNED_PROJECT_ID'
```

A profile saves the account and project used by the CLI. Reuse a correctly configured event profile if one exists. The examples below use the profile name `hackathon`; substitute your existing profile name throughout if it differs. If you need a new profile, create it:

```bash
nebius profile create hackathon --parent-id "$NEBIUS_PROJECT_ID"
```

Here, `hackathon` is the new profile name, a positional argument to [profile create](https://docs.nebius.com/cli/reference/profile/create). The later `--profile hackathon` option selects that saved profile for other commands.

Complete browser sign-in with the identity that accepted your invitation. Wait for the login-success message, then return to the terminal and confirm that it reports the profile as configured. See [CLI profile setup](https://docs.nebius.com/cli/configure) if sign-in does not complete.

**Verify access before creating resources.** Check the project and quotas using the same profile:

```bash
nebius --profile hackathon iam v2 project get \
  --id "$NEBIUS_PROJECT_ID" --format json
nebius --profile hackathon quotas quota-allowance list \
  --parent-id "$NEBIUS_PROJECT_ID" --all --format json
```

Verify the returned project name and region before provisioning. Use `--profile hackathon` and the explicit project ID consistently. In a new terminal, set `NEBIUS_PROJECT_ID` again before running commands that use it. If a command is unavailable, check your CLI version and that command's `--help`, then consult the [CLI documentation](https://docs.nebius.com/cli/install). If access is denied, check the profile and project against the console and ask an organizer if they are correct. Do not create a different tenant to work around the error.

The CLI login manages Cloud resources. It is separate from both the Object Storage access key and the Token Factory API key.

## 6. Retrieve Object Storage credentials

Each participant project has a SecretStash entry named:

```text
hackathon-object-storage
```

To retrieve your team's credentials in the console:

1. Select your assigned tenant and project, then open **SecretStash**.
2. Select **hackathon-object-storage** and open its current (primary) version to view the payload—the stored credential fields.
3. Check `ACCESS_KEY_EXPIRES_AT`. If the key has expired or the payload is unavailable, ask an organizer for help.
4. When configuring an S3-compatible application, copy the matching access-key ID, secret access key, region, and endpoint into its credential settings. The bucket name comes from the bucket you create or reuse below.

The team's group has an additional permission to read this secret's payload. It contains:

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

### Use the credentials to upload and download files

The access-key ID and secret access key work together to authenticate an **S3-compatible client** to your team's storage. The client signs requests for you. Use the credentials already provided in SecretStash; no new service account or access key is needed.

The example below uses the **AWS CLI** to connect to **Nebius Object Storage**. You do not need an AWS account. This is a separate tool and profile from the `nebius` CLI used to manage Cloud workloads.

**1. Install the client and configure a local profile.** Follow the [AWS CLI installation guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) for your operating system, then run:

```bash
aws --version
aws configure --profile hackathon-storage
```

At each prompt, enter the following:

| AWS CLI prompt | What to enter |
| --- | --- |
| AWS Access Key ID | The value of `AWS_ACCESS_KEY_ID` from SecretStash |
| AWS Secret Access Key | The value of `AWS_SECRET_ACCESS_KEY` from the same secret |
| Default region name | `uk-south2` |
| Default output format | `json` |

Enter the key values at the interactive prompts, rather than including them in shell commands. This [named profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) stores credentials in your local `~/.aws/credentials` file in plain text. Configure it on your own trusted computer and keep that file private; do not upload it to the bucket or repository.

**2. Select your bucket and storage endpoint.** In Bash or Zsh, replace the bucket placeholder with the name you created or obtained from your teammate:

```bash
export HACKATHON_BUCKET='YOUR_TEAM_BUCKET_NAME'
export HACKATHON_STORAGE_ENDPOINT='https://storage.uk-south2.nebius.cloud'
aws --profile hackathon-storage --endpoint-url "$HACKATHON_STORAGE_ENDPOINT" \
  s3 ls "s3://$HACKATHON_BUCKET/"
```

The endpoint is the `AWS_ENDPOINT_URL` value from SecretStash. Keep the explicit `--endpoint-url` on each command so the client connects to Nebius. A successful listing with no output means the bucket is empty.

**3. Upload a small test file, then download it.** Run these commands in the same terminal. They use a temporary local directory and a unique object name to avoid replacing a teammate's file:

```bash
STORAGE_TEST_DIR=$(mktemp -d)
STORAGE_TEST_KEY="connection-checks/$(basename "$STORAGE_TEST_DIR")/hello.txt"
printf 'Hello from the hackathon!\n' > "$STORAGE_TEST_DIR/hello.txt"

aws --profile hackathon-storage --endpoint-url "$HACKATHON_STORAGE_ENDPOINT" \
  s3 cp "$STORAGE_TEST_DIR/hello.txt" "s3://$HACKATHON_BUCKET/$STORAGE_TEST_KEY"
aws --profile hackathon-storage --endpoint-url "$HACKATHON_STORAGE_ENDPOINT" \
  s3 cp "s3://$HACKATHON_BUCKET/$STORAGE_TEST_KEY" "$STORAGE_TEST_DIR/downloaded.txt"
cmp "$STORAGE_TEST_DIR/hello.txt" "$STORAGE_TEST_DIR/downloaded.txt" \
  && echo 'Upload and download verified.'
```

After the success message, remove just the test object from the bucket:

```bash
aws --profile hackathon-storage --endpoint-url "$HACKATHON_STORAGE_ENDPOINT" \
  s3 rm "s3://$HACKATHON_BUCKET/$STORAGE_TEST_KEY"
```

Use the same `s3 cp` pattern for your datasets, checkpoints, and results, changing the local path and object name. See [Nebius instructions for working with objects](https://docs.nebius.com/object-storage/interfaces/aws-cli#working-with-objects).

### Use the credentials in your application

Configure your application's S3 client with the access-key ID, secret access key, region, endpoint URL, and bucket name above. The exact setting names depend on the application. `ACCESS_KEY_EXPIRES_AT` is a reminder for you, not an authentication field to send to S3.

For an application running in a Devlab, job, or endpoint, supply the credentials to that workload through its secret configuration. Your laptop's AWS CLI profile is not automatically available inside a Cloud workload. Where supported, map SecretStash fields to the application's `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables, and configure `AWS_DEFAULT_REGION` and the S3 endpoint explicitly. Check your client's endpoint setting: not every client reads `AWS_ENDPOINT_URL` automatically. Keep credential values out of container images and source code.

If access fails, check the bucket name, Nebius endpoint, region, matching key pair, and expiry. A Cloud console login alone does not configure an S3 client. Ask an organizer if the key has expired or the correct settings still return an access error.

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
| Endpoint returns 503 | Check startup logs for image/model downloads, then application errors and the configured port. See [startup guidance](#allow-time-for-startup-and-model-loading). |
| Endpoint returns 401 | Check the gateway bearer token and any separate application authentication. |
| Browser Devlab returns an authentication error before its login page | Check whether platform bearer authentication was enabled; use the Jupyter/template login setup in [How to connect](#how-to-connect). |
| Object Storage authentication fails | Check the endpoint, region, matching key pair, and expiry in SecretStash. |
| Object Storage bucket missing | Select your assigned project and follow [Create and use your team's bucket](#create-and-use-your-teams-bucket). If the one-bucket limit is reached, check whether a teammate already created it. |
| Token Factory request fails | Check its own API key, credits, selected model, and response error. Cloud access does not establish Token Factory access. |

For console guidance, click **Ask Echo** to open [Nebius Echo](https://docs.nebius.com/overview/nebius-echo). It can explain services and inspect project resources. Tell it your assigned project and the limits in this guide, and review any proposed resource change before approving it. Keep credentials out of the conversation.

Ask the event organizers for help. Provide your project name, resource ID, timestamp, and request/trace ID when available. Redact tokens and credentials.

## 11. Optional: set up a coding agent

Skip this section if you are working without a coding agent. The sections above provide the participant setup and usage instructions. **Nebius skills** are instructions for an agent to operate Cloud resources; you do not need them to use the console or CLI yourself.

Download this README, open or attach it in your coding agent, and ask the agent to read it and follow the setup instructions. No separate prompt, application directory, or project idea is required. You do not need to install the Nebius skills beforehand. Once setup is complete, tell the agent what you want to do.

Use an agent with terminal access. If yours already works, keep its current model provider. If you want to connect **OpenCode to Token Factory**, first follow the [OpenCode setup guide](https://dev.nebius.com/cookbook/opencode-nebius-token-factory). Complete model-provider setup before asking the agent to read this file.

### Instructions for the agent

Your task is to prepare yourself to interact with the participant's assigned Nebius Cloud environment. Follow the setup sequence below. Complete the steps your tools and permissions allow; when the participant must act, explain exactly what to do and verify the result afterward. Ask only for missing setup information, such as the assigned project or a required sign-in. Do not ask for an application directory or development goal.

Sections 1–10 are the shared environment reference for participants and agents. Read them before making Cloud access checks, and reuse any console or CLI setup the participant has already completed. When the participant asks you to follow this setup, install or enable the skills and perform read-only access checks. Do not deploy a workload, create a bucket, or execute every example as part of setup.

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

Check whether the Nebius plugin's skills are already available in this session. If they are, verify that you can read them, then continue to Step 4 without reinstalling a working plugin. After reading the environment description, use **nebius-cloud-basics** in Step 5 for Nebius CLI installation, configuration, authentication, and general CLI errors. Use **nebius-public-docs** for questions about Cloud capabilities.

If the skills are unavailable or cannot load, use the agent client's installation guidance and the recovery steps below. These steps concern skill installation; the Nebius skills can help with Cloud setup once they are accessible.

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

After skill setup, read all of sections 1–10 of this README, including project access, limits, SecretStash, Standard buckets, and workload lifecycle. Summarize the environment constraints relevant to future cloud operations. Identify the participant's assigned tenant and project from available context; ask if the assignment is missing or ambiguous. Never infer their assignment solely from the example project name.

#### Step 5: Configure and verify Nebius Cloud access

Use **nebius-cloud-basics** for CLI installation, version requirements, profiles, authentication, scope resolution, and CLI errors. Follow its preflight and agent-identification instructions before CLI calls. Have the participant check `nebius version`, update with `nebius update` when needed, and check the version again. The skill provides steps for installation and browser sign-in; verify the result afterward. Check the requirements of the Serverless and Devlabs skills as well, not only the foundation skill.

**Reuse authentication in the environment that runs the CLI.** Check the execution user, home directory, and selected profile without reading credential contents. The CLI normally stores configuration under `~/.nebius`, outside the repository. A workspace sandbox, remote container, or different OS user may not be able to use the session established in the participant's terminal. Distinguish a filesystem-access restriction from an expired Cloud session before asking for another login.

If sandbox access is blocked, explain the required CLI configuration/session access and use the client's supported permission flow for that command or location. Do not disable the sandbox, copy credentials into the repository, or ask for tokens in chat. If the agent runs elsewhere, guide the participant through login in that execution environment using the supported browser flow. If that is not possible, have them run the commands in their own terminal and return only non-secret results. After setup, verify access from the agent's environment and reuse that profile; repeat sign-in only when the session requires it. Local filesystem permission prompts are separate from Nebius authentication.

Use the selected profile consistently. Confirm the authenticated identity and the assigned project and `uk-south2` region with read-only requests. Use **nebius-compute-inventory** for platform discovery and **nebius-capacity-quotas** for project quotas and reservation allowances. Check the project's Serverless listings through the relevant skills, projecting only non-secret metadata and state. A successful empty listing is valid; a failed request is not an empty environment. If a tenant-wide query is denied, report that check separately rather than changing permissions or assuming project access is broken.

Use **nebius-public-docs** when product behavior needs clarification. If a skill's command examples differ from current CLI help or public documentation, resolve the difference using those current sources while preserving the hackathon's assigned region and resource constraints.

Apply this README's environment constraints when following general skill examples. Do not create a personal tenant, generate replacement credentials, change permissions, or switch regions/projects to work around an access failure. The supplied Object Storage credentials remain in **hackathon-object-storage**. Never ask for credentials in chat or expose their values.

For subsequent tasks, use **nebius-serverless-endpoints**, **nebius-serverless-jobs**, **nebius-devlabs**, or the relevant Compute skill. Delegate job/endpoint diagnosis to **nebius-serverless-troubleshooting**, and workload storage or secrets to **nebius-serverless-data-secrets**. Token Factory and Tavily setup is described in sections 7 and 8 and is optional until the participant needs those services.

#### Step 6: Report readiness and wait for the participant's task

Report the skills available, the verified Cloud profile and assigned project, the region, which read-only checks succeeded, and any unresolved setup issues. Distinguish successful setup from workload readiness: resource listings do not prove that a deployment will succeed or that a GPU is currently free. Do not describe setup as complete unless the required project-access checks succeeded. If blocked, explain the precise participant action needed to continue.

Once ready, tell the participant they can now ask you to work with Nebius Cloud. Wait for their request before creating workloads, buckets, or other cloud resources. Apply the environment limits and relevant skills when they provide that request.

Before creating a requested workload, explain how the participant will connect, whether it consumes the project's public-IP allowance, and which authentication mechanism they will use. Follow [How to connect](#how-to-connect): prefer managed HTTPS for endpoints and browser Devlabs, and configure SSH only when requested. Use endpoint bearer authentication for API endpoints and the application login for browser Devlabs, leaving the separate Devlab bearer option off as described above. Verify the selected template's authentication flow and keep SSH keys separate from both token types.

#### Before creating the first workload

Setup above is read-only. When the participant later requests a deployment:

1. Resolve the assigned project, region, image/template, GPU or CPU preset, disk size, ports, authentication, and secret references. Check for an existing resource with the intended name to avoid duplicate creation.
2. Run the fully specified create command with `--dry-run` where supported. Both [endpoint create](https://docs.nebius.com/cli/reference/ai/endpoint/create) and [Devlab create](https://docs.nebius.com/cli/reference/ai/devlab/create) support it; confirm with the installed CLI's help. Protect secrets in dry-run output as well as normal output. Validation does not reserve a GPU or prove that the image, model, or application will start successfully.
3. Show the proposed configuration and command with secret references, and explain the resource and cost implications before execution. Use current pricing or the Nebius billing skill for an estimate; clearly label any unknown event-specific billing or reservation treatment. Do not assume unused resources are free because the event has reserved capacity. Running workloads use compute and storage; stopped Devlabs retain billed storage, while stopped endpoints release their container disks. Buckets and other persistent storage are separate. See [Serverless billing](https://docs.nebius.com/serverless/overview#pricing-and-quotas).
4. Obtain the participant's explicit confirmation of the proposed deployment before the first create. If that exact deployment is already approved, use that approval; setup approval alone is insufficient. Execute once, then inspect state before retrying an ambiguous failure.
5. Allow for image downloads and model loading, inspect logs without exposing secrets, and test the application's actual request or browser login. Report readiness only after that test succeeds. State what remains running and how to stop it, including any retained storage.
