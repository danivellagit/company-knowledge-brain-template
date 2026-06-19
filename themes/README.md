# themes/

One MD per **topic / theme** `<COMPANY>` writes about, follows externally, or wants its agents to reason on. Stable wiki-link target so:

- `articles/<slug>.md` declares `themes: [[<theme>]]` — the topics each external article touches.
- Future content, signals, and people expertise can layer onto the same nodes.
- An agent asking "what have we said about `<topic>`?" follows `themes/<topic>.md → articles:` and gets the full set in one hop.

---

## When to create a theme

When a topic appears in the body or `themes:` of an article and no MD exists yet. Pattern: as you save an article, scan its themes; for any not yet in `themes/`, create the MD with a 1-2 sentence definition. A topic that recurs across signals or articles (≥2 mentions or ≥2 sources) is a create candidate.

Don't pre-seed speculative themes — wait for the first article or signal that needs the wiki-link target.

---

## File convention

`<slug>.md`, lowercase, hyphenated. Examples: `<theme-a>.md`, `<theme-b>.md`, `<theme-c>.md`.

### Front matter

```yaml
---
name: <Theme name>
slug: <theme-slug>
aliases: []              # every other way this theme is named — keep rich, empty is a bug
articles: []             # manual reverse of articles.themes — articles tagged with this theme
                         #   - "[[<article-slug>]]"
signals: []              # manual reverse of signals.related_to_themes — signals that explicitly discuss this theme
                         #   - "[[<signal-slug>]]"
---
```

`aliases` should be **rich**: synonyms and alternate phrasings for the topic, so the entity-scan matches an incoming mention onto this MD rather than creating a duplicate. Empty `aliases` on a real entity is a bug.

### Body

```markdown
# <Theme name>

## About

1-2 sentence **stable definition**: what this theme means in `<COMPANY>`'s frame. **Not** the status of any project / initiative tagged with this theme — operational state lives in `signals/` or articles. Not a knowledge dump either — the definition exists so an agent reading the theme MD knows what scope it covers; the "deep" content sits on each article's URL (a single fetch away). See [`canon/operations.md`](../canon/operations.md) → "Entity body discipline — stable facts + staleness check".
```

That's the whole body. Live opinions on the theme live in [`canon/company-rules.md`](../canon/company-rules.md), [`canon/profile.md`](../canon/profile.md), or the relevant team rules — not here.

---

## Forward + reverse pairs

Themes participate in two bidirectional relations — always double-write both sides in the same change:

- `articles/<x>.md → themes: [[<theme>]]` ↔ `themes/<x>.md → articles: [[<article>]]`
- `signals/<source>/<period>/<x>.md → related_to_themes: [[<theme>]]` ↔ `themes/<x>.md → signals: [[<signal>]]`

The signals pair is an explicit exception to the general "signals never get a reverse" rule (see [`canon/anatomy.md`](../canon/anatomy.md) → Frontmatter wiki-link relations → *Exception to the exception — themes*). Justification: themes are a small bounded taxonomy and a per-theme `signals:` list stays linear, while a per-theme query ("show me every meeting that touched `<topic>`") is high-value for agents producing recaps, briefs, content.

When you add or remove a theme from a signal's `related_to_themes:` or from an article's `themes:`, update the matching reverse on each theme file in the same write.

---

## How agents use this

- **Content agent** — given a draft topic, traverse `themes/<topic>.md → articles:` to see what `<COMPANY>` has already engaged with externally. Fetch each article's `url:` for the actual argument.
- **Sales agent** — when a prospect raises a topic, surface relevant article URLs as proof points.
- **Anyone reasoning on a theme** — start from the theme page, then drill into the articles that shaped the position.
