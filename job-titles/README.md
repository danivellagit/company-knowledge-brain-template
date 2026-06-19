# job-titles/

One MD per **job title / role** held by a person — internal or external. Stable wiki-link target so:
- `people/<X>.md` declares `job_title: [[<slug>]]` — the role(s) the person holds.
- Sales / marketing agents query "all CEOs we talk to" or "every IT head in `<vertical>`" in one hop.

Applies to **everyone in [`people/`](../people/)** — internal `<COMPANY>` team and external persons alike. Internal team members carry `job_title:` alongside their `team:` membership; external persons typically only carry `job_title:`.

---

## When to create an MD

When a new person is created and their canonical title doesn't already exist as a slug. Don't pre-seed speculative titles — wait for the first real person who holds it.

## Slug canonicalization

- **One slug per concept.** "VP Engineering", "VP, Engineering", "VP of Engineering" all collapse to `vp-engineering`. Variants do not get their own MDs — list them under `aliases:` instead.
- **Title family > exact phrasing.** "Head of Ecommerce", "Head of Ecommerce & CRM", "IT Head of Ecommerce" all collapse to `head-of-ecommerce`. The expanded responsibility lives in the person's `## About` line (or `## Scope` for internal), not in the slug.
- **Disambiguate when both senses are real.** "CDO" splits into `cdo-digital.md` (Chief Digital Officer) and `cdo-data.md` (Chief Data Officer) only when both appear in your people set. Until then, pick the canonical for your world and add the other when a real person appears.

A person with two hats (founder + CTO) lists both: `job_title: [[cto]], [[founder]]`.

A person whose role is unknown leaves the field empty rather than guessing — and the `## About` body should say so explicitly.

---

## File convention

`<slug>.md`, lowercase, hyphenated. Examples: `ceo.md`, `cto.md`, `head-of-ecommerce.md`, `consultant.md`.

### Front matter

```yaml
---
name: <Title name>
slug: <title-slug>
aliases: []                          # every phrasing/abbreviation that collapses to this title — keep rich, empty is a bug
people: []                           # manual reverse of people.job_title — persons holding this title
                                     #   - "[[<person-slug>]]"
---
```

`aliases` should be **rich**: every phrasing, abbreviation, or localized variant that collapses to this canonical title, so the entity-scan matches an incoming title onto this MD rather than creating a duplicate. Empty `aliases` on a real entity is a bug.

### Body

```markdown
# <Title name>

## About

One line: what this title typically covers in `<COMPANY>`'s context. The per-person detail sits on each person's `## About` line.
```

That's it. No "typical responsibilities", no "skills to look for" — the title MD is a tag, not a job description.

---

## Forward + reverse pair

- `people/<x>.md → job_title: [[<title>]]` (forward — appears on the person page as "Job title → CEO")
- `job-titles/<x>.md → people: [[<person>]]` (manual reverse — appears on the job-title page as "People → …")

When you set or change a person's `job_title`, update the matching reverse on each job-title MD in the same write. This is the reverse-edge double-write. See [`canon/anatomy.md`](../canon/anatomy.md) → Frontmatter wiki-link relations — forward + reverse pairs.

Job-titles carry no product edge. Buyer-persona ↔ product fit lives in your docs system, not in repo frontmatter.

---

## How agents use this

- **Sales prep** — "who do we know in IT?" → open the relevant job-title MD, traverse `people:` to the persons.
- **Cross-vertical query** — "all Heads of Ecommerce in `<vertical>`" → intersect `job-titles/head-of-ecommerce.md → people:` with `industries/<vertical>.md → companies:` via the person's `company` link.
- **Cold-brief a person** — open the person MD, click `job_title:` to see what canonical role they hold and other peers at that level.
