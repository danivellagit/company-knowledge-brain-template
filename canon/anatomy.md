# Anatomy

> How the graph is shaped: folders, frontmatter, wiki-link forward + reverse pairs, external sources of truth, navigation recipes. Not procedures (→ [`operations.md`](operations.md)), not values (→ [`company-rules.md`](company-rules.md)), not company facts (→ [`profile.md`](profile.md)).
>
> When files disagree, [`company-rules.md`](company-rules.md) → "Order of truth" defines who wins.

---

## Domain map — where things live

Every entity in the repo lives in a dedicated folder. Open the folder's README to learn its schema.

| Folder | What it holds | Wiki-link target as |
|---|---|---|
| [`people/`](../people/) | Any person — internal team member or external (customer, prospect, partner, vendor, advisor). Internal vs external is derived from `company:` (internal = `[[<company>]]`). | `[[<firstname-lastname>]]` |
| [`companies/`](../companies/) | Any organization — the home company ([`<company>.md`](../companies/) with `is_self: true`), every external customer / prospect / partner / vendor / advisor / competitor, **and every external software platform customers run** (the tools and applications in a customer's stack), which are companies too and live here as `relationship: [[vendor]]`. | `[[<company-slug>]]` |
| [`canon/`](.) | Canon files — `company-rules.md`, `operations.md`, `anatomy.md`, `profile.md`, `team/<team>.md`. **Every modification goes through a PR.** | _path link only_ |
| [`industries/`](../industries/) | Verticals / market segments <COMPANY> sells into or partners operate in | `[[<industry-slug>]]` |
| [`products/`](../products/) | The AI agents / automations <COMPANY> builds (for itself or to sell). Thin **pointers** to your docs system via `docs_url`; see [`operations.md`](operations.md) → "Product catalog". | `[[<product-slug>]]` |
| [`relationship-types/`](../relationship-types/) | The five kinds of commercial relationship (`customer`, `prospect`, `partner`, `vendor`, `advisor`) | `[[<type-slug>]]` |
| [`job-titles/`](../job-titles/) | Canonical titles held by people (internal and external) — CEO, CRO, CPO, CTO, Software Engineer, Head of Ecommerce, Consultant, … | `[[<title-slug>]]` |
| [`themes/`](../themes/) | Topics / themes <COMPANY> reasons about; targets for article tagging | `[[<theme-slug>]]` |
| [`articles/`](../articles/) | Pointer + TL;DR for external articles that shaped a <COMPANY> POV | _no inbound from external repos_ |
| [`signals/daily-call/YYYY/MM/`](../signals/daily-call/) | Daily call recaps — one file per person per day: `<person-slug>_YYYY-MM-DD.md`. Written by the `daily-call-recap` skill. **Never under `team/`**. | _no inbound; append-only_ |
| [`signals/email/YYYY/MM/`](../signals/email/) | Email-derived signals | _no inbound; append-only_ |
| [`signals/manual/YYYY/MM/`](../signals/manual/) | Manually pushed signals (coffees, hallway chats, off-record intel) — written by the `write-insight` skill | _no inbound; append-only_ |
| [`canon/team/<team>.md`](team/) | The team node — one file per functional team, holding both the team-specific canon **rules** and the team **composition** (`name`, `people_involved`). PR-gated as canon. There is no separate `team/` folder: this file **is** the team, and is the target of the `[[<team-slug>]]` wiki-link (the one canon exception to path-link-only). | `[[<team-slug>]]` |
| [`skills/<slug>/SKILL.md`](../skills/) | Reusable agent behaviors (team-shared or personal) | `[[<skill-slug>]]` |

The full graph — every forward edge with its mandatory reverse — lives in [Frontmatter wiki-link relations — forward + reverse pairs](#frontmatter-wiki-link-relations--forward--reverse-pairs) below.

---

## Folder taxonomy is closed

The Domain map above is the **entire list of folders** an agent may write into. New top-level folders, and new subfolders inside existing folders, are rare and deliberate. The routine exceptions (per-skill subfolders at [`skills/<slug>/`](../skills/), time-bucket subfolders under [`signals/<source>/YYYY/MM/`](../signals/)) are part of the schema. Everything else triggers the challenge-and-PR procedure in [`operations.md`](operations.md) → "Folder creation — challenge first, PR if confirmed".

**Two clarifications the map above implies but worth stating explicitly:**

- **`canon/team/` is for functional teams only** — `leadership`, `sales`, `marketing`, `product`, `engineering`, `customer-success`. One flat `canon/team/<team>.md` per team (rules + composition); there is no `team/` folder and no nesting. **Never a company name** (the home company is [`companies/<company>.md`](../companies/), not a team). **Never a file for a customer / prospect / partner / vendor / advisor** (those are `companies/<slug>.md` entities, with deal state in your CRM and deliverables in your file storage). A path like `canon/team/<company>.md` or a per-prospect team file is structurally invalid by construction.
- **No per-customer subfolder anywhere in the repo.** Customer-specific deliverables (demo decks, onboarding briefs, prospect summaries) are produced on demand by a product-level skill (with the customer as input). The output lands in your file storage / docs system / CRM. The repo holds the stable entity (`companies/<slug>.md`) and the generic skill, never a per-customer working folder.

---

## Navigating the graph for context

When you're asked to do something concrete (write a recap, brief a prospect, configure an account, draft an email), pick the right **entry node** and traverse from there:

- **About a customer / prospect / partner** → start from `companies/<X>.md`. Read its frontmatter: `industry`, `relationship`, `uses_stack`, `people`. Each of those is a wiki-link — follow them to get the company's full context in 1-2 hops. For an active customer account, also follow `docs_url` to the customer hub in your docs system (phase, projects, delivery KPIs). Then read the signals that cite it via your editor's "Referenced by" (or `grep "companies/<X>.md" signals/`).
- **About a person** (internal or external) → start from `people/<slug>.md`. Read `company` (internal = `[[<company>]]`), `job_title`, `team` / `reports_to` / `direct_reports` (typically populated only for internal). For an external person, follow `company` → land on `companies/<X>.md` (one hop) → use the company recipe above. Pivot to `job_title` for peers in the same role across companies.
- **About a product / offering** → start from `products/<slug>.md` for the short `## About` and the `docs_url`, then open the docs-system page for the full spec, the industry / role fit, and the customers running it. Those relations live in your docs system now, not in repo frontmatter.
- **About a topic / theme** → start from `themes/<slug>.md`. Read `articles` to see what we've already engaged with externally. Fetch each article's `url:` via `WebFetch` only if you need the actual content.
- **What's happening now** → query `signals/<source>/YYYY/MM/*.md`. Frontmatter is the routing source: `date`, `teams_impacted`, `related_to_people / companies / products / systems`. Filter by `teams_impacted` for team-relevance, sort by `date` desc for freshness, dereference the `related_to_*` wiki-links to land on the entity briefs.

The forward + reverse pair structure means every node knows both who it points to and who points to it — you can enter the graph from anywhere and reach the rest in 1-3 hops, never more.

---

## Body asymmetry — same folder, different body weight

`people/` and `companies/` are unified folders by design (one folder, one schema, one wiki-link plane). But the **body** of an MD inside them scales with where the source of truth lives:

- **External entities** (any `companies/` other than the home company; any `people/` whose `company:` is not `[[<company>]]`) — body stays minimal: `# Name`, short `## About`, `## Company` wiki-link. Live state and rich context live in your CRM. The MD is a stable wiki-link target, nothing more.
- **Internal entities** (the home `companies/<company>.md`; internal `people/<slug>.md` with `company: [[<company>]]`) — body can grow as needed: scope / anti-scope / decision authority / operating style / voice / how-I-write / preferences / context. There is no CRM for an internal person's tone of voice; the repo IS the source of truth.

The principle: **the body reflects where the truth lives.** When the truth lives in an external system (the CRM for the customer graph, the docs system for the product catalogue), the repo MD is a pointer and stays light. When the truth lives in the repo (a founder's voice, a team's working style), the body carries it. This is why per-person voice / tone / operating-style content lives in `people/<slug>.md` for internal team members and **does not exist** on external `people/` MDs — even though both files live in the same folder and share the same frontmatter shape.

**`crm_url` is mandatory for external MDs, with one exception.** External `people/<slug>.md` and `companies/<slug>.md` may not exist without a populated `crm_url`. The CRM URL is the entity's birth certificate in this repo: no URL, no MD. **The exceptions**: `companies/<slug>.md` with `relationship: [[competitor]]` **or** `relationship: [[vendor]]` technology platforms carry no `crm_url`. Competitors are tracked for competitive intelligence, and technology vendors (the software customers run) are tracked for stack context; neither is a customer-graph entity and neither is in your CRM by design. The `industry` field is likewise optional on both. (A vendor that is your own paid supplier does carry `crm_url` when it exists in the CRM.) Internal people may also have `crm_url:` empty (if they're a CRM user/owner rather than a contact record).

**Customer MDs may carry an optional docs pointer.** `docs_url` points to the customer's hub in your docs system (phase, projects, delivery KPIs), mirroring how `products/<slug>.md` points to the product catalogue. It is populated only for active accounts (`relationship: [[customer]]` with a live or in-flight project); it sits next to `crm_url` and stays empty on prospects until a project hub exists.

The write-time procedure (when/how to create from a mention; what to do when CRM access fails) is in [`operations.md`](operations.md) → "In-repo external entities — create-on-first-mention".

---

## Glossary

Internal terms that appear in transcripts / chat / docs and need a canonical spelling or definition. Add a row as soon as an agent asks "what did they mean by X?" Keep the canonical spelling and a one-line meaning here; the full definition lives in your docs system.

| Term | Canonical spelling | What it means |
|------|-------------------|---------------|
| `<ProductCodename>` | `<Product Codename>` | A product / feature codename so an agent uses the canonical form instead of inventing a variant. |
| `<ACRONYM>` | `<ACRONYM>` | An internal acronym or shorthand, with both the expansion and what it actually means in your context. |

_Examples of what belongs here:_
- _Product / component names (so an agent doesn't invent "Legend Log" when it's actually "LogManager")._
- _Customer names with correct capitalization / spacing (e.g. canonical `<CustomerName>` vs heard variants like `<Customer Name>`, `<CUSTOMERNAME>`)._
- _Internal shorthand for processes, models, or frameworks adopted at a specific point in time (define both the term and what it actually means)._
- _Framework / methodology labels used in calls but not obvious to someone reading the transcript cold._

---

## Customer and company name spellings

_(Optional section — populate when an agent gets a name wrong for the first time.)_

| Heard / read as | Canonical | Notes |
|---|---|---|
| _TBD_ | _TBD_ | _TBD_ |

---

## Meeting rooms

Offices often have **named meeting rooms**. When a call is joined from one of those rooms, the transcript tool labels everyone in the room under a single speaker tag that matches the room name. **A room tag is not a person.** In a given call a room may contain 1 person or 10, and the set changes every call.

**Rooms currently in use**

- **<Room A>** — shared meeting room. Any subset of the team can be inside for a call.
- **<Room B>** — shared meeting room. Same usage pattern.

_Add new rooms here as the office grows._

**How agents must handle a room label in a transcript**

1. Do not map the room label to a fixed person.
2. Determine who was in the room for that specific call (ask the user, check meeting invite, check preceding messages).
3. For each sentence attributed to the room label, disambiguate by:
   - Who is being addressed ("<Name>, what do you think?" → the reply comes from <Name>, if they are in the room).
   - Topic alignment with the `Scope` of the occupants (runway → CFO/CEO; pipeline → Sales Lead; etc. — see [`people/`](../people/)).
4. If unresolvable, attribute to the room label with the occupants listed, e.g. `<Room A> (<Name1> / <Name2> / <Name3>)`. Do not guess a single person.

Room-by-call attendance is ephemeral — it belongs in the meeting notes themselves, not here.

---

## External sources of truth

Agents need information that doesn't live in this repo. The repo curates context; it does **not** duplicate state that lives in another system. When an agent needs that state, it goes to the right system.

| Domain | System of record | How agents access it |
|---|---|---|
| Customer accounts, deals, pipeline | _<your CRM>_ (e.g. HubSpot, Salesforce, Pipedrive) | MCP server |
| Recorded call transcripts | _<your meeting-transcript tool>_ (e.g. Fireflies, Otter, Granola) | MCP server |
| Roadmap, product specs, internal docs | _<your docs system>_ (e.g. Notion, Confluence, Coda) | MCP server |
| Product catalogue (the AI agents / automations catalogue: spec, fit, pricing, customers) | _<your docs system>_ | MCP server; repo `products/<slug>.md` is a pointer via `docs_url` |
| Email exchanges with customers / partners | _<your email>_ (e.g. Gmail, Outlook) | MCP server |
| Team chat (channels, DMs) | _<your team chat tool>_ (e.g. Google Chat, Slack) | MCP server |
| Calendar events (meetings, blocks) | _<your calendar system>_ (e.g. Google Calendar, Outlook) | MCP server |
| Codebase, PRs, issues | _<your code host>_ (e.g. GitHub, GitLab) — _list company repos here_ | `gh` CLI or equivalent |
| On-record signal entries (recaps, digests, snapshots) | This repo — [`signals/`](../signals/) | direct read |

**Rules for skills/agents reading these systems:**

1. **Before writing about a customer**, consult the CRM (current state of the relationship) and the latest email thread (most recent communication). If access fails, declare the gap rather than inventing.
2. **Before writing about a recorded conversation**, read the transcript directly — never paraphrase someone else's summary.
3. **Before writing about a product or roadmap topic**, check the docs system for the canonical version.
4. **The repo wins for company-level context** (rules, goals, voice, conventions, people). External systems win for state that changes constantly.
5. **Never duplicate** content that lives in an external system into the repo. Link to it instead.

If a new source of truth gets adopted, expand the table.

---

## Two-layer graph — frontmatter vs body

The repo is a **navigable context graph**. The graph lives in two layers, with one canonical:

- **Frontmatter wiki-link relations** (YAML, editor-native). This is the **canonical** layer — agents read it, a local Markdown editor (e.g. Obsidian, Tolaria) renders it as dedicated sections, the forward + reverse pairs table below is the authoritative shape of the graph.
- **Body markdown links** (standard `[label](path)`). This is the **human navigation** layer — a recap's body lists the same companies and people as clickable text, the team rules body links to the team lead in `people/`. Body links must stay in sync with frontmatter at write time but are not the source of truth.

To find what cites a file, use the editor's "Referenced by" view (graph traversal) or `grep "<filename>"` from the CLI.

---

## Frontmatter wiki-link relations — forward + reverse pairs

The graph in this repo lives in the **frontmatter** of each MD via wiki-link relations. A local Markdown editor (e.g. Obsidian, Tolaria) renders each as a dedicated section in the Properties panel. **But** the editor's automatic inverse only fires on the default relation names (`belongs_to` / `has` / `related_to`); for our semantic field names we declare both sides of every edge manually.

The **double-write procedure** (every forward edit triggers the matching reverse in the same write) is in [`operations.md`](operations.md) → "Wiki-link relations — always double-write". This table is the **shape** that procedure enforces.

| Forward (declared on …) | Reverse (declared on …) |
|---|---|
| `people/<x>.md → team: [[<team>]]` | `canon/team/<team>.md → people_involved: [[<person>]]` |
| `people/<report>.md → reports_to: [[<manager>]]` | `people/<manager>.md → direct_reports: [[<report>]]` |
| `people/<x>.md → company: [[<company>]]` | `companies/<x>.md → people: [[<person>]]` |
| `people/<x>.md → job_title: [[<title>]]` | `job-titles/<title>.md → people: [[<person>]]` |
| `companies/<x>.md → industry: [[<industry>]]` | `industries/<industry>.md → companies: [[<company>]]` |
| `companies/<x>.md → relationship: [[<type>]]` | `relationship-types/<type>.md → companies: [[<company>]]` |
| `companies/<customer>.md → uses_stack: [[<vendor-company>]]` | `companies/<vendor>.md → used_by: [[<customer>]]` |
| `articles/<x>.md → themes: [[<theme>]]` | `themes/<theme>.md → articles: [[<article>]]` |
| `signals/<source>/<period>/<slug>.md → related_to_themes: [[<theme>]]` | `themes/<theme>.md → signals: [[<signal>]]` |
| `skills/<slug>/SKILL.md → owner: [[<team-or-person>]]` | _no manual reverse for team-owned skills — use the editor's "Referenced by" on `canon/team/<team>.md` (skills are created often; keeping the reverse in canon would PR-gate every new skill). For a personal skill, the reverse `people/<x>.md → skills:` is still declared._ |
| `skills/<slug>/SKILL.md → creator: [[<person-slug>]]` | `people/<person-slug>.md → skills_created: [[<skill>]]` |

**Exception — signals**. Signals (`signals/*/*/*.md`) declare forward `author`, `teams_impacted`, `related_to_people`, `related_to_companies`, `related_to_products`, `related_to_systems`, `related_to_themes`. As a general rule they do **not** get a reverse on the cited entity — signals grow without bound and reverse-maintenance on unbounded taxonomies (people, companies, products, systems) would explode. Inverse lookup on those target pages uses the editor's generic Referenced by view.

**Exception to the exception — themes**. `themes/` is a small, bounded taxonomy that's high-value for cross-signal queries ("show me every meeting that touched Evals"). The reverse `themes/<theme>.md → signals:` **is** declared manually — the maintenance cost stays linear because themes are sparse and a theme rarely lands on more than a handful of signals per week.

---

## Direction rule

Cross-cutting → specific, not viceversa.

- [`people/`](../people/), [`companies/`](../companies/), [`signals/`](../signals/), [`articles/`](../articles/), and [`skills/`](../skills/) are primary cross-cutting locations at the repo root; they link **into** the team nodes at [`canon/team/`](team/).
- A person's MD always lives in [`people/`](../people/) — internal or external, never under a team's folder, even if they only work with one team. Internal vs external is derived from `company:`.
