---
name: import-leads
description: Add people and companies from a list, a CSV, an email signature or a web page to Advanzo, with duplicate checks and staged creation. Use when the user says "add these to the CRM", "import", "create contacts for", or pastes a list of names and companies.
---

# Import leads

## Steps

1. Parse the input into rows: first name, last name, email, phone, job title, company name, company domain, source. Keep what you cannot map in a "notes" column; do not drop data silently.
2. `describe-resource` for `contacts` and `companies` once. Map your columns to the real field names, including custom fields the workspace uses for source or segment.
3. Duplicate check per row with `search-records`: contacts by email, then by name; companies by domain, then by name. Mark each row as new, existing, or unsure.
4. Show the user a table: row, action (create, link to existing, skip), the record it matches when existing. Ask about the unsure ones. Do not create unsure rows.
5. Stage creates with `propose-change`:
   - Companies first, operation `create`, so you have their ids.
   - Then contacts, with `associations: {"companies": [id]}`.
   - For an existing contact that lacks the company link, operation `update` with the association only.
   - Set the lead source from `get-context` if the user named one.
6. Apply after one confirmation for the batch. Report how many were created, linked and skipped, with ids. If writes are routed to the approval inbox, say so.

## Limits

- Stage at most 50 rows per confirmation round so the user can review.
- Never create a contact without at least a name and one of email or company.
- Do not enrich data from memory. If a field is unknown, leave it empty.
- Personal data: only import what the user provided or explicitly asked to fetch.
