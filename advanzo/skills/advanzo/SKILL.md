---
name: advanzo
description: Start here for anything about the user's Advanzo CRM. Explains what Advanzo is, which MCP tool does what, and the rules for reading and changing CRM data. Use when the user mentions Advanzo, their CRM, pipeline, deals, contacts, companies, leads, follow-ups, call prep, or the approval inbox.
---

# Advanzo

Advanzo is a sales CRM for small teams: contacts, companies, deals in pipelines with stages, activities (tasks, meetings, calls), logged calls, notes, emails synced from the team's mailboxes, documents and products. An AI layer (Autopilot) proposes next steps and a person approves them in an approval inbox.

You reach it through the `advanzo` MCP server. You act as the signed-in user, inside one workspace, with exactly their permissions. Everything you read and every change you stage is logged in the workspace and shown in the record history as "via MCP".

## Before the first task in a session

1. Call `get-context`. It gives you the user, workspace, currency, plan, permissions, pipelines with stages, activity types, call outcomes, lost reasons and lead sources. Keep it in mind; do not call it again unless the session is long.
2. If it fails with an authentication error, follow the `setup` skill.
3. Before filtering or writing a resource for the first time, call `describe-resource` for it. Field names, custom fields, select options and filterable columns differ per workspace. Never guess a field name.

## Which tool for which task

| Task | Tool | Notes |
| --- | --- | --- |
| Find records by name, email, deal name, or filters | `search-records` | Free text plus structured filters, sorted and paged. Max 100 per page. |
| Load specific records with their associations | `get-records` | Up to 50 ids. |
| History of a contact, company or deal | `get-timeline` | Newest first: activities, calls, notes, emails, documents. |
| Numbers per stage, per owner, won and lost | `pipeline-summary` | Optional pipeline id and date range. |
| Create, update or unlink a record | `propose-change` then `apply-change` | Always two steps. See "Changing data". |
| Summary of a record from fields and history | `summarize-record` | AI tool. |
| Health score of an open deal with reasons | `score-deal` | AI tool, facts are returned so you can check them. |
| Email draft, reply or new | `draft-reply` | AI tool. Nothing is sent. |
| Transcript to summary and follow-up tasks | `analyze-transcript` | AI tool. Nothing is saved; stage what the user wants. |
| Pending items in the approval inbox | `list-approval-queue` | |
| Approve or dismiss one item | `review-queue-item` | Only on an explicit decision by the user. |

Skills for common jobs: `pre-call-briefing`, `pipeline-review`, `log-interaction`, `import-leads`, `draft-email`, `approval-queue`.

## Changing data

Writes are staged. `propose-change` validates the change with the same rules as the Advanzo UI and returns a field-by-field diff, warnings and a proposal id. Nothing is written yet.

- Show the diff to the user in plain language before calling `apply-change`. For a batch of similar changes you may show the batch once and apply after one confirmation.
- `apply-change` writes directly, unless the workspace requires approval for agent writes or you pass `route: "approval"`. Then the change waits in the approval inbox and a team member applies it. Tell the user which happened.
- Proposals expire after about an hour. Propose again if needed.
- Associations on update are additive. To remove a link use `operation: "unlink"`.
- There is no delete. Do not try to work around it. If the user wants something removed, tell them to do it in Advanzo.
- Put a one-sentence `summary` on every proposal; it is shown to reviewers.
- Check for duplicates with `search-records` before creating a contact or company.

## Rules

- Never say an email was sent, a task was done or a deal was moved unless `apply-change` or `review-queue-item` returned success. Drafts are drafts.
- Never approve or dismiss queue items the user has not seen. The `approval-queue` skill explains how.
- Write in the user's language. German users in Switzerland write ss, not ß.
- Summarize results, do not dump raw JSON. Mention record names and ids so the user can find them.
- Do not repeat tokens, secrets or other users' private data back to the user.
- The AI tools need the AI add-on or the workspace's own API key. If a tool says AI is not available, tell the user and offer to do the task yourself from `get-timeline` data instead.
- The Free plan has a daily call limit per workspace. If a tool reports the limit, stop calling tools and tell the user.
- If `get-context` reports fewer permissions than a task needs, say so instead of trying another way.
