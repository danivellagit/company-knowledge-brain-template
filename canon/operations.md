# Operations

> How agents and humans operate inside this repo: write-time procedures, workflow, sync, entity discipline. Not values (→ [`company-rules.md`](company-rules.md)), not graph shape (→ [`anatomy.md`](anatomy.md)), not company facts (→ [`profile.md`](profile.md)).
>
> When files disagree, [`company-rules.md`](company-rules.md) → "Order of truth" defines who wins. This file is high in that order but not the top — `company-rules.md` overrides it.

---

## Agent stance — proactive, not refusing

The repo is a **growing** context layer, not a closed inventory. The next sentence the user types may reference an entity (a company, a person, a system, a product, a theme, an article) or a capability (a skill) that isn't in the repo yet. The correct move is **not** to conclude "it doesn't exist" or "this isn't the place for that". It is to:

1. **Check the canonical source before concluding absence.** For external `companies/` and `people/`, that source is your CRM (e.g. HubSpot, Salesforce, Pipedrive) via MCP. For taxonomy entities (`themes/`, `industries/`, `job-titles/`, `systems/`, `products/`, `articles/`), it's the per-type heuristic in [Entity-scan discipline](#entity-scan-discipline) → "Per-type creation heuristics". For skills, it's `skills/` itself, plus the option to create a new one.
2. **Propose the create / link / alias action, then proceed.** The graph grows by explicit proposal-and-confirmation, never by silent skip and never by silent create (see [Entity-scan discipline](#entity-scan-discipline) → "Triage before write").
3. **Refusing because "I don't see it in the repo" is a bug.** The repo reflects only what someone has already pulled in. Absence in the repo is a signal to check the source of truth, not a conclusion that the entity does not exist.

This stance applies to **every interaction** — answering a question, writing a file, proposing a plan, scoping a task. Not only to write-bearing skills.

### How this looks in practice

| Situation | Wrong move | Right move |
|---|---|---|
| User asks for work on a company that has no `companies/<slug>.md` yet | "That company isn't in the repo." | Query the CRM. If hit → propose `create companies/<slug>.md` with `crm_url` populated, then proceed. If miss → proceed in plain text and flag the gap. |
| User asks for work on a person that has no `people/<slug>.md` yet | "That person isn't tracked." | For an external person, same shape — query the CRM, propose create with `crm_url`. For an internal person, ask the user (never seed). |
| User asks for a task that doesn't match an existing skill | "There's no skill for that." | Inspect `skills/`. If nothing fits → **propose creating one** (`skill-creator`) with explicit `owner:` and `creator:`, then proceed once the user confirms. Improvising silently — running an ad-hoc pipeline that should have been a skill — is a bug. |
| User pastes a link to an external article | Saying nothing about it. | If the article has shaped or could shape a company POV → propose `create articles/<slug>.md` with TL;DR and themes. |
| User mentions a new system / theme / industry / job-title / product in passing | Silent skip. | Apply [Entity-scan discipline](#entity-scan-discipline) → propose `create` / `alias-merge` / `skip`; never silent skip. |
| User surfaces an observation from outside a tracked source (a competitor podcast, a press release, a SERP scan, a screenshot, an internal note) that bears on positioning / customers / strategy | Acknowledge in chat and move on. | Propose pushing it as a manual signal — `signals/manual/YYYY/MM/<firstname>-<lastname>_<topic-slug>_YYYY-MM-DD.md`. Same write-time discipline as any other signal (citations, English, entity-scan, wiki-links, exclusion list). Full format: [`signals/manual/README.md`](../signals/manual/). |

The proactivity covers every entity type in [`anatomy.md`](anatomy.md) → "Domain map": `themes/`, `people/`, `companies/`, `industries/`, `relationship-types/`, `job-titles/`, `systems/`, `products/`, `articles/`, plus `skills/`. The only enum that **never** gets new entries is `relationship-types/` (fixed: `customer` / `prospect` / `partner` / `vendor` / `advisor`).

---

## Repo scope: knowledge only

This repo holds **knowledge**. Not deliverables, not files for audiences, not binaries. The proactive stance above (propose creates, never silent-skip) is bounded by this scope: the question is not only "should this entity exist" but also "does this content belong in this repo at all, and if so, in which existing slot".

Before creating any new file or folder, the agent runs a two-gate check. The check is non-skippable. If either gate fails, the agent challenges the user in chat instead of writing.

### Gate 1: classify the content

Three surfaces, three destinations. Pick exactly one before writing.

| Surface | Destination | Examples |
|---|---|---|
| **Knowledge.** Canonical rules, stable definitions, append-only signals, taxonomy entries, reusable agent behaviors. | This repo. | `canon/`, `canon/team/<team>.md`, `people/`, `companies/`, `themes/`, `products/`, `systems/`, `industries/`, `job-titles/`, `signals/`, `skills/`. |
| **Document for an audience.** A page written to be read by humans, formatted for consumption: All-hands recap, marketing one-pager, narrative deck content, internal announcement, distilled summary of canon for a specific reader. | Your docs system (e.g. Notion, Confluence), via MCP. | All-hands ICP recap, partner-facing brief, team-level FAQ page. |
| **Binary file.** xlsx, pdf, docx, pptx, png, jpg, mp4, any non-text asset. | Your file storage (e.g. Drive). | Spreadsheets, exports, customer-shareable decks, screenshots, logos, mockups. |

Email drafts go in your email tool. Calendar events go in your calendar. CRM notes go in your CRM. See [Outbound drafts: create in the tool, not the chat](#outbound-drafts-create-in-the-tool-not-the-chat) for the matching MCP rule.

If the content is **not knowledge**, do not write a repo file. Surface the classification and the right destination to the user in chat ("this looks like a docs-system page; I'll create it via the docs-system MCP instead of in `team/marketing/`"), propose creating the artifact in the matching tool, proceed only after the user confirms. The user may override with an explicit reason. Silent acceptance of a non-knowledge write is a bug.

### Gate 2: if knowledge, reuse existing slots first

When the content classifies as knowledge, the next question is *where in the repo it lives*. The default is to extend an existing file, never to create a new one. Walk this order:

1. **Org-level canon.** Does the content fit as a new section in [`canon/company-rules.md`](company-rules.md), [`canon/operations.md`](operations.md), [`canon/anatomy.md`](anatomy.md), or [`canon/profile.md`](profile.md)? Add it there.
2. **Team-level canon.** Does it fit in an existing [`canon/team/<team>.md`](team/)? Add it there.
3. **Entity body.** Does it extend a definition that already lives in a `people/`, `companies/`, `themes/`, `products/`, `systems/`, `industries/`, `job-titles/`, or `relationship-types/` MD? Extend that body, respecting the body-asymmetry rule in [`anatomy.md`](anatomy.md) → "Body asymmetry — same folder, different body weight".
4. **Append-only signal.** Is it a dated observation tied to a meeting, a call, an email, an external event? Push it as a signal under [`signals/`](../signals/).
5. **Skill.** Is it a reusable behavior an agent should invoke? Add or extend a skill under [`skills/<slug>/SKILL.md`](../skills/).

Only if none of the above slots applies does the agent propose a **new** file. New folders trigger the additional discipline in [Folder creation — challenge first, PR if confirmed](#folder-creation--challenge-first-pr-if-confirmed) below: the folder taxonomy in [`anatomy.md`](anatomy.md) → "Domain map" is closed, and adding a new top-level folder is PR-gated work, never an inline write.

Surface the gate-2 reasoning to the user before writing: *"this fits as a new section under `canon/operations.md` § Tagging discipline, ok?"*. Silent new-file creation when an existing slot exists is a bug, even when the content is unambiguously knowledge.

### Why both gates

Gate 1 prevents the repo from accumulating publication-ready drafts that belong in the tools where they ship. A docs-system page lives in the docs system, a deck PDF lives in file storage, an email lives in your email tool. Duplicating them here creates a second copy that drifts and confuses canon with derivative views of canon.

Gate 2 prevents canon drift. The same rule restated in three different files is the failure mode this gate is designed to catch. If a rule already has a home, extend it; do not open a parallel file.

The combined effect: the repo stays small, queryable, and authoritative. Deliverables stay in the tools where they are consumed.

---

## Dates and times

- **Date format:** ISO — `YYYY-MM-DD`. No US ordering, no ambiguous "04/05/2026".
- **Timezone:** _<your team's timezone>_ (e.g. `Europe/Rome`, `America/New_York`). Convert all incoming timestamps to this for display unless the consumer is in another timezone.
- **Quarter boundaries:** Q1 = Jan–Mar, Q2 = Apr–Jun, Q3 = Jul–Sep, Q4 = Oct–Dec. A meeting on the last day of March is Q1. A meeting on April 1 is Q2.
- **Relative date resolution:** when a transcript says "by June", "end of Q2", "Thursday", resolve to an absolute `YYYY-MM-DD` in the output artifact, based on the meeting date. Do not leave relative dates in written deliverables.

---

## Language

**English. Always. No exceptions.** Every file in this repo is written in English — frontmatter, body prose, headings, comments, commit messages, PR titles, signal recaps. The rule applies even when the source material (a recorded meeting, an email thread, a chat exchange) is in another language. The producing skill / human translates at write time.

- Source in another language → recap in English.
- Source in English → recap in English (no transformation).
- Source mixed → recap in English, glossed where a non-English term is a proper noun or a brand name.

This is a hard rule, not a preference. The repo is a context layer read by AI agents and by collaborators across geographies; a single language keeps the graph queryable and the prose grep-able. Internal chat / live calls themselves remain in whichever language is natural — only the written artifact in this repo is English.

---

## Citations — every claim links to its source

Every factual block in a signal, recap, or any agent-produced output carries a **link to the original source** (call transcript URL, deal record, doc URL, email message-id, PR, chat message link). Floating claims — assertions without a citation — are not allowed.

When two sources contradict, **flag the contradiction explicitly** rather than resolving it silently. The reader (human or downstream agent) decides which source wins.

When a source is missing or inaccessible (no CRM access in the current session, transcript not yet ingested), declare the gap rather than inventing.

---

## Source-evidence weight

When an agent generates a **claim** (a fact, a number, a status, an attribution), the weight of each evidence source is ranked:

| Priority | Source | Weight |
|---|---|---|
| 1 | Objective data (CRM, code repo, literal transcripts) | Max |
| 2 | Formalized decisions (signed-off docs, ADRs) | High |
| 3 | The model's interpretation | Only when explicitly invited by a prompt |

Anything not backed by an objective source may appear as a "weak signal," never as fact.

> **Different concept from Order of truth.** Order of truth (defined in [`company-rules.md`](company-rules.md) → "Order of truth") governs **which file wins** when two files inside this repo describe the same topic and disagree — a constitutional question. Source-evidence weight governs **how confident** an agent is when generating a single claim from external evidence — an epistemic question. Both rules apply at once: consult files in Order of truth for the authoritative rule; weigh the evidence in your claim per this table.

---

## Changes — push to main, with one exception

Default workflow: **commit and push directly to `main`**. No pull requests by default.

Why: the repo is a context layer for a small team. Velocity matters more than gatekeeping; the cost of a wrong write is low (revert one commit) and the cost of friction (PR + review + merge for every typo fix) is high. The next reader is the reviewer.

**Host / session branch directives do not change this.** A host layer — Claude Code on the web auto-creating a `claude/<…>` session branch and telling you to "develop on that branch, never push elsewhere", an OpenAI Codex or Cursor worktree, any wrapper that injects a per-session branch instruction — is **host-layer and secondary to this repo** (see [`company-rules.md`](company-rules.md) → "Order of truth" and the CLAUDE.md / AGENTS.md boot doc → "Host-layer memory is secondary"). It does **not** override the rule above: a non-`canon/**` change is committed and pushed **direct to `main`**. If your session opened you on a feature branch for non-canon work, run `git checkout main` and work there — the session branch is the local skin, the repo rule is canon. The user (repo owner) has standing authority to confirm this in chat; you do not need fresh permission each session. The only branch-and-PR path is the `canon/**` exception below.

**The one hard exception — `canon/**` is PR-only.**

Every modification to a file under [`canon/`](.) — `company-rules.md`, `operations.md` (this file), `anatomy.md`, `profile.md`, `team/<team>.md`, `README.md` — **goes through a pull request**. No direct push to `main` from anyone, owner included. This protects the canon from accidental drift: every change to the rules-of-the-game gets a second pair of eyes by construction, not by discipline.

A GitHub push ruleset enforces the rule on the server side. The PR can be merged by the owner after self-review when no other reviewer is available, but the PR step itself is non-skippable.

**Optional PR (outside canon/).**

For everything else, a PR is optional and opened only when the author wants a second pair of eyes — typically for material changes to `.github/**`, `skills/**`, top-level `README.md` / `index.md` / `CLAUDE.md` / `AGENTS.md`. The default is still push direct to `main`.

Even on direct-push, the [`company-rules.md`](company-rules.md) → *What the system MUST NEVER do* → **Scope** rule applies to **commit messages** and **PR titles / bodies** — the commit log and PR list are repo-visible and the exclusions cover them. Never name an excluded item in a commit message or PR text; default phrasing for filtered content is *"details out of the record by design"*.

---

## Folder creation — challenge first, PR if confirmed

The repo's shape is canonical. The folder taxonomy in [`anatomy.md`](anatomy.md) → "Domain map" is the entire list of folders an agent may write into. Creating a folder that is not in the schema is the single fastest way to break the graph, and the rule below applies to every agent and every human committer.

**Routine, no challenge:** per-skill subfolders at [`skills/<slug>/`](../skills/), each owning a directory by design, plus time-bucket subfolders under [`signals/<source>/YYYY/MM/`](../signals/) created automatically as months pass. These follow the standard push-to-`main` flow described above.

**Everywhere else, challenge first.** When an agent is about to create a folder anywhere other than the two routine locations above — a new top-level folder, a new subfolder inside `team/`, `companies/`, `people/`, `products/`, `industries/`, `systems/`, `themes/`, `job-titles/`, `relationship-types/`, `articles/`, `canon/`, or any other existing folder — it stops and runs the sequence below before any write. **Silent `mkdir` is a bug.**

1. **Challenge the user.** Open with: *"You're proposing a new folder at `<path>`. The repo's structure is canonical and new folders are rare. Let me check what you're trying to do before creating it."* Make the suspicion explicit.
2. **Ask questions until intent is clear.** What content will live in the folder? Is it about a specific company, person, product, system or theme (in which case it goes as a flat MD in the existing folder for that type, not as a new subfolder)? Is it customer-specific delivery work (in which case it goes in file storage / docs system / CRM, never the repo)? Is it a skill (in which case it goes under `skills/<slug>/`, which is routine and exempt)? Is it an ad-hoc working area (in which case the answer is no — use [`signals/manual/`](../signals/manual/) for observations, the user's own filesystem for scratch work)?
3. **Route to the standard path first.** In nearly every case, the right answer is "use the existing folder with the right schema". Use [`anatomy.md`](anatomy.md) → "Domain map" + "Navigating the graph for context" as the routing reference. Propose the concrete standard path and proceed from there.
4. **If a new folder is genuinely necessary** — a new structural concept the graph does not already model, confirmed by the user after the challenge — the agent **does not direct-push**. It creates the folder on a feature branch and opens a PR:

   ```
   git switch -c structure/<short-purpose>
   mkdir -p <new-path>
   <add at least one tracked file, typically a README.md describing the schema>
   git add <new-path>
   git commit -m "structure: add <new-path>"
   git push -u origin structure/<short-purpose>
   gh pr create --title "structure: add <new-path>" \
     --body "<what the folder holds, which existing folder was considered and ruled out, what schema files inside it will follow>"
   ```

   The PR body must justify three things: what the folder will hold, which existing folder was considered and ruled out, and what schema files inside the folder will follow. Without this, structure drifts; with it, every structural addition is on the record.

**Common mistakes — routing cheat-sheet.**

| Tempted to create | Correct location |
|---|---|
| `team/<company-name>/` (e.g. `team/<company>/`) | None. `team/` is **functional teams only** (e.g. `leadership` / `marketing` / `product` / `engineering` / `sales` / `customer-success`). The home company lives in [`companies/<company>.md`](../companies/) (`is_self: true`). |
| `team/<any>/prospects/<X>/`, `team/<any>/customers/<X>/`, `team/sales/deals/<X>/` | None. External entities are `companies/<X>.md` (stable facts) + CRM (deal / pipeline state) + file storage (deliverables). Per-customer folders in the repo are forbidden. |
| `<customer>-demo/`, `<customer>-onboarding/`, `prospect-<X>/` at any level | None. Per-customer deliverables are generated on demand by a product-level skill with the customer as input. Output lands in file storage / docs system / CRM, never in the repo. |
| `notes/`, `drafts/`, `wip/`, `inbox/`, `scratch/` | None. One-off observations go to [`signals/manual/YYYY/MM/`](../signals/manual/) following the manual-signal schema; scratch work stays on the user's local filesystem, not committed. |
| `customers/`, `prospects/`, `partners/`, `vendors/`, `advisors/` at any level | None. All five relationship types share [`companies/<X>.md`](../companies/) and are differentiated by the `relationship:` frontmatter wiki-link. |

When in doubt, the answer is **no new folder** — find the existing one, or write the file as a leaf inside it.

---

## Validate markdown links before push

Before pushing any change that adds or modifies a relative link in an `.md` file, run lychee on the repo:

```bash
lychee --offline --no-progress --exclude '<[^>]+>' './**/*.md'
```

Why: the `Link check` GitHub Actions workflow scans the entire repo on every push, not only the diff. A single broken link introduced in one commit makes every subsequent push fail red until it is fixed, including unrelated pushes from other people. Running lychee locally takes about 30ms and catches the issue before it lands.

Scope: applies to every agent (Claude Code, Cursor, Codex) and every human committer. Install lychee (`brew install lychee` on macOS), or open a PR so CI runs before merge instead of after.

---

## Sync model

`origin/main` on GitHub is the **single source of truth**. Every laptop has a local working copy. The model is intentionally minimal so a non-technical team can work in parallel without learning Git.

### Who writes

Two **write** channels:
- **Claude Code skills** — every write-bearing skill runs the full pull-rebase / write / commit / push loop. Primary channel; most file edits land via a skill invocation.
- **A local Markdown editor** (e.g. Obsidian, Tolaria) — humans can edit files directly and commit **locally**. Expected and routine. What the local editor does **not** do is push to origin.

One **push** channel: Claude Code. Every session starts with `git pull --rebase --autostash origin main` (the SessionStart hook). Two mechanisms push back: every write-bearing skill ends with `git push origin main` (the skill write-back loop), and a `Stop` hook commits and pushes whatever the turn changed at the end of every turn (the catch-all, for edits that land outside a skill). Local commits made in the editor reach `origin` via these Claude Code runs — the pull-rebase replays them on top of fresh `main`, and the push lands them all together.

Two reasons we keep push centralized in Claude Code:
- Non-technical users won't reliably run pull-rebase-push by hand. The SessionStart and Stop hooks run the loop on the harness side, deterministically, without relying on human or model discipline. Prose rules are prompts the model may skip; hooks are executed every time.
- It eliminates the cross-channel race where two people push concurrently from different editors. Claude Code's retry-on-non-fast-forward handles the race deterministically.

**The local editor's Push button is not in scope. Don't use it.** Push always goes through Claude Code; local-editor commits stay local until then.

If someone hand-edits in the local editor and **doesn't commit**, their changes get **silently overwritten** on the next Claude Code session, which auto-pulls. The discipline is: hand-edit → commit locally → let Claude Code push next time it runs. Uncommitted changes are at risk.

### The universal loop

Every write that lands on `main` follows the same shape:

```
git pull --rebase --autostash origin main   # see what others wrote first
<make the change>
git add <specific files>                    # never -A or .
git commit -m "<message>"
git push origin main
# on non-fast-forward (someone pushed in the interval):
#   git pull --rebase --autostash; retry the push. Max 3 retries.
# on real merge conflict in rebase:
#   abort the rebase, leave working copy clean, surface error
```

The variation is who runs the loop, when.

### Claude Code — SessionStart hook + Stop hook + skill write-back

Three places where the loop runs:

**SessionStart hook** ([`.claude/settings.json`](../.claude/settings.json), committed). Runs at the start of every Claude Code session. Two steps:

1. Pre-clean stale `index.lock` files (older than 1 minute) across the main repo and all worktrees. A real git operation never holds the lock that long; anything older is an orphan from a crashed process (local-editor auto-commit interrupted, killed Claude session, machine sleep mid-write) and would otherwise block every subsequent write until removed by hand. The 1-minute floor protects an actually-active concurrent git from being trampled.
2. `git pull --rebase --autostash origin main` on the current worktree, then `git -C <main> pull --ff-only` on the main repo if running from a worktree. Working copy is fresh by the time the user types their first prompt. Fails soft on network/auth issues (warns, does not block).

If you hit a stale lock outside a Claude Code session (the local editor shows "Sync failed", a manual `git` command errors with "another git process seems to be running"): run `find <main>/.git -name index.lock -mmin +1 -delete` from anywhere in the tree. Same rule, 1-minute floor.

**Skill write-back**. Every skill that writes files runs the full loop as its closing step. **Mandatory.** With 3-retry on non-fast-forward and clean abort on persistent failure. It stages specific files (`git add <files>`, never `-A`) because the skill knows exactly what it wrote.

**Stop hook** ([`.claude/settings.json`](../.claude/settings.json), committed). Runs at the end of every turn. The catch-all: it commits and pushes any change left in the working copy that a skill did not already push, so an edit made with a bare Edit/Write call (no skill invoked) still reaches `origin` instead of sitting uncommitted until the next pull silently overwrites it. Because it cannot know which files the turn meant to touch, it stages broadly (`git add -A`) — the one place `-A` is intentional, distinct from the skill loop above. Three guards keep it safe: it **skips entirely when the diff touches `canon/`** (canon stays PR-only, see "Changes — push to main, with one exception"), it **skips on a lychee broken-link failure** (so an auto-push never red-walls CI), and it **retries the push on non-fast-forward** (pull-rebase-autostash, then re-push). Fails soft: a commit always lands locally even if the push can't.

The SessionStart hook covers "the working copy is fresh when the user starts". The skill loop covers "the working copy is fresh at the moment a skill writes, and that write lands on main atomically". The Stop hook covers "nothing a turn changed is left behind uncommitted", regardless of whether a skill ran.

### Local editor — local writes OK, push goes through Claude Code

The local editor reads from the local working copy, and **may write to it** — edit files freely, commit locally if you want. To stay fresh with origin, the user can either:
- Open Claude Code first (the SessionStart hook pulls), then switch to the editor.
- Or just `cd <vault> && git pull --rebase` in a terminal.

The editor's UI may also have its own Pull command — fine to use, but not required.

**The local editor's Push button is not in scope.** Don't use it. Push always goes through Claude Code (SessionStart pull + skill write-back); editor-local commits ride along on the next Claude Code run.

If the editor is opened on a stale working copy, the user sees stale data. The cost is a "what's new" question, not data loss. The next Claude Code action pulls. Uncommitted in-progress edits in the editor are the only thing at risk of silent overwrite — commit them locally before walking away.

### `canon/` exception

`canon/**` changes go through a PR, never direct push. The skill loop is the same shape, but on a feature branch:

```
git switch -c canon/<short-purpose>
<edit, commit on the branch>
git push -u origin canon/<short-purpose>
gh pr create
# wait for owner approval, then merge
```

A GitHub Action (the canon direct-push guard, under [`.github/workflows/`](../.github/workflows/)) fails the workflow if any commit on `main` modifies `canon/**` without being a merge commit. Document the full enforcement setup and the upgrade path to a preventive Ruleset alongside the workflow.

### Failure modes — quick reference

| Symptom | What it means | What to do |
|---|---|---|
| Claude Code session prints "⚠️ git pull failed" at start | The SessionStart hook couldn't reach origin (offline, auth issue) | Continue if you trust your local state. Otherwise check `git status` and `git fetch` manually. |
| A skill aborts with "push rejected after 3 retries" | High write contention: 3 other writes landed during your loop | Re-run the skill in 30-60 seconds. If it still fails, ping the operations lead. |
| A skill aborts with "merge conflict during rebase" | Two writers edited the same lines in the same file in the same window | Operations lead resolves manually. Surface the conflicting paths to the lead with the timestamps of both writes. |
| Working copy diverged by many commits | Someone was offline for too long | Don't push anything from the stale machine. Operations lead helps reconcile. |
| The local editor shows "Sync failed" / `git` errors with "another git process seems to be running" | Stale `index.lock` from a crashed git op (auto-commit, killed session, sleep mid-write) | The next Claude Code SessionStart hook auto-removes stale locks (>1 min). To unstick immediately: `find <main>/.git -name index.lock -mmin +1 -delete`. |

### What's deliberately not in this model

- **No background daemon** (launchd / cron). The SessionStart hook covers when the user opens Claude Code; the skill loop covers every write. A daemon adds install complexity on every laptop for no marginal safety.
- **No file locking.** Optimistic concurrency via pull-rebase-retry is enough at this team size.
- **No CRDT layer.** Markdown + Git scales fine to this team size. Revisit at ~50 people.
- **No human push channel via the local editor.** See the read-only rule above.

### Per-user install — one-time setup

1. Clone the repo to the chosen vault directory (the team standard is `~/<company>/ai-native-company/`).
2. Configure git auth: `gh auth login` (preferred) or an SSH key.
3. Install Claude Code. The SessionStart hook in `.claude/settings.json` is picked up automatically the next time Claude Code opens this directory.
4. Open the local Markdown editor and point it at the vault directory.

That's it. No scripts to install, no plist to load. Sync runs by itself thereafter.

---

## Host-memory reconciliation at session start

Host-layer memory (Claude Code's per-user `~/.claude/.../memory/`, the Cursor / Codex / ChatGPT equivalents) is per-person and per-machine: it never reaches a teammate, and it is always secondary to this repo ([`company-rules.md`](company-rules.md) → "Order of truth"; the CLAUDE.md / AGENTS.md boot doc → "Host-layer memory is secondary"). The only layer the whole team shares is the repo, so a personal habit stored locally must never quietly degrade company work. Every agent reconciles the local memory against the repo at session start, non-destructively, touching only what's needed:

1. **Seed the baseline, if absent.** Ensure the local memory carries the three facts that keep it subordinate to the repo: (a) for any company work the repo is the absolute source of truth, and on conflict the repo wins, re-read it rather than trust a local note; (b) skills are authored in the repo under `skills/<slug>/`, never in `~/.claude/skills/`; (c) produced artifacts follow canon Voice, including the flat no-em-dash rule ([`company-rules.md`](company-rules.md) → "Voice").
2. **Reconcile conflicts.** Scan for local entries that contradict canon: a personal style rule that fights a Voice rule (a habit of em-dashes, say), or a stale company fact that current canon supersedes. Canon wins: correct the local note or mark it superseded, and tell the user what changed. Never silently delete a preference that merely *differs* without contradicting.

Leave purely personal memory alone: writing style, who the user is, how they want you to work. That is what local memory is for. The goal is subordination to the repo on company matters, not erasing the person.

Triggering is a SessionStart concern. The directive lives here (canon, shared) and the reconciliation runs in-session (it is judgement, not a deterministic script). A SessionStart hook should inject this reminder so it fires every session rather than depending on the agent re-reading the rule; prose alone does not reliably execute (see "Sync model" for the hook pattern).

---

## Task-routed team loading

**Beyond canon, the agent loads the team rules that own the *task*, not just the team of the user.**

The boot sequence in [`CLAUDE.md`](../CLAUDE.md) / [`AGENTS.md`](../AGENTS.md) loads canon plus the user's team. That answers *who is asking*. It does **not** answer *what is being done*. A leadership user asking for a price quote is doing **sales** work; the rules that govern a quote live in [`canon/team/sales.md`](team/sales.md) (price sheet, deliverables, approval thresholds, customer-facing tone) — not in [`canon/team/leadership.md`](team/leadership.md). Reading only the user's team and improvising on the rest is a bug.

**The rule.** Before answering a request or producing a write, the agent identifies the **kind of task** and loads `canon/team/<owner-team>.md` for the owning team. When freshness matters, read the relevant [`signals/`](../signals/) for that work rather than a precomputed digest. If the task spans multiple teams, load all of them. The user's-team load and the owner-team load are independent — both apply, regardless of overlap.

**Routing examples — non-exhaustive.** (Teams shown are the example set; map them to your own functional teams.)

| Kind of task | Owner team |
|---|---|
| Price quote, commercial conversation, outbound, deal scope, CRM hygiene | [`team/sales`](team/sales.md) |
| Bug investigation, infra change, code review, platform reliability | [`team/engineering`](team/engineering.md) |
| Brand voice, positioning, external messaging, website copy, social, PR | [`team/marketing`](team/marketing.md) |
| Feature spec, roadmap, product definition, product strategy | [`team/product`](team/product.md) |
| Customer-delivery, onboarding, configuration, data ingestion, customer ops | [`team/customer-success`](team/customer-success.md) |
| Founder / executive decision, cross-team strategy, hiring, governance | [`team/leadership`](team/leadership.md) |

When the routing is ambiguous (a sales-engineering edge case on integration scoping; a product-marketing piece bridging spec and external copy), load **all** plausibly-relevant teams rather than pick one. Two loads cost cheap context; one wrong load misses constraints.

**What this is NOT.** This is not a permission rule — a leadership user can still ask for a price quote and a sales user can still ask for a bug fix. It's a *context-loading* rule: the agent reads the rules that own the work it's about to produce, so it doesn't violate them by ignorance.

---

## Tagging discipline at write time

Before saving any file with body prose (signals, articles, recap outputs, anything that mentions external entities), scan the body for mentions of entities and resolve every mention to a wiki link when possible.

| Entity type | Resolution |
|---|---|
| **People — internal** (`company: [[<company>]]`) | Match against [`people/<slug>.md`](../people/) aliases. Wiki-link if MD exists. **Never seed** internal MDs from a body-mention — that slice is curated by humans. |
| **People — external** (anyone whose `company:` is not `[[<company>]]`) | Match against [`people/<slug>.md`](../people/) aliases. If MD exists → wiki-link. If MD does not exist → **query the CRM**; if the CRM has a record, create `people/<slug>.md` with `company: [[<their-company>]]` and `crm_url` populated (see [In-repo external entities](#in-repo-external-entities--create-on-first-mention)) and wiki-link; if no record, plain text. |
| **Companies** (external orgs) | Match against [`companies/<slug>.md`](../companies/) aliases. If MD exists → wiki-link. If not, query the CRM → create-with-`crm_url` or leave plain text. The home company [`companies/<company>.md`](../companies/) (`is_self: true`) is curated by humans, same posture as internal people. |
| **Teams** | Match against [`team/<slug>/`](../team/). Wiki-link if folder exists. |

The rule applies to **every write**, including AI-generated outputs. A skill must perform the CRM lookups inline; the human reviewer should not have to chase missing wiki links after the fact.

---

## Wiki-link relations — always double-write

The graph in this repo lives in the **frontmatter** of each MD via wiki-link relations like `team: [[<team>]]`, `industry: [[<industry>]]`, `uses_product: [[<product>]]`. A local Markdown editor renders each as a dedicated section in the Properties panel. **But** the editor's automatic inverse only fires on the default relation names (`belongs_to` / `has` / `related_to`); for our semantic field names we declare both sides of every edge manually.

**Rule — double-write**. Every time you add or remove a forward wiki-link in one file, add or remove the matching reverse in the partner file **in the same write**. Half-pairs break the dedicated inverse section on the partner page (the link falls into the editor's generic Referenced by view) and break "show me all X" queries for AI agents that read structured frontmatter.

The full table of forward + reverse pairs lives in [`anatomy.md`](anatomy.md) → "Frontmatter wiki-link relations — forward + reverse pairs". Consult it whenever you need to know which field on which file connects to which other field on which other file.

**Exception — signals**. Signals (`signals/*/*/*.md`) declare forward `author`, `teams_impacted`, `related_to_people`, `related_to_companies`, `related_to_products`, `related_to_systems`, `related_to_themes`. As a general rule they do **not** get a reverse on the cited entity — signals grow without bound and reverse-maintenance on unbounded taxonomies (people, companies, products, systems) would explode. Inverse lookup on those target pages uses the editor's generic Referenced by view.

**Exception to the exception — themes**. `themes/` is a small, bounded taxonomy that's high-value for cross-signal queries ("show me every meeting that touched a given theme"). The reverse `themes/<theme>.md → signals:` **is** declared manually — the maintenance cost stays linear because themes are sparse and a theme rarely lands on more than a handful of signals per week. Double-write applies: when you add or remove `related_to_themes` on a signal, update the matching `signals:` list on each theme file in the same write.

Agent producers (skills, future agent code) must do the double-write at production time. A human reviewer should not have to chase missing reverses after the fact.

---

## Cross-team rule

If a contribution touches more than one team, **one MD only**, in a cross-cutting folder ([`signals/`](../signals/) for signals, [`people/`](../people/) for people), with links to every team affected. Do not duplicate the file under each team folder. Do not pick a "main team".

---

## In-repo external entities — create-on-first-mention

The repo holds **lightweight MDs** for external entities under [`companies/`](../companies/) (every org except the home company) and [`people/`](../people/) (every external person — anyone whose `company:` is not `[[<company>]]`). Their only job is to be **stable wiki-link targets**: a `grep "companies/<slug>.md"` across `signals/` returns every recap that touched that company, without the CRM needing to know about the repo and without the repo duplicating the CRM.

The full body asymmetry rule (external = light body, internal = body can grow, `crm_url` mandatory) lives in [`anatomy.md`](anatomy.md) → "Body asymmetry — same folder, different body weight". The rule below is the **procedure on every interaction with an external entity**, not just at file write time.

**Posture: bias toward creation.** The graph grows by every interaction with the CRM-gated set, not by periodic curation. Every conversation that names a real customer, prospect, partner, vendor, advisor, or external contact is an opportunity to land another stable node. The default move on encountering a name that isn't in the repo is `query CRM → propose creation`, not "note and move on". An agent that handles a request about an external entity and leaves the graph unchanged when the CRM had a record is doing the wrong thing.

**Creation rule.** On first contact with an external person or company, if the CRM has a record, create the MD. The trigger is **any task that names an external entity**, not only file writes. Examples that fire the rule:

- Writing a file (signal, article, recap) that mentions the entity.
- Drafting an email, a message, or any outbound where the entity is the recipient or the subject.
- Answering a conversational question about a deal, a meeting, an account, or a person ("what's the status on X?", "who at Y signed off?").
- Planning, scoping, or routing work that turns on the entity (a price quote, a kickoff, an integration task).
- Any read of source material (transcript, email thread, CRM dump) where the entity surfaces and the agent is acting on it.

On the trigger, **check the CRM** (your CRM via MCP for skills, the CRM UI for humans). If the CRM has a matching record, **create** `people/<slug>.md` (with `company: [[<their-company>]]`) or `companies/<slug>.md` immediately with `crm_url` populated, then render the mention as a wiki link in any artifact produced and proceed with the original task. If the CRM has no matching record, leave the mention as **plain text**, flag the gap, and do not create an empty MD.

The CRM is the gating signal: if the CRM says they exist, they're in scope for the repo's wiki-link graph; if not, they're plain text until they show up in the CRM. The trigger is the *interaction*, not the *write* (a query without a written artifact still grows the graph).

**Internal people are different — never seed from a mention.** An MD in `people/` with `company: [[<company>]]` is the home team. That folder slice is **human-curated**; agents never create or modify an internal person MD based on a transcript or signal mention. Resolve the alias if the MD exists; otherwise leave plain text and ask the human.

**`crm_url` is mandatory for external MDs, with one exception.** External `people/<slug>.md` and `companies/<slug>.md` may not exist without a populated `crm_url`. The CRM URL is the entity's birth certificate in this repo: no URL, no MD. **The exception**: `companies/<slug>.md` with `relationship: [[competitor]]` carries no `crm_url`. Competitors are tracked for competitive intelligence and are not in your CRM by design. The `industry` field is also optional on competitor company MDs, since competitors are not in the verticals the company sells into. If the writer cannot resolve `crm_url` at creation time for a non-competitor external entity (no CRM access in that session), they leave the mention as plain text and flag the gap. They do not create a half-blind MD. Internal people may also have `crm_url:` empty (if they're a CRM user/owner rather than a contact record).

---

## Relationship is mutable state — sync from the CRM

The `relationship:` field on `companies/<slug>.md` is **state, not a one-time tag**. Prospects sign and flip to customer; customers churn; partners upgrade to customers; advisors take on a vendor role. Transitions happen in the CRM first — the repo trails it.

**Source of truth: the CRM company record's lifecycle stage** (custom-mapped to the [`relationship-types/`](../relationship-types/) enum by leadership). Before any write that materially depends on the value — creating a `companies/<slug>.md`, producing a signal or recap whose body asserts a relationship ("we're pitching <prospect> on X", "<customer> is rolling out Y"), routing a company through relationship-dependent automation, running a periodic re-scan over the company set — the agent queries the CRM, compares against the MD, and updates the MD if drifted. Silent drift is a graph bug: a signal that assumes `[[prospect]]` is wrong if the CRM already flipped them to customer; an outbound to a `[[customer]]` who churned six weeks ago is worse.

**Reverse-link discipline applies on every flip.** When `relationship: [[prospect]]` becomes `relationship: [[customer]]`, the same write also removes `[[<company>]]` from `relationship-types/prospect.md` and adds it to `relationship-types/customer.md` (per [`anatomy.md`](anatomy.md) → "Frontmatter wiki-link relations — forward + reverse pairs"). Half-pairs break the "show me all customers" query.

**When the state has no slot in the enum** — today the enum is `customer / prospect / partner / vendor / advisor`. Churned customers, lost prospects and dormant accounts have no native slot; surface the gap and propose extending the enum via a canon PR rather than silently parking the company under a wrong type or stripping the `relationship:` field. The handling of churned ex-customers specifically is described in [`relationship-types/README.md`](../relationship-types/) — keep that doc and this rule in sync.

The full conventions live in [`companies/README.md`](../companies/) and [`people/README.md`](../people/).

---

## Product catalog — source of truth is the docs system

The product catalogue is owned by Product, and its **system of record is the docs system** (e.g. Notion, Confluence) — the "Products" database lives at `<docs-url>`, not in the repo. Products are the AI agents / automations your company builds or sells. This is a deliberate exception to the default taxonomy treatment in [Entity-scan discipline](#entity-scan-discipline): for products, **the docs system wins on content**.

`products/<slug>.md` in the repo is a **pointer, not a copy**, the same posture `companies/<slug>.md` has toward the CRM via `crm_url`:

- Each `products/<slug>.md` carries a `docs_url:` pointing to its docs-system page, joined on `slug` (the repo `slug` mirrors the docs-system `Slug` property).
- The repo MD is thin: `name`, `slug`, `category`, `docs_url`, and a short `## About`. It carries **no relational wiki-link fields**. The fit / used-by / interested-role relations (`industries_fit`, `interested_roles`, `used_by`) are properties on the docs-system row now, not repo edges. Keeping them only in the docs system is what makes it the single source of truth.
- Do not hand-edit product content expecting it to flow back to the docs system. Change it there; the repo MD trails it. A new product is born in the docs system, then an agent creates the stub MD with `docs_url` (same shape as create-on-first-mention for companies).

Docs-system database: `<docs-url>`.

---

## Entity body discipline — stable facts + staleness check

The body asymmetry rule in [`anatomy.md`](anatomy.md) → "Body asymmetry — same folder, different body weight" describes *where* the truth lives (external entities = light body, internal entities = body can grow). This section is the cross-cutting rule for *what* the body may contain — **and what every agent must do when it reads one**.

### Stable facts only

Every entity MD body — `themes/<x>.md`, `products/<x>.md`, `systems/<x>.md`, `industries/<x>.md`, `job-titles/<x>.md`, `relationship-types/<x>.md`, every external `companies/<x>.md`, every external `people/<x>.md` — carries **stable, definitional content only**. The `## About` answers *"what does this entity mean / what is this thing in the company's frame"*, in 1-2 sentences, using facts that don't move week to week.

What is **forbidden** in any entity body:

- Operational state ("MVP scheduled for next month", "currently in beta", "rolling out to first customer", "deal in negotiation").
- Project / initiative status that lives elsewhere — those belong in `signals/` or the relevant external source of truth.
- Relationship-vivid prose ("we're pitching them on X", "they churned in Q1", "last touch was Tuesday") — that's CRM state.
- Lists of layers / phases / sub-features that exist only because the implementation is in flight.
- Anything that a reader six months from now would have to verify before trusting.

The exceptions are exactly the cases the body-asymmetry rule already names: **internal** people MDs and the home `companies/<company>.md` can grow scope / anti-scope / voice / operating-style sections because the repo *is* the source of truth there. Every other body stays definitional.

If an agent feels a need to write operational state on an entity body, it's writing in the wrong place. Route it to a signal or the external source of truth.

### Proactive staleness check on read

When an agent reads an entity MD as part of any task (recap, brief, signal write, prospect email, internal Q&A), it scans the body for content that **may have drifted** since the MD was last written — definitions that are now narrower or wider, "About" lines that hint at an in-flight state that has since landed, descriptions that pre-date a known pivot, anything the agent has reason to suspect is no longer accurate.

When the agent notices a suspect line, it **surfaces the suspicion to the user in chat** — concise, specific, with the current text and why it looks stale — and asks whether the line still holds. If the user confirms drift, the agent updates the MD in the same loop (push-to-main per the standard flow, since entity bodies outside `canon/` are not PR-gated). If the user confirms the line is still accurate, the agent moves on without editing.

Silent pass-through of suspect content is a bug. Silent rewrite without asking is a worse bug. The discipline is: **read → suspect → ask → write only on confirmation**.

This applies symmetrically to external and internal MDs. The difference is the source of truth used to triangulate: the CRM for external companies / people (see [Relationship is mutable state — sync from the CRM](#relationship-is-mutable-state--sync-from-the-crm) for the structured-field equivalent), the user / team-rules / signals for internal entities, the agent's own judgment + a question to the user for taxonomy entities (themes, products, systems, industries, job-titles).

---

## Entity-scan discipline

**Any text → scan for new entries; any conversation → scan for missing entities.** Every time an agent reads source content (meeting transcript, email thread, chat message, doc, CRM dump) **OR** responds to a user message that names an entity, its first obsession is: *is this entity in the graph? If not, what's the right create / alias / link action?* This is the entity-level manifestation of the umbrella stance in [Agent stance — proactive, not refusing](#agent-stance--proactive-not-refusing) above. Across all eight entity types in [`anatomy.md`](anatomy.md) → "Domain map":

1. [`themes/`](../themes/) — topics, frameworks, concepts the company reasons about
2. [`people/`](../people/) — internal team members (human-curated) + external contacts (CRM-gated)
3. [`companies/`](../companies/) — every external org (CRM-gated)
4. [`industries/`](../industries/) — verticals / market segments
5. [`relationship-types/`](../relationship-types/) — fixed enum (`customer`, `prospect`, `partner`, `vendor`, `advisor`) — **never create new**
6. [`job-titles/`](../job-titles/) — canonical roles
7. [`systems/`](../systems/) — external software platforms customers run
8. [`products/`](../products/) — company offerings

Silent skip is a bug. An unfamiliar capitalized term, a recurring framework name, a new platform mention — each must go through the steps below before the writer commits the output.

### Aliases are first-class

The `aliases:` field is not a nice-to-have — it is the only check that prevents the graph from growing duplicate MDs for the same entity over time. Three rules:

- **Every MD is born with rich aliases.** At create time, populate every variant the writer can predict: first-name shortenings, full-name combinations (firstname-lastname, lastname-firstname), domain / handle (email local part, social handle), known misspellings. **Empty aliases on an MD is a bug, not a default.**
- **Backfill aliases in use.** When the entity-scan loop encounters a term that *should* match an existing MD but doesn't because the alias is missing, the triage prompt offers `add alias X to <existing-md>` as the canonical action — *not* `create new MD`. The graph stays single-vertex per entity over time.
- **The alias index is global.** Fuzzy-match runs across the aliases of every existing MD in every folder, not just the folder the candidate seems to belong to. A capitalized phrase in a transcript might land on a [`themes/`](../themes/) entry, a [`systems/`](../systems/) entry, a [`companies/`](../companies/) entry — the writer checks all of them.

### Fuzzy-match-before-create

Before creating any entity MD, the writer **must** fuzzy-match the candidate against the global alias index. Match rules, in order:

1. **Exact alias hit** → wiki-link the existing MD. Done.
2. **High-confidence fuzzy match** (Levenshtein-close, common shortening pattern, same email domain, plausible misspelling) → propose `alias-merge: add "<candidate>" to <existing-md>` to the invoker. Do not silently merge.
3. **No match, entity type is CRM-gated** (`people/` external, `companies/`) → query the CRM via MCP; if hit, propose `create with crm_url`; if miss, plain text.
4. **No match, entity type is taxonomy** (`themes/`, `industries/`, `job-titles/`, `systems/`) → propose `create new` if the per-type heuristic below is met; otherwise plain text. (`products/` is excluded here: it is born in the docs system, never from an entity-scan, see [Product catalog — source of truth is the docs system](#product-catalog--source-of-truth-is-the-docs-system).)

### Per-type creation heuristics

| Type | When to propose creation |
|---|---|
| `themes/` | Concept / framework explicitly named and either (a) discussed as a *thing* (not just a passing word) or (b) recurring across ≥ 2 distinct calls / threads / docs in the source set. |
| `people/` — external | CRM has a record. Otherwise plain text. |
| `people/` — internal | **Never create from a body mention.** Human-curated. |
| `companies/` (customer / prospect / partner / vendor / advisor) | CRM has a record. Otherwise plain text. |
| `companies/` (competitor) | Add when monitoring as a tracked competitor in your competitive intelligence. No CRM record required. `crm_url` and `industry` are optional for this relationship type. |
| `industries/` | A new vertical the company is selling / actively prospecting into, **or** an industry term recurring across ≥ 2 distinct sources. Don't pre-seed a vertical from a single passing mention, but recurrence on its own is enough to propose. |
| `relationship-types/` | **Never create.** Fixed enum (`customer`, `prospect`, `partner`, `vendor`, `advisor`). |
| `job-titles/` | A canonical title an external person holds that isn't covered. Propose once the title recurs (≥ 2 mentions / sources) or is strategically meaningful; skip transient one-offs. |
| `systems/` | External software platform a customer / prospect runs (commerce platform, PIM, payment provider, …). Create on first confirmed mention; recurrence reinforces. |
| `products/` | **Never created from an entity-scan.** Born in the docs-system "Products" database (the system of record); the repo MD is a thin pointer created with `docs_url` once the product exists in the docs system. See [Product catalog — source of truth is the docs system](#product-catalog--source-of-truth-is-the-docs-system). |

### Link what exists; recurrence triggers a create

Two failure modes are explicitly bugs, not style choices. Both lean the same way: **under-linking is the default failure of the entity-scan, so bias toward the link and let the invoker say skip.**

1. **A term that maps to an existing MD must be linked, every time.** When the file you are writing names an entity that already has an MD (a system, an industry, a theme, a job-title), you populate its frontmatter edge in the same write, forward **and** reverse per the pairs table in [`anatomy.md`](anatomy.md) → "Frontmatter wiki-link relations". Leaving it as plain prose is an incomplete write, not a stylistic choice. The most-missed edges are the taxonomy ones: `companies/<x> → uses_stack: [[<system>]]`, `companies/<x> → industry: [[<industry>]]`, `signals/<…> → related_to_systems / related_to_themes`. A company body that names its commerce platform while `uses_stack` sits empty is a bug.

2. **Recurrence is itself a create trigger.** Do not wait for a term to feel "important" before proposing it. For the taxonomy types (`systems/`, `industries/`, `themes/`, `job-titles/`), a term that recurs (≥ 2 mentions in the text being processed, or across ≥ 2 distinct sources) is enough to land on the triage list as a `create` candidate. The per-type heuristics above set the floor; recurrence clears it.

This applies on every read and every write, not only to create-bearing skills: when answering a question that leans on a system or vertical that already has an MD, reference it by its node, not as a loose noun.

### Triage before write — never silent inference

Skills and agents that produce a write (signals, recaps, articles) **must batch their candidate list and present it to the invoker in chat before producing the output file.** Shape:

> Reading today's transcripts I found candidates that aren't in the graph yet:
>
> - **`<candidate A>`** — high-confidence fuzzy match to [[<existing-entity>]] (same context, common misspelling). Add as alias?
> - **`<candidate B>`** — CRM has a record `<id>`. Create `companies/<candidate-b>.md`?
> - **`<candidate C>`** — discussed as a framework in 4 calls today. Create `themes/<candidate-c>.md`?
> - **`<candidate D>`** — CRM no match. Plain text in the recap?
>
> Reply with the actions to take (add-alias / create / skip) and I'll write the recap with the right tags.

After the invoker triages, the writer applies the choices and produces the file. Silent inference — guessing a match, silently creating, silently skipping — is forbidden. The graph evolves by explicit choice, not by inference drift.

Internal people are still off-limits to skills (see [In-repo external entities — create-on-first-mention](#in-repo-external-entities--create-on-first-mention) above). The triage list flags ambiguous mentions ("seen `<short name>` — alias on internal [[<person-slug>]]?") but every creation / alias edit on an internal MD is **confirmed by a human writer**, never auto-applied by a skill.

---

## Outbound drafts: create in the tool, not the chat or a repo file

When the user asks for an email ("write a mail to X", "draft an email", "reply to Y"), the agent **creates the draft inside the email tool** via the email MCP (`create_draft`). The user reviews, edits, and sends from the email UI. The chat may include a one-line note ("draft created in your email tool, subject: ...") and the body for inline review, but the canonical artifact lives in the email tool.

**The hard anti-pattern: never save the draft as a repo file.** Writing an email body to `team/<team>/<topic>-email.md`, `signals/manual/<topic>.md`, or anywhere else inside this repo is **forbidden**. The email is a deliverable, not knowledge: it ships from the email tool and lives there. Same logic for any audience-facing document drafted alongside the email (an All-hands page, a partner brief, a deck narrative). The chat note may summarize the deliverable, but the artifact materializes in its destination tool (docs-system page via MCP, deck in file storage, etc.), never in a `.md` file in this repo. See [Repo scope: knowledge only](#repo-scope-knowledge-only) for the underlying classification.

The same blocking rule applies to other outbound surfaces when a tool is connected:

- Email → email MCP (`create_draft`).
- Calendar event → calendar MCP.
- Docs-system page (audience-facing doc) → docs-system MCP.
- CRM note → CRM MCP (see [CRM writes](#crm-writes--notes-over-description-role-disambiguation) below for the Note-vs-Description detail).

**If the relevant MCP is not connected** in the current session: surface the gap explicitly ("the email MCP is not connected here, I can give you the body in chat for you to paste") and fall back to chat text only after flagging. Never silently fall back to writing a `.md` file as a substitute.

Entity-scan discipline still applies: any recipient or named external entity in the draft triggers the create-on-first-mention rule above before the draft is written.

## Write in the subject's voice

Any voiced outbound the agent drafts on behalf of or for a specific person carries **that person's voice**. This covers LinkedIn posts, emails, DMs, messages, and the narrative parts of a deck or page that ship under someone's name. The previous section governs *where* the draft lands; this one governs *how* it reads.

**Load the voice from the subject's MD.** Per-person tone of voice lives in the body of [`people/<slug>.md`](../people/), in a "How I write (tone of voice)" or Operating-style section. There is no external system that holds an internal person's voice, so the repo is the source of truth for it (see [Entity body discipline](#entity-body-discipline--stable-facts--staleness-check) and [`anatomy.md`](anatomy.md) → "body asymmetry"). Read that section before drafting and match it.

**The voice is set by the subject, not the requester.** This is the part agents get wrong. When a teammate asks "write a LinkedIn post for <founder>", the post is written in that founder's voice, not the teammate's and not a generic house voice. The subject of the artifact picks the voice; the requester only picks the task. This is what lets anyone on the team draft on anyone's behalf without collapsing everyone into one flat tone.

**Voice is per-surface.** The same person writes differently across surfaces: an internal team email, a public LinkedIn post, and a warm peer DM are three different registers for one author. Match the surface, not only the person. A well-kept voice section already breaks this down by surface; honor those distinctions.

**No voice section yet.** If the subject's MD has no voice section, infer the voice from their own past writing (previous posts, emails, signals they authored), draft in that inferred voice, and propose adding a "How I write" section to their MD so the next draft starts from a recorded voice instead of a re-inference. For an external person, where the repo holds no voice body by design, infer from available samples and keep the draft conservative.

**The company floor still applies.** A personal voice sits on top of the company-wide Voice rules, it does not override them: [`company-rules.md`](company-rules.md) → "Voice", including the flat no-em-dash rule and the hard exclusions. Personal style flexes the register; it never licenses content canon forbids.

## CRM writes — Notes over Description, role disambiguation

The CRM (e.g. HubSpot, Salesforce, Pipedrive) is the source of truth for external entities (people, companies, deals). Two write-time rules govern how the agent puts content into the CRM, regardless of which team's skill is producing the write.

### Note engagement, not Description field

When the user asks the agent to "write a note", "add context", "log the call", or "put the info" on a CRM deal or company, the agent creates a **Note engagement** (a timeline activity), associated to the deal **and** the parent company in the same write. **Do not put free-text context in the `description` property.**

Why: the `description` field lives in the right-sidebar "About this deal" panel and is easily missed in the CRM UI. Note engagements appear in the central activity timeline, which is where the team actually reads context. A note buried in `description` is a note nobody sees.

- HTML body is fine (`<ul><li>...</li></ul>` for bullets, `<b>` for headers). Set the note timestamp to write time.
- Use `description` only for a short structured one-liner (deal subtitle, brief tag).
- Skip the "Description vs Note" confirmation prompt. Default to Note.

### `jobtitle` is per-contact, not per-associated-company

A contact's CRM `jobtitle` field reflects how the person was first captured and may refer to a **different** entity than the company currently associated with them (a personal venture, a previous role, a board seat held elsewhere). **Never infer role-at-the-associated-company from this field.**

When creating a `people/<slug>.md` MD or writing any artifact (signal, draft email, outbound, CRM note) that asserts a specific role at a specific company, **confirm the role with the user or another source** before writing. Treating `jobtitle` as authoritative for any company other than the one explicitly named alongside the title is the failure mode this rule catches.

---

## Output spec vs content filters — where each rule lives

When writing or extending a rule about how an output looks or what it may contain, distinguish two kinds of rule and route each to its own home:

- **Output spec.** *How* the output looks: which sections appear, formatting, what to silently drop from the body, ordering, file naming. Lives in [`skills/<slug>/SKILL.md`](../skills/). Each skill owns the shape of its own artifacts.
- **Content filter.** *What* may or may not appear in any output produced under a team's umbrella: HR-tinged commentary, equity, fundraising, individual performance, NDA content. Lives in [`canon/team/<team>.md`](team/). Team rules carry the policy; skills inherit it.

Why the split: the same content filter (e.g. "no individual performance commentary in any leadership output") applies to every skill the team owns. Restating it inside each skill duplicates the policy and creates drift. Conversely, an output-shape rule (e.g. "the recap has no `Calls dropped` section, drops are silent") is specific to the artifact and belongs with the procedure that generates it.

Quick test before writing the rule: *does this constrain what can appear regardless of which skill produces it, or how the artifact looks regardless of policy?* The first is a content filter, route to team rules. The second is an output spec, route to the skill.

---

## Skills — proactive creation

Reusable behaviors live under [`skills/`](../skills/) at the repo root. The full schema (naming, frontmatter shape, body conventions, `owner:` vs `creator:` distinction) lives in [`skills/README.md`](../skills/). The rule below is the **procedural one**: when to propose creating a skill, and how to handle invocations.

**Proactive creation — propose, don't refuse.** When a user asks for a task that has no matching skill, the agent does **not** refuse and does **not** improvise silently. The agent inspects [`skills/`](../skills/) to confirm nothing fits, then **proposes** creating a new skill (using `skill-creator`) with explicit `owner:` and `creator:` identified. Improvising an ad-hoc pipeline that should have been a skill is a bug — the next person hitting the same task gets nothing.

**Invocation.** When the user invokes an existing skill by name, find the file and follow its system prompt. If the user's team (the `team:` wiki-link in `people/<slug>.md`) does not match the skill's `owner:` wiki-link target and the skill is not personal to them, refuse the invocation and point them to the team-equivalent.

**Skills live only in this repo, for everyone.** Every skill (team-shared or personal, experiments included) lives under [`skills/`](../skills/) inside this repo. Never place a skill in `.claude/skills/` (project-local) or `~/.claude/skills/` (user-level): those locations are off-limits, so the whole team gets every skill from the repo on the next pull.
