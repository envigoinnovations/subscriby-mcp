# Changelog

All notable changes to this publishing surface are documented here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [3.1.0] - 2026-09-03

### Added

- **`broadcast_message` and `preview_broadcast_audience` accept `plan_id`.** Narrows a segment to one subscription plan instead of replacing it: `customer` plus a plan addresses people paying for that plan right now, `churned` plus a plan addresses people who held it and left. The plan and the state always describe the *same* subscription, so a member paying for one plan who once trialled another is not counted as being on the other.

  `broadcast_message` refuses a plan filter on a segment that cannot use one — `lead`, who never subscribed, and the pass segments, whose plan is already implied by the window. Silently dropping it would return a recipient count for a different audience than the caller described. `preview_broadcast_audience` drops it per segment instead, because its no-audience form sizes every segment in one call; each row reports the `plan_id` it actually applied.

- **Four subscription-state audience segments**, addressable by both tools: `expiring_soon`, `cancelled_still_active`, `paused` and `trialing_cardless`. These describe a subscription rather than a member status, which is what the five original segments could not do — someone who cancelled but has three weeks left carries the same status as someone renewing happily.

- **`expiring_within_days`**, the horizon `expiring_soon` reads. 1–90, default 7.

  An auto-renewing subscription is never `expiring_soon`. A subscription's end date is rewritten to the new period end on every renewal, so a date alone describes the next *invoice*; a member is counted only once their access genuinely lapses, meaning renewal is off or the plan does not renew at all.

- **`preview_broadcast_audience` reports `supports_plan_filter` and `requires_expiring_within_days` per segment**, so an agent can tell which inputs a segment will accept before sending anything.

### Changed

- The `broadcast.queued` and `broadcast.completed` webhook payloads now also carry `plan_id` and `expiring_within_days`, both `null` where they do not apply. No field was renamed or removed.

## [3.0.0] - 2026-08-31

**Breaking.** `create_plan`, `update_plan` and `list_plans` are now discriminated on plan kind. Any agent or script sending the old flat shape must be updated.

### Changed

- **Plans are a discriminated union.** Every plan carries a `kind` — `subscription`, `pass` or `pass_series` — and exactly one matching nested object: `billing`, `pass` or `pass_series`. Which one is present is fully determined by `kind`, so presence is a consequence of the tag rather than a signal to branch on.
- `create_plan` and `update_plan` take `kind` plus the one block that belongs to it. Sending a block belonging to another kind is refused rather than ignored, so a payload that names the wrong shape fails loudly instead of silently dropping fields.
- `list_plans` returns `kind` and the matching block in place of the old `pass_enabled` flag.
- The `pass` block renames `series_ends_at` to `recurrence_ends_at`. The old name described when a *recurrence* stops generating windows, and became ambiguous the moment a pass series existed.

### Added

- **`list_pass_windows`** — lists the concrete access windows a pass plan has generated, with their UUIDs. A pass series points at windows that already exist, so this is how an agent finds them; without it a series could not be composed outside the dashboard at all.
- The `pass_series` block: slate composition by window id, auto-include rules, blackouts, seat cap, sales cutoff, successor plan and holders-only presale.

### Removed

- `pass_enabled`. A boolean could express two plan shapes and there are now three, which is the whole reason `kind` replaced it. Read `kind` instead.
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
