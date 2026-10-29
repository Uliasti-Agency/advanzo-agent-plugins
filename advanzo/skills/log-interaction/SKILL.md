---
name: log-interaction
description: Record a call, meeting or conversation in Advanzo as a note and follow-up activities. Use when the user shares meeting notes, a call summary or a transcript and wants it logged, or says "log this", "add a note", "create the follow-ups".
---

# Log an interaction

## Steps

1. Identify the record the interaction belongs to: a deal when there is one, otherwise the contact or company. `search-records` if the user gave only a name; confirm if ambiguous.
2. Structure the content:
   - With the AI tools and a deal: `analyze-transcript` with the deal id and the text returns a summary, key points and suggested follow-ups with due dates and priorities.
   - Without AI tools, or for a contact or company: write the summary yourself, pull out decisions, open questions and next steps with dates.
3. Show the user what you plan to log: the note text and the list of activities (title, type, due date, owner). Let them edit.
4. Stage the note with `propose-change` on `notes`, operation `create`, fields `body`, `via_resource` and `via_resource_id`. Stage one `activities` create per follow-up; use the activity types and the user id from `get-context`. Link activities to the record through `associations`.
5. Apply each proposal after the user's confirmation. Report what was written and where it waits for approval if the workspace routes agent writes through the inbox.

## Rules

- Keep the note factual and in the user's words. No invented details, no sentiment that is not in the source.
- Dates: convert "next Tuesday" to a real date before proposing.
- If the conversation changed the deal (new amount, new close date, stage), propose that update separately and say why.
- Do not log the same interaction twice. Check the recent timeline first with `get-timeline`, limit 5.
