# signals/

Cross-cutting **signals** — append-only on-record syntheses that feed AI agents as context.

| Subfolder | Origin | Cardinality |
|---|---|---|
| [`signals/daily-call/`](daily-call/) | Recorded meeting transcripts (from a `<transcript tool>` e.g. Fireflies, Otter) | One MD per person per day |
| [`signals/email/`](email/) | Email threads (from `<email>` e.g. Gmail, Outlook) | One MD per person per day |
| [`signals/manual/`](manual/) | Manual push by any team member (anything not from a tracked source — competitor interview, press release, SERP scan, internal observation) | One MD per topic per author per day |

Cross-cutting by design: a single MD can link any team(s), product, person. They are never nested under a team folder, even if a specific recap is mostly relevant to one team. The wiki links inside each MD give human navigation; the **frontmatter** (below) is what skills query for routing.

The output format for each subfolder is documented in its own README. Whatever AI agent (or person) writes here must produce that format — the repo only cares about the file shape, not which tool produced it.

## Frontmatter (mandatory for every signal file)

Every signal entry — regardless of subfolder — carries the same frontmatter at the top of the file:

```yaml
---
date: YYYY-MM-DD            # the day this signal covers.
author:                     # the person who wrote / owns the signal.
  - "[[<person-slug>]]"     # resolves to people/<slug>.md
teams_impacted:             # every team this signal touches — wiki-link list to canon/team/<slug>.md
  - "[[<team-slug>]]"
related_to_people:          # people surfaced in the body — internal and external, excluding the author. Resolves to people/<slug>.md
  - "[[<person-slug>]]"
related_to_companies:       # external companies surfaced (resolves to companies/<slug>.md)
  - "[[<company-slug>]]"
related_to_products:       # company offerings surfaced (resolves to products/<slug>.md) — only when explicitly discussed
  - "[[<product-slug>]]"
related_to_systems:         # external tech vendors mentioned (resolves to the vendor companies/<slug>.md) — only when explicitly discussed
  - "[[<system-slug>]]"
related_to_themes:          # themes surfaced (resolves to themes/<slug>.md) — only when explicitly discussed as a framework / concept, not just a passing word. Bidirectional: also add the signal to themes/<slug>.md -> signals:
  - "[[<theme-slug>]]"
---
```

All relation fields are wiki-link lists so a local Markdown editor (e.g. Obsidian, Tolaria) renders each as a separate section in the Properties panel ("Author", "Teams impacted", "Related to people", "Related to companies", ...). The author and every linked entity show the signal under their "Referenced by" section (the editor's generic incoming-link view).

Splitting `related_to` into one field per entity type (people / companies / products / systems / themes) is intentional: agents and humans skimming the signal see grouped relations rather than a flat blob, and an agent filtering "all signals mentioning product X" reads one specific field instead of disambiguating a mixed list.

`related_to_themes` is the only `related_to_<type>` field that is bidirectional — the matching reverse lives at `themes/<theme>.md -> signals:` and must be written in the same operation. All other `related_to_<type>` fields rely on the editor's generic "Referenced by" view on the target page. See [`canon/anatomy.md`](../canon/anatomy.md) -> Frontmatter wiki-link relations — forward + reverse pairs for the full rationale.

Omit any `related_to_<type>` field when the signal didn't mention that type of entity. Don't list empty arrays.

`teams_impacted` is also the **routing field**. An agent assembling a team's operational picture filters signals by parsing the YAML — the wiki-link list `["[[<team>]]", ...]` matches against the target team slug. **Do not parse wiki-links in the body for routing** — string-matching inline markdown is brittle and slow as volume grows.

The body of the signal still carries human-readable wiki-links to the same teams + person, so a reader sees clickable navigation. **Frontmatter is canonical**: if body and frontmatter disagree, frontmatter wins. The producing skill must keep them in sync at write time. The signal author (the person in `author:`) is **not** repeated in `related_to_people` — authorship already implies relevance.

See each subfolder's README for source-specific body format (per-call section, per-thread section, per-event section, ...).

## Why this folder exists

The repo distinguishes:
- **Stable context** — `canon/`, [`people/`](../people/), [`canon/team/<team>.md`](../canon/team/) — changes rarely, edited by humans.
- **Operational state** — this folder, read natively from the graph (signals + the entities they link). There is no separate per-team digest to maintain.
- **Cross-cutting signals** — this folder. On-record syntheses (`daily-call/`, `email/`, `manual/`, and any future source-derived subfolder) live here because they serve the same purpose for agents: append-only context inputs.

Pulling signals out of `team/` removes a structural problem: a person who attends cross-team calls (a CEO, a cross-team PM, an account manager touching product + sales) had no honest home for their recap under a single team. Now their recap sits at `signals/daily-call/` and links the team(s) impacted.

## Adding a new on-record subfolder

The three sub-sources are fixed: `signals/daily-call/`, `signals/email/`, `signals/manual/`. When a genuinely new tracked source becomes a routine input for agents (e.g. code-host PR digests, CRM deal narratives), create a subfolder here — via PR, never direct-push:

```
signals/
|-- README.md
|-- daily-call/                 # <transcript tool> -> per person per day
|   |-- README.md
|   |-- _TEMPLATE.md
|   \-- YYYY/MM/<person-slug>_YYYY-MM-DD.md
\-- <new-source>/               # only when a real new on-record source emerges
    \-- README.md
```

Required for any new subfolder:
1. A `README.md` documenting filename pattern, cardinality, output format, and any team filtering rules.
2. A row in the table at the top of this README.
3. Wiki link rules added to [`anatomy.md`](../canon/anatomy.md) -> Frontmatter wiki-link relations — forward + reverse pairs for the new file pattern.

Don't pre-create empty subfolders.

## What does NOT go here

- The **raw artifact** (the actual transcript / email / PR / deal). Those stay in their system of record — see [`anatomy.md`](../canon/anatomy.md) -> External sources of truth.
- **Stable context** (rules, profiles, goals, conventions, people).

## Governance

Lightweight and append-only. Anyone produces an entry (via whichever AI skill they use); the lead reviews PRs. Don't hand-edit existing files — if a recap is wrong, re-run the producing skill or write a new MD that supersedes the old one.
