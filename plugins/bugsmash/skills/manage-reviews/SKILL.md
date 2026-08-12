---
name: manage-reviews
description: Manage BugSmash visual review workflows through the hosted BugSmash MCP tools. Use when a user asks to create or find projects, manage versions or folders, read or summarize feedback, update comments, manage replies, or manage webhooks.
---

# Manage BugSmash Reviews

Use the BugSmash MCP tools for live data and actions. Do not call the REST API when an MCP tool covers the request. If the MCP server needs authentication, ask the user to connect their BugSmash account through the browser OAuth flow.

## Workflow

1. Resolve names to IDs with list or detail tools.
2. Read current state before a write when the target is not exact.
3. Use the smallest write that completes the request.
4. Return the relevant project, version, folder, comment, reply, or webhook ID and any review link.

For feedback summaries, request plain text and location metadata. Show active comments before resolved comments. Preserve comment numbers and IDs so the user can act on the result.

For a new version, first confirm the project and its content type. Use a supported public URL. The hosted MCP server cannot read a local file path from the user's computer.

## Safety

- Do not ask for or display a BugSmash API key. The MCP server uses OAuth.
- Before a delete, state the exact target. Ask for confirmation unless the user already gave a clear delete instruction for that target. Pass `confirmDeletion: true` only after confirmation.
- Treat project, version, comment, reply, folder, and webhook deletes as destructive.
- Follow the public folder-delete contract: the folder is deleted and its projects are preserved.
- Tell the user when a write creates or changes a public review link.
- Never display an OAuth token or webhook signing secret.
- Do not retry a write automatically after a timeout. Read current state first to prevent a duplicate write.

## Errors

- `401`: Ask the user to reconnect BugSmash through the browser OAuth flow.
- `403`: Explain that the selected workspace role or OAuth scope does not permit the action. Write tools require an owner or admin role.
- `404`: Verify the ID and confirm that the user connected the correct workspace.
- `422`: Correct the input from the response message. Do not change field names without evidence.
- `429`: Report the limit. Do not retry until the limit resets or the account changes.
- `5xx`: Report a BugSmash service error. Do not claim that the write failed if the result is uncertain.
