---
name: manage-reviews
description: Manage BugSmash visual review workflows through the hosted BugSmash MCP tools. Use when a user asks to create or find projects, manage versions or folders, read or summarize feedback, update comments, manage replies, or manage webhooks.
---

# Manage BugSmash Reviews

Use only the provided BugSmash MCP tools for live data and actions. If an action is not supported, state that the current connector cannot perform it. Do not search for, display, or recommend service implementation details or endpoints as a workaround. If authentication is required, ask the user to connect their BugSmash account through the browser sign-in flow.

## Workflow

1. Resolve names to IDs with list or detail tools.
2. Read current state before a write when the target is not exact.
3. Use the smallest write that completes the request.
4. Return the relevant project, version, folder, comment, reply, or webhook ID and any review link.

For feedback summaries, request plain text and location metadata. Show active comments before resolved comments. Preserve comment numbers and IDs so the user can act on the result.

For project and version creation, identify the content type first. Use `websiteUrl` for website reviews. Image projects can accept multiple files. Other file-based types accept one file.

When the user provides a local file or chat attachment, use `prepare_file_upload` for each file. Upload the exact file bytes to the returned short-lived URL with the returned method and headers. Do not display the upload URL. Pass only the returned `uploadId` to the project, version, or comment tool. This upload is the only allowed non-MCP transfer in this workflow. Do not call or describe service endpoints.

Use a public HTTP or HTTPS file URL with its exact file name only when local file access is unavailable or the user already supplied a URL.

For a comment attachment update, prepare one JPG, JPEG, PNG, or SVG upload and pass its upload ID.

## Safety

- Do not ask for service credentials. Use the browser sign-in flow.
- Before a delete, state the exact target. Ask for confirmation unless the user already gave a clear delete instruction for that target. Pass `confirmDeletion: true` only after confirmation.
- Treat project, version, comment, reply, folder, and webhook deletes as destructive.
- Follow the public folder-delete contract: the folder is deleted and its projects are preserved.
- Tell the user when a write creates or changes a public review link.
- Never display an OAuth token or webhook signing secret.
- Never display, store, or reuse a temporary upload URL.
- Do not retry a write automatically after a timeout. Read current state first to prevent a duplicate write.

## Errors

- If authentication is required, ask the user to reconnect BugSmash through the browser sign-in flow.
- If permission is denied, explain that the selected workspace role does not permit the action. Write tools require an owner or admin role.
- If an item is not found, verify its ID and confirm that the user connected the correct workspace.
- If input is invalid, correct it from the returned message. Do not change field names without evidence.
- If a limit is reached, report the limit. Do not retry until it resets or the account changes.
- If BugSmash returns a service error, report it. Do not claim that a write failed if the result is uncertain.
