# signals/manual/

Manual on-record signals — anything a team member wants to push into the graph that didn't come from a tracked source (call, email). Examples: a competitor positioning shift spotted in an interview or podcast, a market signal from a press release, a customer quote read in the wild, a SERP scan finding, an internal observation worth surfacing for other teams.

On-record cross-cutting signals — see [`signals/`](../).

Same hard rules apply as for any signal. English only; every factual block carries a citation to the original artifact (URL, transcript, PDF, screenshot path); the exclusion list (legal/equity/HR/NDA/personal) applies to body, frontmatter, commit messages and PR titles; entity-scan with wiki-links runs at write time. The producing skill — or the human, when writing by hand — owns enforcing all of this. See [`canon/operations.md`](../../canon/operations.md) -> "Citations", "Tagging discipline at write time", "Entity-scan discipline" for the full procedure.

Unlike `daily-call/`, `email/` — which are produced by skills consuming tracked external sources on a recurring cadence — `manual/` is the catch-all for everything else. Topic-scoped, not per-day-aggregated: one author can push multiple manual signals on the same date if they touch distinct topics.

## Layout

```
signals/manual/
|-- README.md
|-- _TEMPLATE.md
\-- YYYY/
    \-- MM/
        |-- <person-slug>_<topic-slug>_YYYY-MM-DD.md
        \-- ...
```

Nested by year/month. Filename: `<person-slug>_<topic-slug>_YYYY-MM-DD.md` — author + topic-slug + full date (`<person-slug>` is `<firstname>-<lastname>`). Date in the filename so each file is self-identifying even if misplaced. Topic-slug in the filename so multiple manual signals from the same author on the same date don't collide. See [`_TEMPLATE.md`](_TEMPLATE.md) for the schema skeleton.

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

# <Topic title> — <Full Name> — YYYY-MM-DD

**Author**: [<Full Name>](../../../../people/<person-slug>.md)
**Team(s) impacted**: [<team1>](../../../../canon/team/<team1>.md), [<team2>](../../../../canon/team/<team2>.md)

---
```

Frontmatter is the canonical source for routing — see [`signals/README.md`](../) -> Frontmatter for the full schema and field semantics. The body restates the author + impacted teams as markdown wiki-links for human navigation; the two must stay in sync.

`teams_impacted` is one or more teams. Add every team that should pay attention to the signal, not just the author's own team. Omit any `related_to_<type>` field when the signal didn't mention that type of entity — don't list empty arrays.

### Body

Open-form prose. Suggested shape:

```md
## Context

What was observed, where, when. 1-3 sentences. Cite the source.

## Signal

The 2-5 bullets that capture the substance. Each bullet carries a source link if not already covered by Context.

## So what

Why this matters for the company. 1-3 bullets linking to the teams / themes / companies / products that should act on it. Keep it short — the consumer skill or human decides the action; the signal author just surfaces the observation.

## Source

- [<artifact title>](<URL or local path>)
```

`So what` is the only section unique to `manual/`: in `daily-call/` the action items emerge per-call inside the recap, here a manual signal needs an explicit "why this is in the graph" because the author chose to push it.

## Why cross-cutting (not nested under team)

A manual signal often spans teams: a competitor pivot relevant to product + marketing + sales; a press release that touches GTM + leadership; a customer quote that informs roadmap and renewal. The wiki links inside the signal route it correctly via `teams_impacted` frontmatter, regardless of where the file sits. Forcing a team folder would either duplicate the file or hide it from one of the teams that needs it.
