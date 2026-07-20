---
name: hive-axyl
description: Use when integrating Hive Axyl SDKs for Web, Android, iOS, Unity, or Godot games, or when reading and managing Hive Axyl console data through MCP. Covers SDK setup, console login, projects, API keys, metrics, credentials, webhooks, live operations, payments, push, players, and sanctions.
---

# Hive Axyl

Use this skill for external Hive Axyl SDK usage and console automation. Keep the public product name as `Hive Axyl`; keep internal server/proto names such as `hiveng.v1` only when referring to implementation details.

## Source Routing

- For SDK install or game-client integration, read `references/sdk-installation.md`.
- For browser-based console signup/login guidance, read `references/console-browser.md` and use the Browser skill when available.
- For any console read or management operation, read `references/console-mcp.md` and use the `hive_axyl` MCP server tools.
- For full user-facing docs, prefer `https://conx-dev.github.io/hive-axyl-docs/` or local `docs/` pages when working in this repo.

## Console Workflow

1. Call `console_session`. If the MCP server is unavailable, read the connection setup in `references/console-mcp.md`; do not continue until the configured MCP and console services are reachable.
2. If `console_session` reports `authenticated: true`, continue with MCP tools.
3. If unauthenticated, read `references/console-browser.md` and give the user the exact Codex connection URL: `https://gw-test-gcl.c2xstation.net:8099/connect/codex`.
4. Prefer browser handoff. Open the connection URL when browser control is available, but have the user enter login or signup credentials only in the Hive Axyl console. Never ask them to paste a password into chat.
5. The connection page returns after authentication and issues a five-minute, one-time code. Ask the user to copy only that code and paste it into the same conversation. Do not inspect, extract, or repeat it through browser tooling.
6. Call `console_connect` immediately with the code, then call `console_session` again. Confirm success only when it reports `authenticated: true`.
7. If browser control is unavailable, provide the same connection URL and the manual steps from `references/console-browser.md`. Do not silently switch to MCP login.
8. Call `console_login` only when the user explicitly provides their console admin email and password and asks Codex to log in.
9. Console signup requires browser email verification. Never ask for or accept a signup password through MCP.
10. After authentication, use MCP tools for console reads and management instead of browser-controlling `console-web`.
11. Never expose or summarize the admin JWT.
12. Ask for the exact confirmation phrase documented in `references/console-mcp.md` before destructive, access-control, or player-sanction tools.
13. Treat issued API keys, server keys, invitation links, and connection codes as secrets. Use a connection code only as the immediate `console_connect` input; never repeat it or place it in command lines or assistant-authored logs.
14. Use credential values and login passwords only when the user provides them. Never read them back, summarize them, or include them in logs.

## SDK Workflow

1. Identify the target platform: Web, Android, iOS, Unity, or Godot.
2. Provide the platform install snippet from `references/sdk-installation.md`.
3. Tell the user they need `projectId` and `apiKey` from the Hive Axyl console.
4. Keep OAuth client secrets out of SDK/client code. Server-side credentials are configured in the console.
5. Point to the platform docs for deeper auth, session, notice, mailbox, payment, or push behavior.
