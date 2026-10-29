---
name: approval-queue
description: Show and work through the Advanzo approval inbox (Autopilot queue) with the user. Use when the user asks what is waiting for approval, wants to go through pending suggestions, or asks to approve or dismiss a specific item.
---

# Approval queue

The queue holds items that need a human decision: AI-drafted replies, suggested tasks, stage moves, lead intakes, meeting briefs and changes staged by agents (`mcp_change`). Approving runs the item. Dismissing drops it.

## Steps

1. `list-approval-queue`, optionally with `type`. Present the items as a numbered list: type, what it would do, which record, who or what proposed it, when.
2. Walk through them one at a time or let the user pick numbers. For each item the user wants to look at, show the full content: the draft text, the task details, the before and after of a change.
3. Only when the user says approve or dismiss for a specific item, call `review-queue-item` with that `id` and `decision`. If the user asked for an edit first, pass it in `payload` (for example a changed title or body).
4. Report the result per item. A `draft_reply` approval records the decision; it does not send the email.

## Rules

- Never approve or dismiss anything the user has not seen in this conversation. "Approve all" needs the list shown first and a confirmation that names the count.
- Do not approve items proposed by yourself in the same session without saying that you proposed them.
- Items that look wrong or risky: say so before the user decides.
