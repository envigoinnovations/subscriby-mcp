# Changelog

All notable changes to this publishing surface are documented here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- **Linking a place from an agent.** `request_resource_link` asks the creator, through the connector, to pick the channel, group or supergroup a new resource will be; the resource appears the moment they choose and `project.resource.linked` fires. A project running no connected connector is `CONNECTOR_NOT_INSTALLED`. Taking the server to 161 tools.

- **Revenue composition.** `get_revenue_composition` returns the five donuts the dashboards draw: transaction fees by payment provider, settled transactions by plan kind, gross revenue by currency in USD, payment attempts by outcome and, unwindowed because it is a balance, the monthly recurring revenue split by plan; every dataset carries its total, unit and slices sorted largest first with their shares and chart colours. Taking the server to 160 tools.

- **The connector doctor.** `run_connector_doctor` verifies a project's installation of a connector and asks the connector about every resource it gates, answering one report with a finding per check (severity, the connector's own state word and sentence, whether the creator can fix it and where); the report is kept on the installation and `connector.doctor_completed` fires only when the findings changed. Taking the server to 159 tools.

- **Analytics by connector.** `get_connector_analytics` returns one row per connector the creator's projects run or ever granted access on: live installations, members with a linked account (and how many linked one in the window), live and pending grants, grants issued and revoked in the window, gross revenue in USD with its share; a purchase spanning two connectors counts toward both. Taking the server to 158 tools.

- **The notification centre.** `list_notifications` reads every alert the platform sent the creator (a sale, a support backlog, a bot or channel that went silent, a billing or security notice, a pass window, an onboarding nudge), newest first, each with its class, title, body, the dashboard screen it points at as a named route and a URL, and whether it was read; `mark_notification_read` and `mark_all_notifications_read` clear one entry or the backlog. `get_me` gained `unread_notifications`. Taking the server to 157 tools.

- **Uninstalling a connector.** `get_connector_uninstall_preview` shows what removing a connector from a project would touch without changing anything (its resources, the live grants on them, the plans left with nothing to grant, the recurring and one-time subscriptions on those plans, upcoming pass windows, open support threads, member accounts) and `uninstall_connector` (destructive) does it: grants revoked with reason `connector_uninstalled`, resources deactivated as detached with their external ids kept, the installation row kept, identities never removed, no money moved by itself; two opt-ins, on by default, take the emptied plans off sale and cancel their recurring subscriptions at period end with an email to each member. `connector.uninstalled` carries the counts. Taking the server to 154 tools.

- **Connector installations, write side.** Four tools run the acts that need no credential typed by a person: `install_connector` (a pending installation the creator then connects from the dashboard; `CONNECTOR_UNAVAILABLE` for a connector nobody may install today, `CONNECTOR_TIER_REQUIRED` for a second connector when the project owner lacks the Growth `multi_connector` capability), `verify_connector_installation` (a fresh probe recorded on the row), `update_connector_installation_settings` (the connector's declared settings fields, validated by their own rules; values are never returned) and `disconnect_connector` (destructive: the credentials are wiped and the row stays; the neutral twin of `disconnect_bot`). A connector the project does not run is `CONNECTOR_NOT_INSTALLED`. Five `connector.*` webhook events accompany them (installed, connected, status changed, settings updated, disconnected), the neutral family the `project.bot.*` events alias. Taking the server to 152 tools.

- **The Connector Directory.** Four read-only tools and one resource open the catalogue of connectors to agents: `list_connectors` (every connector Subscriby knows, lane by lane: available, experimental, paused, under development, coming soon, each with its badges, category and, when a package exists, the whole manifest and the declarative install and settings fields a client renders as the connect form), `get_connector` (one card by key), `list_connector_installations` (every installation a project holds, live and standby, with its state and health and never a credential) and `get_connector_installation` (a project's live installation of one connector, the neutral twin of `get_bot_status`, which stays as the alias), behind the new `project-connector:view-any` and `project-connector:view` abilities; `subscriby://connectors/catalog` carries the same cards for an agent to read at session start. Taking the server to 148 tools.

- **Disaster Recovery, write side.** Fourteen tools close the loop on the recovery ledger: `get_recovery_settings` and `update_recovery_settings` (automatic failover with the per-email fee consent, how members are told after a swap), `remove_standby_installation`, `get_resource_standby`, `request_resource_standby` and `withdraw_resource_standby_request` (the chat picker the bot sends the creator, since a token never names a platform chat), `use_resource_standby`, `remove_resource_standby`, `set_resource_standby_mirror`, `request_resource_replacement` and `withdraw_resource_replacement_request`, `revert_recovery_operation` (a swapped channel put back, or the previous sign-in account restored, inside the window), `nudge_pending_readmissions` and `notify_members_of_recovery` (the handover mail after a bot replacement, once per recovery). The five that move members or money are destructive; every refusal is the dashboard's own sentence as `VALIDATION_FAILED`. Taking the server to 144 tools.

- **Disaster Recovery, read side.** Five read-only tools open the recovery ledger to agents, for the creator who owns the team the token is scoped to: `list_recovery_incidents` (what the health probes found broken, open by default, with the reason in the connector's words), `list_recovery_operations` (every recovery ever run, filterable by kind and status, each with `revertible` and `revert_window_ends_at`), `get_recovery_roll_call` (where the re-admission after one channel recovery stands and whether a reminder may go out), `get_recovery_allowances` (what the rolling window still allows per kind and what support has released) and `get_recovery_readiness` (the checklist with each line's state and the totals). They answer to the new `project-recovery:view-any` and `project-recovery:view` abilities, and eleven `recovery.*` webhook events accompany them (incidents opening and resolving, recoveries starting, completing, failing and being undone, standbys registered and removed, channels failed over and replaced, accounts relinked). Taking the server to 130 tools.

- **Creator tasks.** A manual resource is a perk no connector can give, so a purchase that includes one now records a `creator_task` grant and opens a task for the creator instead of passing in silence. `list_creator_tasks` lists what a creator still has to hand over in a project (open by default, oldest first; `completed` and `all` for the record) and `complete_creator_task` marks one done, issuing the grant and raising `creator_task.completed` then `member.resource_added`; `creator_task.opened` fires when a task opens. Taking the server to 125 tools.

- **Reissue access.** `reissue_subscription_grants` revokes the grants a member holds on a subscription — every resource, or one — and has fresh ones issued, the members page's "Refresh invite links" for agents; it raises `member.resource_reissued` per resource and `member.resource_added` for each fresh grant, taking the server to 123 tools.

- **Pending access.** A purchase that entitles a member to a resource on a connector they hold no account on now leaves a `pending_identity` grant in the ledger and raises `member.resource_pending`; the grant is issued, and `member.resource_added` raised for the same `grant_id`, the moment the member connects an account. `member.resource_added` and `member.resource_removed` themselves now carry `connector`, `grant_id` and `mode`, and a removal marks the ledger row `revoked`.

- **Access grants.** `list_subscription_grants` reads the access ledger for one purchase — one row per resource and pass window with the connector, how access was given (`bearer_link`, `membership`, `role`, `creator_task`), where it stands (`pending_identity`, `pending`, `held`, `granted`, `revoked`, `failed`) and the classified reason for a failure — and `get_subscription` embeds the same rows as `grants`; taking the server to 122 tools.

- **Who am I.** `get_me` describes the creator the token belongs to — the team it is scoped to (the id every other tool acts in), every team held, the platform plan and its capabilities, the accounts linked on the connectors and where alerts go — so an agent learns who it is acting as before its first write; taking the server to 121 tools.

- **Member identities.** `list_member_identities` lists the platform accounts a project member has connected (connector, the platform's id, name and handle, how the link was proven, which one the project reaches first) and `unlink_member_identity` disconnects one on the creator's behalf, refused when it is the member's last way to sign in — taking the server to 120 tools. Two webhook events accompany them: `member.identity_linked` and `member.identity_unlinked`.

- **OAuth 2.1 sign-in.** The server publishes its protected resource metadata at `https://mcp.subscriby.net/.well-known/oauth-protected-resource`, names `https://app.subscriby.net` as its authorization server, registers clients dynamically (RFC 7591) as public clients, requires PKCE `S256`, and issues one-hour access tokens with 30-day rotating refresh tokens after the creator signs in and approves a consent screen. Personal access tokens keep working on the same endpoint; the `Authorization` header in `server.json` is no longer required.

- **Every tool declares whether it only reads or may change data**: the 49 write tools that carried no hint now declare `destructiveHint` (`false` for the tools that only add — creates, invitations, reminders, retries, test deliveries — `true` for every update, toggle, sync and restore), so a client that honours the annotation asks before running them. Read tools were already `readOnlyHint: true`.

- **`reorder_plans`** pins the order a project's plans appear in on the portal and in the Telegram bot, or resets it; `create_plan` and `update_plan` accept `sales_cap`, and every plan row now carries `sales_cap`, `sales_cap_sold`, `position` and `paused_reason`. Two events accompany them: `plan.sold_out` (a plan paused itself after its last allowed purchase, or a season filled its seats) and `plan.order_changed`.

- **Forty-nine tools**, taking the server from 68 to 117, so every creator operation the dashboard and the REST API offer is callable over MCP on the same business-logic layer and behind the same ability:
  - projects: `delete_project`, `restore_project`
  - coupons: `get_coupon`, `update_coupon`, `delete_coupon`, `activate_coupon`, `deactivate_coupon`
  - plans: `get_plan`, `update_plan`, `delete_plan`, `start_next_season`
  - pass windows: `get_pass_window`, `create_pass_window`, `cancel_pass_window`, `remind_pass_window_queue`
  - members and subscriptions: `get_subscriber`, `get_subscription`, `remind_pass_holder`
  - resources: `update_resource`, `activate_resource`, `deactivate_resource`
  - bot: `disconnect_bot`
  - payment methods: `get_payment_method`, `activate_payment_method`, `deactivate_payment_method`, `sync_payment_method_plans`, `delete_payment_method`
  - support inbox: `assign_support_conversation`, `reopen_support_conversation`, `block_support_contact`, `unblock_support_contact`, `list_canned_replies`, `get_canned_reply`, `create_canned_reply`, `update_canned_reply`, `delete_canned_reply`, `get_support_settings`, `update_support_settings`
  - webhooks: `create_webhook_endpoint`, `get_webhook_endpoint`, `delete_webhook_endpoint`, `rotate_webhook_endpoint_secret`, `test_webhook_endpoint`, `pause_webhook_endpoint`, `resume_webhook_endpoint`, `list_webhook_deliveries`, `get_webhook_delivery`, `retry_webhook_delivery`, `retry_dead_webhook_deliveries`

  Every update tool is partial: an omitted argument keeps its stored value, and a call that changes nothing writes nothing and emits no event. Every tool that changes or removes existing data is annotated destructive (see below), so a client that honours the annotation asks a human before running it.

- **The signing secret is returned once.** `create_webhook_endpoint` and `rotate_webhook_endpoint_secret` answer with `secret` in that response only; no other tool can read it back. This is the single exception to the server's rule that a secret is never surfaced.

- **Nine webhook events** the write tools now announce: `project.resource.updated`, `project.payment_method.updated`, `project.payment_method.deleted`, `support.conversation.blocked`, `support.conversation.unblocked`, `support.canned_reply.created`, `support.canned_reply.updated`, `support.canned_reply.deleted` and `support.settings.updated`. Payment-method payloads name the changed credential keys and never a value; the support-settings payload never carries the relay chat id.

### Changed

- **`list_pass_windows` requires `pass-window:view-any`** instead of `project-subscription-plan:view-any`. Tokens minted with the plan ability keep working, because the server treats it as satisfying the pass-window one; new tokens should carry `pass-window:view-any` explicitly.
- Read tools that returned a row inline (`list_coupons`, `list_plans`, `get_resource`, `list_payment_methods`, `list_subscribers`, `list_pass_windows`, `get_support_conversation`, `list_webhook_endpoints`) now share one row shape per entity with the matching get and write tools, so a row read from a list is identical to the same row read by id.

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
