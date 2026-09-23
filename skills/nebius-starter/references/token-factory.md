# Nebius Token Factory

Read when the application needs hosted model APIs, or when evaluating Sandboxes for code execution.

## Official entry points

- [Documentation index](https://docs.tokenfactory.nebius.com/llms.txt)
- [Model API quickstart](https://docs.tokenfactory.nebius.com/quickstart.md)
- [Token Factory console](https://tokenfactory.nebius.com/)
- [Billing and promo-code redemption](https://docs.tokenfactory.nebius.com/other-capabilities/billing-new.md)
- [Sandboxes coding-agent instructions](https://docs.tokenfactory.nebius.com/sandboxes/cli/commands/agent.md)
- [Sandboxes skill installation](https://docs.tokenfactory.nebius.com/sandboxes/cli/commands/skill.md)

Use the index to find current authentication, model capabilities, pricing, limits, and framework integration guides. Verify availability of the chosen model rather than assuming the model in a quickstart is still suitable.

## Model APIs

Start with the API or SDK that fits the existing application. Token Factory's OpenAI-compatible API can use a compatible client configured for Token Factory's documented base URL and credentials. This does not require changing the coding agent's own model provider or credentials.

Read the current quickstart and authentication guide before setting the base URL and environment-variable names. Keep the key on the server side and scope configuration to this application. Reuse a working integration. Choose a model for the required capabilities, latency, and budget; verify tool calling or structured output support when the application depends on them.

A model API call does not require the Nebius Cloud CLI, Cloud MCP, or a Sandboxes skill. Install a relevant integration skill if the official catalog provides one for the chosen task; do not invent a required general Token Factory skill.

After authentication, run one bounded request with real input. Inspect the useful response and any usage information, then exercise the application's full flow. Check for active dedicated endpoints or other persistent resources if the chosen workflow creates them.

## Sandboxes, only when needed

Use Sandboxes when the build needs isolated code execution. A chatbot or tool-calling agent that only calls APIs does not automatically need it.

Read the coding-agent instructions and the installation guide above, plus the CLI's own manual and help. Verify Sandboxes access independently of inference API access.

The documented `contree skill install` default can target multiple clients. Pass an explicit client/scope specification or supported destination. Check the installed CLI's help and the active harness's discovery locations: documentation examples and installer versions may disagree about paths. Include the generated supporting files and verify discovery in the running agent.

Verify a small execution and retrieve its output. Report active sessions, their limits, any retained artifacts, and how to stop resources created for the demo. Do not install Sandboxes merely to satisfy a generic instruction to install skills.
