---
name: draft-email
description: Draft an email reply or a new email to a contact, company or deal in Advanzo, grounded in the record's history. Use when the user asks to reply to someone, write a follow-up, send a quote reminder, or "draft an email to".
---

# Draft an email

Nothing in Advanzo sends email through the agent. You draft; the user sends from Advanzo or their mail client.

## Steps

1. Find the record and pull `get-timeline` with `types: ["emails"]` to see the thread. Identify the email to reply to and note its `id`.
2. With the AI tools: `draft-reply` with `email_id` for a reply, or `resource` and `id` for a new email, plus `instructions` that say what the email must achieve, `tone` (formal, friendly, brief) and `language` if the user said so.
3. Without the AI tools: write the draft yourself from the timeline. Keep it to the point, answer open questions, state the next step and a date.
4. Show the draft with subject and body. Offer two changes at most if the user hesitates. Then stop.

## Rules

- Facts only from the timeline, the record and what the user told you. No invented prices, dates or names.
- Match the language of the thread. German for Swiss recipients uses ss, not ß, and formal address unless the thread is informal.
- No em dashes. Short paragraphs.
- When the user wants the draft kept, stage a note on the record with the draft text via `propose-change`, and say that it is a note, not a sent email.
- If the user asks you to send it, explain that sending happens in Advanzo or their mail client and offer to create a follow-up activity instead.
