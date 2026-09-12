# Subscriby MCP Server

Connect Claude, Cursor, VS Code and other MCP clients to your Subscriby workspace.

## What is Subscriby?

Subscriby is a Telegram-community monetisation platform — projects, tiered subscriptions, access codes, and analytics for creators. Learn more at [subscriby.net](https://www.subscriby.net).

## What this MCP server exposes

154 tools across your own account (who the token belongs to, its team, plan capabilities, connected accounts and alert routing), the Connectors Marketplace (every connector in its lane, what each can do and the form that connects it) and a project's connector installations (read them with their health; install, verify, configure, disconnect, and uninstall them after an impact preview), projects, plans and pass windows, subscriptions and their access grants, the hand-arranged perks still to deliver, the Disaster Recovery ledger (incidents, recoveries and their undo state, roll calls, allowances, readiness) and its controls (failover settings, standbys and their mirror, chat pickers, undo, reminders, the handover mail), coupons, subscriptions, members and their connected accounts, the support inbox (conversations, saved replies, settings), access codes, payment methods, payments, broadcasts, resources, bots, teams, webhook endpoints and deliveries, and analytics. See the full catalog at [docs.subscriby.net/mcp/tools-reference](https://docs.subscriby.net/mcp/tools-reference). Every tool declares whether it only reads or may change data, so a client can ask before running a destructive one.

## Connect from Claude

Add `https://mcp.subscriby.net` as a custom connector in claude.ai or Claude Desktop, or run `claude mcp add --transport http subscriby https://mcp.subscriby.net` in Claude Code, then sign in when asked. Follow [docs.subscriby.net/mcp/connecting-claude-desktop](https://docs.subscriby.net/mcp/connecting-claude-desktop).

## Connect from Cursor

Follow [docs.subscriby.net/mcp/connecting-cursor](https://docs.subscriby.net/mcp/connecting-cursor).

## Connect from VS Code

Follow [docs.subscriby.net/mcp/connecting-vscode-claude](https://docs.subscriby.net/mcp/connecting-vscode-claude).

## Connect from ChatGPT Desktop

Follow [docs.subscriby.net/mcp/connecting-chatgpt-desktop](https://docs.subscriby.net/mcp/connecting-chatgpt-desktop).

## Authentication

Two credentials work on the same endpoint:

- **OAuth 2.1** — the client discovers the authorization server from the `401` challenge, registers itself dynamically (RFC 7591) and sends you to Subscriby to sign in and authorize; PKCE `S256` is required, access tokens last an hour and refresh tokens rotate. The connection acts as you, on the team you are working in. Nothing to copy.
- **Personal access token** — `Authorization: Bearer sbt_…`, minted at `https://app.subscriby.net/settings/tokens` with only the abilities and team scope you choose.

See [docs.subscriby.net/mcp/authentication](https://docs.subscriby.net/mcp/authentication).

## Required token abilities

Every tool enforces one ability; an OAuth connection satisfies them all as the signed-in creator, a personal access token only those it carries. See [docs.subscriby.net/api/abilities](https://docs.subscriby.net/api/abilities).

## Rate limits

See [docs.subscriby.net/api/rate-limiting](https://docs.subscriby.net/api/rate-limiting).

## Error envelope

See [docs.subscriby.net/api/errors](https://docs.subscriby.net/api/errors).

## Async jobs

See [docs.subscriby.net/mcp/async-jobs](https://docs.subscriby.net/mcp/async-jobs).

## Security

See [`SECURITY.md`](./SECURITY.md).

## Status & versioning

See [`CHANGELOG.md`](./CHANGELOG.md). The version field in [`server.json`](./server.json) is the canonical version published to the MCP Registry.

## License

[MIT](./LICENSE).

## Support

[support@subscriby.net](mailto:support@subscriby.net)
