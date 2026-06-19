# team/

One folder per **functional team**. Each folder holds a single index file, `team/<team>/<team>.md`.

- **Team rules** (prescriptive: scope, hard rules, exclusions, escalation) live under [`canon/team/<team>.md`](../canon/team/) — PR-gated as canon.
- **Team index** (descriptive: who's on the team, what they care about, which skills they own) lives here at `team/<team>/<team>.md` — push direct.
- A team's **operational picture** is not a stored file. Agents read it natively from the graph: filter [`signals/`](../signals/) on `teams_impacted` and follow the entities those signals link.

`team/` is for **functional teams only** (e.g. leadership, sales, marketing, product, engineering, customer-success). Never a company name. Never a per-customer or per-prospect subfolder — those are `companies/<slug>.md` entities. See [`canon/anatomy.md`](../canon/anatomy.md) → "Folder taxonomy is closed".

## Schema — `team/<team>/<team>.md`

Frontmatter (reverse edges, maintained by double-write per [`canon/anatomy.md`](../canon/anatomy.md) → "Frontmatter wiki-link relations"):

```yaml
---
aliases: []
people_involved: []   # ← reverse of people/<x>.md → team: [[<team>]]
skills: []            # ← reverse of skills/<slug>/SKILL.md → owner: [[<team>]]
---
```

Body: one short paragraph on what the team owns day-to-day, and a pointer to the team's rules in `canon/team/<team>.md`. Keep it thin — the rules are canon, the operational picture is the graph.

## Adding a team

1. Create `team/<team>/<team>.md` from [`_TEMPLATE/_TEMPLATE.md`](_TEMPLATE/_TEMPLATE.md).
2. Create the team rules at `canon/team/<team>.md` from [`../canon/team/_TEMPLATE.md`](../canon/team/_TEMPLATE.md) (canon PR).
3. Add the team to [`../canon/profile.md`](../canon/profile.md) → "Team leads".
4. Point each team member's `people/<slug>.md` → `team: [[<team>]]` and double-write the reverse into `people_involved`.
