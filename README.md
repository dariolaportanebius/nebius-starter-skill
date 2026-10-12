# Nebius Starter

Tell your coding agent what you want to build. It helps you choose Nebius AI Cloud, Nebius Token Factory, Tavily, or a combination, connects the tools you need, and works with you until you have a small, verified demo.

For hackathons, workshops, and your first prototype. Bring an idea or an existing project.

**GTC Berlin 2026 participants:** start with the [event participant guide](docs/hackathon/README.md). It describes the assigned-project setup and event limits; use it alongside the general starter instructions below.

**Three products, separate accounts.** For this guided setup, you will create or reuse an account and sign in separately to each product your build needs. Product access, API credentials, and credit balances are separate, even when you use the same Google or GitHub identity. The skill walks you through setup and connects the tools; you complete sign-in, verification, consent, and any required billing details yourself.

**Redeem promo codes separately.** If you have an applicable code for AI Cloud, Token Factory, or Tavily, apply it in that product's own account. Each product's credits must be activated and checked separately; redemption in one does not credit the others. The skill helps you find the right redemption flow and verify the result. You only set up the products your build needs.

## Get started

You need a coding agent with access to your project and terminal. The installer uses Node.js and npm; see the [skills CLI requirements](https://github.com/vercel-labs/skills).

Run **one** command in your project folder, for the agent you are using:

```bash
# Codex
npx skills add antongisli/nebius-starter-skill --skill nebius-starter --agent codex --copy

# Claude Code
npx skills add antongisli/nebius-starter-skill --skill nebius-starter --agent claude-code --copy

# Cursor
npx skills add antongisli/nebius-starter-skill --skill nebius-starter --agent cursor --copy
```

These commands install into the current project. Review the destination shown by the installer. For another client, use the CLI's [supported agents](https://github.com/vercel-labs/skills#supported-agents). Refresh or restart your agent if the skill does not appear.

Then say:

> Use the nebius-starter skill. Help me get started with Nebius for what I want to build.

Or bring a concrete idea:

> Use the nebius-starter skill. I want to build a briefing app that finds recent news and produces a summary with sources. Use this project and help me get a working demo.

In Codex, you can also invoke `$nebius-starter`; in Claude Code, `/nebius-starter`.

If you cannot install skills, give your agent this prompt instead:

> Read https://github.com/antongisli/nebius-starter-skill/blob/main/skills/nebius-starter/SKILL.md and the supporting references relevant to my idea, then follow those instructions. Help me get started with Nebius for what I want to build.

That uses the instructions for this conversation without installing a discoverable skill.

## What it helps you choose

| Product | What it provides | Example builds |
| --- | --- | --- |
| [Nebius AI Cloud](https://docs.nebius.com/llms.txt) | GPU compute, storage, networking, and Serverless AI for your own workloads | Train a model, render a scene, process a dataset, host an AI application |
| [Nebius Token Factory](https://docs.tokenfactory.nebius.com/llms.txt) | Model APIs without managing model-serving infrastructure | Chatbot, document summarizer, agent |
| [Tavily](https://docs.tavily.com/agents.md) | Web search and extraction for current information | Research assistant, news monitor, answers with sources |

A video briefing could use Tavily for sources, Token Factory for its script, and AI Cloud for image generation, rendering, storage, and hosting. A text briefing may need only Tavily and Token Factory. The skill starts with the smallest useful version of your idea.

## What to expect

1. A brief introduction and a recommendation based on your idea, project, time, and budget.
2. Setup for your actual coding agent, using current official guides and existing configuration where possible.
3. Separate account, sign-in, and promo-code steps for each selected product. You complete personal steps and enter credentials through a secure local mechanism, never in chat.
4. A small working example, a real output check, and commands to run it again.
5. A handoff showing configured tools, remaining blockers, running resources, and how to stop ongoing charges.

Installing this starter adds instructions; it does not create accounts, provision cloud resources, or include credits. The agent explains expected costs before paid operations and works within your agreed budget. Accounts, access, and credits are checked separately for each product.

## Contents

- [Starter skill](skills/nebius-starter/SKILL.md)
- Product setup references: [AI Cloud](skills/nebius-starter/references/ai-cloud.md), [Token Factory](skills/nebius-starter/references/token-factory.md), [Tavily](skills/nebius-starter/references/tavily.md)
- [Review scenarios](tests/scenarios.md) for checking routing, setup, and completion behavior

This repository contains onboarding instructions, not bundled platform clients or an application template. Model availability, prices, and installation procedures are resolved from official sources when the skill runs.

## License

[MIT](LICENSE).
