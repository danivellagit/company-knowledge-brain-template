# signals/email/

One MD **per person per day** synthesizing the email threads that person engaged with — sent, received, or read — for that date. Cross-cutting by design: a digest covering threads with two customer accounts is a single MD that links both.

On-record cross-cutting signals — see [`signals/`](../).

Each person produces their own digest from whichever AI skill they use (typically a personal recap skill in [`skills/`](../../skills/)), reading the `<email>` system (e.g. Gmail, Outlook) over MCP. The digest is **append-only** in practice — re-running for the same date overwrites prior runs, so don't hand-edit and expect edits to survive a re-generation. The team's filters live in each team's [`canon/team/<team>.md`](../../canon/team/); per-logger personal filters live in the logger's own skill.

## Layout

```
signals/email/
|-- README.md
|-- _TEMPLATE.md
\-- YYYY/
    \-- MM/
        |-- <person-slug>_YYYY-MM-DD.md
        \-- ...
```

Nested by year/month. Filename: `<person-slug>_YYYY-MM-DD.md` (`<person-slug>` is `<firstname>-<lastname>`) — full date in the filename so each file is self-identifying even if misplaced. See [`_TEMPLATE.md`](_TEMPLATE.md) for the schema skeleton.

## Output format

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

# Email digest — <Full Name> — YYYY-MM-DD

**Person**: [<Full Name>](../../../../people/<person-slug>.md)
**Team(s) impacted**: [<team1>](../../../../team/<team1>/), [<team2>](../../../../team/<team2>/)

---
```

Frontmatter is the canonical source for routing — see [`signals/README.md`](../) -> Frontmatter. The body restates the same info as wiki-links for human navigation; the two must stay in sync at write time.

`teams_impacted` lists every team whose work the digest touches. Use a link to [`profile.md`](../../canon/profile.md) in the body only when truly cross-cutting and no specific team applies (frontmatter then carries `teams_impacted: []`).

See [`operations.md`](../../canon/operations.md) -> Tagging discipline at write time for the full rule set on links inside digests.

### Per-thread section

One section per material thread. Group all replies in the same thread together — do not split a 12-message thread into 12 sections. Drop noise threads (transactional notifications, calendar invites without content) entirely.

```md
## <thread subject> — <counterparty / customer>

- **Counterparty**: <External Name, role @ company> (or [<Full Name>](../../../../people/<person-slug>.md) when an MD exists)
- **Internal participants**: [<Full Name>](../../../../people/<person-slug>.md), ...
- **Teams touched**: [<team>](../../../../team/<team>/), ...
- **Key points**: 3-5 bullets.
- **Decisions / commitments**: ...
- **Action items I own**: ...
- **Risks flagged**: ...
- **Source**: <thread URL or message-id>
```

Resolve participants (internal and external) to wiki links via `people/<person-slug>.md` matching their `aliases`. When no MD exists for an external participant, render as plain text — and per the create-on-first-mention rule in [`canon/operations.md`](../../canon/operations.md) -> "In-repo external entities — create-on-first-mention", query the `<CRM>` and create the MD if a record exists. Link the counterparty's company via [`companies/`](../../companies/) when relevant. Resolve relative dates to absolute. Cite every thread with a direct URL or message-id.

## Why cross-cutting (not nested under team)

Same reason as [`signals/daily-call/`](../daily-call/): a digest can touch multiple teams (a customer thread spans sales + product; an internal thread spans engineering + design). Forcing a single team folder either duplicates the file or hides it from one of the teams that needs it. The wiki links inside the digest route it correctly, regardless of where the file sits.
