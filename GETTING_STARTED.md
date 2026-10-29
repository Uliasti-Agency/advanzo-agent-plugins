# Advanzo getting started guide

This guide is written for an AI coding agent. Follow it step by step in the current session. It installs the Advanzo plugin for your agent and connects it to the user's Advanzo workspace.

The plugin contains no binaries and installs no software. It registers one remote MCP server (`https://advanzo.app/mcp`) and a set of skills.

## 1. Install the plugin

Pick the section for the agent you are running in.

**Claude Code** (v2.1.91 or later)

```
/plugin marketplace add Uliasti-Agency/advanzo-agent-plugins
/plugin install advanzo@advanzo-plugins
```

If you cannot run slash commands yourself, ask the user to run them, or use the shell equivalents:

```bash
claude plugin marketplace add Uliasti-Agency/advanzo-agent-plugins
claude plugin install advanzo@advanzo-plugins
```

**Codex**

```bash
codex plugin marketplace add Uliasti-Agency/advanzo-agent-plugins
```

Then open **Plugins** and install **advanzo**.

**Cursor**

Ask the user to open Settings, Plugins, paste `https://github.com/Uliasti-Agency/advanzo-agent-plugins` and install **advanzo**. If the plugin does not appear after installing, clone the repository and follow the `setup` skill as a runbook:

```bash
git clone --depth 1 https://github.com/Uliasti-Agency/advanzo-agent-plugins.git /tmp/advanzo-agent-plugins
cat /tmp/advanzo-agent-plugins/advanzo/skills/setup/SKILL.md
```

Delete the clone when you are done.

## 2. Sign in

The Advanzo MCP server uses OAuth. The first call fails with an authentication error until the user has signed in once.

- **Claude Code:** run `/mcp`, choose **advanzo**, choose **Authenticate**. A browser window opens Advanzo. The user logs in, picks the workspace the agent may work in, and approves. The token is stored by Claude Code and refreshed automatically.
- **Codex:** run `codex mcp login advanzo` and show the user the URL if the browser does not open.
- **Cursor:** open the MCP settings, find **advanzo** and click the login action.

Some platforms need a restart before the new skills and the server appear. If the `advanzo` tools are missing, ask the user to restart the agent.

## 3. Verify

Call the `get-context` tool. It returns the user you act as, the workspace, its plan, your permissions and the pipelines. Tell the user in one line which workspace you are connected to. Do not repeat tokens or ids.

If `get-context` fails with 401, repeat step 2. If it fails with 403, the user is no longer a member of the chosen workspace or the connection was revoked in Advanzo; sign in again.

## 4. Start working

Read the `advanzo` skill. It explains what Advanzo is, which tool to use for which task, and the rules: show every proposed change before applying it, never claim an email was sent, never approve queue items the user has not seen.

## Switching workspace

One connection is bound to one workspace. To work in another workspace, clear the authentication (Claude Code: `/mcp`, advanzo, Clear authentication) and sign in again, choosing the other workspace. The user can also disconnect from inside Advanzo under Settings, Integrations, AI Assistants.

## Scripts and CI

For unattended use, the user can create a personal access token in Advanzo (Settings, Personal access tokens) and connect without the plugin:

```bash
claude mcp add --transport http advanzo https://advanzo.app/mcp --header "Authorization: Bearer <token>"
```

Treat the token like a password. Never write it into files that are committed.
