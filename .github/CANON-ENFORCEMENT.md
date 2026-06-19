# canon/ enforcement — current state and upgrade path

The rule:
- Default workflow: push direct to `main`.
- Exception: `canon/**` → every change goes through a PR reviewed by the owner.

> SETUP: replace `<org>/<repo>` and `@your-github-handle` below with your real GitHub org/repo and the owner handle.

## Current state — free plan (private repo)

GitHub Rulesets and Branch Protection are not available on the free plan for private repos. The rule is enforced reactively, not preventively:

1. [`.github/CODEOWNERS`](CODEOWNERS) auto-requests `@your-github-handle` as the required reviewer when a PR touches `canon/**`.
2. [`.github/workflows/canon-direct-push-guard.yml`](workflows/canon-direct-push-guard.yml) runs on every push to `main` and fails the workflow if any commit modifies `canon/**` without being a merge commit (i.e. without coming from a PR). The check fails *after* the push lands — visible as a red ❌ on the commit + notification.

Discipline + visibility, no server-side block.

## Upgrade path — GitHub Pro / Team / public repo

When the repo moves to a plan with Rulesets, replace the reactive Action with a preventive Ruleset.

### Recommended setup — Ruleset with required status check

This keeps "direct push to non-canon" working while requiring PR-via-merge for canon changes.

1. **Keep [`canon-direct-push-guard.yml`](workflows/canon-direct-push-guard.yml)** — it now becomes the status check the Ruleset enforces.
2. **Create the Ruleset** that requires this Action to pass before any push lands on `main`. Run from the repo root after switching to a plan with Rulesets:

```bash
gh api -X POST repos/<org>/<repo>/rulesets \
  --input - <<'EOF'
{
  "name": "canon-pr-only",
  "target": "branch",
  "enforcement": "active",
  "conditions": {
    "ref_name": {
      "include": ["refs/heads/main"],
      "exclude": []
    }
  },
  "rules": [
    {
      "type": "required_status_checks",
      "parameters": {
        "required_status_checks": [
          {
            "context": "guard",
            "integration_id": null
          }
        ],
        "strict_required_status_checks_policy": true
      }
    }
  ],
  "bypass_actors": []
}
EOF
```

**What this gets you:**
- Direct push to non-canon paths: ✅ works (the guard Action passes — no canon/ change → workflow exits clean).
- Direct push to canon/**: ❌ blocked at merge time. The guard Action fails → the required status check fails → main is not advanced.
- PR-via-merge for canon/**: ✅ works. Merge commit has 2 parents → guard Action passes → status check passes.
- Owner bypass: none. Owner has to open a PR for canon/** just like anyone else (self-approval is fine).

### Alternative — strict ruleset requiring PR for everything

If the team prefers PR for ALL changes (sacrificing direct push to gain uniform review):

```bash
gh api -X POST repos/<org>/<repo>/rulesets \
  --input - <<'EOF'
{
  "name": "all-changes-pr-only",
  "target": "branch",
  "enforcement": "active",
  "conditions": {
    "ref_name": {
      "include": ["refs/heads/main"],
      "exclude": []
    }
  },
  "rules": [
    {
      "type": "pull_request",
      "parameters": {
        "required_approving_review_count": 1,
        "dismiss_stale_reviews_on_push": false,
        "require_code_owner_review": true,
        "require_last_push_approval": false,
        "required_review_thread_resolution": false
      }
    }
  ],
  "bypass_actors": []
}
EOF
```

Not recommended — it contradicts the "push direct to main" default in `canon/operations.md`. Only worth it if the team explicitly decides to tighten the whole repo.

## After applying the Ruleset

1. Verify the Ruleset is active: `gh api repos/<org>/<repo>/rulesets`.
2. Test: try to push a commit touching `canon/foo.md` directly. The push should land but the next workflow run should fail; the status check fail blocks the next push that doesn't fix it. (Or, with strict mode, the push is rejected outright.)
3. Open a real PR touching `canon/**` and confirm the merge requires the green check from `guard`.

Once verified, this file can stay as historical context.
