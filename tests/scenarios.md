# Review scenarios

Use these as manual acceptance cases when changing the skill. They describe expected behavior, not a claim that live platform tests have passed. Exercise installation in a disposable project. Cloud/API execution requires a test account and an agreed budget; never use production resources for a review.

| Scenario | Expected behavior |
| --- | --- |
| “I'm at a hackathon and don't know what to build.” | Briefly introduces all three products, offers a few small ideas, and asks for a choice before installing product-specific tools. |
| “Add document summaries to this existing app.” | Inspects the stack and recommends Token Factory if suitable. Keeps the model key server-side; does not add web search, a GPU VM, or Sandboxes without a reason. |
| “Build a briefing about recent news with sources.” | Recommends Tavily plus Token Factory, installs relevant skills, integrates runtime APIs, and checks that a real output uses returned sources. |
| “I need to render this scene with my own GPU code.” | Reads Cloud guides, checks workload requirements and target capacity, explains costs, and verifies an actual rendered artifact. |
| “Make a video briefing from today's news.” | Explains how all three products fit; identifies a feasible first milestone and verifies each integration needed for that milestone. |
| “I need an agent to execute generated code.” | Evaluates Sandboxes and checks its access, manual, skill scope, execution result, and session cleanup. |
| Codex is active; Claude Code is also installed. | Uses session evidence to target Codex, preserves other client settings, and verifies the actual skill discovery path. |
| A Sandboxes installer or skill catalog defaults to every client. | Selects an explicit target and required dependencies; does not use the broad default. |
| An operational MCP is configured but absent from this session. | Labels it unverified, gives a continuation prompt if restart is needed, then checks the live connection after restart. |
| Only a documentation MCP is available. | Does not claim account authentication or a usable operational connection. |
| “Use my hackathon credits; I don't know which product they're for.” | Verifies applicability or gives a concrete user check. Does not assume cross-product credits or start unbudgeted paid use. |
| A browser login needs the user's attention. | Gives the exact next action, continues independent work, then verifies the connection after login. Never asks for the key in chat. |
| A create request times out. | Checks whether the resource was created before retrying; avoids duplicate resources and charges. |
| No GPU capacity, unavailable model, missing credential, or inaccessible required guide. | Reports the specific blocker, offers a smaller feasible path, and distinguishes completed work from unverified work. |
| User returns after a restart. | Resumes the pending verification using the handoff instead of repeating introductions and installs. |
| Demo is complete but a VM is stopped and its disk remains. | Reports the disk and any other retained billable resources; provides scoped cleanup steps and preserves needed artifacts. |

## Installation acceptance

From a disposable project, install the local checkout with the supported skills CLI, using one explicit agent and project scope. Check that:

- Exactly `nebius-starter` is discovered from this repository.
- `SKILL.md`, `agents/openai.yaml`, and all three `references/*.md` files arrive intact.
- Every local reference resolves from the installed folder.
- The skill lists in the selected client or its supported discovery mechanism; refresh/restart if needed.
- Existing project skills and settings remain intact.

Installer listing and file checks validate packaging. A real client session validates skill discovery and behavior. Authenticated calls and an end-to-end result validate product setup; do not conflate these checks.
