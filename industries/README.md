# industries/

One MD per **vertical / market segment** `<COMPANY>` sells into or partners operate in. Stable wiki-link target so:
- `companies/<X>.md` declares `industry: [[<slug>]]` — what vertical the company plays in.
- Sales agents query "companies in `<vertical>`" in one hop.

---

## When to create an MD

When `<COMPANY>` starts selling into, or actively prospecting, a vertical. Don't pre-seed verticals "for completeness" — wait for the first real prospect or customer in that segment.

---

## File convention

`<slug>.md`, lowercase, hyphenated. Examples: `<vertical-a>.md`, `<vertical-b>.md`, `<vertical-c>.md`.

### Front matter

```yaml
---
name: <Vertical name>
slug: <vertical-slug>
aliases: []                      # every other way this vertical is named — keep rich, empty is a bug
companies: []                    # manual reverse of companies.industry — companies in this vertical
                                 #   - "[[<company-slug>]]"
---
```

`aliases` should be **rich**: list every synonym, abbreviation, or alternate phrasing the vertical goes by, so the entity-scan fuzzy-matches an incoming mention onto this MD instead of creating a duplicate. An empty `aliases` on a real entity is a bug.

`companies` is a **manually-maintained reverse relation**. A local Markdown editor's automatic inverse only fires on its default relation names; for semantic field names like `industry` we declare both sides of the edge explicitly. When a new company adopts this vertical, update this file too — the forward declaration on the company alone isn't enough to surface as a dedicated section on the industry page. This is the reverse-edge double-write: write `companies/<X>.md → industry: [[<slug>]]` and `industries/<slug>.md → companies: [[<X>]]` in the same change.

### Body

```markdown
# <Vertical name>

## About

1-2 sentences: scope of the vertical in `<COMPANY>`'s frame, plus geographic context if relevant.
```

Keep it tight. Live company-specific intel goes in the company MD and the signals, not here.

---

## Inverse views

The `companies:` field is the manually-maintained reverse of `companies/<X>.md → industry: [[<slug>]]`. A local Markdown editor renders it as a dedicated "Companies" section on the industry page. Plus the editor's generic **Referenced by** view surfaces any signal tagged for this industry over time.
