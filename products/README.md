# products/

One MD per **product**: an AI agent or automation that `<COMPANY>` builds, either for itself or to sell. A product is the concrete job the agent does (a job-to-be-done) and, where relevant, the thing a customer pays for.

The full definition of each product (scope, fit, pricing, customers, KPIs, status) lives in your **docs system** (e.g. Notion, Confluence), which is the **system of record**. The MD in this folder is a thin **pointer**: a stable slug, a category, a one-line description, and a `docs_url` back to the page that owns the detail.

This keeps the repo a fast, greppable index of "what agents do we build?" while the rich, frequently-changing material (commercial terms, rollout state, eval results) lives where it can be edited by the whole team without a git round-trip.

See [`_TEMPLATE.md`](_TEMPLATE.md) for the copy-paste schema with placeholders.

---

## What a product is

A product is an **AI agent / automation** `<COMPANY>` ships. One product maps to one product idea: a specific job done by an agent (or a small set of agents) that advances a business outcome.

A product is **not** a feature, **not** a product pillar, **not** a service line. It's the thing you can point to and say "this is what the agent does, this is roughly what it costs to run or buy, this is the impact, this is what it plugs into."

Born in your docs system. The product is conceived, scoped, and priced there. The repo MD comes second and stays thin, it exists so signals, scripts, and skills can wiki-link a stable target.

---

## Why this exists

So that agents and humans can answer in one hop:

- "What agents / automations do we build?" -> list `products/*`.
- "Where's the full spec for `<product>`?" -> open the MD, follow `docs_url` to the system of record.
- "Which category does this fall under?" -> read the `category` field.

---

## When to create an MD

When `<COMPANY>` has shipped, or is actively building/selling, the agent. Don't seed speculative offerings; the page in your docs system comes first, then the thin pointer MD here once the product is real.

---

## File convention

`<slug>.md`, lowercase, hyphenated. Examples: `lead-qualifier.md`, `support-triage.md`, `report-generator.md`.

### Front matter

```yaml
---
name: <Product Name>                 # canonical spelling, e.g. "Lead Qualifier"
slug: <product-slug>                 # matches the filename, e.g. lead-qualifier
category: <category>                  # your own taxonomy, e.g. sales | support | marketing | ops | data
docs_url: <docs-url>                  # pointer to the page in your docs system (the system of record)
---
```

No relational wiki-link edges live here. Fit (which segments/roles a product suits) and adoption (which customers run it) are relations that change often and carry commercial nuance, so they live in your docs system, not in repo frontmatter. The repo MD stays a pointer, not a mini-CRM.

### Body

Thin. One or two sentences, then stop.

```markdown
# <Product Name>

## About

1-2 sentences: what the agent does and the job it solves. Everything else (fit, pricing,
customers, KPIs, status) lives in the docs page linked from `docs_url`.
```

Do **not** reproduce pricing, customer lists, KPI targets, or rollout status here. That's the docs system's job, and it goes stale in the repo within weeks.

---

## How agents use this

- **Enumerate offerings.** List `products/*` to answer "what do we build / sell?"
- **Route to the spec.** Open an MD, follow `docs_url` to the system of record for the full detail.

---

## What does NOT go here

- Pricing figures, tiers, or commercial terms. Out of the record by design; the live model lives in your docs system / CRM.
- Customer adoption lists, KPI targets, eval results, rollout status. All of that is the docs system.
- Relational wiki-link edges (fit, used-by). Those relations live in the docs system, not in repo frontmatter.

---

## Adding or updating a product

1. Create / update the page in your docs system first, that's the system of record.
2. Create `products/<slug>.md` with the four frontmatter fields + a 1-2 sentence `## About` + the `docs_url` pointing back.
3. Commit with `products: add <name>` or `products: update <name>, <what changed>`. For non-`canon/**` files this commits directly to `main` per [`canon/operations.md`](../canon/operations.md) -> Changes, push to main, with one exception.
