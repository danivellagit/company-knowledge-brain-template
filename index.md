# Index — ai-native-company

> Catalog of everything in this repo. Humans use it as a table of contents; AI agents enter via [`CLAUDE.md`](CLAUDE.md) (the boot sequence) and consult this for navigation.

---

## Reading model — open by default

**Ownership and edits: confined. Reads: open.**

Every team owns a slice of this repo (its rules and team index) and only the team lead — or the CEO — can change those slices. The canon ([`canon/`](canon/)) goes through a PR by hard rule. Cross-cutting folders ([`signals/`](signals/), [`people/`](people/), [`companies/`](companies/), [`articles/`](articles/), [`skills/`](skills/)) and the domain layer ([`industries/`](industries/), [`systems/`](systems/), [`products/`](products/), [`themes/`](themes/), [`relationship-types/`](relationship-types/), [`job-titles/`](job-titles/)) live at root and link teams via wiki links rather than nest under any single one. **But every reader, regardless of team, reads any slice it needs.** A sales skill preparing a deck reads [`team/marketing/`](team/) for brand voice, [`team/product/`](team/) for what's shipping, [`signals/`](signals/) for what was discussed today, and [`companies/<account>.md`](companies/) for the strategic posture — independent of which AI tool the salesperson uses.

There is **no per-team filtering on reads**. The whole repo is the context. Ownership protects writes, not reads.

When two files disagree, [`company-rules.md`](canon/company-rules.md) → "Order of truth" defines precedence: **canon** (company-rules → operations → anatomy → profile → team → skill) overrides **operational state** (signals), which overrides **external inputs** ([`articles/`](articles/) — citation-only, never canon).

If knowledge belongs to one team but is useful to others, it lives under that team's folder anyway — and is read freely by everyone. Don't duplicate. Don't gate.

---

## Canon — `canon/` (PR-only)

Every modification to a file in [`canon/`](canon/) goes through a pull request. See [`canon/README.md`](canon/README.md) and [`canon/operations.md`](canon/operations.md) → Changes — push to main, with one exception.

| File | Purpose |
|---|---|
| [canon/company-rules.md](canon/company-rules.md) | The constitution: identity, positioning, voice, non-negotiable values, hard exclusions, order of truth, customer data rules. |
| [canon/operations.md](canon/operations.md) | How the agent operates: agent stance, write-time procedures, workflow, sync model, citations, source-evidence weight, task-routed team loading, entity-scan discipline, in-repo external entities, wiki-link double-write, entity body discipline. |
| [canon/anatomy.md](canon/anatomy.md) | How the graph is shaped: domain map, folder schemas, body asymmetry, glossary, external sources of truth, frontmatter wiki-link forward + reverse pairs table, direction rule. |
| [canon/profile.md](canon/profile.md) | Structured facts: name, markets, leadership, customers, integrations, mission, annual themes, goals, pillars, GTM posture, operating principles. |
| [canon/icp.md](canon/icp.md) | (Optional) Ideal Customer Profile. |
| [canon/competition.md](canon/competition.md) | (Optional) Competitive posture. |
| [canon/team/<team>.md](canon/team/) | Team-specific rules (per-team Voice / Scope / non-negotiables) — PR-gated as part of canon. One file per team. |

## Agent boot (root)

| File | Purpose |
|---|---|
| [CLAUDE.md](CLAUDE.md) / [AGENTS.md](AGENTS.md) | Boot sequence and order of truth for AI agents. Mirrored — edit one, copy to the other. |

## Entities

| Folder | One MD per | Read for |
|---|---|---|
| [people/](people/) | Any person — internal team member or external. Internal vs external is derived from `company:` (internal = `[[<company>]]`). | Scope and anti-scope (internal), speaker aliases, decision authority (internal), `crm_url` (external), `job_title`. Internal slice is human-curated. External slice is created on first mention if the CRM has a record. |
| [companies/](companies/) | Any organization — the home company (`companies/<company>.md` with `is_self: true`) and every external customer / prospect / partner / vendor / advisor / competitor. | `relationship`, `industry`, `uses_stack`, `people`, `crm_url`. Live state stays in the CRM. |

## Domain layer (curated as work happens)

| Folder | One MD per | Wiki-link target for |
|---|---|---|
| [industries/](industries/) | Vertical you sell into | `companies.industry` |
| [systems/](systems/) | External software platform customers run | `companies.uses_stack` |
| [products/](products/) | An AI agent / automation you build. Thin pointer to your docs system via `docs_url`. | No inbound frontmatter edge; fit / used-by relations live in your docs system |
| [themes/](themes/) | Topic / theme the company reasons about | `articles.themes` |
| [relationship-types/](relationship-types/) | Kind of commercial relationship (customer / prospect / partner / vendor / advisor / competitor) | `companies.relationship` |
| [job-titles/](job-titles/) | Canonical title held by a person — internal or external | `people.job_title` |

## Signals, articles, skills

| Folder | What it holds |
|---|---|
| [signals/](signals/) | Cross-cutting append-only on-record syntheses. Per-tracked-source subfolders are one MD per person per day: meeting-transcript recaps ([`signals/daily-call/`](signals/daily-call/)), email digests ([`signals/email/`](signals/email/)). Manual push ([`signals/manual/`](signals/manual/)) is one MD per topic per author per day. Output format lives in each subfolder's README. |
| [articles/](articles/) | One MD per external article that explicitly changed or reinforced a POV. Frontmatter (url, author, published, themes) + 2-3 sentence TL;DR. Pointers, not clippings. |
| [skills/](skills/) | Reusable agent behaviors. Team-shared (e.g. `<team>-meeting-recap`), personal owner-bound (e.g. `<owner>-blog-writer`), or shared sub-tools bundled inside a parent skill. |

## Teams

Team rules are PR-gated under [`canon/team/`](canon/team/). Team indexes stay under [`team/<team>/`](team/) (push-direct). A team's operational picture is read natively from the graph (signals filtered on `teams_impacted` + the entities they link), not stored as a file.

| Team (example set) | Folder | Rules (PR-only) |
|---|---|---|
| Leadership | [team/leadership/](team/leadership/) | [rules](canon/team/leadership.md) |
| Sales | [team/sales/](team/sales/) | [rules](canon/team/sales.md) |
| _your other teams_ | `team/<team>/` | `canon/team/<team>.md` |

Per-team daily-call recaps **do not** live under team folders. They are written cross-cutting at [`signals/daily-call/`](signals/daily-call/) and link the team(s) impacted; an agent assembling a team's picture filters them at read time.

---

## How a team uses this repo

1. Start with [profile.md](canon/profile.md) + [company-rules.md](canon/company-rules.md) to understand who we are and how we talk.
2. Read [operations.md](canon/operations.md) and [anatomy.md](canon/anatomy.md) to interpret inputs correctly (dates, language, citations, sync — in operations; glossary, external sources of truth, wiki-link relations — in anatomy).
3. For ownership and decision authority, consult [people/](people/).
4. To resolve external names in transcripts and read the strategic posture on a customer / partner cold, consult [people/](people/) (external = `company:` is not `[[<company>]]`) and [companies/](companies/) — pivot to the CRM for live state.
5. To pivot from a customer to its vertical or stack, follow the wiki-links in the company's frontmatter into [industries/](industries/), [systems/](systems/). For the agents a customer runs, the product catalogue lives in your docs system (the [products/](products/) MDs are pointers via `docs_url`).
6. For today's operational picture of your team, scan [`signals/`](signals/) filtered on `teams_impacted` and follow the entities those signals link.
7. For your team's filters and non-negotiables, read `canon/team/<your-team>.md`.
8. For what happened yesterday on tracked channels, scan the files under [`signals/<source>/YYYY/MM/`](signals/) dated yesterday — one per person per source. Each entry's `teams_impacted` frontmatter tells you whether it's relevant to your team.
9. To run your own daily-recap (or any other repeatable task), use your personal skill in [skills/](skills/).

---

## How to search

1. Start here — pick the file or folder that answers your question.
2. Cross-team context: scan [`signals/`](signals/) for other teams (by `teams_impacted`) — that's the whole point of this repo being single-source.
3. Use `ripgrep` (`rg "<term>"`) for free-text search when the index doesn't have it.
