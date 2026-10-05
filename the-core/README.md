# The Core plugin

The Core keeps shared notes, tasks, projects, people, organisations, opportunities and the links between them for you and the agents you authorise, so any agent can pick up where the last one stopped. This plugin connects The Core's hosted MCP server and adds four workflow skills:

- **orient-and-resume**: confirm identity and space, check what is next, search and read before creating anything.
- **capture-and-link**: save decisions, notes, tasks, projects, people, organisations and opportunities without duplicates, link them and read back what was saved.
- **initialise-context**: build your first context from this conversation, files you upload and memory exports you provide, as one previewed batch you approve before anything is written.
- **handoff-and-hygiene**: leave a handoff a fresh agent can continue from; request archiving and restore records.

The skills use the hosted MCP tools only. No shell or CLI is needed.

## What you need

- An admitted The Core alpha account. Signing in does not create an account.
- A host that supports plugins: Claude Code, claude.ai chat or Cowork, or Codex CLI / the ChatGPT desktop app's Codex. The Codex IDE extension does not support plugins.

The plugin contains no keys or tokens. It points at `https://core-edge.slayteksystems.com/mcp`, and your host runs the sign-in. The sign-in page currently asks for a Core key: create one from your account's connect page at `https://core-edge.slayteksystems.com/app/connect`, paste it on the sign-in page and nowhere else. Do not paste the OAuth client ID your host shows.

## Install

The plugin is distributed manually to alpha testers. `<owner>/<repo>` below is the package repository you were given.

### Claude Code

1. `claude plugin marketplace add <owner>/<repo>`
2. `claude plugin install the-core@the-core`
3. Run `/mcp` in Claude Code, choose the `core` server and complete sign-in.

### claude.ai chat and Cowork

1. Open **Customize > Plugins > Add**.
2. Choose **Add marketplace** and enter `<owner>/<repo>`, or choose **Upload plugin** and select the zip file you were given.
3. Open the plugin's **Connectors** tab and connect the server. Adding the plugin does not connect it.

A plugin installed on your claude.ai account is also available in Cowork.

### Codex CLI and ChatGPT desktop app

1. `codex plugin marketplace add <owner>/<repo>`
2. Install **The Core** from `/plugins` in Codex CLI, or from the Plugins directory in the ChatGPT desktop app.
3. Sign in to the `core` server when prompted. If you are not prompted, open `/mcp` and authenticate the server listed there.

## Current limits

- Provenance (`source`) is limited to `agent` or `import`, is set only when a record is created, and is not available on notes.
- Archiving creates an approval request; the record stays visible until a person with approval rights approves it. Restore is immediate.
- First-run setup has no import receipt and no whole-batch undo yet. Mistakes are corrected record by record; the skill tags each batch so its records can be found together.
- Hard deletion, rewriting note content and removing links are not available through MCP.
- No in-chat interface: the plugin is text only.

## Optional preference

The skills never edit your global instructions. If you want orientation and handoffs every session, you can add this to your own preferences:

> When I start work, orient in The Core first: confirm identity and space, check what is next, and search before creating anything. Before you stop, leave a handoff in The Core.

## Maintainers

- Validate: `bun run plugin:check`. Build the claude.ai / Cowork upload zip: `bun run plugin:package` (output in `dist/plugins/`).
- Bump `version` in both `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json` for every release; hosts only update when it changes. Never rename the plugin.
- `evals/` holds `claude plugin eval` cases for skill triggering. They are not included in the upload zip.
