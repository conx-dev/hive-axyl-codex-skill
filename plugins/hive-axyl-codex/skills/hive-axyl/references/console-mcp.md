# Hive Axyl Console MCP

The `hive_axyl` MCP server is a Streamable HTTP server. Its deployed URL is:

```text
https://gw-test-gcl.c2xstation.net:8095/mcp
```

## Connection Setup

The deployed connection flow requires these user-facing services:

- Console web: `https://gw-test-gcl.c2xstation.net:8099`
- MCP server: `https://gw-test-gcl.c2xstation.net:8095/mcp`

The plugin `.mcp.json` must point to the deployed MCP URL. If the MCP server was unavailable when the current Codex task started, open a new task after the service becomes reachable so Codex can load the MCP tools.

## Local Development

For intentional local development, start the services in separate terminals from the repository root:

```bash
make console-service-run
pnpm -C console-web dev
pnpm -C mcp-server dev
```

The local endpoints are Console API `http://localhost:8082`, Console web `http://localhost:5173`, and MCP server `http://localhost:8787/mcp`. Never send a user to localhost unless these services are running on that user's machine.

## Authentication And Account

- `console_signup`, `console_login`, `console_connect`, `console_logout`, `console_session`
- `change_console_password`, `issue_console_connection_code`

Call `console_session` first. If unauthenticated, prefer `https://gw-test-gcl.c2xstation.net:8099/connect/codex` followed by `console_connect`, then call `console_session` again to verify success. Use `console_signup` only when the user provided email, name, password, and exact `SIGNUP <email>` confirmation.

`issue_console_connection_code` requires an already authenticated MCP session and only issues a code for another MCP session. Never use it to bootstrap the current session.

## Projects And API Keys

- Projects: `create_project`, `get_project`, `list_projects`, `update_project_status`, `delete_project`, `upsert_project_app_identifier`
- Members: `list_project_members`, `add_project_member`, `update_project_member_role`, `remove_project_member`
- API keys: `issue_api_key`, `list_api_keys`, `revoke_api_key`

## Metrics

- `get_daily_stats`, `get_monthly_stats`, `get_country_distribution`
- `get_revenue_stats`, `get_monthly_revenue_stats`, `get_retention_stats`

Dates use UTC `YYYY-MM-DD`; months use `YYYY-MM`.

## Credentials And Game Server

- Credentials: `list_credential_definitions`, `list_credentials`, `get_credential`, `set_credential`, `delete_credential`
- Server keys: `issue_server_key`, `list_server_keys`, `set_server_key_allowed_cidrs`, `revoke_server_key`
- Webhooks: `get_player_webhook`, `set_player_webhook`, `list_webhook_delivery_failures`, `list_webhook_delivery_successes`

Credential values and webhook signing secrets are write-only. Responses expose only masked metadata or configuration state.

## Live Operations

- Mailbox: `list_mails`, `create_mail`, `update_mail`, `delete_mail`
- Maintenance: `get_maintenance`, `set_maintenance`, `clear_maintenance`, `preview_maintenance`
- Notices: `list_notices`, `create_notice`, `update_notice`, `delete_notice`
- Remote push: `create_push_campaign`, `update_push_campaign`, `cancel_push_campaign`, `list_push_campaigns`, `list_push_deliveries`

All timestamps are ISO 8601 UTC strings.

## Payments

- Settings: `upsert_payment_market_setting`, `upsert_payment_product_setting`
- Products: `import_payment_products`, `list_payment_products`, `create_payment_product`, `delete_payment_product`
- Purchases: `list_purchases`, `get_purchase`
- Subscriptions: `list_subscriptions`, `get_subscription`

`amount_minor` is a decimal string to preserve 64-bit integer precision.

## Login, Players, And Sanctions

- Login providers: `get_login_provider_mappings`, `set_login_providers`, `delete_login_provider_mapping`
- Players: `search_players`, `get_player_detail`
- Sanctions: `ban_player`, `bulk_ban_players`, `unban_player`, `get_player_sanction`, `batch_get_player_sanctions`, `list_sanctions`

## Accepted Values

- Environment: `sandbox`, `live`
- Market: `google_play`, `app_store`, `steam`, `web`
- Project role: `owner`, `admin`, `viewer`
- Player search field: `player_id`, `nickname`, `email`

Use each tool's schema for the remaining domain enums.

## Exact Confirmations

The user must provide the exact phrase in their own message before these tools are called:

- Password: `CHANGE_PASSWORD`
- Member role: `UPDATE_MEMBER_ROLE <admin_id>`
- Member removal: `REMOVE_MEMBER <admin_id>`
- Credential deletion: `DELETE_CREDENTIAL <target>`
- Server key revocation: `REVOKE_SERVER_KEY <server_key_id>`
- Mail deletion: `DELETE_MAIL <mail_id>`
- Maintenance clear: `CLEAR_MAINTENANCE <project_id>`
- Notice deletion: `DELETE_NOTICE <notice_id>`
- Payment product deletion: `DELETE_PAYMENT_PRODUCT <product_id>`
- Push cancellation: `CANCEL_PUSH_CAMPAIGN <campaign_id>`
- Login mapping deletion: `DELETE_LOGIN_PROVIDER_MAPPING <country>`
- Player ban: `BAN_PLAYER <player_id>`
- Bulk player ban: `BULK_BAN_PLAYERS <project_id>`
- Player unban: `UNBAN_PLAYER <player_id>`

Also warn before `delete_project`, `update_project_status(..., "deleted")`, or `revoke_api_key`.

## Secret Handling

- Never expose or summarize the admin JWT.
- Full API keys and server keys are available only in their issue responses.
- Connection codes expire after five minutes and are single-use.
- Never persist or log passwords, credential values, signing secrets, API keys, server keys, connection codes, or JWTs.

## Logs

The MCP server writes JSON lines to stdout/stderr for startup, health checks, MCP route handling, session lifecycle, and tool start/success/error events. Tool logs include tool names and durations only.
