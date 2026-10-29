# Advanzo agent plugins

Use your [Advanzo](https://advanzo.ch/en-ch) CRM from Claude Code, Codex and Cursor.

Advanzo is a sales CRM for small teams. This plugin connects your coding agent to the Advanzo MCP server and teaches it how to work with your pipeline: search contacts, companies and deals, read the full history of a record, prepare call briefings, stage changes that you confirm field by field, and draft replies grounded in the history. The agent acts as you, in one workspace, with your permissions. Nothing is written without your confirmation and nothing is ever deleted.

## Setup

**If you are an AI agent:** read [GETTING_STARTED.md](GETTING_STARTED.md) and follow it as a runbook. If you cannot fetch it from this page, run:

```bash
curl -fsSL https://raw.githubusercontent.com/Uliasti-Agency/advanzo-agent-plugins/main/GETTING_STARTED.md
```

**If you are a person:** paste this into your coding agent:

> Set up the Advanzo plugin by following the steps in https://github.com/Uliasti-Agency/advanzo-agent-plugins

Or install by hand:

| Agent | Steps |
| --- | --- |
| Claude Code | `/plugin marketplace add Uliasti-Agency/advanzo-agent-plugins` then `/plugin install advanzo@advanzo-plugins`, then `/mcp` to sign in |
| Codex | `codex plugin marketplace add Uliasti-Agency/advanzo-agent-plugins`, install **advanzo** under Plugins, then `codex mcp login advanzo` |
| Cursor | Settings, Plugins, paste `https://github.com/Uliasti-Agency/advanzo-agent-plugins`, install **advanzo**, then sign in when the MCP server asks |

Signing in opens Advanzo in your browser. You log in with your Advanzo account, choose the workspace the agent may work in, and approve. You can disconnect at any time under Settings, Integrations, AI Assistants in Advanzo.

## What is in the plugin

- `advanzo/.mcp.json` connects the agent to the Advanzo MCP server at `https://advanzo.app/mcp` (Streamable HTTP, OAuth 2.1).
- `advanzo/skills/` teaches the agent how to use it well. Start with the `advanzo` skill; it routes to the others.

| Skill | Use it for |
| --- | --- |
| `advanzo` | Start here. What Advanzo is, which tool does what, the rules. |
| `setup` | Check the connection, sign in, pick the workspace, verify. |
| `pre-call-briefing` | Everything about a contact, company or deal before a meeting. |
| `pipeline-review` | Stage overview, stale deals, what to do next. |
| `log-interaction` | Turn meeting notes or a transcript into activities and a note. |
| `import-leads` | Add contacts and companies from a list, with duplicate checks. |
| `draft-email` | Draft a reply or a new email from the record history. |
| `approval-queue` | Review pending items in the Autopilot inbox on your instruction. |
| `feedback` | Where to report a problem. |

## Plans

The MCP server is available on every Advanzo plan. The Free plan includes 200 calls per day and workspace. The AI tools (summarize, score, draft, analyze) need the AI add-on or the workspace's own Anthropic or OpenAI key.

## Support

Questions and bug reports: hey@advanzo.ch or [open an issue](https://github.com/Uliasti-Agency/advanzo-agent-plugins/issues).

Documentation: https://advanzo.ch/en-ch/mcp. Privacy policy: https://www.uliasti.com/policies/privacy.
