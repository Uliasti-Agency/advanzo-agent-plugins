---
name: pre-call-briefing
description: Prepare the user for a call or meeting with a contact, company or deal in Advanzo. Use when the user asks what they should know before a call, wants a summary of a relationship, asks about the last touchpoint, or says "brief me on".
---

# Pre-call briefing

Goal: in under a minute of reading, the user knows who they are talking to, where the deal stands, what was last said, and what to bring up.

## Steps

1. Find the record. `search-records` with the name the user gave. If several match, show the candidates with company and owner and let the user pick. Do not guess.
2. Load it with `get-records` to get fields and associations (contacts of the deal, company of the contact, open deals of the company).
3. Pull the history with `get-timeline`, limit 50. For a deal, also pull the timeline of the main contact if the deal timeline is thin.
4. If the AI tools are available, call `summarize-record` with `focus` set to what the user said the call is about. Check it against the timeline; the summary must not claim things the timeline does not show.
5. For an open deal, `score-deal` gives risks and next actions with the facts behind them.

## Output

Short sections, no raw data:

- **Who**: name, role, company, owner on our side, how long the relationship exists.
- **Where the deal stands**: pipeline, stage, days in stage, amount, expected close, open and overdue activities.
- **Last contact**: the last 3 to 5 touchpoints, newest first, one line each with date and who.
- **Open items**: questions they asked that were not answered, promises we made, documents sent.
- **Suggested talking points**: 3 to 5, each tied to something in the history.

Dates relative to today ("12 days ago") plus the date. Flag anything that looks stale or risky.

## Afterwards

Offer to log the call outcome with the `log-interaction` skill once the call is done.
