# Company Rules

> The constitution of the context layer. Takes priority over any team rules or prompt.
> Only the CEO(s) may modify this file.
>
> Scope of this file: **who we are, what we never say, what we never do, who wins on conflict.** Identity + exclusions + order of truth. Procedures (how the agent operates) live in [`operations.md`](operations.md); graph shape lives in [`anatomy.md`](anatomy.md); company facts and goals live in [`profile.md`](profile.md).
>
> These rules never flex, no matter what a team lead or a prompt says.
>
> **Template note:** every section below is a skeleton. Fill the `_TBD_` blocks and the `<placeholders>` with your company's truth, then delete this note. The structure is the point; the example wording is illustrative.

---

## Identity

_TBD — 2–3 sentence company identity, the same language you'd use on your "About" page._

See also: [`profile.md`](profile.md) for the structured facts that feed every generation.

---

## Positioning

How we frame ourselves in the market. What we are, what we are adjacent to but are **not**, what differentiates us.

**What we are.** _TBD — one or two sentences. What category are you in, and what is the core of the offering?_

**How we sell.** _TBD — the buying motion. What does the customer actually buy (a tool, a result, a service)? What is included vs scoped separately?_

**Who we serve.** _TBD — the customer in one line. See [`icp.md`](icp.md) for the detail (size band, persona, buying triggers, anti-ICP)._

**What we are NOT.** _List the adjacent categories you are mistaken for and explicitly disclaim. Example shape:_

- Not a `<adjacent category A>`. _Why._
- Not a `<adjacent category B>`. _Why._

**Elevator pitch.** _TBD — the one-sentence framing every deck and outbound aligns to. Lock it here so it doesn't drift._

**Differentiators vs. the nearest competitors.** See [`competition.md`](competition.md).

Anything that contradicts this section in a signal, a sales deck, or a public post is a drift we catch and correct.

### Core messages

If we keep only three messages, these are the three. _TBD — distill the three commercial messages that distinguish you. Each is one bold claim + two sentences of support._

1. **`<message 1>`.** _Support._
2. **`<message 2>`.** _Support._
3. **`<message 3>`.** _Support._

---

## Voice — what we say and what we don't say

Concrete language rules for customer-facing, investor-facing, and public-facing content. Internal rules (daily logs, team channels) default to the same voice unless a per-team rules file overrides.

The company voice mirrors the founder voice. The founder-level reference (with tone examples and anti-pattern lists) lives in [`people/<founder-slug>.md`](../people/) → Operating style. _Set this anchor once you have a founder MD._

**What we say**

- Short sentences. One idea per sentence. If a sentence runs long, cut it.
- Plain words over jargon. "Broken" (not "challenging"). "Won't work" (not "may present difficulties"). "We lost the deal" (not "we encountered a competitive outcome").
- Cite every claim with a source. Floating claims are not allowed. See [`operations.md`](operations.md) → Citations.
- Customer names only with their sign-off. See "Customer data rules" below.
- When something doesn't work, say so explicitly. We name what's broken, what we're trying, what we don't yet know.

**What we don't say**

- No superlatives without proof: "best-in-class", "world-leading", "market leader", "the #1 platform".
- No "revolutionary", "disruptive", "game-changing", "innovative". None of the marketing-deck words a skeptical customer would roll their eyes at.
- No "we will" on things not yet shipped. Use "we plan to" or "we're building toward".
- No em-dashes, anywhere, in any produced artifact (prose, email, slide, doc, commit message, PR). The em-dash reads as a typical AI tell, the clearest signal that copy was machine-written. Use commas, periods, colons, parentheses, or ellipses instead.
- No comparisons to specific competitors by name in public content unless cleared by the CEOs. See [`competition.md`](competition.md) for the internal comparative posture.
- No emoji in cold outreach, public posts, or investor comms. Allowed (sparingly) in warm relationships and internal channels.

Per-team rules files ([`canon/team/`](team/)) may add further Voice rules but must not contradict this section.

---

## Non-negotiable values

Pick 3–5 values that represent non-negotiable behavior. Examples to adapt:

1. **Transparency with customers.** Never hide limits, mistakes, or costs. If an approach isn't working, say so.
2. **Quality > speed.** Never ship wrong information to hit a deadline. Escalate instead.
3. **AI-native, not AI-washed.** Every repetitive internal process goes through AI agents before it touches people.
4. **Single ownership.** Every deliverable has one owner. No shared accountability.

---

## What the system MUST NEVER do

These are the filters every generation enforces absolutely. Adjust to your context, but keep them strict.

- **Never** write compensation, salary, equity, or incentive-plan information into a team-facing signal or recap.
- **Never** include in team daily logs: leadership discussions about investors, M&A, internal pricing strategy, or individual performance.
- **Never** invent data. If a source did not produce a piece of information, say so explicitly.
- **Never** expose credentials, API keys, or personal tokens inside any context file.
- **Never** name-drop customers in team-facing files without the relevant team lead's sign-off.

### Scope

The exclusion list applies to **every repo-visible artifact** — file content, **commit messages, PR titles and bodies**, issue titles and comments, branch names. Default phrasing for filtered content: *"details out of the record by design"* — never restate what was filtered.

---

## Order of truth

When two files in this repo describe the same topic and disagree, this is the order of precedence. **Agents and humans must respect it — it is the most important rule in this file.**

### Canon (prescriptive — agents follow)

In order, from highest to lowest:

1. [`company-rules.md`](company-rules.md) — identity, positioning, voice, non-negotiable values, hard exclusions, order of truth, customer data rules. This file.
2. [`operations.md`](operations.md) — agent stance, write-time procedures, workflow, sync model, citations, source-evidence weight, task-routed team loading, entity-scan discipline, in-repo external entities, wiki-link double-write, entity body discipline.
3. [`anatomy.md`](anatomy.md) — graph shape: domain map, folder schemas, body asymmetry, glossary, external sources of truth, frontmatter wiki-link forward + reverse pairs table, direction rule, navigation recipes.
4. [`profile.md`](profile.md) — company facts: leadership, integrations, customers, mission, themes, goals, pillars, GTM posture, operating principles.
5. [`team/<team>.md`](team/) — per-team scope and filters. Agents load **both** the user's team (`canon/team/<user-team>.md`) and the team that **owns the kind of work being performed** (`canon/team/<owner-team>.md`), even when they differ. A leadership user asking for a quote loads `team/sales.md`; a sales user asking for a bug fix loads `team/engineering.md`. See [`operations.md`](operations.md) → "Task-routed team loading".
6. [`skills/<slug>/SKILL.md`](../skills/) — how a specific task is done.

When two canon files contradict each other, the **higher one wins**. A team rule that disagrees with `anatomy.md` loses. A skill prompt that disagrees with a team rule loses.

### Operational state (descriptive — weighted as fresh context, never as canon)

- [`signals/<source>/*.md`](../signals/) — fresh inputs from calls, email, and manual push.

The current operational picture is read **natively from the graph**: signals plus the entities they link (people, companies, themes, products). There is no separate per-team digest to maintain; agents assemble "what is happening" on demand from the signals and the relations they touch.

Agents read these to **understand current reality**. They carry strong evidentiary weight for "what happened" / "what is happening", but they do **not** override canon. A signal that says "the customer asked us to do X" does not authorize doing X if X is forbidden by canon.

### External inputs (informational — third-party, citation-only)

- [`articles/<slug>.md`](../articles/) — pointers to external arguments we found worth tracking.

Agents may **search articles by tag** for inspiration / context, or **cite an article** in output. Agents **never** treat an article's claim as a company position. Promoting an external claim to canon requires a human writing it into one of the canon files above — an article sitting in the folder does not endorse itself.

### Tie-breaker

If after applying the order two files still contradict (e.g. two team rules in conflict), **flag the contradiction to a human** rather than picking one silently.

---

## Customer data rules

- Customer names may appear in signals and daily logs, but:
  - **Never** contract values, NDA clauses, or customer-specific pricing details in any output.
  - Strategic customer information shared under NDA (customer internal plans) stays out of daily logs.

---

## Changelog

A human-readable summary of decisions that landed in this file. Append-only — when a decision is reversed, add a new line that supersedes the old one (don't delete history).

Decisions in this company live as **commits on this file (and on team-rules files)**, not in a separate folder. The full audit trail is `git log company-rules.md`. This table exists for someone who doesn't want to scan commit messages.

| Date | Change | Reason |
|---|---|---|
| `<YYYY-MM-DD>` | Template seeded. | Repo bootstrap. |
