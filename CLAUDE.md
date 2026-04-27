# MemberPassMCP — MCP Registry Publishing Repo

This repo is the _publishing surface_ for the MemberPass remote MCP server. The actual MCP runtime lives in `../MemberPass` (Laravel) and is served at `https://mcp.memberpass.net`. This repo contains zero runtime code.

## Foundational Context

- **Purpose**: publish `net.memberpass/mcp` to the MCP Registry at `registry.modelcontextprotocol.io`. The reverse-DNS namespace mirrors the TLD-first form of `memberpass.net`.
- **Source of truth**: `server.json`. Every published version of MemberPass on the registry is exactly what was in `server.json` at the tagged commit.
- **Version anchor**: `server.json#/version` is the canonical version. `CHANGELOG.md` and the `vX.Y.Z` git tag must match.
- **Tool catalog**: lives on the running MCP server; documentation lives in `../MemberPassDocs/content/mcp/`. Do NOT mirror tool docs here.
- **Auth**: bearer token in the `Authorization` header. The user mints an `mpt_*` personal access token in the MemberPass app settings. No OAuth on the server today.

## Conventions

- Bump `server.json#/version` when any of these change in `../MemberPass`:
  1. A tool is added, removed, renamed, or its input/output shape changes.
  2. The remote URL or transport changes.
  3. Auth requirements change.
  4. A SECURITY advisory is published.
- SemVer: breaking tool removal/rename → major; new tool or new optional input → minor; doc-only or fix-only → patch.
- Update `CHANGELOG.md` (Keep-a-Changelog format) in the same commit as a `server.json` version bump.
- Tag commits as `vX.Y.Z` exactly matching `server.json#/version`. The workflow refuses mismatches.
- Run `npm run schema:fetch && npm run validate` locally before tagging.

## Cross-Project Rules (IMPORTANT)

- A tool catalog change in `../MemberPass` (anything under `routes/ai.php` or `app/Mcp/`) must trigger:
  1. A `server.json` version bump in this repo.
  2. A docs update in `../MemberPassDocs/content/mcp/tools-reference.mdx` and the per-tool MDX file.
  3. A `CHANGELOG.md` entry here.
- Do not bump `server.json#/version` for tool changes that haven't shipped to `https://mcp.memberpass.net`. The registry advertises a remote URL; advertising before deploy is a broken-link hazard.

## Forbidden

- Do not duplicate tool documentation in this repo. The single source is `../MemberPassDocs/content/mcp/`. Link out from `README.md`; never describe tool behaviour here.
- Do not run actual MCP traffic from this repo (no test scripts that hit `https://mcp.memberpass.net` for "verification" — that belongs in `../MemberPass`'s PHPUnit tests).
- Do not commit anything matching `mcp-dns.key`, `.env`, `*.pem`, or any token starting with `mpt_`.
- Do not edit `../MemberPassDocs/content/mcp/` from this repo's tasks. If you find a docs error while updating `server.json`, flag it — don't silently cross-edit.
- Do not create new `.md` files beyond the four declared (`README.md`, `SECURITY.md`, `CHANGELOG.md`, `CLAUDE.md`) without explicit ask.

## Releasing

1. Confirm the tool surface on `../MemberPass` is deployed to `https://mcp.memberpass.net`.
2. Edit `server.json` → bump `version`.
3. Add a `CHANGELOG.md` entry.
4. Commit, then `git tag vX.Y.Z && git push --tags`.
5. The `publish.yml` workflow validates schema, verifies the tag/version match, runs `mcp-publisher login dns`, and publishes.
6. Verify the listing on `registry.modelcontextprotocol.io` within 5 minutes of the workflow completing.
