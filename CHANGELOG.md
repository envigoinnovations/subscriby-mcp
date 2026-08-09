# Changelog

All notable changes to this publishing surface are documented here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-04-28

### Added

- Initial publish to the MCP Registry as `net.subscriby/mcp`.
- `server.json` declares the streamable-HTTP remote at `https://mcp.subscriby.net`.
- Bearer-token authentication via the `Authorization` header, declared as a required secret in the manifest.
- Public landing repo with README, SECURITY policy, MIT license, and CLAUDE conventions.
- DNS-verified GitHub Actions publish workflow gated on a tag-vs-`server.json` version match.
