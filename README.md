# BugSmash AI Plugin

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![MCP](https://img.shields.io/badge/MCP-hosted%20server-blue.svg)](https://api.bugsmash.io/mcp)

Connect Claude, Cursor, and Codex to [BugSmash](https://bugsmash.io) — the visual review and feedback platform for agencies, product, design, and marketing teams.

With this plugin your AI assistant can create review projects, upload versions, read and summarize feedback, reply to comments, organize folders, and manage webhooks — all from the chat you already work in.

---

## What is BugSmash?

BugSmash is a review layer for the internet. Share any digital content — live websites, dashboards, videos, PDFs, images, audio, even mobile apps — and let clients and stakeholders comment directly on it. Comments are pinned exactly where the context is, so feedback, progress, and approvals live in one place instead of scattered screenshots and email threads.

## What this plugin does

This plugin packages BugSmash's hosted MCP server for AI coding assistants. Once installed, you can ask your assistant to:

| Area | What you can do |
| --- | --- |
| **Projects** | List, read, create, update, and delete review projects |
| **Versions** | List, read, create, and delete versions of a project |
| **Folders** | List, read, create, update, and delete folders |
| **Comments** | List, read, create, update, and delete feedback comments |
| **Replies** | List, create, and delete replies on a comment |
| **Webhooks** | List, update, test, and delete webhooks |
| **Uploads** | Attach local files and chat attachments as projects, versions, or comment updates |

The plugin talks to BugSmash's hosted MCP endpoint:

```text
https://api.bugsmash.io/mcp
```

Nothing runs locally and there is no server to host. This repository contains only the plugin manifests and a skill that teaches the assistant how to work with BugSmash safely.

## Requirements

- A [BugSmash](https://bugsmash.io) account
- Membership in at least one workspace as an **owner, admin, or member** with MCP enabled
- One of the supported clients below

Guests and collaborators cannot access a workspace through MCP.

## Authentication

BugSmash uses **OAuth 2.1 with PKCE**. The first time your assistant calls a BugSmash tool, your browser opens a sign-in page where you approve access. That's it.

- No API keys to copy, paste, or store
- The plugin never requests, stores, or proxies a BugSmash API key
- Access follows the same roles, plan, and usage limits as the BugSmash app
- Revoke access at any time from your BugSmash account settings

## Installation

### Claude Code

Add the marketplace and install the plugin:

```text
/plugin marketplace add solutionwise/bugsmash-ai-plugin
/plugin install bugsmash@bugsmash
```

Run `/reload-plugins` if Claude Code asks you to, then `/mcp` to sign in to BugSmash when prompted.

### Claude (web and desktop)

You can connect BugSmash directly as a custom connector — no plugin required:

1. Open **Customize → Connectors**.
2. Click **+** and choose **Add custom connector**.
3. Enter `https://api.bugsmash.io/mcp` as the remote MCP server URL. Leave the OAuth client fields empty — BugSmash supports dynamic client registration.
4. Click **Add**, then **Connect** and approve access in the BugSmash sign-in page.
5. Enable BugSmash in a new chat.

### Cursor

Open **Customize** in the sidebar, search for **BugSmash**, and select **Install** at the user or project scope.

If your team manages plugins centrally, an admin can add this repository as a team marketplace: go to **Dashboard → Plugins & MCPs**, choose **Add Marketplace** under **Team Marketplaces**, select **Import from Repo**, and paste `https://github.com/solutionwise/bugsmash-ai-plugin`.

### Codex

From the Codex CLI:

```bash
codex plugin marketplace add solutionwise/bugsmash-ai-plugin
codex plugin add bugsmash@bugsmash
```

Or open the **Plugins** tab in the ChatGPT desktop app, search for **BugSmash**, and install it. Start a new task after installing; the first BugSmash tool call opens the sign-in flow.

### Any other MCP client

Point your client at `https://api.bugsmash.io/mcp` using the streamable HTTP transport. The server supports OAuth discovery, dynamic client registration, PKCE (S256), token refresh, and revocation.

## Example prompts

```text
List my BugSmash projects. Do not change anything.
```

```text
Summarize the active feedback on the "Homepage redesign" project, grouped by page.
```

```text
Create a BugSmash review project named "Q4 landing page" for https://example.com.
```

```text
Add a new version to the "Mobile app onboarding" project from the screenshot I attached.
```

```text
Reply to comment #14 on "Pricing page v2" saying the spacing issue is fixed in the latest build.
```

## Safety

The bundled skill instructs the assistant to:

- Read current state before writing when the target isn't exact
- State the exact target and ask for confirmation before any delete
- Warn that deleting a folder also deletes its nested folders and every project inside them
- Tell you when a write creates or changes a public review link
- Never display OAuth tokens, webhook signing secrets, or private upload URLs
- Never retry a write automatically after a timeout

## Troubleshooting

| Symptom | What to do |
| --- | --- |
| `401 Unauthorized` | Reconnect BugSmash through the browser sign-in flow (`/mcp` in Claude Code). |
| `403 Forbidden` | Your workspace role, plan, usage limit, or OAuth scope doesn't allow the action. Guests and collaborators can't use MCP. |
| `404 Not Found` | Check the ID and confirm you connected the right workspace. |
| BugSmash tools don't appear | Run `/reload-plugins` (Claude Code), reload the window (Cursor), or start a new session (Codex). |
| `429 Too Many Requests` | You've hit a BugSmash rate or usage limit. Wait for it to reset. |

## Repository layout

```text
.
├── .agents/plugins/marketplace.json     # Codex marketplace
├── .claude-plugin/marketplace.json      # Claude Code marketplace
├── .cursor-plugin/marketplace.json      # Cursor marketplace
└── plugins/bugsmash
    ├── .claude-plugin/plugin.json
    ├── .codex-plugin/plugin.json
    ├── .cursor-plugin/plugin.json
    ├── .mcp.json                        # Hosted MCP endpoint
    ├── assets/                          # Icon
    └── skills/manage-reviews            # Review-management skill
```

## Development

To test changes locally, add this repository as a local marketplace in your client of choice. See [docs/client-setup.md](docs/client-setup.md) for step-by-step instructions for each client and a suggested functional test order.

Validate the manifests before opening a pull request:

```bash
claude plugin validate .
claude plugin validate ./plugins/bugsmash
```

## Links

- [BugSmash](https://bugsmash.io)
- [API documentation](https://docs.bugsmash.io)
- [Help center](https://helpcenter.bugsmash.io)
- [Privacy policy](https://bugsmash.io/privacy-policy/)
- [Terms of use](https://bugsmash.io/terms-of-use/)

Found a problem with the plugin? [Open an issue](https://github.com/solutionwise/bugsmash-ai-plugin/issues).

## License

This plugin package is released under the [MIT License](LICENSE). The license covers the files in this repository only, not the hosted BugSmash service.
