# canon/

The canon of the repo. The files that govern everyone else.

| File | Purpose |
|---|---|
| [`company-rules.md`](company-rules.md) | The constitution: identity, positioning, voice, non-negotiable values, hard exclusions, order of truth, customer data rules. Scope is tight by design — only what changes if you pivot business. |
| [`operations.md`](operations.md) | How the agent operates: agent stance, write-time procedures, workflow, sync model, citations, source-evidence weight, task-routed team loading, entity-scan discipline, in-repo external entities, wiki-link double-write, entity body discipline. |
| [`anatomy.md`](anatomy.md) | How the graph is shaped: domain map, folder schemas, body asymmetry, glossary, external sources of truth, frontmatter wiki-link forward + reverse pairs table, direction rule, navigation recipes. |
| [`profile.md`](profile.md) | Structured facts about the company — name, markets, leadership, sample customers, integrations, mission, annual themes, goals, pillars, GTM posture, operating principles. |
| [`icp.md`](icp.md) | (Optional) Ideal Customer Profile — segments, size band, geography, persona, pain points, buying triggers, anti-ICP. Drives sales and marketing prioritization. |
| [`competition.md`](competition.md) | (Optional) The competitive landscape — categories, key competitors, USP, where you win, where you lose. Companion to `company-rules.md` → "Positioning". |
| [`team/<team>.md`](team/) | Team-specific rules (per-team Voice / Scope / non-negotiables) — PR-gated as part of canon. One file per team. |

---

## The hard rule — PR-only

**Every modification to any file under `canon/` goes through a pull request.** No exceptions. No direct push to `main`. Owner included.

This protects the canon from accidental drift. The rest of the repo runs on push-direct-to-`main` for velocity; the canon doesn't, because the cost of a stale or wrong rule cascades into every agent run that reads it.

The discipline is enforced two ways:

1. **Convention** — declared here, restated in [`operations.md`](operations.md) → "Changes — push to main, with one exception", and in [`../CLAUDE.md`](../CLAUDE.md) / [`../AGENTS.md`](../AGENTS.md) → §3 "Hard rules".
2. **Server-side** — a GitHub push ruleset on `canon/**` blocks direct pushes to `main` for non-owners. Owners may technically bypass; the convention says don't. This ruleset must be configured separately in GitHub repo settings (see [`../.github/CANON-ENFORCEMENT.md`](../.github/CANON-ENFORCEMENT.md)).

### How to change a canon file

1. Branch off `main`: `git switch -c canon/<short-purpose>`.
2. Make the change. Commit. Push the branch.
3. Open a PR. Title is short and descriptive (subject to the same exclusion rules as the file content — see [`company-rules.md`](company-rules.md) → "What the system MUST NEVER do" → "Scope").
4. At least one CODEOWNER (the repo owner) approves. Self-approval is allowed when no other reviewer is available, but the PR step itself is non-skippable.
5. Squash-merge to `main`. Delete the branch.

### What does NOT live here

- Operational state ([`signals/`](../signals/) and the entities they link) — that's descriptive, not prescriptive. Lives outside.
- Skills ([`skills/`](../skills/)) — owned by their owner / team, pushed direct.
- People and companies — [`people/`](../people/), [`companies/`](../companies/), the domain layer, [`articles/`](../articles/), [`signals/`](../signals/). Push direct.

---

## Follow-up — server-side enforcement

The full setup and upgrade path lives in [`../.github/CANON-ENFORCEMENT.md`](../.github/CANON-ENFORCEMENT.md). Short version: configure a GitHub push ruleset (Settings → Rules → Rulesets → New branch ruleset) targeting `main`, with a file-path condition on `canon/**`, requiring a pull request before merging. This is a manual GitHub repo-settings change — not something a commit can do automatically.
