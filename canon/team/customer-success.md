# Customer Success Rules

> Team: Customer Success. Lead: `<Customer Success Lead>` (file in [`people/`](../../people/)).
> Takes effect under the company rules. If in conflict with [`company-rules.md`](../company-rules.md), the company rules win.
> Only the Customer Success Lead may edit this file.
>
> **Example team file.** Replace the bracketed bits with your reality. (In a delivery-heavy business this is where onboarding, configuration, and customer ops live.)

---

## What Customer Success owns

- Onboarding, adoption, and retention.
- Customer delivery, configuration, and ongoing support.
- Standing customer context: which accounts are live, in what phase, with what open issues.

## Anti-scope — what Customer Success does NOT own

- The commercial relationship and renewals pricing (Sales / Leadership).
- Platform architecture and incidents (Engineering).

## Hard rules

1. **Customer-facing deliverables ship from the customer's hub in your docs system / file storage, never as repo files.** The repo holds the stable `companies/<slug>.md` entity and the generic skill, not per-customer working folders.
2. **Standing account context lives on `companies/<slug>.md`** (light body) plus the CRM and an optional `docs_url`; live project status lives in your docs system, not in entity bodies.
3. **Every customer touch that changes the picture is logged as a signal.**

## What Customer Success's signals and recaps must never contain

- Customer-specific contract values, discounts, or NDA-protected internal plans.
- One customer's confidential information in another customer's context.

## Escalation path

- At-risk / churn signal → Customer Success Lead + Sales.
- Technical blocker in delivery → Engineering Lead.
- Scope or commercial dispute → Sales Lead + CEO(s).
