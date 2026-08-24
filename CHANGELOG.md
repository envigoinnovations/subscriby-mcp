# Changelog

All notable changes to this publishing surface are documented here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.1.0] - 2026-08-24

### Added

- `create_plan` accepts `pass_sales_cutoff_anchor`, an optional input choosing which end of a time-limited pass window the sales cutoff counts back from. `before_start` is the default and the behaviour every existing plan has: sales close a set number of minutes before a window opens, and nothing is sold once it is running. `before_end` keeps the window on sale while it runs, so a buyer can join a session already in progress and still lose access when that window closes.
- `pass_sales_cutoff_minutes` is now described against the anchor rather than against the window start.

### Changed

- `before_end` carries constraints the tool rejects rather than silently accepting: a minimum of 5 minutes, a value below the shortest slot `duration_minutes` on the plan, and refusal of the anchor outright when that shortest slot is 5 minutes or less. Window lengths are per-slot, so one plan can mix a three-hour and a fourteen-hour window, and a single cutoff measured from the end lands differently on each.

## [2.0.0] - 2026-08-09

### Changed

- **Breaking.** The server is published as `net.subscriby/mcp`, replacing its former registry name. A server name is its registry identity, so existing clients must re-add it rather than receiving this as an update.
- The streamable-HTTP remote moved to `https://mcp.subscriby.net`, and the token-minting URL in the `Authorization` header description to `https://app.subscriby.net/settings/tokens`.
- Repository, website URL, description and branding assets updated to match.

## [1.0.0] - 2026-04-28

### Added

- Initial publish to the MCP Registry as `net.subscriby/mcp`.
- `server.json` declares the streamable-HTTP remote at `https://mcp.subscriby.net`.
- Bearer-token authentication via the `Authorization` header, declared as a required secret in the manifest.
- Public landing repo with README, SECURITY policy, MIT license, and CLAUDE conventions.
- DNS-verified GitHub Actions publish workflow gated on a tag-vs-`server.json` version match.
