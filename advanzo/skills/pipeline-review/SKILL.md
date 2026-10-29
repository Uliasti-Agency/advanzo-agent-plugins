---
name: pipeline-review
description: Review the user's sales pipeline in Advanzo. Use when the user asks how the pipeline looks, which deals are stale or at risk, what closes this month, what to focus on, or for a weekly sales review.
---

# Pipeline review

## Steps

1. From `get-context`, list the pipelines and their stages. If the user has several pipelines and did not name one, ask which one, or review each briefly.
2. `pipeline-summary` for the pipeline: open deals per stage with amounts, per owner, won and lost in the range the user cares about (default last 30 days).
3. Find the deals that need attention with `search-records` on `deals`. Use `describe-resource` once to confirm column names. Typical filters:
   - Stale: open deals sorted by last update or last activity, oldest first.
   - Closing soon: `expected_close_date` within the next 30 days, open.
   - Overdue: `expected_close_date` before today, still open.
   - Big: amount above a threshold the user gives.
4. For the 3 to 5 most important open deals, `score-deal` if the AI tools are available. Otherwise pull `get-timeline` with limit 10 and judge from the last touchpoint and open activities.

## Output

- **Numbers**: a small table, stage by stage, count and amount. Then won and lost in the period with the win rate if both are above zero.
- **Needs attention**: a list of deals with one line each: name, amount, stage, days since last touch, and why it is on the list.
- **Suggested actions**: concrete and few. "Call X about Y", "Move Z to lost, no reply since 60 days". Each action tied to a deal.

Do not pad with generic sales advice.

## Acting on it

For every action the user picks, stage it with `propose-change`: an activity for the follow-up, a stage move, or a lost reason. Show the diffs, then apply. Never move or close deals without a confirmation.
