# skills/

One folder per **skill** the team uses. A skill is a reusable Claude Code-style prompt + workflow, with all the supporting sub-tools and filters bundled inside its own folder.

Three kinds of skill coexist:

- **Team-shared** — owned by one team, used by anyone on that team. Same skill folder, behavior parametrized by the invoker's identity. Naming: `<team-slug>-<purpose>` (e.g. `leadership-meeting-recap`). Filters / scope come from the team's `team/<team>/<team>.md` and from the company canon — never per-person preferences baked into the file. Anyone on the team can edit; the change applies to everyone the next time they invoke it.
- **Personal / owner-bound** — bound to one specific person, with that person's filters, voice, vocabulary, exclusions. Naming: `<owner-slug>-<purpose>` (e.g. `<person-slug>-blog-writer`). The owner edits everything inside their skill's folder freely; nobody else gates them. To diverge from a team-shared skill, a person forks it into a personal one and customizes.
- **Shared sub-tool** — reusable building block invoked by a parent skill (e.g. a `<transcript tool>` fetch step, a per-call summarizer). Naming: role-based. Lives **inside** the parent skill's folder by default; only promoted to top-level when proven reuse across multiple parents emerges. Don't pre-share — let the duplication speak first.

When a skill needs sub-tools, those sub-tools live **inside the skill's folder**. Each parent skill has full control of its pipeline; nothing crosses skill boundaries by default. Anyone, regardless of team, can **invoke** any skill — the team affiliation governs ownership and editing, not invocation.

---

## Skills live only in this repo — for everyone

Every skill anyone uses lives **here, inside the `<company>` repo**, under `skills/<slug>/`. This holds for **everyone** who uses the repo, and for **every** kind of skill — team-shared and personal alike, experiments included. Skills are **never** placed in `.claude/skills/` (project-local) or `~/.claude/skills/` (user-level); those locations are off-limits. If it is a reusable agent behavior, it goes in this folder so the whole team gets it on the next pull.

The only thing that may live outside this folder is a skill that belongs to a **separately-deployed AI agent with its own repo** (not invoked from here). When unsure: it goes **here**, named clearly.

---

## File convention

```
skills/
|-- README.md
|-- _TEMPLATE/
|   \-- SKILL.md                        # schema skeleton to copy when adding a skill
\-- <skill-slug>/
    |-- SKILL.md                        # frontmatter + the prompt body (the user-facing skill)
    |-- <helper-slug>/                  # bundled sub-tool — same shape, but not user-facing
    |   \-- SKILL.md
    \-- ...                             # other supporting files (templates, filter lists, etc.)
```

`<skill-slug>` is lowercase, hyphenated.

- **Team-shared skill**: `<team-slug>-<purpose>` — e.g. `leadership-meeting-recap`, `sales-deal-summary`.
- **Personal / owner-bound skill**: `<owner-slug>-<purpose>` — e.g. `<person-slug>-blog-writer`.
- **Shared top-level skill** (rare; emerges from proven reuse): role-based — e.g. a generic transcript-fetch helper used by multiple recap skills.

### `SKILL.md` frontmatter

Standard Claude Code skill schema, plus **two** mandatory wiki-link relations — `owner:` and `creator:`:

```yaml
---
name: <skill-slug>
description: One sentence — what this skill does and when to invoke it. This is what Claude Code matches against to decide whether to use the skill.
owner:                    # MANDATORY for user-facing skills. Wiki-link list — who the skill SERVES (team or person whose filters / preferences / exclusions it carries).
  - "[[<team-slug>]]"     # team-shared: one or more of leadership / sales / marketing / product / engineering / customer-success
                          # personal: a single [[<person-slug>]] pointing to people/<slug>.md
creator:                  # MANDATORY for user-facing skills. Wiki-link list — who WROTE the skill (the human author, distinct from owner).
  - "[[<person-slug>]]"
teams: []                 # optional — extra teams that routinely invoke the skill, beyond owner.
---
```

Field usage by skill kind:

| Field | Team-shared | Personal | Sub-tool |
|---|---|---|---|
| `owner` | **required** — `[[<team>]]` wiki-link(s) | **required** — `[[<person-slug>]]` | **omit** (inherits from parent) |
| `creator` | **required** — `[[<person-slug>]]` who wrote it | **required** — typically same as `owner` | **omit** (inherits from parent) |

**`owner` and `creator` are different fields, both mandatory** on every user-facing skill (`skills/<slug>/SKILL.md`).

- `owner` answers *who does this skill serve?* — the team or person whose filters and preferences the skill carries. A local Markdown editor (e.g. Obsidian, Tolaria) renders it in the Properties panel ("Owner -> Leadership" clickable). Agents use it to filter "which skills are relevant to me / to team X". The team or person page shows the skill in its "Referenced by" view.
- `creator` answers *who wrote it?* — the human who authored the skill, even if the skill serves a different team. The next reader who hits an ambiguity in the skill body knows who to ping. On a team-shared skill, `creator` is one specific person; on a personal skill, `creator` is usually the same as `owner` (but populate it anyway — the schema stays uniform).

A team-shared skill that serves more than one team can list multiple owners — `owner: ["[[leadership]]", "[[product]]"]`. A skill serving every team unconditionally is modelled as a shared sub-tool invoked by parent skills rather than `owner: [[all]]`.

Declaring neither is **not allowed** on user-facing skills — both fields force explicit decisions rather than silent defaults.

In the body, restate ownership and authorship with wiki links so any reader sees who to ping:

```markdown
# Personal / owner-bound skill
**Owner**: [<full name>](../../people/<person-slug>.md)
**Creator**: [<full name>](../../people/<person-slug>.md)

# Team-shared skill
**Owned by**: [team/<team-slug>/](../../team/<team-slug>/) (team-shared skill — any team member invokes it).
**Created by**: [<full name>](../../people/<person-slug>.md)
```

The body of `SKILL.md` is the system prompt: how it works, what it expects as input, what it produces, where it writes (if it writes), what it must never do.

- **Personal skills** are where the owner encodes their filtering preferences inline — that's the whole point of being owner-bound.
- **Team-shared skills** keep filters at the team / company level only (read from `team/<team>/<team>.md` and the company canon at run time). Per-person preferences do **not** belong in a team-shared file. If someone needs personal filters, they fork into a personal skill.

### Bundled sub-tools

When a parent skill (team-shared or personal) has multiple internal steps worth isolating (a fetch step, a summarizer, a formatter), bundle them as sub-folders inside the skill:

```
skills/<skill-slug>/
|-- SKILL.md
|-- <fetch-helper>/
|   \-- SKILL.md
\-- <summarizer-helper>/
    \-- SKILL.md
```

Sub-tools follow the same `SKILL.md` shape but **omit both `owner` and `creator`** — they inherit both from the parent. They are not user-facing; the parent skill's `SKILL.md` invokes them.

### What goes inside is up to the owner

Once the frontmatter is correct, the rest of `skills/<slug>/` is the owner's playground:

- Inline the entire prompt in `SKILL.md`, or split into supporting files / sub-tools.
- Add templates, examples, glossaries — whatever helps the skill do its job.

The repo does not enforce a body format. The frontmatter is the contract. A copy-ready skeleton lives at [`_TEMPLATE/SKILL.md`](_TEMPLATE/SKILL.md).

---

## Output format conventions

When a skill writes back to the repo (e.g. a recap into `signals/daily-call/`), the **output format** lives in the destination's README, not in the skill. Skills **link** to the output-format spec; they don't duplicate it.

Example: a recap skill says "follow the format in [`signals/daily-call/README.md`](../signals/daily-call/) -> Per-call section". If the README format changes, the skills automatically inherit it.

If you find yourself copy-pasting an output format into a skill, stop and link instead.

---

## Wiki links

Every user-facing `skills/<slug>/SKILL.md` must wiki-link to its ownership AND authorship targets:

- **Personal skill** -> `owner: [[<person-slug>]]` and `creator: [[<person-slug>]]` (typically the same person).
- **Team-shared skill** -> `owner: [[<team-slug>]]` and `creator: [[<person-slug>]]` (the human who wrote it).

The reverse relations are mandatory too (double-write rule):

- `owner: [[<team>]]` -> `team/<team>/<team>.md -> skills: [[<skill>]]`.
- `owner: [[<person>]]` -> `people/<person-slug>.md -> skills: [[<skill>]]`.
- `creator: [[<person>]]` -> `people/<person-slug>.md -> skills_created: [[<skill>]]`.

See [`anatomy.md`](../canon/anatomy.md) -> Frontmatter wiki-link relations — forward + reverse pairs for the full table. Bundled sub-tool `SKILL.md` files have no mandatory outbound wiki links (they inherit ownership and authorship from the parent).

---

## Adding a skill

1. Pick a slug — `<team-slug>-<purpose>` for team-shared, `<owner-slug>-<purpose>` for personal, role-based for the rare top-level shared sub-tool.
2. Copy [`_TEMPLATE/SKILL.md`](_TEMPLATE/SKILL.md) to `skills/<slug>/SKILL.md` and fill the frontmatter (mandatory `owner:` wiki-link and mandatory `creator:` wiki-link) and the prompt body.
3. Add the reverse wiki-links to the partner files: `team/<team>/<team>.md -> skills:` (or `people/<owner>.md -> skills:` for personal skills) AND `people/<creator>.md -> skills_created:`. Both in the same write.
4. If the skill needs internal steps as sub-tools, add them as `skills/<slug>/<helper>/SKILL.md` (omit `owner` and `creator` on those — they inherit from parent).
5. If the skill writes to the repo, link to the destination's output-format spec instead of inlining it.
6. Commit with `skills: add <slug>`.

`skills/` is not gated for review — any team member can land changes. For team-shared skills, this means any team member can edit; respect the "no per-person filters in team-shared skills" rule above.
