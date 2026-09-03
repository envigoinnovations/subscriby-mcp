# Subscriby MCP Server

Connect Claude, Cursor, VS Code and other MCP clients to your Subscriby workspace.

## What is Subscriby?

Subscriby is a Telegram-community monetisation platform — projects, tiered subscriptions, access codes, and analytics for creators. Learn more at [subscriby.net](https://www.subscriby.net).

## What this MCP server exposes

68 tools across projects, plans, subscriptions, members, access codes, payments, broadcasts, webhooks, and analytics. See the full catalog at [docs.subscriby.net/mcp/tools-reference](https://docs.subscriby.net/mcp/tools-reference).

## Connect from Claude Desktop

Follow [docs.subscriby.net/mcp/connecting-claude-desktop](https://docs.subscriby.net/mcp/connecting-claude-desktop).

## Connect from Cursor

Follow [docs.subscriby.net/mcp/connecting-cursor](https://docs.subscriby.net/mcp/connecting-cursor).

## Connect from VS Code

Follow [docs.subscriby.net/mcp/connecting-vscode-claude](https://docs.subscriby.net/mcp/connecting-vscode-claude).

## Connect from ChatGPT Desktop

Follow [docs.subscriby.net/mcp/connecting-chatgpt-desktop](https://docs.subscriby.net/mcp/connecting-chatgpt-desktop).

## Authentication

Paste a personal access token (`sbt_*`) in the `Authorization: Bearer` header. Mint one at `https://app.subscriby.net/settings/tokens` with the `mcp:full` ability. See [docs.subscriby.net/mcp/authentication](https://docs.subscriby.net/mcp/authentication).

## Required token abilities

See [docs.subscriby.net/api/abilities](https://docs.subscriby.net/api/abilities).

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
