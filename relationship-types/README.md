# relationship-types/

One MD per **kind of commercial relationship** `<COMPANY>` has with an external entity. Wiki-link target so `companies/<X>.md` declares `relationship: [[<type>]]` and a local Markdown editor can navigate / filter / inverse-view by type.

This is a structured wiki-link relation that:
- Renders in the editor's Properties panel as `Relationship → Customer` clickable.
- Lights up the inverse view on each type page (`partner.md` → ← Referenced by the companies of that type).
- Makes "show me all partners" a one-click query for any agent.

---

## This is a CLOSED, fixed enum

The set below is the **entire** list of relationship types. It is a fixed enum:

- **Never create a new entry.** Do not add a new type because a company "feels different". B2B engagement maps onto the set below.
- **No `_TEMPLATE.md`.** There is nothing to template — the enum is complete.
- **Extending the enum is canon-level work.** If a genuinely new kind of relationship is needed, it requires a **canon PR** (the same gate as any `canon/**` change), with justification, never a direct push.

The five **commercial** types:

- **[[customer]]** — paying for the product / service.
- **[[prospect]]** — in active sales conversation but not signed yet.
- **[[partner]]** — co-sell / co-deliver relationship (SIs, channel partners, technology alliances).
- **[[vendor]]** — provides goods or services `<COMPANY>` consumes (freelancers, tools, agencies).
- **[[advisor]]** — provides counsel (formal advisory board members, informal mentors).

Plus one **non-commercial** type for competitive intelligence:

- **[[competitor]]** — a company tracked for competitive intelligence, not a commercial relationship. Special-cased in [`../canon/anatomy.md`](../canon/anatomy.md) → "Body asymmetry": a `companies/<slug>.md` with `relationship: [[competitor]]` carries **no `crm_url`** (competitors aren't in your CRM by design) and `industry` is optional.

A `churned` ex-customer keeps `relationship: [[customer]]` and adds a body note about the churn — they're still classified as customer historically. We don't model `churned` as a separate type to avoid bookkeeping.

---

## File convention

`<slug>.md`, lowercase. The slugs are fixed: `customer.md`, `prospect.md`, `partner.md`, `vendor.md`, `advisor.md`, `competitor.md`.

### Front matter

```yaml
---
name: <Type name>
slug: <type-slug>
aliases: []                      # alternate phrasings for this relationship type
companies: []                    # manual reverse of companies.relationship — companies of this type
                                 #   - "[[<company-slug>]]"
---
```

### Body

```markdown
# <Type name>

## About

1-2 sentences: what this commercial relationship implies for `<COMPANY>`.
```

That's it — the relationship type is a tag plus the explicit list of companies of that type.

---

## Inverse view

The `companies` field is a **manually-maintained reverse relation** of `companies/<X>.md → relationship: [[<slug>]]`. A local Markdown editor renders it as a dedicated "Companies" section on the relationship-type page. This is the reverse-edge double-write: when a company adopts this type, update both files in the same change. New incoming signals citing the type still surface in the editor's generic Referenced by view.
