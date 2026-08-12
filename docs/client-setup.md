# Client setup and testing

## Prerequisite

The hosted endpoint must be available at:

```text
https://bugsmash-backend.swdv.duckdns.org/mcp
```

An unauthenticated request must return `401 Unauthorized` with a `WWW-Authenticate` header that points to the BugSmash protected-resource metadata.

## Codex local marketplace test

From the parent directory of this repository:

```bash
codex plugin marketplace add ./bugsmash-ai-plugin
codex plugin add bugsmash@bugsmash
```

Start a new Codex task. Enable BugSmash and complete browser authentication when requested.

## Claude Code local marketplace test

In Claude Code:

```text
/plugin marketplace add /absolute/path/to/bugsmash-ai-plugin
/plugin install bugsmash@bugsmash
/reload-plugins
/mcp
```

Confirm that the BugSmash MCP server is connected.

## Claude custom connector test

1. Open **Customize → Connectors**.
2. Select **Add custom connector**.
3. Enter `https://bugsmash-backend.swdv.duckdns.org/mcp`.
4. Leave client credentials empty. BugSmash supports dynamic client registration.
5. Select **Connect**.
6. Sign in to BugSmash, select a workspace, and approve access.
7. Enable BugSmash in a new chat.

## Functional test order

1. List projects.
2. Read one project and its versions.
3. List comments for a project.
4. Create a test project as a workspace owner or admin.
5. Update the test project.
6. Request deletion and verify that the agent asks for confirmation.
7. Confirm the deletion and verify the result.
8. Revoke the OAuth connection in BugSmash and verify that the old connection no longer works.

Also test a member account. Read tools must work. Write tools must return a permission error.
