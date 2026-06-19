---
# people/<firstname>-<lastname>.md
# One schema, two shapes. Internal vs external is derived from `company:`.
# Internal -> company points to the home company (the MD with is_self: true).
# External -> company points to any other companies/<slug>.md.
#
# Delete the comments and the block you don't need before saving.

# --- Required for everyone ---
name: <Full Name>
short: <First name>                  # optional, how they're addressed day-to-day
aliases: []                          # MANDATORY, never empty. Every spelling/handle/email seen in
                                     # transcripts, chat, mail. e.g. ["<First>", "<f.last@domain>"]
linkedin: <https://www.linkedin.com/in/...>   # saved-once stable ID
company:
  - "[[<company>]]"                  # internal -> [[<company>]] (home); external -> [[<their-company>]]
job_title:
  - "[[<title>]]"                    # wiki link to job-titles/<title>.md

# --- Internal-only (keep ONLY when company is the home company) ---
founder: true                        # only if the person is a founder; otherwise delete
reports_to:
  - "[[<manager-slug>]]"             # omit for top-level
direct_reports: []                   # manual reverse of reports_to; e.g. ["[[<report-slug>]]"]
start_date: <YYYY-MM-DD>
timezone: <timezone>                 # e.g. Europe/Rome
team:
  - "[[<team>]]"                     # leadership | sales | marketing | product | engineering | customer-success

# --- External-only (keep ONLY when company is NOT the home company) ---
crm_url: <CRM contact record URL>    # MANDATORY for external (e.g. HubSpot, Salesforce, Pipedrive). Leave empty for internal.
twitter:                             # optional
website:                             # optional
---

BODY ASYMMETRY: the body weight follows where the truth lives.

- INTERNAL = rich body. The repo is the source of truth for internal context.
- EXTERNAL = light body. The CRM is the source of truth; this MD is just a pointer.

Keep ONLY the shape that matches this person. Both shapes are shown below as fenced examples; copy one into the live body (outside the fence) and fill it in.

## Shape A: INTERNAL team member (rich body)

```markdown
# <Full Name>

## Company

[<COMPANY>](../companies/<company>.md)

## Team

[[<team>]]   <!-- single-team, or cross-team (typical for leadership) -->

## Scope, what I own

- <Concrete responsibility, stated as a verb, not a title>
- <...>

## Anti-scope, what I do NOT own

- <A wrong assumption an agent might otherwise make about this person>
- <...>

## Decision authority

What this person signs off on unilaterally vs. what needs another signer.

## Operating style (optional)

Things that help an agent draft communication in this person's voice.

## How I write (optional)

Tone of voice. Sentence rhythm, register, what to avoid. An agent drafting outbound
on this person's behalf loads this section and writes in their voice.

## Context (optional)

Background, prior roles, domain expertise.
```

## Shape B: EXTERNAL person (light body, nothing else)

```markdown
# <Full Name>

## About

1-2 sentences: who they are, their role, optionally relevant background. Stable facts only.

## Company

[<their-company>](../companies/<their-company>.md)
```

For EXTERNAL: stop at `## Company`. Do NOT add Notes, Context, deal involvement, history, preferences, or any prose about the relationship. That lives in the CRM (via `crm_url`) and goes stale here in weeks.
