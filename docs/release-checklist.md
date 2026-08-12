# Public release checklist

## Hosted MCP server

- Deploy `https://api.bugsmash.io/mcp` with a valid public TLS certificate.
- Confirm OAuth discovery, dynamic client registration, PKCE S256, token refresh, and revocation.
- Confirm that MCP and REST access tokens are resource-bound.
- Confirm read, write, delete, role, scope, timeout, and rate-limit behavior.
- Confirm that reverse proxies and the WAF do not block Codex or Claude requests.
- Run MCP Inspector against the production endpoint.

## Plugin package

- Run the Codex plugin validator.
- Run `claude plugin validate .` and `claude plugin validate ./plugins/bugsmash`.
- Install both local marketplaces in clean Codex and Claude Code environments.
- Confirm that browser OAuth completes without an API key or client secret.
- Confirm that all 26 hosted MCP tools are visible.
- Add final BugSmash logo assets when approved.
- Add verified privacy policy, terms, and support URLs to the store listings.

## Review accounts

- Prepare one populated owner or admin workspace.
- Prepare one populated member workspace.
- Include safe projects, versions, comments, replies, folders, and webhooks.
- Provide clear reviewer instructions.
- Remove or rotate reviewer access after the review is complete.

## Submission

- Make this GitHub repository public before Claude submission.
- Submit the hosted MCP server and plugin metadata through the OpenAI plugin submission portal.
- Submit the public GitHub plugin URL through the Claude plugin submission form.
- Track review feedback and update this repository for each release.
