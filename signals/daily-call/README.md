# signals/daily-call/

One MD **per person per day** synthesizing the recorded meetings that person was on. Every team member — including team leads and CEOs — has their own entry. Cross-team by design: a recap covering calls for two teams is a single MD that links both.

On-record cross-cutting signals — see [`signals/`](../).

Each person produces their own recap from whichever AI skill they use (typically a personal recap skill in [`skills/`](../../skills/), reading a `<transcript tool>` e.g. Fireflies, Otter over MCP). The recap is **append-only** in practice — re-running for the same date overwrites prior runs, so don't hand-edit and expect edits to survive a re-generation. The team's filters (what to emphasize, what to filter out) live in each team's [`canon/team/<team>.md`](../../canon/team/), which the producing skill reads at recap time. Per-logger personal filters live in the logger's own recap skill.

## Layout

```
signals/daily-call/
|-- README.md
|-- _TEMPLATE.md
\-- YYYY/
    \-- MM/
        |-- <person-slug>_YYYY-MM-DD.md
        \-- ...
```

Nested by year/month. Filename: `<person-slug>_YYYY-MM-DD.md` (`<person-slug>` is `<firstname>-<lastname>`) — full date in the filename so each file is self-identifying even if misplaced. See [`_TEMPLATE.md`](_TEMPLATE.md) for the schema skeleton.

## Output format

Whatever agent produces the recap, the file must follow this shape so cross-team consumers (humans and other agents reading `signals/daily-call/`) can parse it.

### Mandatory header block

```md
---
date: YYYY-MM-DD
author:
  - "[[<person-slug>]]"
teams_impacted:
  - "[[<team-slug>]]"
related_to_people:
  - "[[<person-slug>]]"
related_to_companies:
  - "[[<company-slug>]]"
related_to_products:
  - "[[<product-slug>]]"
related_to_systems:
  - "[[<system-slug>]]"
related_to_themes:
  - "[[<theme-slug>]]"
---

# Daily call recap — <Full Name> — YYYY-MM-DD

**Person**: [<Full Name>](../../../../people/<person-slug>.md)
**Team(s) impacted**: [<team1>](../../../../canon/team/<team1>.md), [<team2>](../../../../canon/team/<team2>.md)

---
```

Frontmatter is the canonical source for routing — see [`signals/README.md`](../) -> Frontmatter for the full schema and field semantics. The body restates the author + impacted teams as markdown wiki-links for human navigation; the two must stay in sync at write time.

`teams_impacted` is one or more teams. The author's own team is always present; add others when calls covered cross-team work. Omit any `related_to_<type>` field when the day's calls didn't mention that type of entity — don't list empty arrays.

See [`operations.md`](../../canon/operations.md) -> Tagging discipline at write time for the full rule set on links inside recaps.

### Per-call section

Then one section per call:

```md
## <call title or topic> — <HH:MM-HH:MM>

- **Attendees**: [<Full Name>](../../../../people/<person-slug>.md), <External Name, role @ company>, ...
- **Teams touched**: [<team>](../../../../canon/team/<team>.md), ...
- **Key points**: 3-5 bullets.
- **Decisions**: ...
- **Action items I own**: ...
- **Risks flagged**: ...
- **Source**: [<transcript tool>](<full transcript URL>)
```

Resolve attendees (internal and external) to wiki links via `people/<person-slug>.md` matching their `aliases`. When no MD exists for an external attendee, render as plain text — and per the create-on-first-mention rule in [`canon/operations.md`](../../canon/operations.md) -> "In-repo external entities — create-on-first-mention", query the `<CRM>` and create the MD if a record exists. Resolve relative dates to absolute. Cite every call with its transcript URL.

## Why cross-cutting (not nested under team)

A recap can cover calls relevant to multiple teams. A CEO's recap covers all teams. An account manager's recap may touch product + sales. Forcing a single team folder either duplicates the file or hides it from one of the teams that needs it. The wiki links inside the recap route it correctly, regardless of where the file sits.
