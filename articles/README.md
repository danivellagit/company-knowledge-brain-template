# articles/

One MD per **external article** (essay, post, paper, talk transcript) that **explicitly changed or reinforced a POV** of yours, the team's, or `<COMPANY>`'s. Pointer + your TL;DR + wiki-links to entities the article touches. Nothing else.

**Articles are citation-only, never canon.** An article MD is a frozen pointer to someone else's writing plus your reaction to it; it carries no prescriptive authority. Canon lives in [`canon/`](../canon/). An article is something to cite, never something an agent obeys.

The article itself lives on the web. This folder is **a graph of pointers + reactions**, not a clippings library.

---

## Why this exists

So that a skill (or a human) writing on a topic can:
- open `themes/<topic>.md` → read its manually-maintained `articles:` list (one-hop lookup), then drill into each article's TL;DR;
- follow wiki links from `signals/`, `canon/profile.md`, team rules → land on the article MD and pivot to the live URL via `WebFetch` for the full content.

The MD is a stable hook in the wiki graph + a frozen-in-time TL;DR of why you saved it. The fresh content is always one fetch away.

---

## When to create an MD here

Only when the article **changed or reinforced a POV** that has a home in this repo. Strict bar:

- It made you update something you believe about strategy / product / GTM / ops.
- It introduced a frame / concept you'll reuse in talks, decisions, docs.
- It validated a thesis you'd already started writing somewhere in this repo.

If it's "interesting but not actionable", keep it in your personal bookmarks — **not here**. The folder must stay small and high-signal. If you're unsure, default to **not** creating one.

---

## File convention

One file per article: `<source-prefix>-<slug>.md`, lowercase, kebab-case, no spaces. Source prefix is a short tag for the publisher (`<publisher-a>`, `<publisher-b>`, `personal-blog`, etc.) so the folder sorts by source. Slug is a 3-6 word kebab of the title.

Examples:
- `<publisher-a>-<short-title-slug>.md`
- `<publisher-b>-<short-title-slug>.md`

### Front matter (mandatory)

```yaml
---
title: "<Article title>"
url: https://<article-url>
author: <name(s)>                     # author(s) of the article
published: 2026-01-15                  # YYYY-MM-DD if known, YYYY-MM otherwise
added: 2026-01-20                      # the date this MD was created
themes: []                             # wiki-link list — resolves to themes/<slug>.md
                                       #   - "[[<theme-slug>]]"
---
```

`themes` is the navigable axis. Each value is a wiki-link to a file in [`themes/`](../themes/) — same pattern as `industries/` / `products/`. If a theme doesn't have an MD yet, create one (1-2 sentence definition + the manual reverse `articles: [[<this-article>]]`); see [`themes/README.md`](../themes/) for the rule. The forward `articles → themes` and the reverse `themes → articles` are double-written in the same change.

### Body

```md
# <title>

**TL;DR**: 2-3 sentences. What's new in the argument, what it confirms of your prior view, why you're keeping it.

**Mentions**: [companies/<slug>.md](../companies/<slug>.md), [people/<slug>.md](../people/<slug>.md)
```

That's the whole body. Do **not** rewrite the article. Do **not** paste extended quotes. Do **not** maintain a running "thoughts as I reread" log. If you find yourself writing more than the TL;DR + Mentions, you're writing the article — go put your version where it belongs (`canon/profile.md`, a team rules doc, a talk script).

`Mentions` includes wiki links only when an MD already exists in `companies/` or `people/`. **Never seed an MD just so an article can link to it.**

---

## How agents use this

- **Find articles informing a topic.** Open `themes/<topic>.md` — its `articles:` field lists every article tagged with that theme. Read each TL;DR. Fetch the URL via `WebFetch` only if you need the actual content.
- **Follow incoming wiki links.** When a context file or signal cites `articles/<slug>.md`, traverse the link and read the TL;DR before forming an opinion.
- **Don't paraphrase the TL;DR as the article's argument.** The TL;DR is the human's reaction at write-time. For the article's actual argument, fetch the URL.
- **Never treat an article as canon.** It's a citation. If a position needs to be authoritative, it belongs in `canon/`, with the article cited as evidence.

---

## Wiki links

| Direction | Where |
|---|---|
| `articles/<slug>.md` → `companies/<slug>.md` / `people/<slug>.md` | When the article explicitly mentions them **and an MD exists**. Omit otherwise. |
| Any context file (`signals/`, `canon/profile.md`, team rules, …) → `articles/<slug>.md` | When citing the article as input to a position, frame, or decision. |

See [`canon/anatomy.md`](../canon/anatomy.md) → Frontmatter wiki-link relations — forward + reverse pairs for the full table.

---

## What does NOT go here

- Articles that are "just interesting." Bookmarks tool, not this repo.
- Articles about your own company / product. Those facts go in `canon/profile.md` / `canon/`.
- Copies of the article content. The web has the content; you have the URL + TL;DR.
- Drafts of your own thinking. Put those where they belong (team rules, `canon/profile.md`, a talk script).
- Status fields (`read` / `to read` / `starred`). The folder is "read and decided to keep" by construction.

---

## Lifecycle

**Quarterly review.** Skim the folder. For each MD: does the TL;DR still describe a position you hold? If yes, keep. If no — either rewrite the TL;DR with the updated view (and bump `added`), or delete the file.

**Linkrot.** If the URL dies, the MD is still useful — the TL;DR captured the insight. Leave it; mark `url:` as `[dead]` in front matter if you want signal.

**No archiving.** Either an article still informs something — keep — or it doesn't — delete. No `articles/archive/` subfolder.

---

## Governance

Free. Anyone adds, anyone edits. No gate. A light wiki-link layer, not a curated knowledge base. Push direct to `main` — articles are not under `canon/`.
