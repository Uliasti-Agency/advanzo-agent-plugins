---
name: setup
description: Connect and verify the Advanzo MCP server. Use when an advanzo tool returns an authentication or authorization error, when the advanzo tools are missing, when the user wants to switch workspace, or right after installing the plugin.
---

# Advanzo setup

The plugin registers the remote MCP server `https://advanzo.app/mcp`. There is nothing to install. The user signs in once; the agent stores and refreshes the token.

## 1. Check the state

Call `get-context`.

- Success: you are connected. Go to step 3.
- Authentication error (401): go to step 2.
- Forbidden (403): the connection is bound to a workspace the user has left, or it was revoked in Advanzo. Go to step 2 and sign in again.
- Tool not found: the plugin is not loaded. Ask the user to restart the agent. If it still is missing, repeat the install from `GETTING_STARTED.md` in the plugin repository.

## 2. Sign in

Tell the user what will happen: a browser window opens Advanzo, they log in, choose the workspace the agent may work in, and approve.

- **Claude Code:** ask the user to run `/mcp`, choose **advanzo**, then **Authenticate**. If the browser does not open, the command prints a URL to copy.
- **Codex:** run `codex mcp login advanzo`. Show the user the URL if needed. Do not use a built-in browser.
- **Cursor:** ask the user to open the MCP settings, find **advanzo** and click the login action.
- **Remote sandboxes without a browser:** the user can create a personal access token in Advanzo (Settings, Personal access tokens) and connect with a bearer header instead, see `GETTING_STARTED.md`.

## 3. Verify

Call `get-context` again and tell the user in one or two lines: the workspace name, the user you act as, and whether the AI tools are available (plan or own key). Do not print ids or tokens.

## Switching workspace

One connection is bound to one workspace. To change it: clear the authentication (Claude Code: `/mcp`, advanzo, Clear authentication), then sign in again and choose the other workspace. The user can also disconnect in Advanzo under Settings, Integrations, AI Assistants, which revokes the token immediately.
