# companies/

One MD per **company** that matters to your context. Two kinds live here:

- **The home company** (your own): exactly one MD, marked `is_self: true`. It is the anchor of the graph. Internal people wiki-link to it via `company: [[<company>]]`. Its body can be rich: positioning, voice anchor, what the company is.
- **External companies** that recur in your context: customers, prospects, partners, vendors, advisors' ventures, competitors. The MD's job is to be a **stable wiki-link target** carrying the canonical name, aliases, and a pointer to the CRM record. **Nothing else.**

Live state for external companies (deal stage, pipeline, owner, history, last touch, contacts list) lives in the **CRM**. An agent that needs that state queries the CRM via MCP.

External MDs are short on purpose. Don't pad them.

See [`_TEMPLATE.md`](_TEMPLATE.md) for the copy-paste schema with placeholders.

---

## Why this exists

So that a skill (or a human) can grep `"companies/<slug>.md"` across `signals/` and find every recap that touched that company, without the CRM needing to know about the repo and without the repo duplicating the CRM.

That's it. An external MD is a hook for wiki links, not a knowledge base. The home-company MD is the one exception: it is the canonical statement of who you are.

---

## When to create an MD here

**On first mention, if the CRM has a matching record.** Whenever an agent or human writes a file (signal, article, recap) and the body mentions an external company, the writer must:

1. Match the name against existing `companies/<slug>.md` aliases. If a match exists -> wiki-link it. Done.
2. If no MD exists -> **query the CRM** (e.g. HubSpot, Salesforce, Pipedrive) for a company matching the name / domain / alias.
3. If the CRM returns a record -> **create** `companies/<slug>.md` with `crm_url` populated, and wiki-link the mention.
4. If the CRM has no matching record -> leave the mention as **plain text**. Do not create an empty MD.

The CRM is the gating signal: if the CRM says they exist, they're in scope; if not, they're plain text until they show up in the CRM.

**Never seed an MD without `crm_url`** (competitor excepted, see below). A half-blind MD (no CRM link) is worse than no MD: it pretends the entity is in the system without giving any traversal path back to live state. The `crm_url` is the entity's birth certificate.

---

## File convention

One file per company: `<slug>.md`, lowercase, hyphenated, no spaces. Examples: `acme-corp.md`, `globex.md`.

### Front matter

```yaml
---
name: Acme Corp                       # canonical spelling
is_self: true                         # ONLY on the home company's MD. Omit for every external company.
aliases:                              # every spelling/handle that appears in transcripts, chats, emails
  - AcmeCorp
  - acme.com
linkedin:                             # company LinkedIn URL (https://www.linkedin.com/company/<slug>/)
crm_url:                              # paste the CRM company record URL (e.g. HubSpot, Salesforce, Pipedrive)
industry:                             # MANDATORY (optional for competitor), wiki-link to industries/<slug>.md
  - "[[<industry>]]"
relationship:                         # MANDATORY, wiki-link to relationship-types/<slug>.md
  - "[[<relationship-type>]]"         # customer | prospect | partner | vendor | advisor | competitor
uses_stack:                           # wiki-link list to systems/<slug>.md, external software the company runs (when known)
  - "[[<system>]]"
docs_url:                             # optional, link to the company's hub/page in your docs system (e.g. Notion, Confluence)
people:                               # wiki-link list, people MDs that belong to this company
  - "[[jane-doe]]"
  - "[[john-smith]]"
---
```

The `people` field is a wiki-link list to the people MDs that belong to this company. A local Markdown editor (e.g. Obsidian, Tolaria) renders it in the Properties panel as "People -> Jane Doe, John Smith" clickable. Each person's reciprocal `company: [[<this-company>]]` declares the inverse, so opening the person page shows the company under "Company" automatically. Leave the `people:` field empty if no person MDs exist for this company yet.

**`is_self`** is set on exactly one MD: the home company. It marks the anchor every internal person points to via `company:`. External companies never carry it.

**Mandatory wiki-link fields** (no TBD allowed). Every external company MD must carry them at create time:
- `industry: [[<industry>]]` links the company to its industry in [`industries/`](../industries/). If the existing industries don't cover the company, **create the industry MD first**, then link. **Optional for** `relationship: [[competitor]]`.
- `relationship: [[<type>]]` links to [`relationship-types/<slug>.md`](../relationship-types/). One of customer / prospect / partner / vendor / advisor / competitor.

**`crm_url`** is mandatory for external companies, with one exception: `relationship: [[competitor]]`. Competitors are tracked for competitive intelligence and aren't in your CRM by design, so they carry no `crm_url`. Full rule in [`canon/operations.md`](../canon/operations.md) -> "In-repo external entities: create-on-first-mention".

**Highly recommended (populate when there's evidence)**:
- `uses_stack` external software the company runs (commerce platform, data warehouse, payment provider, ...). Lands on [`systems/<slug>.md`](../systems/). Drives "which customers run `<system>`?" lookalike queries.
- `docs_url` an optional pointer to the company's hub or page in your docs system, where any audience-facing or richer material lives. The repo stays a thin index; the docs system holds the pages.

Don't speculate on these, populate only when a recap, CRM record or explicit signal confirms the value.

`aliases` is the critical field (same role as in `people/`): it's what lets agents resolve a transcript mention to this MD. Never leave it empty.

`crm_url` is the link out to the source of truth for **live state** (deal stage, owner, history, contacts list). If empty (and not a competitor), the MD is half-blind.

`linkedin` is a stable identifier, saved once so agents don't re-search the web every time.

### Body

**External company:** a short description of what the company is, 1-2 sentences max. Stable facts only (sector, business model, geography). Live state stays in the CRM.

```markdown
# Acme Corp

## About

National retail chain, ~200 stores, with a private-label line on top of third-party brands.
```

**Home company (`is_self: true`):** the body can be richer. This is the one place a company MD is allowed to carry positioning, a one-line mission, and a voice anchor, because it is the canonical statement of who you are and what every agent represents.

```markdown
# <COMPANY>

## About

What the company is, in one or two sentences.

## Positioning (home company only)

Who you serve, what you sell, the category you play in.

## Voice (home company only)

The tone every agent uses when it speaks for the company.
```

**Competitor exception:** a competitor MD carries no `crm_url`, may omit `industry`, and keeps the lightest possible body (one line on what they offer and how they overlap with you).

**Do not add** (external) `## Why we care`, `## Notes`, `## Context`, deal info, "current status", risks, last-touch dates, or any prose that describes the relationship. All of that lives in the CRM and goes stale here in weeks.

---

## How agents use this

- **Resolve company mentions in transcripts.** Match the name/alias against this folder before writing the wiki link.
- **Pivot to the CRM for state.** When the agent needs deal stage / pipeline / owner / history, it follows `crm_url` (or queries the CRM via MCP). For an external company it does **not** read the MD body, there isn't a meaningful one.
- **Anchor internal identity.** The `is_self: true` MD is what internal people point to via `company:` and what the home team's rules orbit.

---

## Wiki links

An external company MD is a near-leaf. Inbound links come from:

- `people/<person>.md` -> `companies/<slug>.md` via the `company:` frontmatter and the `## Company` section.
- `signals/*/YYYY/MM/*.md` (any signal subfolder) -> `companies/<slug>.md` when the company is mentioned. **Create the MD on first mention if the CRM has a record** (see [When to create an MD here](#when-to-create-an-md-here)); plain text only when the CRM has no match.

Outbound: `industry:`, `relationship:`, `uses_stack:` (when known).

See [`anatomy.md`](../canon/anatomy.md) -> Frontmatter wiki-link relations for the full table; [`operations.md`](../canon/operations.md) -> Tagging discipline at write time for the procedure.

---

## What does NOT go here

- A per-customer or per-prospect subfolder. One flat MD per company; no nesting.
- Companies that are not in the CRM (competitor excepted). If the CRM has no record, the mention stays as plain text in the citing file. Do not create an MD.
- **Any prose** (external) describing the relationship, deal, status, risks, history, contacts, last interaction. All of that is the CRM. If you find yourself writing it here, stop.

---

## Adding or updating a company

1. Query the CRM (via MCP or UI) for a record matching the name / domain / aliases. **If no record exists, stop** (competitor excepted), leave the original mention as plain text, do not create the MD.
2. Create `companies/<slug>.md` with front matter (including `crm_url`) + the `# CompanyName` heading + a 1-2 sentence `## About`. Nothing else for external.
3. Commit with `companies: add <name>` or `companies: update <name>, <what changed>`. Per [`operations.md`](../canon/operations.md) -> Changes, push to main, with one exception, this commits directly to `main`.

### For agents with CRM MCP access

Every recap / digest / signal you produce: scan the body for external company mentions and resolve each one. The default action is **create the MD with `crm_url` from the CRM record**, not "render as plain text". Plain text is the fallback only when the CRM has no match. Do not skip the lookup. Do not seed a half-blind MD without `crm_url`. If the CRM call fails (rate limit, transient error), flag the gap rather than guess.
