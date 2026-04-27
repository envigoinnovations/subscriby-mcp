# MemberPass MCP Server

Connect Claude, Cursor, VS Code and other MCP clients to your MemberPass workspace.

## What is MemberPass?

MemberPass is a Telegram-community monetisation platform — projects, tiered subscriptions, access codes, and analytics for creators. Learn more at [memberpass.net](https://www.memberpass.net).

## What this MCP server exposes

42 tools across projects, plans, subscriptions, members, access codes, payments, webhooks, and analytics. See the full catalog at [docs.memberpass.net/mcp/tools-reference](https://docs.memberpass.net/mcp/tools-reference).

## Connect from Claude Desktop

Follow [docs.memberpass.net/mcp/connecting-claude-desktop](https://docs.memberpass.net/mcp/connecting-claude-desktop).

## Connect from Cursor

Follow [docs.memberpass.net/mcp/connecting-cursor](https://docs.memberpass.net/mcp/connecting-cursor).

## Connect from VS Code

Follow [docs.memberpass.net/mcp/connecting-vscode-claude](https://docs.memberpass.net/mcp/connecting-vscode-claude).

## Connect from ChatGPT Desktop

Follow [docs.memberpass.net/mcp/connecting-chatgpt-desktop](https://docs.memberpass.net/mcp/connecting-chatgpt-desktop).

## Authentication

Paste a personal access token (`mpt_*`) in the `Authorization: Bearer` header. Mint one at `https://app.memberpass.net/settings/tokens` with the `mcp:full` ability. See [docs.memberpass.net/mcp/authentication](https://docs.memberpass.net/mcp/authentication).

## Required token abilities

See [docs.memberpass.net/api/abilities](https://docs.memberpass.net/api/abilities).

## Rate limits

See [docs.memberpass.net/api/rate-limiting](https://docs.memberpass.net/api/rate-limiting).

## Error envelope

See [docs.memberpass.net/api/errors](https://docs.memberpass.net/api/errors).

## Async jobs

See [docs.memberpass.net/mcp/async-jobs](https://docs.memberpass.net/mcp/async-jobs).

## Security

See [`SECURITY.md`](./SECURITY.md).

## Status & versioning

See [`CHANGELOG.md`](./CHANGELOG.md). The version field in [`server.json`](./server.json) is the canonical version published to the MCP Registry.

## License

[MIT](./LICENSE).

## Support

[support@memberpass.net](mailto:support@memberpass.net)
