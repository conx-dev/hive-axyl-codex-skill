# Hive Axyl Console Browser Signup/Login

Use this flow when the user wants to sign up or log in through the deployed Hive Axyl console UI instead of giving credentials to MCP.

## Console URLs

- Console: `https://gw-test-gcl.c2xstation.net:8099/`
- Signup page: `https://gw-test-gcl.c2xstation.net:8099/signup`
- Login page: `https://gw-test-gcl.c2xstation.net:8099/login`
- Codex connection page: `https://gw-test-gcl.c2xstation.net:8099/connect/codex`

## Assistant Workflow

1. Check whether the console web server is reachable at `https://gw-test-gcl.c2xstation.net:8099/`.
2. Open `https://gw-test-gcl.c2xstation.net:8099/connect/codex` when browser control is available. Use this connection URL instead of sending the user through account settings.
3. If the user is signed out, the console opens the login page. The user can follow the signup link if they need an account.
4. Let the user type their own email, name, and password in the console. Do not generate, infer, read back, log, or summarize passwords.
5. After successful authentication, the console returns to `/connect/codex` and automatically issues one connection code.
6. Ask the user to select the copy button and paste only the displayed code into the same Codex conversation. The code expires after five minutes and works once.
7. Do not inspect, extract, copy, or repeat the connection code through browser tooling.
8. Call `console_connect` immediately after the user provides the code, then call `console_session` to verify authentication.
9. If the code expires, ask the user to reload the connection URL to issue a new one.
10. If browser control is unavailable, tell the user that Codex cannot currently view or control the browser and give the manual steps below.
11. Do not fall back to `console_signup` unless the user explicitly provides email, name, password, and exact `SIGNUP <email>` confirmation phrase.

## Manual Connection Steps

Tell the user to follow these steps:

1. Open `https://gw-test-gcl.c2xstation.net:8099/connect/codex`.
2. Sign in on the page that appears. If the user does not have an account, follow the signup link and create one.
3. Enter the password only in the Hive Axyl console.
4. Wait for the browser to return to the Codex connection page and display a connection code.
5. Select the copy button next to the code.
6. Paste only the code into the same Codex conversation within five minutes.
7. Keep the page open until Codex confirms that the console session is connected.

## Browser Setup Guidance

If browser control reports no available browser, explain that browser control is not connected in the current Codex session. Ask the user to enable or connect one of:

- Codex in-app browser
- Chrome browser connector/extension supported by their Codex setup

Until one is available, Codex can still provide the URL and manual steps, but it cannot see the screen or confirm completion through the browser.

## Local Development

When intentionally running the repository services locally, replace the console origin with `http://localhost:5173`.
