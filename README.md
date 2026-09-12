# BugSmash AI plugin

This repository packages the BugSmash hosted MCP server for Codex, Claude Code, and Cursor.

The plugin connects to:

```text
https://api.bugsmash.io/mcp
```

BugSmash uses OAuth 2.1 with PKCE. Users sign in through their browser and approve access. The connection can use each MCP-enabled workspace where the user is an owner, admin, or member. The plugin does not request, store, or proxy a BugSmash API key.

## Capabilities

- Prepare private uploads for local file attachments
- List, read, create, update, and delete projects
- List, read, create, and delete versions
- List, read, create, update, and delete folders
- List, read, create, update, and delete comments
- List, create, and delete replies
- List, update, test, and delete webhooks

Tools use the same role, plan, and usage limits as BugSmash. Guests and collaborators cannot access a workspace through MCP. Delete tools require explicit confirmation.

## Repository layout

```text
.
├── .agents/plugins/marketplace.json
├── .claude-plugin/marketplace.json
├── .cursor-plugin/marketplace.json
└── plugins/bugsmash
    ├── .claude-plugin/plugin.json
    ├── .codex-plugin/plugin.json
    ├── .cursor-plugin/plugin.json
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

## Test and submit the Cursor plugin

The Cursor manifest reuses `.mcp.json`, the logo, and the review skill in
`plugins/bugsmash`. It does not contain a second MCP configuration.

For local package discovery, link the plugin into Cursor's local plugin directory:

```bash
mkdir -p ~/.cursor/plugins/local
ln -s /absolute/path/to/bugsmash-ai-plugin/plugins/bugsmash ~/.cursor/plugins/local/bugsmash
```

Reload Cursor and open **Customize** to check the plugin. Local plugin imports
must be permitted by your team. Connecting to the hosted MCP server accesses the
production BugSmash service; do this only when that production test is authorized.

After testing, submit the public repository link at
[Cursor Marketplace](https://cursor.com/marketplace/publish).
This package has not been submitted. Grok Bot availability must be confirmed
separately; a Cursor manifest does not guarantee a Grok Bot listing.

## Use as a Claude custom connector

The plugin package is not required for a direct Claude connector test. In Claude, open **Customize → Connectors → Add custom connector** and enter:

```text
https://api.bugsmash.io/mcp
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
- [Privacy policy](https://bugsmash.io/privacy-policy/)
- [Terms of use](https://bugsmash.io/terms-of-use/)
- [BugSmash website](https://bugsmash.io)

## License

This plugin package is available under the [MIT License](LICENSE). The license does not apply to the hosted BugSmash service or its backend source code.
