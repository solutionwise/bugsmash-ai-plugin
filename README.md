# BugSmash AI plugin

This repository packages the BugSmash hosted MCP server for Codex and Claude Code.

The plugin connects to:

```text
https://bugsmash-backend.swdv.duckdns.org/mcp
```

BugSmash uses OAuth 2.1 with PKCE. Users sign in through their browser, select a workspace, and approve access. The plugin does not request, store, or proxy a BugSmash API key.

## Capabilities

- List, read, create, update, and delete projects
- List, read, create, and delete versions
- List, read, create, update, and delete folders
- List and read comments and replies
- Create replies and update or delete feedback
- List, create, update, test, and delete webhooks

Write tools require a BugSmash workspace owner or admin role. Delete tools require explicit confirmation.

## Repository layout

```text
.
├── .agents/plugins/marketplace.json
├── .claude-plugin/marketplace.json
└── plugins/bugsmash
    ├── .claude-plugin/plugin.json
    ├── .codex-plugin/plugin.json
    ├── .mcp.json
    └── skills/manage-reviews
```

The repository does not contain another MCP server implementation. The Laravel BugSmash backend is the single source of truth for the MCP tools, OAuth flow, permissions, and API behavior.

## Install in Codex

Add this GitHub repository as a marketplace and install the plugin:

```bash
codex plugin marketplace add solutionwise/bugsmash-ai-plugin
codex plugin add bugsmash@bugsmash
```

Start a new task after installation. The first MCP use opens the BugSmash OAuth flow.

## Install in Claude Code

Add the marketplace and install the plugin:

```text
/plugin marketplace add solutionwise/bugsmash-ai-plugin
/plugin install bugsmash@bugsmash
```

Run `/reload-plugins` after installation. Use `/mcp` if Claude Code asks you to authenticate.

## Use as a Claude custom connector

The plugin package is not required for a direct Claude connector test. In Claude, open **Customize → Connectors → Add custom connector** and enter:

```text
https://bugsmash-backend.swdv.duckdns.org/mcp
```

Then select **Connect** and complete the BugSmash OAuth flow.

## Test

See [client setup](docs/client-setup.md) for local marketplace checks and [release checklist](docs/release-checklist.md) for public directory preparation.

Useful test prompts:

```text
List my BugSmash projects. Do not change anything.
```

```text
Create a BugSmash review project named MCP Test for https://example.com.
```

```text
Delete the MCP Test project after I confirm the exact project.
```

## Documentation

- [BugSmash API documentation](https://docs.bugsmash.io)
- [BugSmash help center](https://helpcenter.bugsmash.io)
- [BugSmash website](https://bugsmash.io)
