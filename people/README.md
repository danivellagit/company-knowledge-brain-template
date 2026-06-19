# people/

One MD per **person**, internal or external. There is **one folder**, **one schema**. Internal vs external is derived from the `company:` wiki-link in the frontmatter:

- **Internal team** -> `company: [[<company>]]` (your own company, the one MD with `is_self: true`). Human-curated. Carries `team:`, `reports_to:`, `direct_reports:`, and the `## Scope` / `## Anti-scope` / `## Decision authority` body sections.
- **External** (customer contact, prospect contact, partner contact, vendor, advisor) -> `company:` points to any other `companies/<slug>.md`. Created on first mention if the CRM (e.g. HubSpot, Salesforce, Pipedrive) has a record. Carries `crm_url:`, a short `## About`, and a `## Company` body link. The internal-only fields (`team:`, `reports_to:`, etc.) are omitted.

The single source of truth for **who is who**, **how they show up in source systems** (meeting-transcript speakers, chat handles, email), **what they own**, and **what they do NOT own**.

Readable by every agent that reads this repo. Anything genuinely confidential (compensation, performance notes, succession plans) does **not** go here.

See [`_TEMPLATE.md`](_TEMPLATE.md) for the copy-paste schema with placeholders.

---

## Body asymmetry: internal vs external

**Same folder. Same wiki-link target. Different body weight.** The `people/` folder is unified by design (see *Why one folder* below), but the body of each MD scales with where the source of truth lives:

| | Internal team member | External person |
|---|---|---|
| `company:` | `[[<company>]]` (the home company) | any other `companies/<slug>.md` |
| Source of truth for personal context | **the repo** | **the CRM** (gated by `crm_url`) |
| Body sections | `## Company`, `## Team`, `## Scope`, `## Anti-scope`, `## Decision authority`, optionally `## Operating style`, `## Voice`, `## How I write`, `## Preferences`, `## Context` | `## About` (1-2 sentences), `## Company` wiki-link. **Nothing else.** |
| Creation | human-curated, never seeded from a mention | create-on-first-mention if the CRM has a record |

The principle: **the body reflects where the truth lives.** The CRM is the source of truth for the customer graph. Every external `people/<slug>.md` is a stable pointer back to a CRM contact, and its body stays light because the rich context lives there and goes stale here within weeks. The repo is the source of truth for internal team members: there is no CRM record for a founder's voice, a CEO's operating style, an engineer's anti-scope. So those bodies can grow as needed to carry that context.

A founder writing `## Voice` (how I sound when I write) inside `people/<founder-slug>.md` is correct. The same section on an external `people/<slug>.md` is **wrong**: that content belongs on the CRM record (or nowhere, if the CRM doesn't model it).

See [`canon/operations.md`](../canon/operations.md) -> "In-repo external entities: create-on-first-mention" for the procedure, and [`canon/anatomy.md`](../canon/anatomy.md) -> "Body asymmetry" for the same principle applied to `companies/`.

---

## Why one folder

Splitting internal team members into `people/` and external persons into a separate `contacts/` folder buys nothing: same shape, two folders, two READMEs, two schemas to maintain. Queries like "all CEOs we know" would have to union two folders, and `signals/` would have to carry both `related_to_people` and `related_to_contacts`.

One folder, one slice. The `company:` wiki-link does all the work the folder split used to. Cross-cutting queries are trivial: "all Heads of Sales" -> traverse `job-titles/<title>.md -> people:`. "Everyone at `<customer>`" -> traverse `companies/<customer>.md -> people:`. "All engineers on the home team" -> filter `people/` by `company: [[<company>]]` AND `team: [[engineering]]`.

---

## When to create an MD here

**Internal team members:** human-curated. A founder or manager creates the MD on hire (or backfills it for existing staff). **Never seeded from a body mention.**

**External persons:** **on first mention, if the CRM has a matching record.** Whenever an agent or human writes a file (signal, article, recap) and the body mentions an external person:

1. Match the name against existing `people/<slug>.md` aliases. If a match exists -> wiki-link it. Done.
2. If no MD exists -> **query the CRM** (e.g. HubSpot, Salesforce, Pipedrive, via MCP) for a contact matching the name / email / alias.
3. If the CRM returns a record -> **create** `people/<firstname>-<lastname>.md` with `company: [[<their-company>]]` + `crm_url:` populated, then wiki-link the mention.
4. If the CRM has no matching record -> leave the mention as **plain text**. Do not create an empty MD.

**Never seed an external MD without `crm_url`.** A half-blind MD (no CRM link) is worse than no MD: it pretends the entity is in the system without giving any traversal path back to live state. The `crm_url` is the entity's birth certificate.

---

## File convention

One file per person: `<firstname>-<lastname>.md`, lowercase, hyphenated. Examples: `jane-doe.md`, `john-smith.md`.

### Front matter: required for everyone

```yaml
---
name: Jane Doe                       # canonical spelling
short: Jane                          # optional, how they're addressed day-to-day
aliases:                             # every way they appear in source systems, never leave empty
  - Jane
  - jane.doe@<their-domain>
linkedin:                            # personal LinkedIn URL, saved-once stable ID
company:                             # wiki link, required for everyone
  - "[[<company>]]"                  # internal -> the home company; external -> their company
job_title:                           # wiki link, canonical title from job-titles/
  - "[[<title>]]"
---
```

### Front matter: internal-only (when `company: [[<company>]]`, the home company)

```yaml
founder: true                        # only if the person is a founder
reports_to:                          # wiki link to their manager (omit for top-level)
  - "[[<manager-slug>]]"
direct_reports:                      # manual reverse of reports_to, only if they have any
  - "[[<report-slug>]]"
start_date: YYYY-MM-DD
timezone: <timezone>                 # e.g. Europe/Rome
team:                                # wiki-link list, the team(s) this person belongs to
  - "[[<team>]]"
```

### Front matter: external-only (when `company:` is anything other than the home company)

```yaml
crm_url:                             # paste the CRM contact record URL, MANDATORY
twitter:                             # optional, only if they have a public X/Twitter handle
website:                             # optional, only if they have a personal site / blog
```

### Body

**Internal team member:**

```markdown
# Jane Doe

## Company

[<COMPANY>](../companies/<company>.md)

## Team
Wiki link(s) to the team folder(s). Single-team or cross-team (typical for leadership).

## Scope, what I own
Bulleted list of concrete responsibilities. Verbs, not titles.

## Anti-scope, what I do NOT own
Bulleted list. Every item is a wrong assumption an agent might otherwise make.

## Decision authority
What this person signs off on unilaterally vs. what needs another signer.

## Operating style (optional)
Things that help an agent draft communication in this person's voice.

## How to work with me (optional)
Self-authored note on how this person collaborates best: async style, debate posture, what energizes vs. drains them, how to unblock them.

## Context (optional)
Background, prior roles, domain expertise.
```

**External person:**

```markdown
# John Smith

## About
1-2 sentences: who they are, their role, optionally relevant background. Stable facts.

## Company
[<their-company>](../companies/<their-company>.md)
```

Do **not** add `## Notes`, `## Context`, deal involvement, communication preferences, history, or any prose about the relationship on external MDs. That lives in the CRM and goes stale here in weeks.

---

## How agents use this

- **Resolve speakers in transcripts.** Match the alias against all files in this folder before assigning ownership. Disambiguate internal vs external via `company:`.
- **Validate action-item ownership.** Before writing "Owner: X" for an internal person, check X's `## Anti-scope`. If the action falls under anti-scope, flag the mismatch rather than silently assigning.
- **Route escalation.** Use `reports_to` + `## Decision authority` on internal MDs.
- **Cold-brief an external person.** Open their MD -> follow `company:` to the company brief -> pivot to `crm_url` for live state (deal involvement, last touch).
- **Cross-person queries.** Traverse `job-titles/<title>.md -> people:` for everyone with that title. Traverse `companies/<X>.md -> people:` for everyone associated with that company.

---

## What does NOT go here

- Compensation. Performance reviews. Succession planning. Anything a founder would not want every team agent to read.
- People who are not in the CRM (for external). If the CRM has no record, the mention stays plain text. Do not create an MD.
- **Long prose** about the relationship, deal involvement, history (for external). All of that is the CRM. If you find yourself writing it, stop.

---

## Wiki-link relations

| Forward (on `people/<x>.md`) | Reverse (on ...) |
|---|---|
| `company: [[<company>]]` | `companies/<company>.md -> people: [[<person>]]` |
| `job_title: [[<title>]]` | `job-titles/<title>.md -> people: [[<person>]]` |
| `team: [[<team>]]` (internal only) | `team/<team>/<team>.md -> people_involved: [[<person>]]` |
| `reports_to: [[<manager>]]` (internal only) | `people/<manager>.md -> direct_reports: [[<report>]]` |

Maintained as double-writes per [`canon/anatomy.md`](../canon/anatomy.md) -> Frontmatter wiki-link relations (forward + reverse pairs, the table) and [`canon/operations.md`](../canon/operations.md) -> Wiki-link relations, always double-write (the procedure).

---

## Adding or updating a person

1. Internal: create / edit `people/<firstname>-<lastname>.md` with the required + internal-only frontmatter, plus the body sections.
2. External: query the CRM first. If no record exists, stop, leave the mention as plain text. If a record exists, create the MD with required + external-only frontmatter (including `crm_url`).
3. In either case, update the matching reverse on partner files (`companies/<company>.md -> people:`, `job-titles/<title>.md -> people:`, `team/<team>/<team>.md -> people_involved:`, manager's `direct_reports:`) in the **same write**.
4. Commit. For non-`canon/**` files this goes direct to `main` per [`canon/operations.md`](../canon/operations.md) -> Changes, push to main, with one exception.

Keep it honest. A stale `## Scope` list is worse than no list: it actively misleads agents.
