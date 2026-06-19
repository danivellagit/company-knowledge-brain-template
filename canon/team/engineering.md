# Engineering Rules

> Team: Engineering. Lead: `<Engineering Lead>` (file in [`people/`](../../people/)).
> Takes effect under the company rules. If in conflict with [`company-rules.md`](../company-rules.md), the company rules win.
> Only the Engineering Lead may edit this file.
>
> **Example team file.** Replace the bracketed bits with your reality.

---

## What Engineering owns

- Architecture, infrastructure, and the production platform.
- Customer integrations and technical delivery.
- Code review, incident response, and reliability.

## Anti-scope — what Engineering does NOT own

- Feature prioritization (Product owns the roadmap; Engineering sizes and builds).
- Commercial commitments and delivery dates promised to customers (Sales / Customer Success own the customer relationship; Engineering signs off on feasibility).

## Hard rules

1. **No delivery date leaves the building without Engineering's sign-off.** Sales routes integration scoping here first.
2. **Incidents are written up as signals** with a timeline and a root cause, citing the relevant PRs and logs. No blameless-postmortem detail leaks individual performance into team-facing files.
3. **Credentials, API keys, and tokens never enter the repo.** See [`company-rules.md`](../company-rules.md) → "What the system MUST NEVER do".

## What Engineering's signals and recaps must never contain

- Secrets, tokens, connection strings, or customer credentials.
- Individual performance assessments dressed up as incident analysis.

## Escalation path

- Production incident → on-call lead, then Engineering Lead.
- Security concern → Engineering Lead + CEO(s).
- Scope conflict with a committed deal → Engineering Lead + Sales Lead.
